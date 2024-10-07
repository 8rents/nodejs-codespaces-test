# Node.js Codespaces Test

> ***Test node.js application to learn about using GitHub codespaces and GitHub Actions.***

---

## This Guide’s Table of Contents

1. Installing node.js on Windows without admin rights
2. Creating a Simple Node.js app
3. Setting up vscode for use with node.js
4. Customizing vscode for node.js
5. Writing GitHub Actions Workflow

---

## Installing node.js on Windows without admin rights

installing node on windows without admin can be a little bit tricky. Even using nvm-no-install.zip admin is required for a bit. 

The best method I’ve found is using FNM (fast node manager) installed using win get.  

> ### *Some node.js gotchas on Windows…*
>
> - *Using `nodejs`* installed by `scoop` simple scripts error out. 
> - Using `nvm` installed by scoop requires admin to use `nvm on`.
> - `nvm-no-install.zip` requires admin and getting into the system properties panel.

#### Installing on Windows using “Fast Node Manager”

```powershell
# installs fnm (Fast Node Manager)
winget install Schniz.fnm

# configure fnm environment
# MAKE SURE TO OPEN A NEW COMMAND LINE WINDOW HERE
fnm env --use-on-cd | Out-String | Invoke-Expression

# download and install Node.js
fnm use --install-if-missing 18

# verifies the right Node.js version is in the environment
node -v # should print `v18.20.4`

# verifies the right npm version is in the environment
npm -v # should print `10.7.0`
```

**Finally a working version of node on Windows!**

---

## Creating a Simple Node.js app

We are going to create a very simple web server in node.js

1. Create a folder to hold your Project. I made mine in `~/dev/nodejs-codespaces-test`.

2. In your Terminal of choice navigate to this directory.

   ```bash
   cd ~/dev/nodejs-codespaces-test
   ```

3. Create a file called `server.js` inside the application folder.

4. Add the following to the file:  `~/dev/nodejs-codespaces-test/server.js`

    ```javascript
const { createServer } = require('node:http');
const hostname = '127.0.0.1';
const port = 3000;
const server = createServer((req, res) => {
    res.statusCode = 200;
    res.setHeader('Content-Type', 'text/plain');
    res.end('Hello World');
});
server.listen(port, hostname, () => {
    console.log(`Server running at http://${hostname}:${port}/`);
});
```

#### Running the `server.js` file from the terminal

In the terminal make sure you are inside the `~/dev/nodejs-codespaces-test` and run the `server.js` file.

```bash
node server.js
```

You should now see the feedback:

```bash
Server running at http://127.0.0.1:3000/
```

---

### Creating a `package.json` file

A `package.json` file is like the central file of any project. It directs the commands issued to `npm` and allows abstracting other facets. 

In terminal run `npm init`.

You can optionally add a `-y` to answer yes to all the questions default answers. **I find it’s usually better to simply go through the interactive set up and alter the answers as needed.**\

Now to run the `server.js` file again, we don’t need to type `node server.js` we can simply type: `npm start`.

### Initializing a git repo and uploading it to GitHub

Run `git init` to create a git repository out of the directory or create new repository in *GitHub Desktop*.

Once the repo is created `add` any changes, `commit` them and then `push` them to GitHub.

If you were working on the command line it would look like this:

```bash
# add all changes to be committed
git add -a

# commit all added changes
git commit -m "initial import"

# push the changes to github
git push origin main
```

It’s even easier with *GitHub Desktop*.

## Setting up vscode for use with node.js

You have a few different options for trying to get vsCode installed on your Windows computer:

### Installing vsCode with either scoop or winget

Installing **with Scoop:**

```powershell
scoop install vscode
```

If you have some issues with the `scoop` installed version try uninstalling and then reinstalling with `winget`.

**Uninstalling Scoop installed vscode**

```powershell
scoop uninstall vscode
```

And installing it **With WinGet**:

```powershell
winget install Microsoft.VisualStudioCode
```

Hopefully that should work better… If not and you’re still running into issues with either of these installs, try simply using [vscode.dev](https://vscode.dev) which will run directly in your browser.

---

### Customizing vscode for node.js

> ***Mostly taken from the guide:** [Node.js tutorial in Visual Studio Code](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial)*

Essential Extensions & Downloads so far:

- My Favorite Theme: [One Dark Pro](https://vscode.dev/github/8rents/nodejs-codespaces-test/blob/main) & Custom [File Icons](https://marketplace.visualstudio.com/items?itemName=file-icons.file-icons)
- AI Assessed Development: [IntelliCode](https://marketplace.visualstudio.com/items?itemName=VisualStudioExptTeam.vscodeintellicode) & Intellisense: [Path names](https://marketplace.visualstudio.com/items?itemName=christian-kohler.path-intellisense), [Node Modules](https://marketplace.visualstudio.com/items?itemName=leizongmin.node-module-intellisense), [SCSS](https://marketplace.visualstudio.com/items?itemName=mrmlnc.vscode-scss)
- Code Linter & Formatter: [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode),. [ES Lint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint), [Beautify](https://marketplace.visualstudio.com/items?itemName=HookyQR.beautify)
- Language Support: [EditorConfig](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig), [YAML](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml), [PowerShell](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell), [Markdown](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one), [GitGraph](https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph)

---

## Writing GitHub Actions Workflow

>  *Taken from the Guide: [Building and testing Node.js Apps on GitHub](https://docs.github.com/en/actions/use-cases-and-examples/building-and-testing/building-and-testing-nodejs)*

### Creating a Continuous Integration Workflow

#### File: `.github/workflows/node.js.yml`

>  *This file can be created in the main repository page under “actions”. Select a Continuous Integration Workflow and then select Node.*

This file will test version `18`, `20` & `22` of Node.js when ran.

```yaml
# This workflow will do a clean installation of node dependencies, cache/restore them, build the source code and run tests across different versions of node

