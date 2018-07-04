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

Javascript is a single thread languange.

Even for backend `NodeJS`, the logic runs on a single thread.

Therefore we can't put thread into `sleep` mode and block thread.

So event queue is the corner stone of understanding javascript async nature.

https://blog.risingstack.com/node-js-at-scale-understanding-node-js-event-loop/


