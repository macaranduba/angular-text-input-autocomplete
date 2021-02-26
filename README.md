# angular text input autocomplete

[![Build Status](https://travis-ci.org/mattlewis92/angular-text-input-autocomplete.svg?branch=master)](https://travis-ci.org/mattlewis92/angular-text-input-autocomplete)
[![codecov](https://codecov.io/gh/mattlewis92/angular-text-input-autocomplete/branch/master/graph/badge.svg)](https://codecov.io/gh/mattlewis92/angular-text-input-autocomplete)
[![npm version](https://badge.fury.io/js/angular-text-input-autocomplete.svg)](http://badge.fury.io/js/angular-text-input-autocomplete)
[![devDependency Status](https://david-dm.org/mattlewis92/angular-text-input-autocomplete/dev-status.svg)](https://david-dm.org/mattlewis92/angular-text-input-autocomplete?type=dev)
[![GitHub issues](https://img.shields.io/github/issues/mattlewis92/angular-text-input-autocomplete.svg)](https://github.com/mattlewis92/angular-text-input-autocomplete/issues)
[![GitHub stars](https://img.shields.io/github/stars/mattlewis92/angular-text-input-autocomplete.svg)](https://github.com/mattlewis92/angular-text-input-autocomplete/stargazers)
[![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)](https://raw.githubusercontent.com/mattlewis92/angular-text-input-autocomplete/master/LICENSE)

## Demo

https://mattlewis92.github.io/angular-text-input-autocomplete/

## Table of contents

- [About](#about)
- [Installation](#installation)
- [Documentation](#documentation)
- [Development](#development)
- [License](#license)

## About

A angular 6+ directive for adding autocomplete functionality to text input elements, built around composability

## Installation

Install through npm:

```
npm install angular-text-input-autocomplete
```

For older browsers you will need the `keyboardevent-key-polyfill` polyfill:

```
npm install keyboardevent-key-polyfill
```

Then include in your apps module:

```typescript
import { polyfill as keyboardEventKeyPolyfill } from 'keyboardevent-key-polyfill';
import { NgModule } from '@angular/core';
import { TextInputAutocompleteModule } from 'angular-text-input-autocomplete';

keyboardEventKeyPolyfill();

@NgModule({
  imports: [TextInputAutocompleteModule]
})
export class MyModule {}
```

Finally use in one of your apps components:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'mwl-demo-app',
  template: `
    <mwl-text-input-autocomplete-container>
      <textarea
        placeholder="Type @ to search..."
        class="form-control"
        rows="5"
        [(ngModel)]="formControlValue"
        mwlTextInputAutocomplete
        [findChoices]="findChoices"
        [getChoiceLabel]="getChoiceLabel">
      </textarea>
    </mwl-text-input-autocomplete-container>
  `
})
export class DemoComponent {
  formControlValue = '';

  findChoices(searchText: string) {
    return ['John', 'Jane', 'Jonny'].filter(item =>
      item.toLowerCase().includes(searchText.toLowerCase())
    );
  }

  getChoiceLabel(choice: string) {
    return `@${choice} `;
  }
}
```

You may also find it useful to view the [demo source](https://github.com/mattlewis92/angular-text-input-autocomplete/blob/master/demo/demo.component.ts).

### Customising the menu component UI

By default the component works out of the box with bootstrap, but if you're using a different UI framework then you can customise the menu component that gets displayed.

1.  Create a custom menu component and add it to your components NgModule `declarations` (If you're not using ivy then you will need to add it to `entryComponents` as well)

```typescript
import { Component } from '@angular/core';
import { TextInputAutocompleteMenuComponent } from 'angular-text-input-autocomplete';

@Component({
  template: `
    I'm a custom menu template!
    <ul 
      *ngIf="choices?.length > 0"
      #dropdownMenu
      class="dropdown-menu"
      [style.top.px]="position?.top"
      [style.left.px]="position?.left">
      <li
        *ngFor="let choice of choices; trackBy:trackById"
        [class.active]="activeChoice === choice">
        <a
          href="javascript:;"
          (click)="selectChoice.next(choice)">
          {{ choice }}
        </a>
      </li>
    </ul>
  `
})
class CustomMenuComponent extends TextInputAutocompleteMenuComponent {}
```

2.  Pass the component to the `menuComponent` input of the `mwlTextInputAutocomplete` directive

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'mwl-demo-app',
  template: `
    <mwl-text-input-autocomplete-container>
      <textarea
        mwlTextInputAutocomplete
        [menuComponent]="menuComponent">
      </textarea>
    </mwl-text-input-autocomplete-container>
  `
})
export class DemoComponent {
  menuComponent = CustomMenuComponent;
}
```

## Documentation

All documentation is auto-generated from the source via [compodoc](https://compodoc.github.io/compodoc/) and can be viewed here:
https://mattlewis92.github.io/angular-text-input-autocomplete/docs/

## Related

[angular-text-input-highlight](https://github.com/mattlewis92/angular-text-input-highlight) - a component for highlighting parts of a textarea

## Development

### Prepare your environment

- Install [Node.js](http://nodejs.org/) and NPM
- Install local dev dependencies: `npm install` while current directory is this repo

### Development server

Run `npm start` to start a development server on port 8000 with auto reload + tests.

### Testing

Run `npm test` to run tests once or `npm run test:watch` to continually run tests.

### Release

```bash
npm run release
```

## License

MIT
