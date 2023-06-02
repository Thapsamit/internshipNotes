## Create a yml file for workflows


- Create a .github folder in project root directory
- create workflows folder 
- create yml file like node.yml

### name.yml file content

```yml
name: Project name
on:
  push:
    branches:[master]
  pull_request:
    branches:[master]
jobs:
   build:
    runs-on: ubuntu-latest
    steps: 
      - uses: actions/checkout@v2
      - uses: actions/setup-node@v2
        with:
          node-version:'16'
      - run: npm install
      - run: npm run build --prefix client
```

- push the file along with project in github repo



##Common Build Pipeline Errors
Heads up! If your build pipeline is failing at the "run npm install" step with the following error:

Error: Process completed with exit code 137.

This happens when the build server runs out of memory during the install step, and terminates the build process.

To fix the issue, look for the following line in your server/package.json and client/package.json:

"nasa-project": "file:..",

If you see this line, remove it. It is sometimes unintentionally added by your development environment, and it creates a circular dependency. The install will take up more and more memory until the build fails!

With this line removed from all package.json files, you should no longer get this common error. Woohoooo!
