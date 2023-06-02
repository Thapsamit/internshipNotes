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
