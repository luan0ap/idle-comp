# IdleComp
A **comp**osable way to perform *non-bloking* **comp**utations in JavaScript.

## Usage
Lets try some computating some things.
First, lets define an dragons array.
```javascript
var dragons = [
    { age: 2, name: 'Halph' },
    { age: 5, name: 'Pottus' },
    { age: 3, name: 'Traus' },
    { age: 1, name: 'Nelf' },
    { age: 4, name: 'Gart' },
    { age: 7, name: 'Mange' },
    { age: 6, name: 'Zalu' }
]
```

Now a logging helper
```javascript
// Just log x and then return it
var log = x => {
  console.log(x)
  return x
}
```

Now lets sort the dragons by age in descending order, get the last and shout it's name
```javascript
var idleName = IdleComp
  .of(dragons) // Bring our dragons to the idle realm
  .map(dragons => dragons.sort((dA, dB) => dB.age - dA.age)) // sort them by age
  .map(dragons => dragons[6]) // get the last
  .map(log) // log out or dragon and return it
  .map(lastDragon => lastDragon.name)
  .map(name => name.toUpperCase())

console.log('First me')
console.log('Than me')

var name = idleName.fold() // Forces all remaning idles to run synchronously

console.log(name)

idleName
  .map(name => name[0]) // resumes the chain
  .map(firstLetter => firstLetter + 'ICE')
  .map(log)

console.log('end of file')
```

When this example is ran, we got the pritings
```
First me
Than me
{ age: 1, name: 'Nelf' }
NELF
end of file
NICE
```

As you see, the fisrt two `console.log`s are executed first and the executation of the first `.map(log)` (as well as the entire map chain) is deffered until we explicit request the value with `.fold()`.

As we resume the mapping chain, we deffer the rest of the execution to either the next `.fold()` or the next iddle slice of time, whatever comes first.

## Composition

A insteristing property of IdleComp is that doing this:
```javascript
IdleComp
  .of(5)
  .map(increment)
  .map(double)
```
Is equivalent to this:
```javascript
IdleComp
  .of(5)
  .map(x => double(increment(x)))
```
This mean that calling subsequents `map`s is like composing functions!

## Roadmap

Those are features that are on our backlog.

 - Creation of an async chainable method, that turns a IdleComp chain into asynchronous. (e.g. waits for promises to resolve value, then wait for the next idle cicle to process). [#1](https://github.com/munizart/idle-comp/issues/1)
 - Testing [#2](https://github.com/munizart/idle-comp/issues/2)
 - Github pages with docs [#3](https://github.com/munizart/idle-comp/issues/3)
 - Automatizate docs with jsdocs, npm scripts and CI [#4](https://github.com/munizart/idle-comp/issues/4)