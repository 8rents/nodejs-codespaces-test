# Node.js Codespaces Test

> ***Test node.js application to learn about using GitHub codespaces and GitHub Actions.***

---

## Documents Followed So Far

1. [Introduction to Node.js](https://nodejs.org/en/learn/getting-started/introduction-to-nodejs)
2. [Introducing asynchronous JavaScript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Introducing)
3. [Building and testing Node.js Apps on GitHub](https://docs.github.com/en/actions/use-cases-and-examples/building-and-testing/building-and-testing-nodejs)
4. [Writing GitHub Workflows](https://docs.github.com/en/actions/writing-workflows)
5. [Understanding GitHub Actions](https://docs.github.com/en/actions/about-github-actions/understanding-github-actions)

## Writing GitHub Actions Workflow

*Taken from the Guide: [Building and testing Node.js Apps on GitHub](https://docs.github.com/en/actions/use-cases-and-examples/building-and-testing/building-and-testing-nodejs)*

### Creating a Continuous Integration Workflow

#### File: `.github/workflows/node.js.yml`

>  *This file can be created in the main repository page under “actions”. Select a Continuous Integration Workflow and then select Node.*

This file will test version 18, 20 & 22 of Node.js when ran.

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

---

**🤍2024 [Brenton Holiday](https://github.com/8rents)**
