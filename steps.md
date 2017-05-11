# angular-redux
> I'm working with ng2-redux

#### Intro
First question I made myself:
what do I want to achieve with this action from the component:

  - open/close something?
  - show/hide    "?
  - call api from server?
  - etc...

Lets say we want to show/hide a search box (searchBox) when a button is clicked.

#### Actions

To interact with searchBox we need **actions**

> check that on your actions file (I name it layout.actions.ts) you imported your store (ngRedux), state (IAppState) and Injectable.

```
import { NgRedux } from 'ng2-redux';
import { IAppState } from '../store';
import { Injectable } from '@angular/core';
```

create and export your constants (If you are using redux DevTools on your browser or redux extension you will see the message SHOW_SEARCHBOX when that action is triggered):

```
export const SHOW_SEARCHBOX = 'SHOW_SEARCHBOX'
export const HIDE_SEARCHBOX = 'HIDE_SEARCHBOX'
```

My methods will be like this:

```
@Injectable()
export class LayoutActions {

    constructor (
        private ngRedux: NgRedux<IAppState>,
    ) {}

    showSearchBox() {
        this.ngRedux.dispatch({type: SHOW_SEARCHBOX, searchBox: true })
    }

    hideSearchBox() {
        this.ngRedux.dispatch({type: HIDE_SEARCHBOX, searchBox: false })
    }

}
```

#### State
Now I need to update my IAppState interface ( IAppState.ts file )

```
export interface IAppState {
    ...
    searchBox: boolean;
}
```

#### Reducers
here we use our current **state** and our **actions** to create our **new state**

import your variables ( SHOW_SEARCHBOX, HIDE_SEARCHBOX ) from layout.actions.ts to your file (reducer.ts).

```
import { 
  ...
  SHOW_SEARCHBOX, 
  HIDE_SEARCHBOX } from '../shared/layout.actions';
```

update your initialState to include showSearchBox ( I want this to initialize hiden so I'll set it to false )

```
const initialState: IAppState = {
    ...
    searchBox: false,
};
```

On your reducer function you add your cases:

```
export function reducer (state = initialState, action) {
    switch (action.type) {
        ...

        case SHOW_SEARCHBOX: 
            return showSearchBox(state, action);
        case SHOW_SEARCHBOX: 
            return hideSearchBox(state, action);

        default:
            return state;
    }
}
```

Now create your auxiliary functions ( showSearchBox and hideSearchBox ).
Here we recreate our state changing only that part that we need (searchBox)

```
function showSearchBox(state, action): IAppState {

    return Object.assign({}, state, {
        searchBox: action.searchBox
    })
}

function hideSearchBox(state, action): IAppState {

    return Object.assign({}, state, {
        searchBox: action.searchBox
    })
}
```

#### Component
Now you need to trigger that actions [from your component(s)] by calling the functions we created in layout.actions.ts file [ showSearchBox() and hideSearchBox() ].

Lets say we want to call showSearchBox method ( from layoutActions.ts file ) on click a button on header.component.html

> remember to import your reducers, state and actions and construct your component with them

```
import { NgRedux } from 'ng2-redux';
import { IAppState } from '../store';
import { LayoutActions } from "../shared/layout.actions";

...

constructor(
  ...
  private ngRedux: NgRedux<IAppState>,
  private layoutActions: LayoutActions) {}

```

From the html file (header.component.html) we bind our click to our method openSearchBox on the ts file (header.component.ts)

```
(click)="openSearchBox()"
```

and from header.component.ts file we call the showSearchBox method from our layoutActions.ts file

```
openSearchBox() {
    this.layoutActions.showSearchBox();
}
```

This is enough to change our state, from showSearchBox: false to true. You can double check from redux extension or devTool.

----

Now let's say I want to close that search box from another component (navSearch).

> Plus: we will add animations to show/hide the search box
> Be sure to import your reducers, state and actions to this component too and construct your component with them

```
import { NgRedux, select } from 'ng2-redux';
import { IAppState } from '../../store';
import { LayoutActions } from "../../shared/layout.actions";

...

constructor(
  ...
  private ngRedux: NgRedux<IAppState>,
  private layoutActions: LayoutActions) {}
  
```








