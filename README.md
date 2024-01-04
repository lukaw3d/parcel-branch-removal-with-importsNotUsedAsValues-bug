# Parcel's branch removal doesn't combine with TypeScript's import elision

Current docs: https://github.com/parcel-bundler/website/blob/c0d46b1/src/features/production.md#development-branch-removal-branch-removal

Building [./a.ts](./a.ts) should be the same as building [./b.ts](./b.ts), but it isn't

```sh
$ yarn build
warning package.json: No license field
$ parcel build *.ts --no-source-maps
✨ Built in 595ms

dist/a.js    116 B     83ms
dist/b.js      0 B    112ms
Done in 0.96s.
```

- [./dist/a.js](./dist/a.js) includes (but doesn't use) the imported `@parcel/cache/lib/constants`.
- [./dist/b.js](./dist/b.js) is empty.

This looks as if Parcel is overriding TypeScript's import elision (verbatimModuleSyntax, importsNotUsedAsValues, preserveValueImports), but what probably happens is:
- compile TypeScript with import elision, while branch is there and import is used
- process JavaScript to remove unused branch
