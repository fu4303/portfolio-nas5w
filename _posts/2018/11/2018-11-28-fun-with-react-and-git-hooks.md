---
layout: article
title: Fun with React and Git Hooks!
date: 2018-11-28 20:00:00+0200
coverPhoto: https://upload.wikimedia.org/wikipedia/commons/a/a7/React-icon.svg
---

One topic I have gotten more and more excited about throughout my software development career is quality! Perhaps I've been burned one too many times. Alas, I decided to test adding git hooks to a React project using the `husky` package. My goal was to make it so that, prior to either committing code or pushing to a git repository, both the `eslint` linter and `jest` test suite must run. The code repository that accompanies this post can be found [here](https://github.com/nas5w/fun-with-react-and-git-hooks).

## Set Up from Scratch

Setting this up from scratch turned out to be fairly trivial. I started out by boostapping with `create-react-app`.

```bash
create-react-app fun-with-git-hooks
cd fun-with-git-hooks
```

Next, I installed [husky](https://github.com/typicode/husky), which claims to be "git hooks made easy." (Accurate!). Since it's only necessary in the dev environment, only install it as a dev dependency.

```bash
npm install husky --save-dev
```

We actually end up needing one additional dev dependency called `cross-env`, which will allow us to configure a CI environment variable in whatever environment we're currently in.

```bash
npm install cross-env --save-dev
```

Finally, let's make some modifications to our `package.json` file to accomplish a few things:

- Reconfigure `jest` tests to be run in Continuous Integration mode
- Add a linting command (we didn't have to install `eslint` seperately as it bootstraps with `create-react-app`)
- Configure our `husky` hooks to first lint and then test

```json
"scripts": {
  "start": "react-scripts start",
  "build": "react-scripts build",
  "test": "cross-env CI=true react-scripts test --env=jsdom",
  "eject": "react-scripts eject",
  "lint": "eslint src"
},
"husky": {
  "hooks": {
    "pre-commit": "npm run lint && npm test",
    "pre-push": "npm run lint && npm test"
  }
}
```

And that's it! Now, whenever you try to commit or push your code, you will be prevented from doing so if linting or testing fails.

Quality for the win!
