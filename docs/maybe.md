## The Maybe Monad

### Intro

- Maybe (no pun intended) we should think of it as a well-known `Promise`.

- A Promise lets us write code without worrying about whether data is asynchronous or not. The Maybe monad lets us write code without worrying whether data is empty or not.

- In essence, a Monad is simply a wrapper around a value. We can create that with a value that holds a single property:

```js
let Maybe = function(val) {
  this.__value = val;
}

const maybeOne = new Maybe(1);
```

- In order to avoid having to to type the `new` keyword every time we create an `of()` method:

```js
Maybe.of = function(val) {
  return new Maybe(val);
}

const maybeOne = Maybe.of(1);
```

- As the point of the Maybe monad is to protect us from empty values, we'll write a helper to test the value of out `Maybe`:

```js
Maybe.prototype.isNothing = function() {
    return (this.__value === null || this.__value === undefined);
};
```

- Until now, our `Maybe` does nothing. So we'll make it able to receive a value and do something with it:

```js
Maybe.prototype.map = function(f) {
    if (this.isNothing()) {
        return Maybe.of(null);
    }
    return Maybe.of(f(this.__value));
};
```

- With all this setup, we'll be able to write a function as this:

```js
function getUserBanner(banners, user) {
  return Maybe.of(user)
    .map(prop('accountDetails'))
    .map(prop('address'))
    .map(prop('province'))
    .map(prop(R.__, banners));
}
```


### Maybe of a Maybe

- Let's write a function to grab a banner's URL:

```js
let getProvinceBanner = function(province) {
    return Maybe.of(banners[province]);
};
```

- With that, we can add it in our `getUserBanner()` function:

```js
function getUserBanner(user) {
  return Maybe.of(user)
      .map(prop('accountDetails'))
      .map(prop('address'))
      .map(prop('province'))
      .map(getProvinceBanner);
}
```

- But now we have a problem. Instead of returning a Maybe with a string inside it, we get back a Maybe with another Maybe inside it. 

- To do something with the value, we would have to add a map inside a map:

```js
getUserBanner(user)
    .map(function(m) {
        m.map(function(banner) {
            // I now have the banner,
            // but this is too many maps
        }
    })
```

- But, it's shit code.

- We need a way of joining the Maybes together, and for that we create a `join()` method:

```js
Maybe.prototype.join = function() {
    return this.__value;
};
```

- This lets us flatten back to just one layer:

```js
function getUserBanner(user) {
  return Maybe.of(user)
    .map(prop('accountDetails'))
    .map(prop('address'))
    .map(prop('province'))
    .map(getProvinceBanner)
    .join();
}
```

- But, if we’re mapping and joining a lot, we might as well combine them into a single method. It allows us to chain together functions that return Maybes:

```js
Maybe.prototype.chain = function(f) {
    return this.map(f).join();
};
```

- Now, using `.chain()`, our function has one less step:

```js
function getUserBanner(user) {
  return Maybe.of(user)
    .map(prop('accountDetails'))
    .map(prop('address'))
    .map(prop('province'))
    .chain(getProvinceBanner);
```

- With `chain()` we now have a way of interacting with functions that return other Maybe monads.

- With this code, there’s no if-statements in sight. We don’t need to check every possible little thing that might be missing. If a value is missing, the next step just isn’t executed.


### But what do you do with it?

- What if we wanted to have a fallback banner to use if we get back an empty Maybe? For this we’ll need one more little method:

```js
Maybe.prototype.orElse = function(default) {
    if (this.isNothing()) {
        return Maybe.of(default);
    }

    return this;
};
```

- So we can:

```js
// Provide a default banner with .orElse()
let bannerSrc = getUserBanner(user)
             .orElse('/assets/banners/default-banner.jpg');
// Grab the banner element and wrap it in a Maybe too.
var bannerEl = Maybe.of(document.querySelector('.banner > img'));
```

- 