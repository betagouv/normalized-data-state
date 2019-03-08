# normalized-data-state

A simple lib to obtain a state of normalized data.

## Basic Usage

### Merge
If you want to merge patched data into an already existing state:

```javascript
import { getNormalizedMergedState } from 'normalized-data-state'

const state = {
  authors: [{ id: 0, name: "John Marxou" }],
  books: [{ authorId: 0, id: 0, text: "my foo" }]
}

const patch = {
  books: [
    {
      author: { id: 1, name: "Edmond Frostan" },
      id: 1,
      text: "you foo"
    }
  ]
}

const config = {
  normalizer: {
    books: {
      normalizer: {
        // short syntax here: <datumKey>: <stateKey>
        author: "authors"
      },
      stateKey: "books"
    }
  }
}

const nextState = getNormalizedMergedState(state, patch, config)

console.log(nextState)
```

We have:

```javascript
{
  authors: [
    { id: 0, name: "John Marxou" },
    { id: 1, name: "Edmond Frostan" }
  ],
  books: [
    { authorId: 0, id: 0, text: "my foo" },
    {
      authorId: 1,
      id: 1,
      text: "you foo"
    }
  ]
}
```

### Delete
import { getNormalizedDeletedState } from 'normalized-data-state'

```javascript
const state = {
  authors: [{ id: 0, name: "John Marxou" }],
  books: [{ authorId: 0, id: 0, text: "my foo" }]
}

const patch = {
  books: [{ id: 1 }]
}

const nextState = getNormalizedDeletedState(state, patch, config)

console.log(nextState)
```

We have:



```javascript
{
  authors: [
    { id: 0, name: "John Marxou" }
  ],
  books: []
}
```

## Usage with config

### Merge

config of getNormalizedMergedState can have:

| name | type | example | isRequired | default | description |
| -- | -- | -- | -- | -- | -- |
| isMergingArray | bool | See test | non | `true` | decide if nextState.<arrayName> will be a merge of previous and next data or just a replace with the new array |
| isMergingDatum | bool | See test | non | `false` | decide if `nextState.<arrayName>[...<datum>]` will be a merge from previous and next datum or just a replace with next datum |
| isMutatingArray | bool | See test | non | `true` | decide if nextState.<arrayName> will be a concat or a merge from previous array |
| isMutatingDatum | bool | See test | non | `false` | decide if `nextState.<arrayName>[...<datum>]` will be a clone or a merge into the previous datum |
| normalizer | objet | See previous section | non | `null` | a nested object giving relationships between datumKeys and entities to be store at stateKeys |
