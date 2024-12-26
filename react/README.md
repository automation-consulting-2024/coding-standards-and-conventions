# General

## Basic Component Structure

- Simple Component Structure

```jsx
function Component() {
    // MARK: - States
    const [state, setState] = useState()

    // MARK: - Values mapped from States
    const isState = state.isState

    // MARK: - Event Handlers
    const onClick = () => {
        return onClick()
    }

    // MARK: - Side Effecs
    useEffect(() => {}, [
        setState(true)
    ])

    // MARK: - Renderer
    return (
        <>
            {content..}
        </>
    )
}
```

## Performance

### Prefering Pure-Components

- Based on the concept of purity in functional programming paradigms, a function is said to be pure if it meets the following two conditions:
  - Its return value is only determined by its input values
  - Its return value is always the same for the same input values

```jsx
function PercentageStat({ label, score = 0, total = Math.max(1, score) }) {
  return (
    <div>
      <h6>{label}</h6>
      <span>{Math.round((score / total) * 100)}%</span>
    </div>
  );
}
```

### Move Constants & Pure function to outside component

- Everything inside a component have to be re-initialize every time it re-render, move
  them to outside of component help JS to only initialize them one.

```jsx
const DEFAULT_MULTIPLY = 2;

const multiplyBy = (a, b) => {
  return a + b * DEFAULT_MULTIPLY;
};

const Component = ({ a, b }) => {
  const c = add(a + b);

  return <>{c}</>;
};
```

### Initialize expensive state by arrow function

```jsx
// instead of this which would be executed on every re-render:
const [state, setState] = React.useState(myExpensiveFn());

// prefer this which is executed only once:
const [state, setState] = React.useState(() => myExpensiveFn());
```

### Before you Memo

- Not every performance problem end with `useMemo` or `React.memo`.

Here a `slow` React component:

```jsx
import { useState } from 'react';

export default function App() {
  let [color, setColor] = useState('red');
  return (
    <div>
      <input value={color} onChange={(e) => setColor(e.target.value)} />
      <p style={{ color }}>Hello, world!</p>
      <ExpensiveTree />
    </div>
  );
}

function ExpensiveTree() {
  let now = performance.now();
  while (performance.now() - now < 100) {
    // Artificial delay -- do nothing for 100ms
  }
  return <p>I am a very slow component tree.</p>;

```

Problem: Everytime `color` state change, `ExpensiveTree` will re-render.

#### Solution 1: Move State Down

- As you see above, only `input` and `p` element care about `color`
  state, so we should able to extract relative logic to they own component
  and move the state down like bellow:

```jsx
export default function App() {
  return (
    <>
      <Form />
      <ExpensiveTree />
    </>
  );
}

function Form() {
  let [color, setColor] = useState('red');
  return (
    <>
      <input value={color} onChange={(e) => setColor(e.target.value)} />
      <p style={{ color }}>Hello, world!</p>
    </>
  );
}
```

#### Solution 2: Lift content up

- Same logic as above, but now we doing it reversed, we keep the logic
  part that doesn't care about `color` state in `App` and move others logic
  above 1 level by using HoC concept.

```jsx
export default function App() {
  return (
    <ColorPicker>
      <p>Hello, world!</p>
      <ExpensiveTree />
    </ColorPicker>
  );
}

function ColorPicker({ children }) {
  let [color, setColor] = useState('red');
  return (
    <div style={{ color }}>
      <input value={color} onChange={(e) => setColor(e.target.value)} />
      {children}
    </div>
  );
}
```
