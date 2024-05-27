A commit message should start with a category of change. You can pretty much use the following 4 categories for everything: feat, fix, refactor, and chore.

- `feat`: is for adding a new feature
- `fix`: is for fixing a bug
- `refactor`: is for changing code for peformance or convenience purpose (e.g. readibility)
- `chore`: is for everything else (writing documentation, formatting, adding tests, cleaning useless code etc.)

`git commit -m '<category: do something; do some other things>'`

#### Examples:

- `git commit -m 'feat: add new button component; add new button components to templates'`
- `git commit -m 'fix: add the stop directive to button component to prevent propagation'`
- `git commit -m 'refactor: rewrite button component in TypeScript'`
- `git commit -m 'chore: write button documentation'`
