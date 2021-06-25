# Directives

There are three types of directives:

1. Components: directives with a template (discuss in the component page)

2. Attribute directives: Change the appearance or behaviour of an element, component or another directive. (Change attributes)

3. Structural directives: Change the DOM layout by adding and removing DOM lement. (Change structure)

## Some built-in attribute directives

### `NgClass`

Use this to add/remove an amount of classes. E.g.:

```html
<div [ngClass]="currentClasses">div...</div>
```

For the above example, `currentClasses` is an attribute storing the status of classes.

```typescript
this.currentClasses =  {
    saveable: this.canSave,
    modified: !this.isUnchanged,
    special:  this.isSpecial
};
```

Note:

- To add or remove a single class, it is better to use **class binding**.

### `NgStyle`

Similar to `NgClass`, it is used to add/remove an amount of style.

### `NgModel`

`NgModel` is a two-way binding that allows displaying a data property, and at the same time update the property when user make changes.

`NgModel` is available with the import `FormsModule`.

## Built-in structural directive

### `NgIf`

Can add or remove element from DOM by applying the `NgIf` directive to a host element. E.g.
```html
<app-item-detail *ngIf="isActive" [item]="item"></app-item-detail>
```

Usage:

- Add or remove element

- Defend against null, only show element when value is not null.

Note:

- `NgIf` is different from hiding an element with style. `NgIf` will actually remove the element and its descendants from the DOM, thus releasing the resources.

### `NgFor`: Repeater directive

`NgFor` allows presenting a list of items, e.g.:

```html
<div *ngFor="let item of items">{{item.name}}</div>

<!--or to components-->
<app-item-detail *ngFor="let item of items" [item]="item"></app-item-detail>
```

Note:

- The `let xxx of xxxs` is a microsyntax, see more at **structural directives**.

- To avoid Angular from reloading the same DOM element when items are added or deleted form the list of `ngFor`, you should use `trackBy`.


### `NgSwitch`

Similar to `switch-case` statements, it allows manipulation of multiple DOM elements.

## Attribute directives

An attribute directives changes the appearance or behavior of a DOM element. It is created by a controller class annotated with `@Directive`.

Usage:

- Dynamic CSS style that depends on user input

## Structural directive

Structural directives are responsible for the HTML layout, that is, to shape the DOM structure, through adding, removing or manipulating elements. Syntax of structural directive is to precede the directive attribute name with an `*`.

### Behind `*ngIf`

An normal `*ngIf` definition:

```html
<div *ngIf="hero" class="name">{{hero.name}}</div>
```

Behind the scene, it is translated to:

```html
<ng-template [ngIf]="hero">
  <div class="name">{{hero.name}}</div>
</ng-template>
```

