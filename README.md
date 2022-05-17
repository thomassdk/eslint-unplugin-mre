# Minimal Reproducible Example - vite-plugin-eslint vs. unplugin-icons

There is a clash when trying to use the vite-plugin-eslint package with unplugin-icons in a svelte-vite-typescript project.

This repo produces a Minimal Reproducible Example of the error.

Steps to create the repo:

## Init the project 

Using the Vite Svelte Typescript Template:
``` shell
#+begin_src shell
~/code
❯ npm create vite@latest
✔ Project name: … eslint-unplugin-mre
✔ Select a framework: › svelte
✔ Select a variant: › svelte-ts

Scaffolding project in /home/thomassdk/code/eslint-unplugin-mre...

Done. Now run:

  cd eslint-unplugin-mre
  npm install
  npm run dev
#+end_src
```

## Install unplugin-icons:

``` shell
eslint-unplugin-mre on  main [!?] via  v16.15.0
❯ npm i -D unplugin-icons @iconify/json

added 50 packages, and audited 51 packages in 12s

15 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
```

Configure Vite:
``` javascript
// vite.config.js
import { defineConfig } from 'vite'
import { svelte } from '@sveltejs/vite-plugin-svelte'
import Icons from 'unplugin-icons/vite'

export default defineConfig({
  plugins: [
    svelte(),
    Icons({
      compiler: 'svelte',
    }),
  ],
})
```

At this point the project works importing icons however after configuring eslint importing the icons breaks

## Installing Eslint

``` shell
eslint-unplugin-mre on  main [!] via  v16.15.0 took 3s
❯ npm install eslint eslint-plugin-svelte3 @types/eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin vite-plugin-eslint --save-dev

added 3 packages, and audited 185 packages in 1s

34 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
```

``` shell
eslint-unplugin-mre on  main [!?] via  v16.15.0 took 14s
❯ npx eslint --init
You can also run this command directly using 'npm init @eslint/config'.
✔ How would you like to use ESLint? · problems
✔ What type of modules does your project use? · esm
✔ Which framework does your project use? · none
✔ Does your project use TypeScript? · No / Yes
✔ Where does your code run? · browser
✔ What format do you want your config file to be in? · JavaScript
The config that you've selected requires the following dependencies:

A config file was generated, but the config file itself may not follow your linting rules.
Successfully created .eslintrc.cjs file in /home/thomassdk/code/eslint-unplugin-mre
```

After configuring eslint to be compatible with svelte and adding the vite-plugin-eslint to the Vite config the project now fails to import an icon from unplugin-icons with the error:
``` shell
eslint-unplugin-mre on  main [!?] via  v16.15.0 took 1m1s 130
❯ npm run dev

> eslint-unplugin-mre@0.0.0 dev
> vite


  vite v2.9.9 dev server running at:

  > Local: http://localhost:3000/
  > Network: use `--host` to expose

  ready in 265ms.

No files matching '~icons/carbon/accessibility.svelte' were found.
10:04:37 PM [vite] Internal server error: No files matching '~icons/carbon/accessibility.svelte' were found.
  Plugin: vite-plugin-eslint
  File: ~icons/carbon/accessibility.svelte
      at FileEnumerator.iterateFiles (/home/thomassdk/code/eslint-unplugin-mre/node_modules/eslint/lib/cli-engine/file-enumerator.js:318:27)
      at iterateFiles.next (<anonymous>)
      at CLIEngine.executeOnFiles (/home/thomassdk/code/eslint-unplugin-mre/node_modules/eslint/lib/cli-engine/cli-engine.js:788:48)
      at ESLint.lintFiles (/home/thomassdk/code/eslint-unplugin-mre/node_modules/eslint/lib/eslint/eslint.js:550:23)
      at TransformContext.transform (file:///home/thomassdk/code/eslint-unplugin-mre/node_modules/vite-plugin-eslint/dist/index.mjs:97:35)
      at processTicksAndRejections (node:internal/process/task_queues:96:5)
      at async Object.transform (/home/thomassdk/code/eslint-unplugin-mre/node_modules/vite/dist/node/chunks/dep-59dc6e00.js:38900:30)
      at async doTransform (/home/thomassdk/code/eslint-unplugin-mre/node_modules/vite/dist/node/chunks/dep-59dc6e00.js:55857:29)
```
