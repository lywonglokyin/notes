# Templates

## Text interpolation

Text interpoltation allows dynamically changing what appears in an application view. Using `{{ }}`, we can display value of a variable of the corresponding component.

## Expression interpolation

Similar to text interpolation, but this time with an expression. The steps are:

1. Evaluate the expression in `{{}}`.
2. Convert the expression to strings
3. Link the result to any adjacent literal string (?)
4. Assign the composite to an element or directive property.

## Using template variable

Other than using variables provided by the components, we can also reference variables of the templates:

### template input variable

```html
<ul>
  <li *ngFor="let customer of customers">{{customer.name}}</li>
</ul>
```

### template reference variable

```html
<input #customerInput>{{customerInput.value}}
```

## Pipe operator

Pipe is a good way to format data, e.g. string formats, currencies, or other display data. For example:

```html
<h1>
{{object.name | uppercase}}
</h1>
```

It will be costly to execute the pipe for every change detection loop. Angular optimize this by only executing the pipe whenever the actual value of the input changed. (Therefore, if an array to supplied to the pipe, even if items are added or deleted from the array, the pipe will not execute because only the **reference** is supplied to the pipe.)

To make Angular execute your custom pipe on every change detection loop, change your pipe into impure.

## Property binding

Property binding is a one-way binding that moves value from component's property into a target element property, or directives. E.g.

```html
<!-- isUnchanged is a property of the component-->
<button [disabled]="isUnchanged">Disabled Button</button>
```

Another use of property binding is that it allow data to flow from parent component to child component:
```html
<app-item-detail [childItem]="parentItem"></app-item-detail>
```
in which the `childItem` is an `@Input`:

```typescript
@Input() childItem: string;
```

## Attribute binding

Recall that attribute of HTML tag and property of DOM are **different**. Angular's syntax of attribute binding is similar to property binding, with an extra pre-fix `attr.`:

```html
<p [attr.attribute-you-are-targeting]="expression"></p>
```

Similarly, for `class` and `style`:

```html
<p [class.sale]="onSale"></p>
<p [style.width]="width"></p>
```

## Capturing HTML attribute to a component

To past the value of an HTML attribute to a component/directive constructor, we can use the `@Attribute` decorator.

## Event Binding

Event binding allows listening for and respond to user actions such as keystrokes, mouse movements, clicks and touches. E.g.

```html
<button (click)="onSave()">Save</button>
```

For the example above, `click` is the target event name, and `onSave()` is the component's method that will be invoked.

### Custom target event

Child of a components can define `EventEmitter`, which in turns can be catched by the parent component.

## Two-way binding

Two-way binding is like a combination of event binding and property binding. See more on drawn notes.

Because the native HTML element does not follow the **x** and **x**Change() format, if we wish to use two-way binding with form elements, we need to use `NgModel`.

## Template reference variables

Template variables help you use data from one part of a template in another part of a template. E.g.

```html
<input #phone placeholder="phone number" />

<!-- lots of other elements -->

<!-- phone refers to the input element; pass its `value` to an event handler -->
<button (click)="callPhone(phone.value)">Call</button>
```





## `ngIf`, hiding details

## Interpolation binding

### `routerLink` in `ngFor`