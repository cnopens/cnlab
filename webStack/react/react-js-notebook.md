#react-js-notebook.md

---

## install && init react app project

1. npx

npx create-react-app my-app

(npx comes with npm 5.2+ and higher, see instructions for older npm versions)


2. npm

npm init react-app my-app

npm init <initializer> is available in npm 6+


3. Yarn

yarn create react-app my-app

yarn create is available in Yarn 0.25+


## Selecting a template
npx create-react-app my-app --template [template-name]

You can find a list of available templates by searching for "cra-template-*" on npm.

https://www.npmjs.com/search?q=cra-template-*

Our Custom Templates documentation describes how you can build your own template.


## Creating a TypeScript app

You can start a new TypeScript app using templates. To use our provided TypeScript template, append --template typescript to the creation command.

npx create-react-app my-app --template typescript

If you already have a project and would like to add TypeScript, see our Adding TypeScript documentation.

https://create-react-app.dev/docs/adding-typescript
If you already have a project and would like to add TypeScript, see our Adding TypeScript documentation.


## Selecting a package manager

When you create a new app, the CLI will use Yarn to install dependencies (when available). If you have Yarn installed, but would prefer to use npm, you can append --use-npm to the creation command. For example:

npx create-react-app my-app --use-npm

## Scripts

Inside the newly created project, you can run some built-in commands:
#
npm start or yarn start

Runs the app in development mode. Open http://localhost:3000 to view it in the browser.

The page will automatically reload if you make changes to the code. You will see the build errors and lint warnings in the console.

##npm test or yarn test

Read more about testing.(https://create-react-app.dev/docs/running-tests)
Runs the test watcher in an interactive mode. By default, runs tests related to files changed since the last commit.

Read more about testing.

## npm run build or yarn build
Builds the app for production to the build folder. It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.

Your app is ready to be deployed.


## resources 

1.[quick start] (https://create-react-app.dev/docs/getting-started/)