#sort_algorithm

```typescript
function insertionSort(passed: number[]) {
  const list = [...passed];

  for (let pass = 1; pass < list.length; pass++) {
    const previousIdx = pass - 1;
    const next = list[pass];

    while(list[previousIdx ] > next && previousIdx >= 0)  {
    console.log('a')

     previousIdx--
    }
  }

  return list
}

insertionSort([1, 4, 3, 4, 3]);
```
