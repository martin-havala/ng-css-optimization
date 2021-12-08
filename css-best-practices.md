# CSS best practices

Hahah! I wanted to write a nice summary, but a nice list is already there by [before-semicolon](https://medium.com/before-semicolon/50-css-best-practices-guidelines-to-write-better-css-c60807e9eee2).

Nevertheless from the  best practices on which to focus while writing CSS I suggest to refresh these:

 - no unnecessary selectors (tags)  ~~ul~~.list  
 - no redundant ancestors (~~html div table tr td~~)
 - no chaining (.pretty.little.thing => pretty-little-thing)  
 - eliminate nesting (don't over-specify your selectors)  
   E.g. [Jon Rohan](https://vimeo.com/54990931s) suggests to stack maximum of 3 levels.
 - eliminate useless HTML tags
 - use [CSS shorthands](https://developer.mozilla.org/en-US/docs/Web/CSS/Shorthand_properties#see_also)
 - create utility classes for often duplicated code / common tweaks  
   A good example here could be any flex layout, where you usually need to specify direction / alignment / flow e.g. for centering of the content... and this is duplicated so many times throughout the app.
 - use the RIGHT-most selector as specific as possible
  This one is actually a browser-first approach. As browsers walk the DOM, they try to match each style selector from the element they are currently rendering. That means less specific rightmost selector = more style rules to evaluate.



## tl;dr;

- [Organizing your CSS](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Organizing)
- [before-semicolon](https://medium.com/before-semicolon/50-css-best-practices-guidelines-to-write-better-css-c60807e9eee2)
- [Jon Rohan : GitHub's CSS Performance](https://vimeo.com/54990931s)
- [OOCSS](https://github.com/stubbornella/oocss/wiki)