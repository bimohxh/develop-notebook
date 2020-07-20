<!--
 * @Author: bimohxh
 * @Date: 2020-07-17 15:31:16
 * @LastEditTime: 2020-07-17 16:04:02
 * @LastEditors: Please set LastEditors
 * @Description: In User Settings Edit
 * @FilePath: \develop-notebook\Git-Work-Flow.MD
-->

# Pre Commit Hook

We must ensure some condition before commit. We need `husky` to add git hook, just install it

```
npm install --save-dev husky
```

## Code Syntax Checking

Different language and framework use different check tool. for example `vue` add following config to package.json

```json
{
  "husky": {
    "hooks": {
      "pre-commit": "npm run lint"
    }
  }
}
```
this will lead all files in this project to be lint, which will need much time, and `lint-staged` can lint the files just be staged. first install

```
npm install --save-dev lint-staged
```

then `pakckage.json` should be changed to 

```json
"husky": {
  "hooks": {
    "pre-commit": "lint-staged"
  }
},
"lint-staged": {
  "*.{js,vue}": [
    "vue-cli-service lint",
    "git add"
  ]
}
```
Refer https://cli.vuejs.org/zh/guide/cli-service.html#git-hook


## Commit Message Fomart Checking

> Formated commit messages will bring us very benefits.

install [`commitlint`](https://commitlint.js.org/) and init configuration file

```
npm install --save-dev @commitlint/{cli,config-conventional}
echo "module.exports = {extends: ['@commitlint/config-conventional']};" > commitlint.config.js
```

add config in `package.json`

```json
{
  "husky": {
    "hooks": {
      "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
    }
  }
}
```

At last, the package.json will like

```json
{
  ...
  "husky": {
      "hooks": {
        "pre-commit": "lint-staged",
        "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
      }
    },
  "lint-staged": {
    "*.{js,vue}": [
      "vue-cli-service lint",
      "git add"
    ]
  }
  ...
}
```

# Tools

> Somtimes we need tools to help us complete these works.

## Git Commit Fomarter Tool

1、install [`commitizen`](https://github.com/commitizen/cz-cli) globaly

```
npm install -g commitizen
```

2、run following commander to enable Angular commit message format

```
commitizen init cz-conventional-changelog --save-dev --save-exact
```
if you run this command fail, you can run following commands instead

```
npm install --save-dev cz-conventional-changelog
```

add config in package.json

```
"config": {
  "commitizen": {
    "path": "cz-conventional-changelog"
  }
}
```

3、just use `git cz` replace `git commit` command.


## Git ChangeLog Generate Tool

install [`conventional-changelog`](https://github.com/conventional-changelog/conventional-changelog/tree/master/packages/conventional-changelog-cli) gloably

```
npm install -g conventional-changelog-cli
```

then generate changelog file at `CHANGELOG.md`

```
conventional-changelog -p angular -i CHANGELOG.md -s -r 0
```

add the command to `scripts` field in package.json for convenience

```json
{
  "scripts": {
    "changelog": "conventional-changelog -p angular -i CHANGELOG.md -s -r 0"
  }
}
```

Tips
- `conventional-changelog` will only show `Feature`, `Fix`, `Performance Improvement` or `Breaking Changes` commit types.
- must use `npm version xxx` to change version, or all the changelogs will be grouped at the same version.


recommand work flow

- change code.
- `git cz` commit.
- `npm version 1.x.x` change version and add tag.
- `npm run changelog` generate changelog.
- `git cz` push changelog (choose `docs` type and this will no appeared in changelog file itself).
- `git push origin v1.x.x` push above tag to online.


# Standard

## [Semver Version](https://semver.org/lang/zh-CN/)

Given a version number MAJOR.MINOR.PATCH, increment the:

- `MAJOR` version when you make incompatible API changes,
- `MINOR` version when you add functionality in a backwards compatible manner, and
- `PATCH` version when you make backwards compatible bug fixes.

Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.

Examples
- `major`  3.1.0 -> 4.0.0
- `premajor` 2.1.0 –> 3.0.0-0
- `minor` 2.0.1 -> 2.1.0
- `prepatch` 3.0.0–>3.0.1-0
- `patch` 2.0.0-0 -> 2.0.0 / 2.0.0 -> 2.0.1


Best practices

- The simplest thing to do is start your initial development release at `0.1.0`
-  your software is being used in production, it should probably already be `1.0.0`


# Refer
- http://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html
- https://github.com/angular/angular/blob/master/CONTRIBUTING.md
