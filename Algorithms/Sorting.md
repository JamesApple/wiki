# Bubble Sort O(N^2)

Push the highest element to the top of the list

```typescript

function bubbleSort(passed: number[]) {
    const list = [...passed]

    for (let passNo = 0; passNo < list.length; passNo++) {
        const iterations = list.length - passNo - 1
        console.log(list)

        for (let idx = 0; idx < iterations; idx++) {

            const current = list[idx]
            const next = list[idx + 1]

            if(current > next) {
               list[idx] = next
               list[idx + 1] = current
            }
        }
    }
    return list;
}


const randomList = Array.from({length: 5}, () => Math.floor(Math.random() * 100))
const sorted = [ ...randomList ].sort((a, b) => a - b)

const assert = require('assert')
assert.deepEqual(sorted, bubbleSort(randomList))
```

```markdown-runner
[ 12, 53, 81, 14, 61 ]
[ 12, 53, 14, 61, 81 ]
[ 12, 14, 53, 61, 81 ]
[ 12, 14, 53, 61, 81 ]
[ 12, 14, 53, 61, 81 ]
```


# Insertion Sort


```typescript
function insertionSort()
```

