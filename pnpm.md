The are are two key uses of `npx`.

**Running executables inside your downloaded dependencies**
- For example `npx jest`.
- The pnpm equivalent is `pnpm exec jest`.

**Running executable commands in packages you want to download transiently**
- For example `npx create-react-app my-app`.
- The pnpm equivalent of this is `pnpm dlx create-react-app my-app`.

**Keeping exact dependencies**
- `.npmrc` -> `save-exact=true`
