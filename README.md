# Queue Implementation in C

A simple **FIFO (First In, First Out) Queue** implemented in C using a singly linked list, with custom integer type aliases for portability.

---

## Data Structures

### `Node`
Each element in the queue is a `Node` holding:
- `value` — a 16-bit signed integer (`i16`)
- `right` — a pointer to the next node in the queue

### `Queue`
The queue itself tracks:
- `head` — the front of the queue (next to be removed)
- `tail` — the back of the queue (where new items are added)
- `size` — the current number of elements

```
head                        tail
 |                           |
[4] --> [5] --> [6] --> [7] --> NULL
 ^                           ^
dequeue from here     enqueue here
```

---

## API

| Function | Description |
|---|---|
| `createQueue()` | Allocates and returns a new empty queue |
| `size(queue)` | Returns the number of elements in the queue |
| `isEmpty(queue)` | Returns `true` if the queue has no elements |
| `peek(queue, status)` | Returns the front value without removing it |
| `enqueue(queue, value)` | Adds a value to the back of the queue |
| `dequeue(queue, status)` | Removes and returns the value from the front |
| `destroyQueue(queue)` | Frees all nodes and the queue itself |

> `status` is a `bool*` output parameter — set to `false` if the operation fails (e.g. peeking or dequeuing an empty queue), `true` on success.

---

## How It Works

### `enqueue`
Creates a new node and appends it to the tail. If the queue is empty, both `head` and `tail` point to the new node.

### `dequeue`
Reads the value from the head node, advances `head` to the next node, and frees the old head. If only one element remains, both `head` and `tail` are set to `NULL`.

### `destroyQueue`
Walks the entire linked list from `head` to `NULL`, freeing each node, then frees the queue struct itself.

---

## Example Usage

```c
Queue *queue = createQueue();

enqueue(queue, 4);
enqueue(queue, 5);
enqueue(queue, 6);
// Queue: [4] -> [5] -> [6]

bool status;
i16 front = peek(queue, &status);   // front = 4, status = true
i16 val   = dequeue(queue, &status); // val = 4,   status = true
// Queue: [5] -> [6]

printf("Size: %hd\n", size(queue)); // Size: 2

destroyQueue(queue);
```

---

## Type Aliases

The file defines shorthand aliases over `<stdint.h>` types for brevity:

| Alias | Underlying Type |
|---|---|
| `i8` / `i16` / `i32` / `i64` | Signed integers |
| `u8` / `u16` / `u32` / `u64` | Unsigned integers |
| `f32` / `f64` | `float` / `double` |

---

## Notes

- All heap allocations are checked for `NULL` — `createQueue` and `enqueue` handle allocation failures gracefully.
- The queue stores `i16` values; adapting it to other types would require changing `Node.value` and the relevant function signatures.
- Memory is fully cleaned up via `destroyQueue` — no leaks when used correctly.
