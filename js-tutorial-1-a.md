class: center, middle

# Random JS Tutorial - Part 1

Sync, Async

---

# Agenda

*  Event Queue

*  Callback vs Promise

*  Regenerator

*  Promise vs Async/Await

---

# Event Queue

Javascript is a single thread languange;

We can't put thread into `sleep` mode and block thread;

Event queue is the corner stone;

https://blog.risingstack.com/node-js-at-scale-understanding-node-js-event-loop/

---

# Event Queue -- diagram

![Event Loop](./images/the-Node-js-event-loop.png)

---

# Callback

```javascript
function serverCall(onSuccess) {
    //making asynchronise call, ajax/timeout/etc.
    $.ajax('some-url', some-data, {
        success: onSuccess
    })
}
```

--

Hell !!😱😱😱
```javascript
function main() {
    createOrder(function(orderId){
        addProductsToOrder(orderId, products, function(transactionNumber){
            sendConfirmationEmail(transactionNumber, function(){
                andOthers(function() {
                    andOthers2(function(){
                        andOther3(function(){})
                    })
                })
            })
        })
    })
}
```

---

# Callback (better?) Hell 😅

```javascript
function main() {
    createOrder(addProductsToOrder)
}
function addProductsToOrder(orderId) {
    serverCall_AddProducts(orderId, products, sendConfirmation, addProductFail)
}
function addProductFail() {
    //blahblah
}
function sendConfirmation(transactionNumber) {
    serverCall_SendConfirmation(andOthers, sendConfirmFail);
}
function sendConfirmFail() {
    //blah blah
}
```

Cons:

* code flow is broken into sub-functions, very hard to understand
* handling failure is also hard

---

# `Promise` to resuce

```javascript
function main() {
    const initialPromise = createOrder();
    const anotherPromise = initialPromise
        .then(addProductsToOrder);
        .catch(onFailAddProduct)
        .then(function(transactionNumber){
            return sendConfirmationEmail(transactionNumber);
        });
        .catch(onFailSendConfirm)
    anotherPromise
        .then(andOthers)
        .then(andOthers2)
        .catch(onGenericFailure);
}
```

Pros:
* Easy to understand code workflow
* Easy to handle error

Cons:
* Sometimes you need to wrap your head around the `then`, `catch`
* watch the return value in `then` and `catch`, the result can be completetly different