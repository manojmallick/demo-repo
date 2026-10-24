# demo-repo

A tiny Node.js project whose tests and lint **fail on purpose**. It is a fixture for trying out CI-failure diagnosis and auto-fix tools (for example [copilot-ci-doctor](https://github.com/manojmallick/copilot-ci-doctor)): the bugs are small, obvious and different in kind, so a tool's fixes are easy to check.

> Do not copy this code as an example of good practice. Each file contains a bug marked with a `BUG:` comment.

## The planted bugs

| File | Bug | Caught by |
|---|---|---|
| `src/sum.js` | `sum(a, b)` returns `a - b` instead of `a + b` | `__tests__/sum.test.js` (2 of its 3 tests fail; `0 + 0` still passes by coincidence) |
| `src/formatName.js` | Joins first and last name without a space; also uses `var` | `__tests__/formatName.test.js` (both tests fail) and ESLint `no-var` |
| `src/fileStore.js` | Uses `var`, an unused variable and `==` instead of `===` | ESLint `no-var`, `no-unused-vars`, `eqeqeq` |

`readData` itself behaves correctly, so `__tests__/fileStore.test.js` passes; the problems in that file are lint-only.

## Run it

Requires Node.js 18 or newer.

```bash
npm install
npm test        # Jest: 4 of 7 tests fail (sum and formatName)
npm run lint    # ESLint: 5 errors (4 in fileStore.js, 1 in formatName.js)
```

Expected result today: both commands exit with errors. After fixing the bugs (`a + b`, `first + ' ' + last`, `const`/`let`, `===`, remove the unused variable) both pass.

## Project layout

```
src/            sum.js, formatName.js, fileStore.js (the buggy modules)
__tests__/      Jest tests, one file per module
jest.config.cjs Jest settings (cache in /tmp/jest-cache)
.eslintrc.json  ESLint rules: no-unused-vars, eqeqeq, no-var
```
