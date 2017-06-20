# angular-redirect-from-class

What do I want?

I want to redirect to another page from within the class, inside a method.

easy peasy lemon squeezy...

go to your page (navsearch.component.ts on my case)

import Router:

```
import { Router } from '@angular/router';
```

and within your method you can redirect...

```
select(item) {
   ...do some stuff...
   this.router.navigate(['entities/asset']);
}
```

Done!
