# [Bower](https://bower.io/) 已過時

## How to install

```bash
npm install -g bower
```

## Command

```bash
# installs the project dependencies listed in bower.json
bower install
# registered package
bower install jquery
# GitHub shorthand
bower install desandro/masonry
# Git endpoint
bower install git://github.com/user/package.git
# URL
bower install http://example.com/script.js
```

### 使用npm的registry [bower-npm-reolver](https://github.com/mjeanroy/bower-npm-resolver)

A [custom Bower resolver](http://bower.io/docs/pluggable-resolvers/) supporting installation of NPM packages.
This resolver should be used if the package (or the version of the package you want to use) is not available on default
resolvers (bower registry, github, etc.).

#### Install

```bash
npm install bower-npm-resolver
```

#### Configuration and usage

Add the resolver in your `.bowerrc` file:

```json
{
  "resolvers": [
    "bower-npm-resolver"
  ]
}
```

Once configured, your `bower.json` files may reference packages using `npm:` prefix:

```json
{
  "dependencies": {
    "jquery": "npm:jquery#1.0.0",
    "lodash": "4.0.0"
  }
}
```

Note that you can also specify scope package:

#### [With detail settings](https://github.com/mjeanroy/bower-npm-resolver/blob/master/README.md)