# For more information see: https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-nodejs

# Workflow Name / Type
name: Node.js CI
    strategy:
      matrix:
      	# Which node.js version should be supported?
      	# Test in 3 versions of Node
        node-version: [18.x, 20.x, 22.x]
    
    # What steps should this workflow follow?
    steps:
    - uses: actions/checkout@v4
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v4
      with: 
        node-version: ${{ matrix.node-version }}
        cache: 'npm'
    
    # NPM Scripts
    - run: npm ci
    - run: npm run build --if-present
    - run: npm test

```

### Testing on One or more than One versions of Node.js

Change the yaml string in the `node.js.yml` file.

#### Multiple Versions

```yaml
node-version: [18.x, 20.x, 22.x]
```

#### Tests in a Single version of Node

```yaml
node-version: '22.x'
```

### Installing Node Package Dependencies

Most node.js apps will need to install dependencies using a package manager like `npm`, `yarn` or `pnpm`. Here’s how to do that with a GitHub Runner. With this approach you also have the ability to [cache dependencies to speed up development](https://docs.github.com/en/actions/using-workflows/caching-dependencies-to-speed-up-workflows).

#### Example using `npm`

```yaml
steps:
- uses: actions/checkout@v4
- name: Use Node.js
  uses: actions/setup-node@v4
  with:
    node-version: '20.x'
- name: Install dependencies
  run: npm ci
```

Using `npm install` installs the dependencies defined in the *`package.json`* file. For more information, see [`npm install`](https://docs.npmjs.com/cli/install).

#### Example using `yarn`

This example installs the dependencies defined in the *yarn.lock* file and prevents updates to the *`yarn.lock`* file. For more information, see [`yarn install`](https://yarnpkg.com/en/docs/cli/install).

```yaml
steps:
- uses: actions/checkout@v4
- name: Use Node.js
  uses: actions/setup-node@v4
  with:
    always-auth: true
    node-version: '20.x'
    registry-url: https://registry.npmjs.org
    scope: '@octocat'
- name: Install dependencies
  run: npm ci
  env:
    NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
```

#### Example using `pnpm`

#### Example caching dependencies

You can cache and restore the dependencies using the [`setup-node` action](https://github.com/actions/setup-node).

The following example caches dependencies for `npm`.

```yaml
steps:
- uses: actions/checkout@v4
- uses: actions/setup-node@v4
  with:
    node-version: '20'
    cache: 'npm'
- run: npm install
- run: npm test
```

## Up Next:

[Publishing Node Packages on CI completion](https://docs.github.com/en/actions/use-cases-and-examples/publishing-packages/publishing-nodejs-packages)

---

## External Links & References

1. [Introduction to Node.js](https://nodejs.org/en/learn/getting-started/introduction-to-nodejs)
2. [Setting up node.js on Windows without admin rights](https://yumingchang1991.medium.com/set-up-nodejs-without-administrator-right-in-2023-29e2abd05852)
3. [Using vsCode as your Node.js IDE](https://code.visualstudio.com/docs/nodejs/nodejs-tutorial)
4. [Introducing asynchronous JavaScript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Introducing)
5. [Building and testing Node.js Apps on GitHub](https://docs.github.com/en/actions/use-cases-and-examples/building-and-testing/building-and-testing-nodejs)
6. [Writing GitHub Workflows](https://docs.github.com/en/actions/writing-workflows)
7. [Understanding GitHub Actions](https://docs.github.com/en/actions/about-github-actions/understanding-github-actions)

---

**🤍2024 [Brenton Holiday](https://github.com/8rents)**
