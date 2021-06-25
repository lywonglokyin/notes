# Angular

## Modules (NgModule)

An NgModule is basically a class (marked with the `@NgModule` decorator) that goes with an important metadata object (specified with decorator).

## Functionality of the metadata

1. Specify how to compile a component's template (?)

2. Specify how to create an injector at runtime (?)

3. Identifies the components, directive, and pipes.

## 4 main elements in the decorator

### `declarations`

Specify the **declarables** (components, directive, and pipes) that belongs to this NgModule. Note that all new NgModule has a component called `AppComponent`.

Declarables specified here is only visible within the NgModule, and is invisible to different module unless they are exported from this module, and the other module import this module. Because of that, if a declarable is shared between two modules, make one import from the other one, instead of putting it in both modules.

### `imports`

Speific the other NgModules that you are using, so that their declarable can be used. Note that all new NgModule imports `BrowserModule` in order to use broser-specific services such as DOM rendering.

### `providers`

Specify the providers of services that components in other NgModules can use. (?)

### `bootstrap`

Specify the entry component that Angular creates and inserts to index.html host web page (Usually would also be the `AppComponent`). The bootstrapping process will then create the components listed, and insert each one to the DOM.


## Angular change detection

Default: "CheckAlways"

Possible: "CheckOnce"