# angular-redux search box example

> I'm working with [ng2-redux]

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

Now we create auxiliary functions ( showSearchBox and hideSearchBox ).
Here we recreate our state using pure functions, changing only that part of the state that we need (searchBox)

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
Now you need to trigger that actions [from your component(s)] by calling the functions created in layout.actions.ts file [ showSearchBox() and hideSearchBox() ].

Lets say we want to call showSearchBox method ( from layoutActions.ts file ) by clicking a button on header.component.html

> remember to import your store, state and actions and construct your component with them

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

> We changed out state but nothing happens on the UI yet. we want to attach the state with the component navsearch, which it will appear when searchBox property is true on our state

----

Now let's say I want to close that search box from inside navSearch component.

> Plus: we will add animations to show/hide the search box from this component
> 
> If you want to learn more about animations check [ng-docs] or [ng2-animations]
> 
> Be sure to import your store, state and actions to this component too and construct your component with them


In our **navsearch.component.html** file we will apply two animations...

```
<section class="main-wrap" [@slideInOut]="searchBoxState">
    <div class="search" [@searchInput]="searchBoxState">
```

In our **navsearch.component.ts** file

```
import { NgRedux, select } from 'ng2-redux';
import { IAppState } from '../../store';
import { LayoutActions } from "../../shared/layout.actions";
import { Observable } from 'rxjs/Observable';

...

constructor(
  ...
  private ngRedux: NgRedux<IAppState>,
  private layoutActions: LayoutActions) {}
  
```
Let's see our animations (inside our @Component decorator ) in our navsearch.component.ts

```
animations: [
```

Trigger's name: slideInOut ( we've used this in our html file )

```
  trigger('slideInOut', [
```  

On our 'out' state (when our element is out of the window) we will apply some style to our html tag element 'main-wrap'
We want our element totally moved to the left of our window

```
    state('out', style({          
      left: '100%',
    })),
```

On our 'in' state we want our element back to normal

```
    state('in', style({
    })),
      left: 0,
    })),
```    

We apply different animation properties

```    
    transition('in => out', animate('0.3s 0.3s ease-in')),
    transition('out => in', animate('0.3s ease-out'))
  ]),
```  

The second animation works with the same 'mechanics'

```
  trigger('searchInput', [
  ]),
    state('out', style({
      opacity: 0,
      width: 0
    })),
    state('in', style({
      opacity: 1,
      width: '100%'
    })),
    transition('in => out', animate('0.3s ease-in')),
    transition('out => in', animate('0.3s 0.4s ease-out'))
  ]),
]
```

In our class we need to check for searchBox state

```
@select('searchBox') searchBox$: Observable<any>;
```

we create a property searchBoxState which we'll use to connect our state with our animation

```
searchBoxState: string;

constructor (...) {
  this.searchBox$.subscribe( state => {
      this.searchBoxState = state ? 'in' : 'out';
  })
}
```

This means: if searchBox property (from our state) it's true we assign 'in' to searchBoxState property on our class, else we assign 'out'

by default, on our initialState (check line 83 ) we set our searchBox to false, so our element of class main-wrap will start with the state 'out', that means, moved entirely to the left ( left:100% - check line 214 )










[ng2-redux]: <https://www.npmjs.com/package/ng2-redux>
[ng-docs]: <https://angular.io/docs/ts/latest/guide/animations.html>
[ng2-animations]: <https://coursetro.com/posts/code/25/Understanding-Angular-2-Animations-Tutorial>

