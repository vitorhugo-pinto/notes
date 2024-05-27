A git branch should start with a category. Pick one of these: feature, bugfix, hotfix, or test.

- `feature`: is for adding, refactoring or removing a feature
- `bugfix`: is for fixing a bug
- `hotfix`: is for changing code with a temporary solution and/or without following the usual process (usually because of an emergency)
- `test`: is for experimenting outside of an issue/ticket

After the category, there should be a "/" followed by the reference of the issue/ticket you are working on. If there's no reference, just add no-ref.

`git branch <category/reference/description-in-kebab-case>`

#### Examples:

- You need to add, refactor or remove a feature: `git branch feature/issue-42/create-new-button-component`
- You need to fix a bug: `git branch bugfix/issue-342/button-overlap-form-on-mobile`
- You need to fix a bug really fast (possibly with a temporary solution): `git branch hotfix/no-ref/registration-form-not-working`
- You need to experiment outside of an issue/ticket: `git branch test/no-ref/refactor-components-with-atomic-design`