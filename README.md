# ng-css-optimization

> ERNI senior project where I'd like to find a best practice how to keep [Angular](http://angular.io) project slim & maintainable when it comes to CSS.

As the web-app development goes on, any non-single-person team tends to duplicate style rules throughout the codebase. These later pollute users DOM and may lead to performance drops on different levels, from longer download times to rendering issues thanks to a bloated document.

## TLDR; _A Quickcheck_

Usage of global stylesheet(s) is better than component ones for [several reasons](./quickcheck.md). Flattened styles can save even more space when you are forced to use default - __emulated view__ encapsulation,  as compiler will omit `_ngcontent-%COMP%`  identifier of each attribute in the selector chain.

More details from my small experiment are summarized [here](./quickcheck.md).

## Tools
From the list of tools I only recommend to use VS Code plugin [Unused CSS Classes for JavaScript/Angular/React](https://dev.to/dylanvdmerwe/reduce-angular-style-size-using-purgecss-to-remove-unused-styles-3b2k). Dot.Although [purge-css](https://purgecss.com/) may fix reduce your stylesheet sizes, you must be extra cautious about script-generated classes or complex selectors. But more on tools you can find [here](./tools.md).

## Project setup
Some optimizations can already happen here. As most of the styles are imported when you're using some shared UI framework, just check the frameworks documentation.
> For example [Angular Material](https://material.angular.io/) theme styles can be generated globally for all components, but you can also select just a subset of imports by omitting ```@include mat.all-component-themes($my-theme)``` and using partial imports like  ``` @include mat.button-theme($my-theme)``` instead. For more information check the [official guide](https://material.angular.io/guide/theming#applying-a-theme-to-components).  
> [Bootstrap](https://ng-bootstrap.github.io/#/home) offers alternative [approach](https://ng-bootstrap.github.io/#/getting-started#imports).

# Approach shift

It still doesn't seem to be a common practice, but what if we insted of eliminating problems ex-post switch from agile-everything and put a bit more effort into design at the first place? It could be a basis of shared styles, which can be reused all around the application.

## Design system
From various definition, I would agree with one by [Audrey Hacq](https://uxdesign.cc/everything-you-need-to-know-about-design-systems-54b109851969):
> A Design System is the single source of truth which groups all the elements that will allow the teams to design, realize and develop a product.
> 
### Visual language first
One of basic deliverables of a good design system is definition of a visual identity and design elements and principles which should guarantee unified user experience across the products / applications.
From a developer perspective it means we can build a solid base of CSS variables and classes for later use. Design system provides us with low-level things like:
- colors
- fonts
- spacing 
- shapes
- icons

but also advanced principles and patterns for components layout, user interaction (animations / transitions).
Thinking about design system at first place may help to prevent reoccurring definitions for layout and spacing, as these tend to be applied throughout most of the components.