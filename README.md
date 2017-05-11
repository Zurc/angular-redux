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

check that on your actions.ts file you imported your store (ngRedux), state (IAppState) and Injectable.

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








