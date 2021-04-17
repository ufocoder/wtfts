# wtfts
Tricky and non obvious TypeScript Examples

# Table of Contents

* [Enum option](#enum-option)
* [Interface redeclaration](#interface-redeclaration)

## Enum option

TypeScript code:

```typescript
const optionA = 'variableValueA';

enum CustomEnum {
    optionA = 'enumValueA',
    optionB = optionA
};

// CustomEnum.optionB !== optionA
```

Will be compiled to:

```javascript
"use strict";
const optionA = 'variableValueA';
var CustomEnum;
(function (CustomEnum) {
    CustomEnum["optionA"] = "enumValueA";
    CustomEnum["optionB"] = "enumValueA";
})(CustomEnum || (CustomEnum = {}));
;
```

Value for `optionB` use `optionA` from enum score


[Playground](https://www.typescriptlang.org/play?ts=4.2.3#code/MYewdgzgLgBCAOUCW4CCMC8MDkA3AhgE5L4BGANgKYBq+5ArpatgNwCwAUJ5WPQLYwAwvWgg+AUV4CA3pxjy4iFGHRZsPfrQZNsAGjkKEycACFMi4ys4BfFkA)


## Interface redeclaration

The following code:

```typescript
interface Box {
    size: number;
    color: string;
}

// .. many lines of code here

interface Box {
    weight: number;
}

const box: Box = {
    weight: 5
}
```

has compile error:

```
Type '{ weight: number; }' is missing the following properties from type 'Box': size, color
```

Interfaces not are overridden by new declations

[Playground](https://www.typescriptlang.org/play?ts=4.2.3#code/JYOwLgpgTgZghgYwgAgEIHsAeyDeBYAKGWOQGdgAvCALmRAFcBbAI2gG5CTkF0AbdKLVJgooAOYcCAX0KEA9HOQA6JckZwQAT2S9QEUsnQxu6ACYoAFtAiyCoSLEQoM2fERIB3CMDEWwtBhZ2QhkCQh4QYWRmLFoXZABeXE5Pb19-ZABWENsgA)
