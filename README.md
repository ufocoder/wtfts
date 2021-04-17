# wtfts
Tricky and non obvious TypeScript Examples


### Enum option

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

Enum value for `optionB` use `optionA` from enum score

[Playground](https://www.typescriptlang.org/play?ts=4.2.3#code/MYewdgzgLgBCAOUCW4CCMC8MDkA3AhgE5L4BGANgKYBq+5ArpatgNwCwAUJ5WPQLYwAwvWgg+AUV4CA3pxjy4iFGHRZsPfrQZNsAGjkKEycACFMi4ys4BfFkA)
