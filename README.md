# Config Manager

[![Downloads][npm-downloads]][npm-url] [![version][npm-version]][npm-url]
[![Build Status][travis-status]][travis-url]

A [Probot](https://probot.github.io) plugin to manage configuration files effectively. 
Configuration files are validated by Joi and cached appropriately.

## Example


```typescript
# models 
import Joi = require("joi");

export interface IComment {
  comment: string;
  label: string;
}

export interface IConfig {
  comments?: IComment[];
}

//
// comments:
// - label: needs-area
//   comment: |
//     There is no area label added to this issue/PR.
//     Please add an area:<team> label
export const schema = Joi.object().keys({
  comments: Joi.array().items(
    Joi.object().keys({
      comment: Joi.string(),
      label: Joi.string()
    })
  )
});
```

```typescript
import { IConfig, schema } from "./models";
const configManager = new ConfigManager<IConfig>("comment.yml", {}, schema);
  app.on(events, async (context: Context) => {
    const config = await configManager.getConfig(context).catch(err => {
      context.log.error(err);
      return {} as IConfig;
    });
  });
```



## Contribute

If you have suggestions for how this bot could be improved, or want to report a bug, open an issue! We'd love all and any contributions.

[travis-status]: https://travis-ci.org/lswith/probot-config-manager.svg?branch=master
[travis-url]: https://travis-ci.org/lswith/probot-config-manager
[npm-downloads]: https://img.shields.io/npm/dm/probot-config-manager.svg?style=flat
[npm-version]: https://img.shields.io/npm/v/probot-config-manager.svg?style=flat
[npm-url]: https://www.npmjs.com/package/probot-config-manager
