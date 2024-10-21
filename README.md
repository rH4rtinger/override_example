# Overrides example

This is a example project for npm `overrides` attribute in combination with `install-links` flag.

## Setup of the projects

Run `npm install`. This should automatically install in every child project.

The project `child` has one simple dependency `"@types/semver": "^6.x"`.


The `project_links_false` has set `install-links=false`. The `project_links_true` has set `install-links=true`.

Both projects have the same dependencies and forces an override of our given dependency to version `7.x`:

```json
 "overrides": {
    "@types/semver": "^7.x"
   },
  "dependencies": {
    "@types/semver": "^7.x",
    "child": "../child"
  }
```

### What is the problem:

When I run `npm ls --all` in my projects, I get different results:

The `project_links_true` with `install-links=true` produces the following result. Here, the `overrides` is respected for my child.

```
project_links_true@1.0.0 C:\dev\overrides_example\project_links_true
├── @types/semver@7.5.8 overridden
└─┬ child@1.0.0
  └── @types/semver@7.5.8 deduped
```


The `project_links_false` with `install-links=false` does not respect `overrides` and has a different version in its child dependencies.

```
project_links_false@1.0.0 C:\dev\overrides_example\project_links_false
├── @types/semver@7.5.8 overridden
└─┬ child@1.0.0 -> .\..\child
  └── @types/semver@6.2.7
```

