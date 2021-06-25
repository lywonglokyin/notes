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

## Component lifecycle hooks

A component typically has a lifecycle of creation, changes, and deletion. Lifecycle hooks methods allows tapping into key events of the lifecycle and perform actions (e.g. initialization, updating, logging)


### `ngOnChange()`

`ngOnChange()` fires when Angular sets or resets data-bound input properties (**`@Input()`**). It receives a `SimpleChanges` object of current and previous value.

Note:

- The `ngOnChanges()` method is your first opportunity to access those properties. Angular calls `ngOnChanges()` before `ngOnInit()`, but also many times after that. It only calls `ngOnInit()` once.

- Because this function is likely to be called many times, the performance impact will be huge if operation of this function is costly.

### `ngOnInit()`

`ngOnInit()` performs the initialization tasks. Usage:

- Perform complex initialization, which should not be included in the constructors of componenets.
- Setup the component after the constructor initialized values to simple values.

### `ngDoCheck()`

`ngDoCheck()` detects and act on *irregular* changes (changes that `ngOnChange()` cannot catch).

Note:

- This method might trigger even more frequently than `ngDoCheck()` (e.g. cursor click textbox, etc), so it has to be extremely lightweight.

### `ngAfterContentInit()`


### `ngAfterContentChecked()`

### `ngAfterViewInit()`

### `ngAfterViewChecked()`

### `ngOnDestroy()`

Performs cleanup just before Angular destroys the directive or component. Usage:

- Unsubsribe Observables

- Detach event handlers

- Stop interval timers

- Notify another part of the application that the component is going away






## Component properties

1. Define a property


## Capturing HTML attribute to a component

See html.md