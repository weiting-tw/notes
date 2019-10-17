# JavaScript

## Tips

### Get last word

```javascript
var url = 'www.example.com';
console.log(url.split('.').pop());
// com
console.log(url.slice(-3));
// com
```

### Get duplicated object from array

```javascript
let objArray = [
    { name: '1', id: 0 },
    { name: '2', id: 1 },
    { name: '3', id: 2 },
    { name: '3', id: 3 },
    { name: '3', id: 4 },
    { name: '1', id: 5 },
];

// 所有重複的 obj
// [ { name: '1', id: 0 }, { name: '3', id: 2 }, { name: '3', id: 3 }, { name: '3', id: 4 }, { name: '1', id: 5 } ]
objArray.filter((thing, index, self) =>
  self.findIndex((t, i) => {
    return t.name === thing.name && index2 !== index
  }) > -1
)

// 去除重複的
// [ { name: '1', id: 0 }, { name: '2', id: 1 }, { name: '3', id: 2 } ]
objArray.filter((thing, index, self) =>
  index === self.findIndex((t) => {
    return t.name === thing.name
  })
)

// 取得重複 obj
// [ { name: '3', id: 3 }, { name: '3', id: 4 }, { name: '1', id: 5 } ]
objArray.filter((thing, index, self) =>
  index !== self.findIndex((t) => {
    return t.name === thing.name
  })
)
```
