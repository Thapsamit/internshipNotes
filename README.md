# Basic Notes:-

### What is the difference between npm vs npx?
npm and npx are both tools that come with Node.js and are used for managing and executing packages. While they may seem similar, there are some key differences between the two and when to use each one.

npm is the Node.js package manager that is used to install, manage, and publish packages to the npm registry. npm is typically used to install packages globally or locally in a project. When you use npm to install a package, the package is downloaded and saved to your local machine, and you can use it in your project by requiring or importing it in your code.

npx, on the other hand, is a tool that allows you to run a package without installing it first. With npx, you can execute a package that is not installed on your system by downloading it temporarily and running it. npx is primarily used to run command-line tools or scripts that are not installed globally on your system.

In general, you can use npm to manage packages that you need to use frequently or in multiple projects, and you can use npx to quickly run command-line tools or scripts without having to install them globally. Here are some scenarios where you might want to use npm or npx:

When to use npm:

To install and manage packages that you need to use in multiple projects
To install and manage global command-line tools like create-react-app
To publish packages to the npm registry
When to use npx:

To run a command-line tool or script that you don't want to install globally
To try out a package without installing it first
To run a version of a package that is different from the one installed globally on your system
Overall, npm and npx are both important tools in the Node.js ecosystem, and understanding the differences between the two can help you use them more effectively in your projects.
