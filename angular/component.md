# Component

## What is component?

Component is the building block for Angular applications, consist of:

1. An HTML template that declares what renders on the page.
2. A Typestript class that define behavior
3. A CSS selector that defines how the component is used in a template
4. A testing specification (`.spec.ts`)

## Creating a component

1. With cli

```bash
ng generate component <component_name>
```

## Component lifecycle

## Component properties

1. Define a property




## Common function in a component class

1. `ngOnInit()`

`ngOnInit()` is a lifecycle hook, that is, the function that performs the initialization tasks. Usage:

- Perform complex initialization, which should not be included in the constructors of componenets.
- Setup the component after the constructor initialized values to simple values.

Note:

- The `ngOnChanges()` method is your first opportunity to access those properties. Angular calls `ngOnChanges()` before `ngOnInit()`, but also many times after that. It only calls `ngOnInit()` once.


