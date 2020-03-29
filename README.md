# src-mp

Consume source maps from the command line.

```
$ npx src-mp functions/index.js.map 6 550
webpack:///node_modules/agentkeepalive/lib/https_agent.js:18:13

  16 |     this.maxCachedSessions = this.options.maxCachedSessions;
  17 |     /* istanbul ignore next */
> 18 |     if (this.maxCachedSessions === undefined) {
     |             ^
  19 |       this.maxCachedSessions = 100;
  20 |     }
  21 |

```

## Objective

The objective of this cli is to easily allow to see what code is behind a line and column of a transpiled code.

With the rise of transpilers (and compilers) for server code, I often found myself debuging stack traces with incomprenhensible combination of lines and column. I would always resort to installing [`source-map`](https://github.com/mozilla/source-map) and manually consumming the source map. This tool does just that, but without the need running manually the code.


## Example

If you are using [razzle](https://github.com/jaredpalmer/razzle), [typescript](https://www.typescriptlang.org/) and frebase, reading the logs from the command line tools is something like the following:

```
$ firebase functions:log
2020-03-26T03:08:30.256Z ? myFunction: Unhandled error TypeError: u is not a function
2020-03-26T03:08:30.256Z ? myFunction:     at c.addPersonalization (/srv/functions/index.js:199:115683)
2020-03-26T03:08:30.256Z ? myFunction:     at c.addTo (/srv/functions/index.js:199:115881)
2020-03-26T03:08:30.256Z ? myFunction:     at e.exports.deepMergeDynamicTemplateData (/srv/functions/index.js:121:57684)
2020-03-26T03:08:30.256Z ? myFunction:     at c.applyDynamicTemplateData (/srv/functions/index.js:199:116432)
2020-03-26T03:08:30.256Z ? myFunction:     at c.fromData (/srv/functions/index.js:199:114449)
2020-03-26T03:08:30.256Z ? myFunction:     at new c (/srv/functions/index.js:199:113313)
2020-03-26T03:08:30.256Z ? myFunction:     at Function.create (/srv/functions/index.js:199:119725)
2020-03-26T03:08:30.256Z ? myFunction:     at e.exports.send (/srv/functions/index.js:1:176323)
2020-03-26T03:08:30.256Z ? myFunction:     at /srv/functions/index.js:204:548418
2020-03-26T03:08:30.256Z ? myFunction:     at Generator.next (<anonymous>)
2020-03-26T03:08:30.256Z ? myFunction:     at o (/srv/functions/index.js:204:547402)
```

This tool helps you to understand where the error really was:

```
$ npx src-mp functions/index.js.map 199 115683
webpack:///node_modules/@sendgrid/helpers/classes/mail.js:262:11

  260 |     //If this is dynamic, set dynamicTemplateData, or set substitutions
  261 |     if (this.isDynamic) {
> 262 |       this.applyDynamicTemplateData(personalization);
      |           ^
  263 |     }
  264 |     else {
  265 |       this.applySubstitutions(personalization);
```

