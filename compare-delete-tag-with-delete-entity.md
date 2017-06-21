# Compare delete tag with delete entity


What do I want to do? Compare both deletions

Why? On delete tag refreshes UI automatically, is not the case for entities

### Deleting tags

We have two stages, on click delete icon fires up two different functions:

- open confirmation modal
- pass which tag from which entity you want to delete



```
<span (click)="areYouSureModal.show(); deleteTag(tag, entity)">
  <i class="fa fa-trash ph-5 card-item-button"></i>
</span>
```
