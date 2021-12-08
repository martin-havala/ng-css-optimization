# 1. QuickCheck

I made a tiny project to test first three assumptions:
1. View encapsulation alters DOM tree generation  
 _Can we pick better?_
2. Global styles will compile to smaller bundle  
 _Why do we stuff everything into components?__
3. Compiled component JS may contain better formatted structure to optimize bundle size  
 _…or maybe not_

## Experiment
To validate my assumption I've created a [silly little project](./ng-project), containing component with following
 HTML template:
```html
<section class="wrapper">
  <h1 class="title">Title</h1>
  <p class="paragraph">
    this is the body and
    <span class="highlight">highlight</span>
  </p>
</section>
```
and CSS
```scss
.wrapper {
  .title { … }
  .paragraph {
    .highlight { … }
  }
}
```
... or a flattened version of CSS 
```scss
.wrapper { … }
.title { … }
.paragraph { … }
.highlight { … }
```
and I moved style definitions to global / components file while I also did play with Angular view encapsulation.

## Results

We could sum up results into small table (more detailed one can be found under [docs](./docs/experiment-results.xlsx)).

| #   | SCSS place | SCSS version | Encapsulation      | Package sizes |
| --- | ---------- | ------------ | ------------------ | ------------- |
| 1.  | component  | structured   | None / Shadow DOM  | 139.73 kB     |
| 2.  | component  | structured   | default - emulated | 139.87 kB     |
| 3.  | component  | flat         | None / Shadow DOM  | 139.69 kB     |
| 4.  | component  | flat         | default - emulated | 139.75 kB     |
| 6.  | global     | structured   | None / Shadow DOM  | 139.73 kB     |
| 6.  | global     | structured   | default - emulated | 139.71 kB     |
| 5.  | global     | flat         | default - emulated | 139.67 kB     |
|     |

Here I'd like to point out to several things:

1. Global stylesheet leads to a smaller bundle, as it doesn't need to take care about scoping. Moreover it's a plain CSS, which is in contrast to component styles packed in JS. Here we need some syntactic sugar for style definition & encapsulation variant.
2. Flattened CSS has always better result, which is a natural thing. The important thing to notice here is that Angular ads an specific attribute selector template `[_ngcontent-%COMP%]` to emulate view encapsulation. And it does it for **each** selector in the chain, so the redundancy is multiplied.  
   Excerpt from the compiled JS file looks like:
   ```css 
   .wrapper[_ngcontent-%COMP%] .paragraph[_ngcontent-%COMP%]   .highlight[_ngcontent-%COMP%]{text-decoration: …}
   ```
3. Even using global CSS the bundle size is affected by components encapsulation mode, as it may be part of JS file when using emulation _none / shadow DOM_.
4. When it comes to compiled component JS, component styles are already transpiled from SCSS into pure CSS, so no benefit if SCSS nesting applies here (not even in JIT compilation mode)

## Wrap up
Global stylesheet are naturally better than components one, and flattened styles can save much more space when you are forced to use default-emulated view encapsulation, as you'll omit generated virtual identifier attribute in the selector chain.