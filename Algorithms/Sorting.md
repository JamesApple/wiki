# Bubble Sort

N-1

Push the highest element to the top of the list

```typescript

const assert = require('assert')

function bubbleSort(passed: number[]) {
    const list = [...passed]
    const lastIdx = list.length - 1

    for (let passNo = 0; passNo < list.length; passNo++) {
        console.log(JSON.stringify( list ))

        const iterations = list.length - passNo - 1
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

const randomList = Array.from({length: 100}, () => Math.floor(Math.random() * 100))
console.log(randomList)

assert.deepEqual(randomList.sort((a, b) => a - b), bubbleSort(randomList))
```
