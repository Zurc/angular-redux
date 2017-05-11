# angular-redux
note: working with ng2-redux

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

note: check that on your actions.ts file you imported your store (ngRedux), state (IAppState) and Injectable.

create and export your constants (you can see the message SHOW_SEARBOX with redux DevTools or in the console using redux extention):

```
export const SHOW_SEARCHBOX = 'SHOW_SEARCHBOX'
export const HIDE_SEARCHBOX = 'HIDE_SEARCHBOX'
```

In my case I'm using actions from my layout.actions.ts file. My methods will be like this:

```
@Injectable()
export class LayoutActions {

    constructor (
        private ngRedux: NgRedux<IAppState>,
    ) {}

    showSearchBox() {
        this.ngRedux.dispatch({type: SHOW_SEARCHBOX, showSearchBox: true })
    }

    hideSearchBox() {
        this.ngRedux.dispatch({type: HIDE_SEARCHBOX, showSearchBox: false })
    }

}
```

#### State
Now I need to update my IAppState interface ( IAppState.ts file )

```
export interface IAppState {
    ...
    showSearchBox: boolean;
}
```

#### Reducers
here we use our current **state** and our **actions** to create our **new state**

import your variables ( SHOW_SEARCHBOX, HIDE_SEARCHBOX ) from layout.actions.ts to your file (reducer.ts).

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

Now create your auxiliary functions ( showSearchBox and hideSearchBox ):

```
function showSearchBox(state, action): IAppState {

    return Object.assign({}, state, {
        showSearchBox: action.showSearchBox
    })
}

function hideSearchBox(state, action): IAppState {

    return Object.assign({}, state, {
        hideSearchBox: action.hideSearchBox
    })
}
```





