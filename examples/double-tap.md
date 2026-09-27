# Worked example: is this a double-tap bug?

A synthetic example. The shape comes from a real labeling session; the code is invented.

## The situation

```kotlin
class CheckoutViewModel(private val scope: CoroutineScope, private val orders: Orders) {
    private val pendingCart = MutableStateFlow<Cart?>(null)

    fun onConfirmClicked() {
        val cart = pendingCart.value ?: return      // A
        pendingCart.value = null                    // B
        scope.launch {                              // C
            orders.place(cart)
        }
    }
}
```

```
tap 1 -> A: cart present, continue
         B: pendingCart = null      (synchronous, no suspend)
         C: launch the order
tap 2 -> A: ???
```

**Facts behind the options**
- A, B and C run synchronously with no suspend point between them.
- Tap 2 reaches A after B has run, as long as both taps arrive on the same thread.
- UI events arrive on the main thread in this app (checked: the screen dispatches every event with a main-thread coroutine scope).

## The matrix

| # | Race | Fix | Why | Holds if | Means for the code |
|---|---|---|---|---|---|
| a | no | no | B clears the cart before the launch, so tap 2 returns at A | Both taps are handled on one thread | Guarded already |
| b | yes | no | Early return in place (first-pass answer) | Inconsistent: if the early return blocks tap 2, there is no race | Reason and label disagree |
| c | yes | yes | Check-then-clear isn't atomic across threads | Taps can be handled on two threads at once | Add a Job check or confine to main |
| d | unsure | - | Depends on the event thread | You want the dispatch path checked first | Hold the label |
| reframe | - | - | The question assumes the risk is a double tap. If `place()` is retried by the network layer, the real question is idempotency on the server | The client retries failed requests | Different fix, in a different layer |

What row is missing?

- Pick (before check): c
- Run check first? yes
- Pick (after check): a
- Notes: the check showed one thread, so c's premise was false.

## What happened

The first pick (c) was reasonable without the dispatch path. The check didn't change the human's judgment. It changed which premise was true, and the pick followed. That is the point of the "Holds if" column.
