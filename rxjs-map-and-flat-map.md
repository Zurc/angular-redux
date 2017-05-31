# Rxjs map and flat map

first let's create an stream source and subscribe to it:

```
var source = Rx.Observable.interval(100).take(10)

source.subscribe( x => console.log(x));
```

// 0  1  2  3  4  5  ...


I'm subscribing every tenth of a second and only take the first 10 numbers

***map*** works similar to array.map: It give's me a new array where I can transform each value...

Let's say I want to multiply every value by 2.

```
var source = Rx.Observable.interval(100).take(10)
  .map( x => x * 2 )

source.subscribe( x => console.log(x));
```

// 0  2  4  6  8  10 ...

