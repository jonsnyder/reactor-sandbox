# extension-support-sandbox
[![Build Status][status-image]][status-url] [![NPM version][npm-image]][npm-url] [![NPM Dependencies][npm-dependencies-image]][npm-dependencies-url]

This project provides a sandbox in which you can manually test your extension. You can test both (1) your views that will eventually appear in the DTM application and (2) your library logic that will eventually run on the user's website.

## Installing the Sandbox

To use this project you will need to have [Node.js](https://nodejs.org/en/) installed on your computer. Do a Google search for finding the best method to install it on your computer's operating system. Once you install Node.js you will also have access to the [npm](https://www.npmjs.com/) package manager for JavaScript. You will need a version of npm greater than 2.7.0. You can check the installed version by running

```
npm -v
```

After you have installed Node.js on your machine, you will need to initialize your project. Create a folder for your project if you don't already have one. Inside the folder, run

```
npm init
```

You will need to provide the information requested on the screen. After this process is complete, you should have a file called `package.json` inside your folder.

You will then need to install both turbine and extension-support-sandbox and save them in your project's [`devDependencies`](https://docs.npmjs.com/files/package.json#devdependencies) by running
```
echo "@reactor:registry=https://artifactory.corp.adobe.com/artifactory/api/npm/npm-mcps-release-local/" > .npmrc
npm install @reactor/turbine @reactor/extension-support-sandbox --save-dev
```

While that will install the latest version, you my find a list of all versions under the [turbine package](https://artifactory.corp.adobe.com/artifactory/webapp/#/artifacts/browse/tree/General/npm-mcps-release-local/@reactor/turbine/-/@reactor) and [extension-support-sandbox package](https://artifactory.corp.adobe.com/artifactory/webapp/#/artifacts/browse/tree/General/npm-mcps-release-local/@reactor/extension-support-sandbox/-/@reactor) within Artifactory.

## Running the Sandbox

To run the sandbox, run `node_modules/.bin/reactor-sandbox` from the command line within your project's directory. A server will be started at [http://localhost:3000](http://localhost:3000). If you navigate to that address in your browser, you will see the view sandbox which allows you to test your views. You'll need to have previously created a view in your extension for it to show up in the sandbox. You can switch between views you would like to test using the controls toward the top of the page.

You may also click on the "Go to library sandbox" button at the top-right of the page to navigate to the library sandbox where you can test your library logic. See [Configuring the Sandbox](#configuring-the-sandbox) for how to configure the library sandbox for proper testing.

Rather than type the path to the `reactor-sandbox` script each time you would like the run the sandbox, you may wish to set up a [script alias](https://docs.npmjs.com/misc/scripts) by adding a `scripts` node to your `package.json` as follows:

```javascript
{
  ...
  "scripts": {
    "sandbox": "reactor-sandbox"
  }
  ...
}
```

Once this is in place, you may then run the sandbox by executing the command `npm run sandbox` from the command line.

## Configuring the Sandbox

To configure the portion of the sandbox that allows you to test your library logic, you'll want to run `node_modules/.bin/reactor-sandbox init` from the command line within your project's directory. This will generate a directory within your project named `.sandbox` that contains two files you may edit to configure the sandbox:

  * `container.js` When DTM publishes a library, it consists of two parts: (1) a data structure that stores information about saved rules, data elements, and extension configuration and (2) an engine to operate on such a data structure. `container.js` is the data structure (not the engine) that you may modify to simulate saved rules, data elements, and extension configuration. This template will be used to produce a complete `container.js` (JavaScript from your extension will be added) which will be used in tandem with the Turbine engine inside the sandbox. If you need help understanding how to modify `container.js` for your needs, please let the DTM team know and we can help you out.
  * `libSandbox.html` includes some simple HTML along with script tags to load a complete `container.js` and the Turbine engine. `libSandbox.html` can be modified to manually test whatever you would like. For example, if you're testing that your awesome new "focus" event feature works, you can add a text input to the web page to ensure your dummy rule fires when a form element receives focus.

You are welcome to commit `.sandbox` to your version control repository.

[status-url]: https://dtm-builder.ut1.mcps.adobe.net/job/extension-support-sandbox
[status-image]: https://dtm-builder.ut1.mcps.adobe.net/buildStatus/icon?job=extension-support-sandbox
[npm-url]: https://artifactory.corp.adobe.com/artifactory/webapp/#/artifacts/browse/tree/General/npm-mcps-release-local/@reactor/extension-support-sandbox/-/@reactor
[npm-image]: https://dtm-builder.ut1.mcps.adobe.net/view/Reactor-Frontend/job/extension-support-sandbox/ws/badges/npm.svg
[npm-dependencies-url]: https://dtm-builder.ut1.mcps.adobe.net/view/Reactor-Frontend/job/extension-support-sandbox/ws/dependencies.txt
[npm-dependencies-image]: https://dtm-builder.ut1.mcps.adobe.net/view/Reactor-Frontend/job/extension-support-sandbox/ws/badges/dependencies.svg
