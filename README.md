# Angular notes

### Folder structure for angular

``` bash
src
|-- app
    |-- core
    |   |-- models        (shared models/interfaces)
    |   |-- services      (shared services)
    |   |-- guards        (route guards)
    |   |-- interceptors  (HTTP interceptors)
    |
    |-- shared
    |   |-- components    (reusable UI components)
    |   |-- directives    (reusable directives)
    |   |-- pipes         (reusable pipes)
    |
    |-- modules
    |   |-- feature1      (e.g., customer module)
    |   |   |-- components
    |   |   |-- services
    |   |   |-- models
    |   |   |-- feature1.module.ts
    |   |   |-- feature1.routing.module.ts
    |   |
    |   |-- feature2      (e.g., product module)
    |   |   |-- components
    |   |   |-- services
    |   |   |-- models
    |   |   |-- feature2.module.ts
    |   |   |-- feature2.routing.module.ts
    |
    |-- shared-modules
    |   |-- shared.module.ts     (reusable components, directives, pipes)
    |
    |-- layout
    |   |-- components    (layout components: header, footer, sidebar, etc.)
    |
    |-- core.module.ts    (main application module)
    |-- app.module.ts     (imports AppModule and bootstraps the app)
    |-- app.component.ts  (root component)
|
|-- assets                (static assets: images, fonts, etc.)
|-- environments          (environment-specific configurations)
|-- styles                (global styles, SCSS files)

```





