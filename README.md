# wtfts
Tricky and non obvious TypeScript Examples

👋🏻 Welcome to add new trick examples or to improve old one

# Table of Contents

* [Enum option](#enum-option)
* [Interface redeclaration](#interface-redeclaration)
* [Invariant](#invariant)
* [Comparing two functions](#comparing-two-functions)
* [Interfaces with similar structure](#interfaces-with-similar-structure)

## Enum option

TypeScript code:

```typescript
const optionA = 'variableValueA';

enum CustomEnum {
    optionA = 'enumValueA',
    optionB = optionA
}

// CustomEnum.optionB !== optionA
```

[Playground](https://www.typescriptlang.org/play?ts=4.2.3&ssl=1&ssc=1&pln=6&pc=2#code/MYewdgzgLgBCAOUCW4CCMC8MDkA3AhgE5L4BGANgKYBq+5ArpatgNwCwAUJ5WPQLYwAwvWgg+AUV4CA3pxjy4iFGHRZsPfrQZNsAGjkKEycACFMi4ys4BfIA)

**Explanation**: 

Value for `optionB` use `optionA` from enum score.

Will be compiled to:

```javascript
"use strict";
const optionA = 'variableValueA';
var CustomEnum;
(function (CustomEnum) {
    CustomEnum["optionA"] = "enumValueA";
    CustomEnum["optionB"] = "enumValueA";
})(CustomEnum || (CustomEnum = {}));
```

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

[Playground](https://www.typescriptlang.org/play?ts=4.2.3#code/JYOwLgpgTgZghgYwgAgEIHsAeyDeBYAKGWOQGdgAvCALmRAFcBbAI2gG5CTkF0AbdKLVJgooAOYcCAX0KEA9HOQA6JckZwQAT2S9QEUsnQxu6ACYoAFtAiyCoSLEQoM2fERIB3CMDEWwtBhZ2QhkCQh4QYWRmLFoXZABeXE5Pb19-ZABWENsgA)

**Explanation**: Interfaces not are overridden by new declations. 


## Invariant

The following code will be compiled without errors:

```typescript
interface Animal { name: string }
interface Cat extends Animal { meow: () => string }
interface Dog extends Animal { wow: () => string }

const addAnimal = (animals: Animal[]) => {
    const cat: Cat = { name: 'Cat #1', meow: () => 'meow' };
    animals.push(cat);
}

const dogs: Dog[] = [];

addAnimal(dogs);

dogs[0].wow()
```

[Playground](https://www.typescriptlang.org/play?ts=4.2.3#code/JYOwLgpgTgZghgYwgAgIImAWzgG2Qb2RDkwgC5kBnMKUAc2QF8BYAKFElkRQGE4xkEAB6QQAE0poM2PIVIB7AO4UAFAEpkAXgB8VGvSZsO0eEmQAReQ2GiJUrLgLJFS1Rp17aIBi1ZsE8iDUyHBiYugOeJrIKnDSuJQUETIA2gC67rr4bMi5yAFBAgj8FHwC0YTEpBQA5GXIAMQAjDUANMgKyjGZyDWdNUwA3Dl5cZGUAHQADgCulAAWKsVgasOsvv6BwWJWiRZW6VrI6WtsoeHxOCo7dJSrbGw3lCkADGkTLorqbEA)


## Comparing two functions

```typescript
let x = (a: number) => a;
let y = (b: number, s: string) => s ? b : 0;

y = x; // OK
x = y; // Error
```

[Playground](https://www.typescriptlang.org/play?ssl=1&ssc=1&pln=5&pc=16#code/DYUwLgBAHhC8EAoCGAuCA7ArgWwEYgCcBKOAPgiQG4BYAKFEgE85Fc0s9CAaCAZzV5gCAS3QBzErHK8IAfgi4IaAAw1adZvCiUIAel0QA8gGs6MeIx36IAUQIEA9gSA)

Explanation from [official documentation](https://www.typescriptlang.org/docs/handbook/type-compatibility.html#comparing-two-functions)

## Interfaces with similar structure

```typescript
interface Entity {
  name: string;
}


interface Enemy {
  name: string;
  health: number;
  attack: number;
}

interface Player {
  name: string;
  health: number;
  attack: number;
}

let entity: Entity = { name: 'Mario' }
let enemy: Enemy = { name: 'Shredder', health: 100, attack: 20 };
let player: Player = { name: 'John Cena', health: 110, attack: 35 };

enemy = player; // OK ?!
player = enemy; // OK ?!

entity = enemy; // OK ?!
enemy = entity; // Error
```

[Playground](
https://www.typescriptlang.org/play?ssl=1&ssc=1&pln=27&pc=1#code/JYOwLgpgTgZghgYwgAgKLmGAnsg3gWAChlkQ4BbCALmQGcwpQBzAbiIF8ivDRJZEU6CORwFipCtToNmbcQAsIcADZh5NEAFdyAI2hyScMGEQBrDdr1Q5nQkV7R4SZAAVlcLNDxESZSjXpGEFYfZEUVNQtdfVCjEwRzUksYwlsiZQgwZAgMbBp0MEwcAF48CX9kAHIAWThGAHtK5FsMrJzhLHyQDuRS3HKpSoBleSgIABNx6EqAGjClVXVkAEYABlW5uLMaACZV5rlW5AAHd08oGjcPLz6BmkqAKXr5EGQAYRy4WfmIpeW1zbGbbIADMAFYDtx2iJeiczvpkAB6RHIADyAGlkAB+ACERFO1ygsOhWBYSJRGOxeLshByhWwxO6IjJyLRmNxRBJjPppPJaCgUHqUCIQA)

Explanation: TypeScript is based on structural subtyping
