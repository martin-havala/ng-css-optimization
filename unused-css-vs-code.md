### ngx-unused-css

Small npm tool which checks your styles. It has a few bugs (at least for me it did not work for another project with very similar setup as the second one, for which it did, but i was not able to find a reason why).
Setup needs to define a small config file which could be initiated by calling

   > ngx-unused-css --init   

Also, it is better to say 'yes' to anything the tool provides and correct it afterwards. I had the lock and answered `no` to default main scss path and the initializer died ungracefully  ¯\\_(ツ)_/¯.

Basic config is a json file `.ngx-unused-css.json` and it looks like this one, again a small test has shown that all parameters are kinda important to have:  

```json
{
  "path": "src/app",
  "ignore": [
    ".mat-"
  ],
  "globalStyles": "src/theme/styles.scss",
  "styleExt": ".scss"
}
```


As you see here I did decide to ignore Angular Material library (default option)
and once I've tried it without this ignore option I got a list of all Material selectors, so no use here.

Although all these bugs it may provide a useful feedback when it comes to unused CSS rules.

```console
> ngx-unused-css
Unused CSS classes were found for the following files:

src\app\layout\header\header.component.html
src\app\layout\header\header.component.scss
╔═════════════════╗
║ #headerUserIcon ║
╚═════════════════╝
src\app\layout\navbar\navbar.component.html
src\app\layout\navbar\navbar.component.scss
╔══════════════════════╗
║ .navbar-center > div ║
╚══════════════════════╝
***** GLOBAL UNUSED CSS *****
***** GLOBAL UNUSED CSS *****
╔════════════════════════════════════╗
║ .cdk-visually-hidden               ║
║ .cdk-overlay-container             ║
║ .cdk-global-overlay-wrapper        ║
║ .cdk-overlay-container:empty       ║          
```