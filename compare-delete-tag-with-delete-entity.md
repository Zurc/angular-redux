# Compare delete tag with delete entity


What do I want to do? Compare both deletions

Why? On delete tag refreshes UI automatically, is not the case for entities

### Deleting tags (entity-details component)

We have two stages:

***1st Stage:*** on click delete icon fires up two different functions:

- open confirmation modal
- pass which tag from which entity you want to delete

HTML

```
<span (click)="areYouSureModal.show(); deleteTag(tag, entity)">
  <i class="fa fa-trash ph-5 card-item-button"></i>
</span>
```

TS

```
deleteTag (tag: Tag, entity: Entity) {
  // save temp copy. User needs to confirm
  this.tagToDelete = tag;
  this.entityToDeleteTagFrom = entity;
}
```

***2nd Stage:*** when we confirm that we want to delete that tag

- fire up confirmation function and call the respective action

HTML

```
<button type="button"
  class="btn btn-success btn-outline">
  <i class="fa fa-check"></i>
  <span (click)="confirmDeleteTag(true); areYouSureModal.hide()">Delete</span>
</button>
```

TS

```
confirmDeleteTag (confirmed: Boolean) {
  if (confirmed) {
    this.entityActions.deleteTag(this.tagToDelete, this.entityToDeleteTagFrom, this.hierarchy);
  } else {
    console.log('what?');   // not for production, but for fun
  }
}
```

we call deleteTag method from our actions file...

### entity.actions.ts

Here we call a service ( to remove element/tag trough HTTP call )...

...and we dispatch the response ( all the other tags -whithout the deleted one - from that entity )

```
deleteTag (tag: Tag, entity: Entity, hierarchy: String) {
    this.entityService.deleteTag(tag, entity).subscribe(tags => {
        this.ngRedux.dispatch({type: DELETE_TAG_SUCCESS, entity: {tags: tag}, hierarchy: hierarchy});
    });
}
```

Now on our Store reducers we need to update the state to show the new state without deleted tag

### entity.service.ts

here we do our http.delete request

```
deleteTag (tag: Tag, entity: Entity) {
    const jwtoken = localStorage.getItem('token');
    const headers = new Headers ();
    headers.append('Authorization', 'Bearer ' + jwtoken);
    const options = new RequestOptions({headers: headers});

    return this.http.delete(`${this.BASE_URL}/entities/${entity.code}/tags/${tag.key}`, options)
      .map( (res: Response) => {
          const body = res.json();
          return body || {};
      })
      .catch(this.handleError);
}
```







