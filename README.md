<div align="center">
  <img src="https://github.com/LesMbenguistes/assets/blob/main/images/Mbeng.png" width="300px" alt="" />
</div>

# Mbeng Eslint Config (JSON)

Mbeng's eslint configuration based off our own config.

## Basic Rules

- Only include one React component per file.
- However, multiple [Stateless, or Pure, Components]() are allowed per file. eslint: `react/no-multi-comp`.
- Always use JSX syntax.
- Only component(s) can be written in `.tsx` files. The others are written in `.ts` files.

## Class vs function

There is no syntax imposed between the two, but you should prefer components as functions rather than as classes.

Apart from the case of stateless functions, if you are really in love with classes you can use them. However, keep in mind the re-rendering problem with this (see [Dan Abramov's article](https://overreacted.io/fr/how-are-function-components-different-from-classes/))

## Naming

- **Extensions**: Use .tsx extension for React components.
- **Filename**: Use PascalCase for filenames. E.g. `TaskItem.tsx`.
- **Reference Naming**: Use PascalCase for React components and camelCase for their instances. eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

```tsx
// ❌ bad
import taskItem from "./TaskItem";

// ✅ good
import TaskItem from "./TaskItem";

// For a FlatList for example

// ❌ bad
const RenderTaskItem = <TaskItem />;

// ✅ good
const renderTaskItem = <TaskItem />;
```

- **Component Naming**: Use the filename as the component name. For example, `TaskItem.tsx` should have a reference name of `TaskItem`. However, for root components of a directory, use `index.tsx` as the filename and use the directory name as the component name:

```tsx
// ❌ bad
import Footer from "./Footer/Footer";

// ❌ bad
import Footer from "./Footer/index";

// ✅ good
import Footer from "./Footer";
```

- **Props Naming**: Avoid using DOM component prop names for different purposes.

> ⚠️**Note**: People expect props like style and className to mean one specific thing. Varying this API for a subset of your app makes the code less readable and less maintainable, and may cause bugs.

```tsx
// ❌ bad
<MyComponent style="fancy" />

// ❌ bad
<MyComponent class="fancy" />

// ❌ bad
<MyComponent className="fancy" />

// ✅ good
<MyComponent variant="fancy" />
```

## Declaration

- Do not use `displayName` for naming components. Instead, name the component by reference.

```tsx
// ❌ bad
export default function() {
    return <View />;
}

// ✅ good
export default function MyComponent() {
    return <View />;
}
```

## Alignment

- Follow these alignment styles for JSX syntax. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md), [`react/jsx-closing-tag-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-tag-location.md)

```tsx
// ❌ bad
<Foo superLongParam="bar"
     anotherSuperLongParam="bar" />

// ✅ good
<Foo 
    superLongParam="bar"
    anotherSuperLongParam="bar"
/>

// If props fit in one line then keep it on the same line
<Foo bar="bar" />

// Children get indented normally
<Foo
    superLongParam="bar"
    anotherSuperLongParam="baz"
>
  <Quux />
</Foo>
```

## Quotes

- Always use double quotes (`"`) for all.

## Spacing 

- Always include a single space in your self-closing tag. eslint: [`no-multi-spaces`](https://eslint.org/docs/rules/no-multi-spaces), [`react/jsx-tag-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-tag-spacing.md)

```tsx
// ❌ bad
<Foo/>

// ❌❌ very bad
<Foo                 />

// ❌ bad
<Foo
 />

// ✅ good
<Foo />
```

- Do not pad JSX curly braces with spaces. eslint: [`react/jsx-curly-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)

```tsx
// ❌ bad
<Foo bar={ bar } />

// ✅ good
<Foo bar={bar} />
```

## Props

- Always use CamelCase for prop names.

```tsx
// ❌ bad
<Foo
    UserName="hello"
    phone_number={12345678}
/>

// ✅ good
<Foo
    userName="hello"
    phoneNumber={12345678}
/>
```

- Omit the value of the prop when it is explicitly `true`. eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

```tsx
// ❌ bad
<Foo
    hidden={true}
/>

// ✅ good
<Foo 
    hidden
/>
```

- Avoid using an array index as `key` prop, prefer a unique ID. ([why?](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318))

```tsx
// ❌ bad
{todos.map((todo, index) => (
    <Todo
        {...todo}
        key={index}
    />
)}

// ✅ good
{todos.map((todo, index) => (
    <Todo
        {...todo}
        key={todo.id}
    />
)}
```

## Refs

- Always use ref callbacks. eslint: [`react/no-string-refs`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-string-refs.md)

```tsx
// ❌ bad
<Foo 
    ref="myRef"
/>

// ✅ good
<Foo 
    ref={(ref) => { myRef = ref; }}
/>
```

## Tags 

- Always self-close tags have no children. eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

```tsx
// ❌ bad
<Foo variant="stuff"></Foo>

// ✅ good
<Foo variant=""stuff" />
```

## Methods 

- Use arrow functions to close over local variables.

```tsx
function ItemList(props) {
  return (
    <View>
      {props.items.map((item, index) => (
        <Item
          key={item.key}
          onPress={() => doSomethingWith(item.name, index)}
        />
      ))}
    </View>
  );
}
```

- Bind evnt handlers for the render method in the constructor. eslint: [`react/jsx-no-bind`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)

> ⚠️**Note**: A bind call in the render path creates a brand new function on every single render.

```tsx
// ❌ bad
class extends React.Component {
  onPress() {
    // do stuff
  }

  render() {
    return <Pressable onPress={this.onPress.bind(this)} />;
  }
}

// ✅ good
class extends React.Component {
  constructor(props) {
    super(props);

    this.onPRess = this.onPress.bind(this);
  }

  onPress() {
    // do stuff
  }

  render() {
    return <Pressable onPress={this.onPress} />;
  }
}
```

## Type vs interface

- Use `interface` to describe props object.
- And `type` to describe other objects (e.g. *state object*) or describe a new _type_.

```tsx
// ❌ bad
interface dataType {
    id: number;
    value?: string | number;
}

const data: dataType = [
    // Some data
];

// ✅ good
type dataType =  {
    id: number;
    value?: string | number;
}

const data: dataType = [
    // Some data
];

// ❌ bad
type FooProps {
    // Some attributes
}

const Foo: React.FC<FooProps> = () => {
    // Stuff
}


// ✅ good
interface FooProps {
    // Some attributes
}

const Foo: React.FC<FooProps> = () => {
    // Stuff
}
```

## Constants and variables

Variable definitions are not allowed in specific conditions in React, but as noted in the documentation for the [eslint file configuration](https://github.com/LesMbenguistes/eslint-config-json), the `var` keyword is prohibited. 

**Quick reminder:** 

[`const`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const) is a signal that the variable won’t be reassigned.
[`let`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let) is a signal that the variable may be reassigned. 

**Additional things to ponder:** 

- Use `const` by default
- Use `let` only if rebinding is needed
- `const` does not indicate that a value is ‘constant’ or immutable.

```tsx
const foo = {};
foo.bar = 10;
console.log(foo.bar); // --> 10
```

Only the binding is immutable. ie using an assignment operator or a unary or postfix -- or ++ operator on a const variable throws a TypeError exception.

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/lesmbenguistes/eslint-config-json.

## License

Mbeng Eslint Config is released under the [MIT License](/LICENSE.txt)
