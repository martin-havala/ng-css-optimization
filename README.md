# ng-css-optimization

> ERNI senior project where I'd like to find a best practice how to keep [Angular](http://angular.io) project slim & maintainable when it comes to CSS.

As the web-app development goes on, any non-single-person team tends to duplicate style rules throughout the codebase. These later pollute users DOM and may lead to performance drops on different levels, from longer download times to rendering issues thanks to a bloated document.

## TLDR; _A Quick-check_

Usage of global stylesheet(s) is better than component ones for [several reasons](./quickcheck.md). Flattened styles can save even more space when you are forced to use default - __emulated view__ encapsulation,  as compiler will omit `_ngcontent-%COMP%`  identifier of each attribute in the selector chain.

More details from my small experiment are summarized [here](./quickcheck.md).

## Tools
From the list of tools I only recommend to use VS Code plugin [Unused CSS Classes for JavaScript/Angular/React](https://dev.to/dylanvdmerwe/reduce-angular-style-size-using-purgecss-to-remove-unused-styles-3b2k). Dot.Although [purge-css](https://purgecss.com/) may fix reduce your stylesheet sizes, you must be extra cautious about script-generated classes or complex selectors. But more on tools you can find [here](./tools.md).

## CSS optimizations

Some performance & bundle size optimization can be achieved also through [CSS Optimizations](./css-best-practices.md). Here I'd like to highlight three:
- eliminate nesting (evaluation get's complex and with emulated view encapsulation doubles the specificity for each selector level)
- use CSS variables, they have wide support already
- create utility classes for often duplicated code

More of them is listed within  [CSS Optimizations](./css-best-practices.md).

## Project setup
Some optimizations can already happen here. As most of the styles are imported when you're using some shared UI framework, just check the frameworks documentation.
> For example [Angular Material](https://material.angular.io/) theme styles can be generated globally for all components, but you can also select just a subset of imports by omitting ```@include mat.all-component-themes($my-theme)``` and using partial imports like  ``` @include mat.button-theme($my-theme)``` instead. For more information check the [official guide](https://material.angular.io/guide/theming#applying-a-theme-to-components).  
> [Bootstrap](https://ng-bootstrap.github.io/#/home) offers slightly different [modus operandi](https://ng-bootstrap.github.io/#/getting-started#imports).

# Approach shift

It still doesn't seem to be a common practice, but what if we instead of eliminating problems ex-post switched from agile-everything approach and put a bit more effort into design at the first place? It could be a basis of shared styles, which can be reused all around the application and staying code more DRY.

## Design system
From various definition, I would agree with one by [Audrey Hacq](https://uxdesign.cc/everything-you-need-to-know-about-design-systems-54b109851969):
> A Design System is the single source of truth which groups all the elements that will allow the teams to design, realize and develop a product. 
> 
### Visual language first
Basic design system deliverables consist of definitions of a visual identity, design elements and principles which should guarantee unified user experience across the products or applications.
From a developer perspective it means we can already build a solid foundation of CSS variables and low-level class definitions for later use. By using design system rules for:
- colors
- fonts
- spacing
- shapes
- icons

we can setup design primitives. Later, analyzing principles and patterns we could implement more complex rules either as a CSS classes, maybe even as a new reusable components.
Thinking about design system at first place hence may help us to prevent reoccurring definitions for layout and spacing, as these tend to be applied throughout most of the components.

As suggested by [Therese Fessenden](https://www.nngroup.com/articles/design-systems-101/) from Nielsen Norman Group, we don't necessarily need to build up whole design system on our own, rather we can think of three levels of implementation:
- adopt a design system:   
  We just reuse a design system created by 3rd party. In most cases here we're sacrificing branding and identity for a quick & cheap setup.
- adapt a design system:  
  Here we're still using a 3rd party design system, but we customize it to meet the desired look and feel. 
- create a design system:  
  All on your own. Expensive, but completely tailor-made.

These three may have a development counter-trio:  
- adopt a UI framework
- adapt a UI framework  
  Some libraries like [PrimeNG](https://www.primefaces.org/primeng/) provide wide range of possible customization out of the box, while others, e.g. [Angular Material](https://material.angular.io/) have this set much more limited and you may need to invest more time to get the desired result & debug it.
- create your own

These although do not match 1:1. You still may have completely tailor-made design system, but your development team may use ready-made framework for it's realization, even just to cover most of generic behaviors with ease.

# Wrap up

Please consider using a design system to speed up your development workflow. Although it may sound like a burden from the beginning, but even a medium sized project will benefit from it in later stages.