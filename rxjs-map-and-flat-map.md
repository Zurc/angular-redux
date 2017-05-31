# Rxjs map and flat map

first let's create an stream source and subscribe to it:

```
var source = Rx.Observable.interval(100).take(10)

source.subscribe( x => console.log(x));
```
