#### Intro
What do I want to achieve?
On mouseEnter (hover on an entity List item) I want to show edit and delete buttons...

#### Actions

To interact with searchBox we need **actions**

export the constant...

```
export const ENTITY_MOUSE_ENTER = 'entities/ENTITY_MOUSE_ENTER';
```

I've passed entity as parameter because I want to change mouseover property to true only on that object

```
mouseEnter (entity: Entity) {
    this.ngRedux.dispatch({type: ENTITY_MOUSE_ENTER, mouseover : true, entity: entity})
}
```

previously I want to add that property (mouseover: set to false) to each element when I create the first column...

so inside get entities I do that

```
getEntities () {
    this.entityService.getEntitiesChildren('oe1').subscribe(entities => {
        // create a new array of entities with an extra property
        const newEntities = entities.map( entity => {
            // assign new property mouseover (set to false) for each entity
            return Object.assign({}, entity, entity['mouseover'] = false )
        })
        this.ngRedux.dispatch({type: REQUEST_ENTITIES_SUCCESS, newEntities});
    });
}
```

> I should do something like that when I build the entityTree (on each list)


#### IAppState

I didn't change the state interface, because that property is inside entity that already exists...

I've added mouseover property on my entity.model.ts file

```
export interface Entity {
    ...
    mouseover?: boolean;
}
```

#### Reducer

import the constant...

```
import { ...
         ENTITY_MOUSE_ENTER
        } from '../entities/entity.actions';
```

add to entity constant...

```
const entity: Entity = {
    ...
    mouseover : false,
};
```

on my reducer function I've added another case

```
case ENTITY_MOUSE_ENTER:
    return mouseEnter(state, action);
```

and the auxiliary function

```

function mouseEnter (state, action): IAppState {
    // entityTree.map((list,index) => {
    //   const entityCode = list.entities.filter(x => x.code === entity.code)
    //   console.log(entityCode[0])
    const ente = state.entityTree.map((list,index)=> {
        const entityHovered =  list.entities.filter(x=>x.code === action.entity.code)
        // console.log(entityHovered[0])
        return entityHovered
    })
    console.log(ente[0][0])
    // returning the initial state at the moment
    return Object.assign({}, state, {
            entity: state.entity,
            entityTree: state.entityTree
        } )
}
```

#### component

on my entity-list.component.html...

```
...
<li *ngFor="let entity of entityList.entities"
    [class.selected]="entity.code === entityList.asset_parent"
    (click)='onSelectEntity(entity)'
    (mouseenter)="onEnter(entity)"
    >
...
```

on my entity-list.component.ts...

```
onEnter(entity) {
    // console.log(entity.mouseover)
    this.ngRedux.getState().entityTree.map((list,index) => {
        const entityCode = list.entities.filter(x => x.code === entity.code)
        // console.log(entityCode[0])
        this.entityActions.mouseEnter(entityCode[0])
    })
}
```



