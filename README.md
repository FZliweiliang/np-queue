# np-queue
[![Build Status](https://www.travis-ci.org/ngtmuzi/np-queue.svg?branch=master)](https://www.travis-ci.org/ngtmuzi/np-queue)

A queue to control Promise task's concurrency and pause/resume.

## Install

```
npm i np-queue
```

## Usage

```javascript
const q = new Queue();
const delay = (value) =>  
  new Promise(resolve => {
    setTimeout(() => resolve(value), 1000);  
  });

q.add(()=>delay(1)).then(console.log);
q.add(()=>delay(2)).then(console.log);

const delay_wrap = q.wrap(delay);

delay_wrap(3).then(console.log);
delay_wrap(4).then(console.log);
```
You will see it output 1,2,3,4 interval by 1 seconds.

## API

### new Queue({promiseLibrary,concurrency})

#### promiseLibrary
You can choose the queue's Promise library, default is `global.Promise`.

#### concurrency
Limit how much Promise task can concurrency run, default is 1.

### queue.add(fn)

#### fn
The async function you define, it return a `Promise` or anything, note it will not receive any arguments so you must wrap your arguments in its code.

### queue.wrap(fn, thisArg)

It will be return a function that wrap the `fn`, use the queue's concurrency to limit how much `fn` can be execute on same time.
 
For example, you can fast define one function it only can serial execute:

```javascript
const serial_fn = new Queue().wrap(fn);
```

On many time it's useful.

### queue.pause & queue.resume

Pause/resume this queue, no more word.

## Test
```
mocha
```