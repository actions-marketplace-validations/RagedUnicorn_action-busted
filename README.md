# action-busted

> A GitHub action that uses busted to run Lua unit tests on a codebase or specific spec files

# How to use

#### Basic usage

```yaml
on: [push]

jobs:
  test-busted:
    runs-on: ubuntu-latest
    name: Busted tests
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4
      - name: Run Busted
        uses: RagedUnicorn/action-busted@[version]
```

#### Custom Path and Arguments

```yaml
on: [push]

jobs:
  test-busted:
    runs-on: ubuntu-latest
    name: Busted tests
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4
      - name: Run Busted
        uses: RagedUnicorn/action-busted@[version]
        with:
            files: spec/
            args: --verbose --output=TAP
```

# Inputs

| Name    | Required | Default | Description                                                                                  |
| ------- | -------- | ------- | -------------------------------------------------------------------------------------------- |
| `files` | no       | `.`     | Spec files or directories to run busted against. Paths are relative to the workspace root.   |
| `args`  | no       | `''`    | Additional command-line arguments forwarded to busted (e.g. `--verbose`, `--output=junit`).  |

# Notes

- This action runs as a Docker container action and is therefore **Linux-only** (use `runs-on: ubuntu-latest` or any other Linux runner).
- It pulls [`ghcr.io/ragedunicorn/docker-busted`](https://github.com/RagedUnicorn/docker-busted) and forwards the inputs to busted, so the busted version is pinned by the action version. To upgrade busted, upgrade the action.
- A `.busted` configuration file in the repository root is picked up automatically by busted.

# License

MIT License

Copyright (c) 2026 Michael Wiesendanger

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
"Software"), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE
LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION
OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION
WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
