## Checkpoint.js

Abstraction for `IntersectionObserver` that returns the current scroll state:

---

```js
import Checkpoint from 'checkpoint';

// Cache all '.captions', each a child of '.feature'
const captions = document.querySelectorAll('.caption');

const handleIntersect = ({ index, state, scroll }, entry, observer) => {
    const ratio = entry.intersectionRatio;

    if (state === 'enter' && scroll === 'down' ||
        state === 'leave' && scroll === 'up') {
        captions[index].style.transform = `translateY(${50 * (1 - ratio)}%)`;
        captions[index].style.opacity = ratio;
    }
};

const observer = new Checkpoint({
    elements: '.feature',
    threshold: { steps: 50 },
    onIntersect: handleIntersect,
});
```

## Options

### elements

| Default | Type   |
| ------- | ------ |
| `null`  | String | Element | NodeList | Array |

### threshold

| Default                         | Type   |
| ------------------------------- | ------ |
| `{ min: 0, max: 1, steps: 0 }`  | Object |

A range of thresholds can easily be generated by passing a number to `steps`. Passing 4 will generate: `[0, 0.25, 0.5, 0.75 ,1.0]`, as `(max - min) / steps = 0.25`.

If you want to pass a single value, you only have to pass `min`.

### onIntersect

| Default | Type     |
| ------- | -------- |
| `null`  | Function |

Handler that receives the following arguments:

```ts
{
  index: number,
  scroll: 'up' | 'down',
  state: 'enter' | 'leave',
},
entry: IntersectionObserverEntry,
observer: IntersetionObserver),
```

The `index` for each added element allows you to build, access a cache and not have to make costly `querySelector` from inside the callback.

### namespace

| Default        | Type   |
| -------------- | ------ |
| `__checkpoint` | String |

Each HTMLElement passed to `elements` will have properties attached to them to keep track of the element's `index`, `scroll` direction (up/down), `state` (enter/leave). The default namespace should be fine, but it can be changed if there is a conflict.

## Methods

### `disconnect()`

Disconnects the IntersectionObserver and clears the elements that were passed to it.

