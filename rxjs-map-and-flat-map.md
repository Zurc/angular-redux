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

what happen if I want to do something asyncrhonous inside the map. Let's say I want to return an observable 'timer' from my map, who is going to wait for half of a second and then map to exactly the same value...

```
var source = Rx.Observable.interval(100).take(10)
  .map( x => Rx.Observabel.timer(500).map( () => x )

source.subscribe( x => console.log(x.toString()));
```

// "[object Object]"  "[object Object]"  "[object Object]"  ...

returns "[object Object]" because this objects are observables

but I really want to get my values back into my stream!

there is two ways to achieve this:

```
var source = Rx.Observable.interval(100).take(10)
  .map( x => Rx.Observabel.timer(500).map( () => x )
  .mergeAll()

source.subscribe( x => console.log(x.toString()));
```

// "1"  "2"  "3"  ...


***mergeAll*** takes in an stream of observables, or receives an observable of observables and merges them together

to do that in a more concise way...

```
var source = Rx.Observable.interval(100).take(10)
  .flatMap( x => Rx.Observabel.timer(500).map( () => x )

source.subscribe( x => console.log(x.toString()));
```

// "1"  "2"  "3"  ...

***flatMap***  performs the map function and the subscribe to each observable returned by the map






