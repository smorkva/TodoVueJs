# todo-list

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

## Toolchain note

The components use `vue-property-decorator`, which is not listed in
`package.json`. Installing dependencies will not reproduce a working build
until that is resolved.

## Backstop note

Claire reconciles a merged pull request from its webhook, and from a poll a
minute later if the delivery never arrives.
