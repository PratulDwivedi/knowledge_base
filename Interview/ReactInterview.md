# Top 200 React with TypeScript Interview Questions & Answers

> A comprehensive guide covering beginner to advanced topics for React + TypeScript interviews.

---

## Table of Contents

1. [React Basics](#react-basics) (Q1–Q30)
2. [TypeScript Fundamentals](#typescript-fundamentals) (Q31–Q60)
3. [React + TypeScript Integration](#react--typescript-integration) (Q61–Q100)
4. [Hooks](#hooks) (Q101–Q130)
5. [State Management](#state-management) (Q131–Q150)
6. [Performance Optimization](#performance-optimization) (Q151–Q165)
7. [Testing](#testing) (Q166–Q180)
8. [Advanced Patterns](#advanced-patterns) (Q181–Q200)

---

## React Basics

### Q1. What is React?
**Answer:** React is an open-source JavaScript library developed by Facebook for building user interfaces, particularly single-page applications. It uses a component-based architecture where UIs are broken into reusable, independent pieces, and it maintains a virtual DOM to efficiently update the real DOM.

---

### Q2. What is the Virtual DOM?
**Answer:** The Virtual DOM is a lightweight in-memory representation of the real DOM. When state or props change, React creates a new virtual DOM tree, diffs it against the previous one (reconciliation), and applies only the necessary changes to the real DOM — making updates fast and efficient.

---

### Q3. What are components in React?
**Answer:** Components are the building blocks of a React application. They are independent, reusable pieces of UI that can be either **functional** (plain functions returning JSX) or **class-based** (ES6 classes extending `React.Component`). Modern React strongly prefers functional components with hooks.

---

### Q4. What is JSX?
**Answer:** JSX (JavaScript XML) is a syntax extension that lets you write HTML-like markup inside JavaScript. It is transpiled to `React.createElement()` calls by tools like Babel. JSX makes it easier to visualize UI structure directly in code.

```tsx
const element = <h1 className="title">Hello, World!</h1>;
// Transpiles to:
const element = React.createElement('h1', { className: 'title' }, 'Hello, World!');
```

---

### Q5. What are props?
**Answer:** Props (short for properties) are read-only inputs passed from a parent component to a child component. They allow data to flow down the component tree and make components reusable and configurable.

```tsx
function Greeting({ name }: { name: string }) {
  return <h1>Hello, {name}!</h1>;
}
```

---

### Q6. What is state in React?
**Answer:** State is mutable data that is managed within a component. When state changes, React re-renders the component to reflect the new data. In functional components, state is managed using the `useState` hook.

```tsx
const [count, setCount] = React.useState<number>(0);
```

---

### Q7. What is the difference between props and state?
**Answer:**
- **Props** are passed from parent to child and are immutable within the child.
- **State** is managed internally within a component and can be changed using setter functions.
- Both trigger re-renders when changed, but only state can be mutated by the component itself.

---

### Q8. What is a controlled component?
**Answer:** A controlled component is a form element whose value is controlled by React state. Every state change is handled through an `onChange` handler, keeping the UI in sync with the state.

```tsx
const [value, setValue] = useState('');
<input value={value} onChange={(e) => setValue(e.target.value)} />
```

---

### Q9. What is an uncontrolled component?
**Answer:** An uncontrolled component manages its own state internally via the DOM. You use a `ref` to access the value when needed, rather than keeping it in React state.

```tsx
const inputRef = useRef<HTMLInputElement>(null);
<input ref={inputRef} />
// Access value: inputRef.current?.value
```

---

### Q10. What is the component lifecycle?
**Answer:** React components go through three lifecycle phases:
1. **Mounting** – Component is created and inserted into the DOM (`constructor`, `render`, `componentDidMount`)
2. **Updating** – Component re-renders due to state/props changes (`shouldComponentUpdate`, `render`, `componentDidUpdate`)
3. **Unmounting** – Component is removed from the DOM (`componentWillUnmount`)

In functional components, these are handled via the `useEffect` hook.

---

### Q11. What is reconciliation?
**Answer:** Reconciliation is the process by which React updates the DOM. When state or props change, React generates a new virtual DOM tree and compares it with the previous one using a diffing algorithm. It then makes the minimum number of changes required to update the real DOM.

---

### Q12. What are keys in React and why are they important?
**Answer:** Keys are special string attributes used when rendering lists. They help React identify which items have changed, been added, or removed, enabling efficient re-renders. Keys must be unique among siblings.

```tsx
const items = ['a', 'b', 'c'];
items.map((item) => <li key={item}>{item}</li>);
```

---

### Q13. What is the Context API?
**Answer:** The Context API allows you to share data across the component tree without manually passing props at every level (prop drilling). It consists of `React.createContext`, a `Provider`, and a `Consumer` (or `useContext` hook).

---

### Q14. What is prop drilling and how do you avoid it?
**Answer:** Prop drilling is passing data through many intermediate components that don't need it just to get it to a deeply nested child. You can avoid it using:
- **Context API**
- **State management libraries** (Redux, Zustand, Jotai)
- **Component composition**

---

### Q15. What are Higher-Order Components (HOCs)?
**Answer:** A Higher-Order Component is a function that takes a component and returns a new enhanced component. HOCs are used to share logic across components without duplicating code.

```tsx
function withLogger<T>(WrappedComponent: React.ComponentType<T>) {
  return (props: T) => {
    console.log('Rendered with props:', props);
    return <WrappedComponent {...props} />;
  };
}
```

---

### Q16. What are render props?
**Answer:** The render props pattern passes a function as a prop that a component uses to determine what to render. This allows sharing logic between components dynamically.

```tsx
<DataFetcher render={(data) => <DisplayComponent data={data} />} />
```

---

### Q17. What is `React.Fragment`?
**Answer:** `React.Fragment` (shorthand `<>...</>`) lets you group multiple elements without adding an extra node to the DOM. This is useful when a component needs to return multiple sibling elements.

```tsx
return (
  <>
    <h1>Title</h1>
    <p>Paragraph</p>
  </>
);
```

---

### Q18. What is lazy loading in React?
**Answer:** Lazy loading defers loading a component until it's needed, reducing the initial bundle size. It uses `React.lazy()` with `Suspense` to dynamically import components.

```tsx
const LazyComponent = React.lazy(() => import('./LazyComponent'));

<Suspense fallback={<div>Loading...</div>}>
  <LazyComponent />
</Suspense>
```

---

### Q19. What is `React.StrictMode`?
**Answer:** `React.StrictMode` is a development tool that helps identify potential problems. It activates additional warnings and double-invokes certain lifecycle methods and hooks to detect side effects. It has no effect in production.

---

### Q20. What is the difference between `React.memo` and `useMemo`?
**Answer:**
- `React.memo` is a higher-order component that memoizes a **component**, preventing re-renders if props haven't changed.
- `useMemo` is a hook that memoizes a **computed value** inside a component, recalculating only when dependencies change.

---

### Q21. What is `useCallback`?
**Answer:** `useCallback` returns a memoized version of a callback function that only changes if its dependencies change. It's useful when passing callbacks to child components to prevent unnecessary re-renders.

```tsx
const handleClick = useCallback(() => {
  doSomething(id);
}, [id]);
```

---

### Q22. What is the difference between `useEffect` and `useLayoutEffect`?
**Answer:**
- `useEffect` runs **asynchronously** after the browser has painted. Best for data fetching, subscriptions.
- `useLayoutEffect` runs **synchronously** after DOM mutations but before the browser paints. Best for measuring DOM elements or triggering immediate DOM changes.

---

### Q23. What is Error Boundary?
**Answer:** An Error Boundary is a class component that catches JavaScript errors in its child component tree and displays a fallback UI instead of crashing the entire app. It uses `componentDidCatch` and `getDerivedStateFromError`.

---

### Q24. What is `Suspense` in React?
**Answer:** `Suspense` is a React component that lets you specify a fallback UI while waiting for something (like a lazy-loaded component or async data) to load. It works with `React.lazy` and, in React 18+, with data-fetching libraries using `use()`.

---

### Q25. What is the difference between class components and functional components?
**Answer:**
- **Class components** use ES6 classes, have lifecycle methods, and use `this.state`/`this.setState`.
- **Functional components** are simpler functions, use hooks for state/lifecycle, are easier to test, and are the modern standard.

---

### Q26. What is `forwardRef`?
**Answer:** `forwardRef` allows a component to forward a ref it receives to a DOM element or another component inside it. This is useful for parent components that need direct access to child DOM nodes.

```tsx
const Input = React.forwardRef<HTMLInputElement, InputProps>((props, ref) => (
  <input ref={ref} {...props} />
));
```

---

### Q27. What is `useImperativeHandle`?
**Answer:** `useImperativeHandle` customizes the instance value (the ref object) exposed to parent components when using `forwardRef`. It lets you control what the parent can do with the ref.

```tsx
useImperativeHandle(ref, () => ({
  focus: () => inputRef.current?.focus(),
}));
```

---

### Q28. What is the significance of the `key` prop when it's changed?
**Answer:** When the `key` prop of a component changes, React treats it as a completely new component — unmounting the old one and mounting a fresh one. This is a useful trick to reset a component's state.

---

### Q29. What are synthetic events in React?
**Answer:** Synthetic events are React's cross-browser wrappers around native browser events. They provide a consistent API across all browsers. React uses event delegation, attaching a single listener at the root rather than on every element.

---

### Q30. What is the difference between `createElement` and JSX?
**Answer:** JSX is syntactic sugar for `React.createElement`. Both produce the same React element objects. JSX is preferred for readability; `createElement` is what the transpiler outputs.

```tsx
// JSX
const el = <div className="box">Hello</div>;
// createElement equivalent
const el = React.createElement('div', { className: 'box' }, 'Hello');
```

---

## TypeScript Fundamentals

### Q31. What is TypeScript?
**Answer:** TypeScript is a statically typed superset of JavaScript developed by Microsoft. It adds optional type annotations, interfaces, generics, and other features to JavaScript. TypeScript code is compiled (transpiled) to plain JavaScript.

---

### Q32. What are the basic types in TypeScript?
**Answer:** The basic types are: `string`, `number`, `boolean`, `null`, `undefined`, `any`, `unknown`, `never`, `void`, `object`, `symbol`, and `bigint`. Arrays use `string[]` or `Array<string>` syntax.

---

### Q33. What is the difference between `interface` and `type` in TypeScript?
**Answer:**
- Both can describe object shapes and be extended.
- `interface` supports **declaration merging** (you can declare the same interface twice to merge them).
- `type` can represent **unions, intersections, primitives, and mapped types** — more flexible.
- Prefer `interface` for object shapes (especially public APIs), `type` for complex type compositions.

---

### Q34. What are generics in TypeScript?
**Answer:** Generics allow writing reusable, type-safe functions, classes, and interfaces that work with multiple types without sacrificing type safety.

```tsx
function identity<T>(value: T): T {
  return value;
}
identity<string>('hello'); // returns string
identity<number>(42);      // returns number
```

---

### Q35. What is `any` vs `unknown` in TypeScript?
**Answer:**
- `any` disables all type checking — dangerous, should be avoided.
- `unknown` is the type-safe counterpart: you must perform type checks before using a value of type `unknown`.

```tsx
let x: unknown = 'hello';
if (typeof x === 'string') {
  console.log(x.toUpperCase()); // safe
}
```

---

### Q36. What is the `never` type?
**Answer:** `never` represents values that never occur. It's used for functions that always throw or never return (infinite loops), and as the bottom type in exhaustive checks.

```tsx
function throwError(msg: string): never {
  throw new Error(msg);
}
```

---

### Q37. What is `void` in TypeScript?
**Answer:** `void` represents the absence of a return value. It's used for functions that don't return anything meaningful (they may return `undefined` implicitly).

---

### Q38. What is a union type?
**Answer:** A union type allows a variable to be one of several types using the `|` operator.

```tsx
type Status = 'loading' | 'success' | 'error';
let currentStatus: Status = 'loading';
```

---

### Q39. What is an intersection type?
**Answer:** An intersection type combines multiple types into one using `&`. The resulting type has all properties of all combined types.

```tsx
type Admin = User & { adminLevel: number };
```

---

### Q40. What are TypeScript utility types?
**Answer:** TypeScript provides built-in utility types to transform types:
- `Partial<T>` – makes all properties optional
- `Required<T>` – makes all properties required
- `Readonly<T>` – makes all properties read-only
- `Pick<T, K>` – picks a subset of properties
- `Omit<T, K>` – omits a subset of properties
- `Record<K, V>` – creates an object type with keys K and values V
- `Exclude<T, U>` – excludes types from T that are assignable to U
- `Extract<T, U>` – extracts types from T that are assignable to U
- `ReturnType<T>` – gets the return type of a function type
- `Parameters<T>` – gets parameter types of a function type

---

### Q41. What is type narrowing?
**Answer:** Type narrowing is the process of refining a broad type to a more specific one using type guards like `typeof`, `instanceof`, `in`, or user-defined type guards.

```tsx
function process(value: string | number) {
  if (typeof value === 'string') {
    return value.toUpperCase();
  }
  return value.toFixed(2);
}
```

---

### Q42. What is a type guard?
**Answer:** A type guard is a runtime check that narrows a type within a conditional block. User-defined type guards use the `is` keyword in the return type.

```tsx
function isString(value: unknown): value is string {
  return typeof value === 'string';
}
```

---

### Q43. What are enums in TypeScript?
**Answer:** Enums define a set of named constants. They can be numeric (default) or string-based.

```tsx
enum Direction {
  Up = 'UP',
  Down = 'DOWN',
  Left = 'LEFT',
  Right = 'RIGHT',
}
```

---

### Q44. What is a tuple in TypeScript?
**Answer:** A tuple is a fixed-length array with known types at specific positions.

```tsx
const pair: [string, number] = ['age', 30];
```

---

### Q45. What are decorators in TypeScript?
**Answer:** Decorators are experimental features that allow annotating and modifying classes, methods, properties, or parameters at design time. They're widely used in frameworks like Angular and NestJS.

```tsx
@Component({ selector: 'app-root' })
class AppComponent {}
```

---

### Q46. What is `as const` in TypeScript?
**Answer:** `as const` asserts that a value should be treated as a readonly literal type, preventing widening. It's useful for defining constants with precise types.

```tsx
const COLORS = ['red', 'green', 'blue'] as const;
// Type: readonly ['red', 'green', 'blue']
```

---

### Q47. What is the `satisfies` operator?
**Answer:** Introduced in TypeScript 4.9, `satisfies` validates that an expression matches a type without changing its inferred type, giving you both type safety and precise inference.

```tsx
const config = {
  port: 3000,
  host: 'localhost',
} satisfies Record<string, string | number>;
```

---

### Q48. What is a mapped type?
**Answer:** A mapped type creates a new type by transforming the properties of an existing type.

```tsx
type Optional<T> = { [K in keyof T]?: T[K] };
```

---

### Q49. What is a conditional type?
**Answer:** Conditional types use the ternary-like `T extends U ? X : Y` syntax to select types based on conditions.

```tsx
type IsString<T> = T extends string ? 'yes' : 'no';
type Result = IsString<string>; // 'yes'
```

---

### Q50. What is `keyof` and `typeof` in TypeScript?
**Answer:**
- `keyof T` produces a union of the property keys of type `T`.
- `typeof x` (in a type position) gets the TypeScript type of variable `x`.

```tsx
type Keys = keyof { name: string; age: number }; // 'name' | 'age'
const config = { theme: 'dark' };
type Config = typeof config; // { theme: string }
```

---

### Q51. What is `infer` in TypeScript?
**Answer:** `infer` is used within conditional types to capture and infer a type variable from a matched type.

```tsx
type UnpackPromise<T> = T extends Promise<infer U> ? U : T;
type Result = UnpackPromise<Promise<string>>; // string
```

---

### Q52. What is declaration merging in TypeScript?
**Answer:** Declaration merging allows multiple declarations of the same name to be combined into a single definition. It works with interfaces, namespaces, and certain function overloads.

```tsx
interface User { name: string; }
interface User { age: number; }
// Merged: { name: string; age: number }
```

---

### Q53. What are function overloads in TypeScript?
**Answer:** Function overloads let you define multiple signatures for a single function to handle different input types.

```tsx
function format(value: string): string;
function format(value: number): string;
function format(value: string | number): string {
  return String(value);
}
```

---

### Q54. What is the `readonly` modifier?
**Answer:** `readonly` prevents a property from being reassigned after initialization.

```tsx
interface Point { readonly x: number; readonly y: number; }
const p: Point = { x: 1, y: 2 };
p.x = 5; // Error!
```

---

### Q55. What is strict mode in TypeScript?
**Answer:** Enabling `"strict": true` in `tsconfig.json` turns on a set of strict type checks including `strictNullChecks`, `strictFunctionTypes`, `noImplicitAny`, `strictBindCallApply`, and more. It's strongly recommended for all projects.

---

### Q56. What is `strictNullChecks`?
**Answer:** With `strictNullChecks` enabled, `null` and `undefined` are not assignable to other types. This forces explicit handling of nullability, catching many common bugs.

---

### Q57. What is a namespace in TypeScript?
**Answer:** Namespaces are a way to organize code and avoid naming collisions by grouping related code under a named scope. In modern TypeScript with modules, namespaces are rarely needed.

---

### Q58. What is the `Pick` utility type?
**Answer:** `Pick<T, K>` constructs a type by picking a set of properties `K` from type `T`.

```tsx
interface User { id: number; name: string; email: string; }
type UserPreview = Pick<User, 'id' | 'name'>;
// { id: number; name: string }
```

---

### Q59. What is the `Omit` utility type?
**Answer:** `Omit<T, K>` constructs a type by removing properties `K` from type `T`.

```tsx
type UserWithoutEmail = Omit<User, 'email'>;
// { id: number; name: string }
```

---

### Q60. What is `Record<K, V>`?
**Answer:** `Record<K, V>` creates an object type with keys of type `K` and values of type `V`. Useful for dictionaries and lookup maps.

```tsx
const scores: Record<string, number> = { alice: 95, bob: 87 };
```

---

## React + TypeScript Integration

### Q61. How do you type functional component props?
**Answer:** You can type props inline, with a separate `interface`, or with `type`. Using `React.FC` is optional and often discouraged for its implicit `children` prop behavior.

```tsx
interface ButtonProps {
  label: string;
  onClick: () => void;
  disabled?: boolean;
}

const Button = ({ label, onClick, disabled }: ButtonProps) => (
  <button onClick={onClick} disabled={disabled}>{label}</button>
);
```

---

### Q62. Should you use `React.FC` or not?
**Answer:** `React.FC` (also `React.FunctionComponent`) is debated. Downsides include implicit `children` typing (fixed in React 18) and issues with generics. Modern preference is to type props directly as function parameters without `React.FC`.

---

### Q63. How do you type `children` in React with TypeScript?
**Answer:** Use `React.ReactNode` for the most permissive children type (includes strings, numbers, elements, arrays, null, etc).

```tsx
interface ContainerProps {
  children: React.ReactNode;
}
```

---

### Q64. How do you type event handlers?
**Answer:** Use `React.ChangeEvent<T>`, `React.MouseEvent<T>`, etc., where `T` is the HTML element type.

```tsx
const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
  setValue(e.target.value);
};

const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => {
  e.preventDefault();
};
```

---

### Q65. How do you type the `useState` hook?
**Answer:** TypeScript usually infers the type from the initial value. For complex types or when starting with `null`, provide an explicit generic.

```tsx
const [name, setName] = useState<string>('');
const [user, setUser] = useState<User | null>(null);
```

---

### Q66. How do you type the `useReducer` hook?
**Answer:** Define types for state and action separately, then pass them as generics.

```tsx
type State = { count: number };
type Action = { type: 'increment' } | { type: 'decrement' };

function reducer(state: State, action: Action): State {
  switch (action.type) {
    case 'increment': return { count: state.count + 1 };
    case 'decrement': return { count: state.count - 1 };
  }
}

const [state, dispatch] = useReducer(reducer, { count: 0 });
```

---

### Q67. How do you type the `useRef` hook?
**Answer:** Provide the element type as a generic. Initialize with `null` for DOM refs.

```tsx
const inputRef = useRef<HTMLInputElement>(null);
// Access: inputRef.current?.focus()
```

---

### Q68. How do you type the `useContext` hook?
**Answer:** Define the context type when creating the context with `createContext`. Use `useContext` with the created context.

```tsx
interface ThemeContextType { theme: 'light' | 'dark'; toggle: () => void; }
const ThemeContext = createContext<ThemeContextType | undefined>(undefined);

// In consumer:
const ctx = useContext(ThemeContext);
if (!ctx) throw new Error('Must be inside ThemeProvider');
```

---

### Q69. How do you type `useEffect`?
**Answer:** `useEffect` takes a callback returning `void` or a cleanup function `() => void`. TypeScript infers these automatically, but you must ensure the cleanup is typed correctly.

```tsx
useEffect(() => {
  const id = setInterval(() => tick(), 1000);
  return () => clearInterval(id); // cleanup
}, []);
```

---

### Q70. How do you type a custom hook?
**Answer:** Type the return value explicitly, especially when returning tuples (use `as const` or explicit tuple types).

```tsx
function useCounter(initial: number): [number, () => void, () => void] {
  const [count, setCount] = useState(initial);
  const increment = () => setCount(c => c + 1);
  const decrement = () => setCount(c => c - 1);
  return [count, increment, decrement];
}
```

---

### Q71. How do you type generic components?
**Answer:** Use generic type parameters on the component function. Note: with arrow functions you may need a trailing comma to avoid JSX parsing ambiguity.

```tsx
interface ListProps<T> {
  items: T[];
  renderItem: (item: T) => React.ReactNode;
}

function List<T>({ items, renderItem }: ListProps<T>) {
  return <ul>{items.map((item, i) => <li key={i}>{renderItem(item)}</li>)}</ul>;
}
```

---

### Q72. How do you type `forwardRef` with TypeScript?
**Answer:** Pass two generics to `forwardRef`: the ref type and the props type.

```tsx
interface InputProps { placeholder?: string; }

const Input = React.forwardRef<HTMLInputElement, InputProps>(
  ({ placeholder }, ref) => <input ref={ref} placeholder={placeholder} />
);
```

---

### Q73. How do you handle optional props with default values?
**Answer:** Declare props as optional with `?` and use JavaScript default parameter values.

```tsx
interface ButtonProps {
  label: string;
  variant?: 'primary' | 'secondary';
}

function Button({ label, variant = 'primary' }: ButtonProps) {
  return <button className={variant}>{label}</button>;
}
```

---

### Q74. How do you type a component that accepts `style` prop?
**Answer:** Use `React.CSSProperties` for inline style objects.

```tsx
interface BoxProps {
  style?: React.CSSProperties;
}
```

---

### Q75. How do you type the `className` prop?
**Answer:** Use `string` for a single class, or `string | undefined` if it's optional.

```tsx
interface Props { className?: string; }
```

---

### Q76. How do you spread HTML attributes onto a component?
**Answer:** Extend your props interface with the appropriate HTML element attributes using `React.HTMLAttributes` or more specific types.

```tsx
interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: 'primary' | 'secondary';
}

function Button({ variant = 'primary', ...rest }: ButtonProps) {
  return <button className={variant} {...rest} />;
}
```

---

### Q77. What is `ComponentProps` utility type?
**Answer:** `React.ComponentProps<T>` extracts the props type from a component. Useful for wrapping or extending existing components.

```tsx
type MyButtonProps = React.ComponentProps<'button'>;
type CustomInputProps = React.ComponentProps<typeof Input>;
```

---

### Q78. How do you type async event handlers?
**Answer:** Mark the handler as `async`. The event type remains the same — just add `async` before the arrow function.

```tsx
const handleSubmit = async (e: React.FormEvent<HTMLFormElement>) => {
  e.preventDefault();
  await submitData();
};
```

---

### Q79. How do you type a context with a default value that avoids undefined?
**Answer:** Use a non-null assertion or a custom hook that throws if the context is undefined.

```tsx
function useTheme() {
  const ctx = useContext(ThemeContext);
  if (!ctx) throw new Error('useTheme must be used within ThemeProvider');
  return ctx;
}
```

---

### Q80. How do you type `React.lazy`?
**Answer:** `React.lazy` accepts a function that returns a promise of a module with a default export. TypeScript infers the type automatically.

```tsx
const LazyPage = React.lazy(() => import('./Page'));
// TypeScript knows LazyPage has the same props as Page
```

---

### Q81. How do you type `useCallback`?
**Answer:** TypeScript usually infers the callback type. When ambiguous, provide generics or explicit parameter/return types.

```tsx
const fetch = useCallback(async (id: number): Promise<User> => {
  const res = await api.getUser(id);
  return res.data;
}, []);
```

---

### Q82. How do you type `useMemo`?
**Answer:** TypeScript infers the return type from the factory function. You can be explicit if needed.

```tsx
const sorted = useMemo<string[]>(
  () => [...items].sort(),
  [items]
);
```

---

### Q83. How do you type HOCs in TypeScript?
**Answer:** Use generics to preserve the wrapped component's prop types.

```tsx
function withAuth<P extends object>(WrappedComponent: React.ComponentType<P>) {
  return function AuthenticatedComponent(props: P) {
    const isAuth = useAuth();
    if (!isAuth) return <Redirect to="/login" />;
    return <WrappedComponent {...props} />;
  };
}
```

---

### Q84. How do you type render props with TypeScript?
**Answer:** Type the render function prop explicitly.

```tsx
interface RenderProps<T> {
  data: T;
  render: (data: T) => React.ReactNode;
}

function DataProvider<T>({ data, render }: RenderProps<T>) {
  return <>{render(data)}</>;
}
```

---

### Q85. How do you type a custom Error Boundary?
**Answer:** Error boundaries must be class components. Type state with an `hasError` flag.

```tsx
interface State { hasError: boolean; error?: Error; }

class ErrorBoundary extends React.Component<React.PropsWithChildren, State> {
  state: State = { hasError: false };

  static getDerivedStateFromError(error: Error): State {
    return { hasError: true, error };
  }

  componentDidCatch(error: Error, info: React.ErrorInfo) {
    logError(error, info);
  }

  render() {
    if (this.state.hasError) return <div>Something went wrong.</div>;
    return this.props.children;
  }
}
```

---

### Q86. What is `React.ElementType` vs `React.ComponentType`?
**Answer:**
- `React.ComponentType<P>` accepts class or function components with props `P`.
- `React.ElementType` is broader — it includes strings (like `'div'`, `'span'`) and components, making it suitable for polymorphic components.

---

### Q87. How do you build polymorphic components?
**Answer:** Polymorphic components render as different elements based on an `as` prop. Type it carefully with generics.

```tsx
type PolymorphicProps<C extends React.ElementType> = {
  as?: C;
} & React.ComponentPropsWithoutRef<C>;

function Text<C extends React.ElementType = 'p'>({ as, ...props }: PolymorphicProps<C>) {
  const Component = as ?? 'p';
  return <Component {...props} />;
}
```

---

### Q88. How do you type discriminated unions for component variants?
**Answer:** Use discriminated unions to type different component states exclusively.

```tsx
type AlertProps =
  | { type: 'success'; message: string }
  | { type: 'error'; message: string; code: number };

function Alert(props: AlertProps) {
  if (props.type === 'error') {
    return <div>Error {props.code}: {props.message}</div>;
  }
  return <div>{props.message}</div>;
}
```

---

### Q89. How do you type `useImperativeHandle`?
**Answer:** Define a handle interface and use it as the generic for both `forwardRef` and `useImperativeHandle`.

```tsx
interface InputHandle { focus: () => void; clear: () => void; }

const Input = forwardRef<InputHandle, InputProps>((props, ref) => {
  const inputRef = useRef<HTMLInputElement>(null);
  useImperativeHandle(ref, () => ({
    focus: () => inputRef.current?.focus(),
    clear: () => { if (inputRef.current) inputRef.current.value = ''; },
  }));
  return <input ref={inputRef} {...props} />;
});
```

---

### Q90. How do you configure `tsconfig.json` for a React project?
**Answer:** Key settings for React + TypeScript:

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "lib": ["DOM", "DOM.Iterable", "ESNext"],
    "jsx": "react-jsx",
    "strict": true,
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "noEmit": true,
    "esModuleInterop": true,
    "skipLibCheck": true
  }
}
```

---

### Q91. What is `@types/react`?
**Answer:** `@types/react` is the TypeScript type definitions package for React. It provides types for all React APIs, JSX, DOM events, and component types. Install with `npm install -D @types/react @types/react-dom`.

---

### Q92. How do you type a form submission handler?
**Answer:** Use `React.FormEvent<HTMLFormElement>` for the event type.

```tsx
const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
  e.preventDefault();
  const formData = new FormData(e.currentTarget);
  const name = formData.get('name') as string;
};
```

---

### Q93. How do you type a ref callback?
**Answer:** Use the function form of ref with a typed parameter.

```tsx
<div ref={(el: HTMLDivElement | null) => { if (el) el.scrollIntoView(); }} />
```

---

### Q94. How do you use `as` type assertions safely?
**Answer:** Use `as` only when you know more than TypeScript about the type, and prefer type guards where possible. Double assertions (`as unknown as T`) are a code smell.

```tsx
const input = document.getElementById('email') as HTMLInputElement;
// Better: use a type guard
const el = document.getElementById('email');
if (el instanceof HTMLInputElement) { /* safe */ }
```

---

### Q95. How do you type API responses?
**Answer:** Define interfaces matching the expected response shape and assert or validate them.

```tsx
interface UserResponse { id: number; name: string; email: string; }

async function fetchUser(id: number): Promise<UserResponse> {
  const res = await fetch(`/api/users/${id}`);
  return res.json() as Promise<UserResponse>;
}
```

---

### Q96. What is `PropsWithChildren`?
**Answer:** `React.PropsWithChildren<P>` adds `children?: React.ReactNode` to an existing props type.

```tsx
type CardProps = React.PropsWithChildren<{ title: string }>;
```

---

### Q97. How do you type SVG imports in TypeScript?
**Answer:** Declare a module for SVG files in a `.d.ts` file, or use tools like SVGR that provide typed React components.

```tsx
// declarations.d.ts
declare module '*.svg' {
  const ReactComponent: React.FC<React.SVGProps<SVGSVGElement>>;
  export default ReactComponent;
}
```

---

### Q98. How do you handle index signatures with TypeScript in React?
**Answer:** Use index signatures to allow dynamic keys while still typing values.

```tsx
interface Styles {
  [key: string]: React.CSSProperties;
}
```

---

### Q99. How do you use template literal types?
**Answer:** Template literal types let you build string types by combining other string types.

```tsx
type EventName = 'click' | 'focus' | 'blur';
type Handler = `on${Capitalize<EventName>}`; // 'onClick' | 'onFocus' | 'onBlur'
```

---

### Q100. How do you avoid the `any` type in React components?
**Answer:** Replace `any` with:
- `unknown` + type guards for truly unknown values
- Generics for flexible but typed components
- Union types for multiple known types
- `React.ReactNode` for content/children
- `Record<string, unknown>` for dynamic objects

---

## Hooks

### Q101. What are React hooks?
**Answer:** Hooks are functions introduced in React 16.8 that let functional components use state, lifecycle features, context, and other React features without writing class components. Core rules: only call hooks at the top level and only inside React functions.

---

### Q102. What are the rules of hooks?
**Answer:**
1. **Only call hooks at the top level** — not inside loops, conditions, or nested functions.
2. **Only call hooks from React functions** — functional components or custom hooks, not regular JS functions.
These rules ensure that hook call order is consistent across renders.

---

### Q103. What does `useState` return?
**Answer:** `useState` returns a tuple: `[currentState, setterFunction]`. The setter can accept a new value or an updater function `(prev) => newValue`.

---

### Q104. What is the functional form of `setState`?
**Answer:** When the new state depends on the previous state, pass an updater function to avoid stale closure issues.

```tsx
setCount(prev => prev + 1);
```

---

### Q105. How does `useEffect` work?
**Answer:** `useEffect(effect, deps)` runs the effect after render. The dependency array controls when it re-runs:
- Empty `[]` — runs once after mount
- No array — runs after every render
- `[dep1, dep2]` — runs when any dependency changes
The returned function is a cleanup that runs before the next effect or on unmount.

---

### Q106. How do you prevent `useEffect` from running on initial render?
**Answer:** Use a ref to track whether it's the first render.

```tsx
const isMounted = useRef(false);
useEffect(() => {
  if (!isMounted.current) { isMounted.current = true; return; }
  // Your effect here
}, [dependency]);
```

---

### Q107. What is `useLayoutEffect` used for?
**Answer:** `useLayoutEffect` fires synchronously after DOM mutations but before the browser paints, making it ideal for measuring DOM elements, synchronizing scroll positions, or preventing visual flickering.

---

### Q108. How does `useCallback` prevent re-renders?
**Answer:** By memoizing the function reference, child components that receive it as a prop (and are wrapped in `React.memo`) won't re-render because the function reference stays the same between parent renders.

---

### Q109. What is `useMemo` and when should you use it?
**Answer:** `useMemo` memoizes an expensive computation, recalculating only when dependencies change. Use it for computationally heavy operations or when referential equality of an object/array matters for preventing child re-renders.

---

### Q110. What is `useRef` used for beyond DOM refs?
**Answer:** `useRef` can store any mutable value that persists across renders without triggering re-renders — perfect for interval IDs, previous values, or caching without state.

```tsx
const prevCount = useRef(count);
useEffect(() => { prevCount.current = count; });
```

---

### Q111. How do you implement `usePrevious`?
**Answer:** Store the previous value in a ref and update it in a `useEffect`.

```tsx
function usePrevious<T>(value: T): T | undefined {
  const ref = useRef<T>();
  useEffect(() => { ref.current = value; }, [value]);
  return ref.current;
}
```

---

### Q112. How do you create a custom hook for data fetching?
**Answer:**

```tsx
function useFetch<T>(url: string) {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);

  useEffect(() => {
    let cancelled = false;
    fetch(url)
      .then(r => r.json())
      .then(d => { if (!cancelled) setData(d); })
      .catch(e => { if (!cancelled) setError(e); })
      .finally(() => { if (!cancelled) setLoading(false); });
    return () => { cancelled = true; };
  }, [url]);

  return { data, loading, error };
}
```

---

### Q113. What is `useReducer` and when to prefer it over `useState`?
**Answer:** `useReducer` manages complex state logic with multiple sub-values or when the next state depends on the previous state in non-trivial ways. It mirrors the Redux pattern: `(state, action) => newState`.

---

### Q114. What is `useId`?
**Answer:** `useId` (React 18+) generates a unique ID that is stable across server and client renders, preventing hydration mismatches. Use it for accessibility attributes like `htmlFor` and `aria-labelledby`.

```tsx
const id = useId();
return <label htmlFor={id}>Name<input id={id} /></label>;
```

---

### Q115. What is `useTransition`?
**Answer:** `useTransition` (React 18+) marks state updates as non-urgent, allowing urgent updates (like typing) to interrupt them. Returns `[isPending, startTransition]`.

```tsx
const [isPending, startTransition] = useTransition();
startTransition(() => setSearchResults(filter(query)));
```

---

### Q116. What is `useDeferredValue`?
**Answer:** `useDeferredValue` defers updating a value, keeping the UI responsive. Similar to `useTransition` but works with values from props or other sources you don't control.

```tsx
const deferredQuery = useDeferredValue(query);
```

---

### Q117. What is `useSyncExternalStore`?
**Answer:** `useSyncExternalStore` (React 18+) is the recommended way to subscribe to external stores. It correctly handles concurrent rendering and avoids tearing.

```tsx
const state = useSyncExternalStore(store.subscribe, store.getSnapshot);
```

---

### Q118. What is `useInsertionEffect`?
**Answer:** `useInsertionEffect` fires before DOM mutations and is intended for CSS-in-JS libraries to inject styles before any layout effects read them. Not meant for general use.

---

### Q119. How do you debounce a value with a custom hook?
**Answer:**

```tsx
function useDebounce<T>(value: T, delay: number): T {
  const [debounced, setDebounced] = useState(value);
  useEffect(() => {
    const timer = setTimeout(() => setDebounced(value), delay);
    return () => clearTimeout(timer);
  }, [value, delay]);
  return debounced;
}
```

---

### Q120. How do you detect a click outside a component?
**Answer:**

```tsx
function useClickOutside(ref: React.RefObject<HTMLElement>, callback: () => void) {
  useEffect(() => {
    const handler = (event: MouseEvent) => {
      if (ref.current && !ref.current.contains(event.target as Node)) {
        callback();
      }
    };
    document.addEventListener('mousedown', handler);
    return () => document.removeEventListener('mousedown', handler);
  }, [ref, callback]);
}
```

---

### Q121. How do you listen to window resize events?
**Answer:**

```tsx
function useWindowSize() {
  const [size, setSize] = useState({ width: window.innerWidth, height: window.innerHeight });
  useEffect(() => {
    const handleResize = () => setSize({ width: window.innerWidth, height: window.innerHeight });
    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);
  return size;
}
```

---

### Q122. What is `useOptimistic` in React 19?
**Answer:** `useOptimistic` allows showing an optimistic UI state immediately while an async operation is in progress, and automatically reverts to the real state when the operation completes.

---

### Q123. How do you implement a `useLocalStorage` hook?
**Answer:**

```tsx
function useLocalStorage<T>(key: string, initialValue: T) {
  const [value, setValue] = useState<T>(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch { return initialValue; }
  });

  const setStoredValue = (newValue: T) => {
    setValue(newValue);
    window.localStorage.setItem(key, JSON.stringify(newValue));
  };

  return [value, setStoredValue] as const;
}
```

---

### Q124. What is the problem with stale closures in hooks?
**Answer:** A stale closure occurs when an effect or callback captures an outdated version of a variable (e.g., state or props) from a previous render. Solve with the functional updater form of `setState`, `useRef` to store current values, or including all dependencies in the dependency array.

---

### Q125. What is `useEvent` (proposed hook)?
**Answer:** `useEvent` is a proposed React hook that would give you a stable function reference that always has access to the latest state/props, solving the stale closure vs. memoization tradeoff. Currently available as a userland pattern using `useRef`.

---

### Q126. How do you test custom hooks?
**Answer:** Use `@testing-library/react`'s `renderHook` utility to test hooks in isolation.

```tsx
import { renderHook, act } from '@testing-library/react';
const { result } = renderHook(() => useCounter(0));
act(() => result.current.increment());
expect(result.current.count).toBe(1);
```

---

### Q127. How do you handle race conditions in `useEffect`?
**Answer:** Use a cleanup flag or `AbortController` to cancel in-flight requests when the component unmounts or the effect re-runs.

```tsx
useEffect(() => {
  const controller = new AbortController();
  fetch(url, { signal: controller.signal })
    .then(r => r.json()).then(setData)
    .catch(err => { if (!controller.signal.aborted) setError(err); });
  return () => controller.abort();
}, [url]);
```

---

### Q128. What is `useContext` and when should you not use it?
**Answer:** `useContext` reads and subscribes to a context. Every context change causes all consumers to re-render. Avoid for high-frequency updates; use state management libraries for that. Good for: themes, locale, auth state.

---

### Q129. How do you memoize context to prevent re-renders?
**Answer:** Wrap the context value in `useMemo` so the object reference stays stable when its dependencies haven't changed.

```tsx
const value = useMemo(() => ({ theme, toggle }), [theme]);
return <ThemeContext.Provider value={value}>{children}</ThemeContext.Provider>;
```

---

### Q130. What is the `use` hook in React 19?
**Answer:** The `use` hook allows reading the value of a Promise or Context inside a component (or even inside conditions and loops). When passed a Promise, it integrates with Suspense.

```tsx
const data = use(fetchDataPromise); // Suspends until resolved
```

---

## State Management

### Q131. What is Redux and how does it work?
**Answer:** Redux is a predictable state management library. It maintains a single global store. State changes happen via pure functions called reducers, triggered by dispatched actions. React components connect to the store using hooks like `useSelector` and `useDispatch`.

---

### Q132. What is Redux Toolkit (RTK)?
**Answer:** Redux Toolkit is the official, opinionated way to write Redux logic. It includes utilities like `createSlice`, `createAsyncThunk`, and `configureStore` that reduce boilerplate significantly.

---

### Q133. How do you type Redux state and actions with TypeScript?
**Answer:**

```tsx
interface CounterState { value: number; }
const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 } as CounterState,
  reducers: {
    increment: state => { state.value += 1; },
  },
});

// Type RootState and AppDispatch
type RootState = ReturnType<typeof store.getState>;
type AppDispatch = typeof store.dispatch;
```

---

### Q134. What is Zustand?
**Answer:** Zustand is a lightweight, minimal state management library for React. It uses a simple hook-based API without providers, actions, or reducers. It's popular for its simplicity and small bundle size.

```tsx
const useStore = create<State>((set) => ({
  count: 0,
  increment: () => set(state => ({ count: state.count + 1 })),
}));
```

---

### Q135. What is Jotai?
**Answer:** Jotai is an atomic state management library inspired by Recoil. State is broken into atoms (small pieces), and components subscribe only to the atoms they need, minimizing re-renders.

---

### Q136. What is Recoil?
**Answer:** Recoil is Facebook's state management library that uses atoms and selectors. Atoms hold state; selectors compute derived state. Components subscribe to atoms/selectors and only re-render when subscribed values change.

---

### Q137. What is the Context + useReducer pattern?
**Answer:** Using React Context with `useReducer` provides a Redux-like pattern without external libraries. Good for medium complexity apps.

```tsx
const CounterContext = createContext<{ state: State; dispatch: Dispatch<Action> } | undefined>(undefined);

function CounterProvider({ children }: { children: React.ReactNode }) {
  const [state, dispatch] = useReducer(reducer, initialState);
  return <CounterContext.Provider value={{ state, dispatch }}>{children}</CounterContext.Provider>;
}
```

---

### Q138. What is React Query (TanStack Query)?
**Answer:** React Query (now TanStack Query) is a data-fetching and server state management library. It handles caching, background refetching, loading/error states, and stale-while-revalidate patterns with minimal boilerplate.

```tsx
const { data, isLoading, error } = useQuery<User>({
  queryKey: ['user', id],
  queryFn: () => fetchUser(id),
});
```

---

### Q139. What is SWR?
**Answer:** SWR (stale-while-revalidate) is Vercel's data-fetching hook library. It returns cached data immediately, then revalidates in the background. Simple, lightweight, and great for read-heavy data.

---

### Q140. When should you use global state vs local state?
**Answer:**
- **Local state** (`useState`) — data specific to one component (form input values, toggle states, UI details)
- **Global state** (Context, Redux, Zustand) — data shared across many components (auth user, theme, cart)
- **Server state** (React Query, SWR) — data that lives on a server and needs caching/sync

---

### Q141. What is the flux pattern?
**Answer:** Flux is the application architecture pattern that Redux is based on. Data flows in one direction: Action → Dispatcher → Store → View. The view dispatches new actions to complete the cycle. It prevents the complex state bugs of bidirectional data flow.

---

### Q142. What is `immer` and how is it used with Redux?
**Answer:** Immer allows writing "mutating" code that actually produces immutable updates. Redux Toolkit uses Immer internally, enabling you to write `state.count += 1` in reducers instead of returning a new object.

---

### Q143. How do you handle optimistic updates?
**Answer:** Update the UI immediately (optimistically) before the async operation confirms, then revert or confirm based on the result.

```tsx
// React Query optimistic update
useMutation({
  mutationFn: updateTodo,
  onMutate: async (newTodo) => {
    await queryClient.cancelQueries({ queryKey: ['todos'] });
    const previous = queryClient.getQueryData(['todos']);
    queryClient.setQueryData(['todos'], old => [...old, newTodo]);
    return { previous };
  },
  onError: (err, newTodo, context) => {
    queryClient.setQueryData(['todos'], context?.previous);
  },
});
```

---

### Q144. What is `atom` in Jotai?
**Answer:** An `atom` in Jotai is the smallest unit of state. It's created with `atom(initialValue)` and accessed with `useAtom(myAtom)`. Derived atoms can compute values from other atoms.

---

### Q145. How does Redux DevTools help?
**Answer:** Redux DevTools is a browser extension that lets you inspect every dispatched action, view the state tree after each action, time-travel to previous states, and replay actions — making debugging much easier.

---

### Q146. What is `createSelector` in Redux?
**Answer:** `createSelector` (from Reselect, included in RTK) creates memoized selectors that recompute only when their inputs change, preventing unnecessary re-renders.

```tsx
const selectTotal = createSelector(
  (state: RootState) => state.cart.items,
  items => items.reduce((sum, item) => sum + item.price, 0)
);
```

---

### Q147. What is `configureStore` in Redux Toolkit?
**Answer:** `configureStore` sets up the Redux store with sensible defaults: Redux DevTools enabled, `redux-thunk` middleware included, and Immer integration. It's the RTK replacement for `createStore`.

---

### Q148. How do you persist Redux state?
**Answer:** Use `redux-persist` to serialize and save the Redux store to `localStorage` (or other storage). Configure which slices to persist and rehydrate on app load.

---

### Q149. What is `createAsyncThunk`?
**Answer:** `createAsyncThunk` handles async action creators, automatically dispatching `pending`, `fulfilled`, and `rejected` actions based on a Promise's lifecycle.

```tsx
export const fetchUser = createAsyncThunk('users/fetch', async (id: number) => {
  const res = await api.getUser(id);
  return res.data;
});
```

---

### Q150. What are the tradeoffs between Redux and Zustand?
**Answer:**
- **Redux/RTK**: Verbose, structured, powerful DevTools, best for large teams and complex apps.
- **Zustand**: Minimal API, less boilerplate, easier to start with, great for small-to-medium apps. Less opinionated structure.

---

## Performance Optimization

### Q151. What is code splitting?
**Answer:** Code splitting breaks the JavaScript bundle into smaller chunks that are loaded on demand. React supports it natively via `React.lazy` + dynamic `import()`. Tools like Webpack and Vite handle the actual splitting.

---

### Q152. How does `React.memo` work?
**Answer:** `React.memo` wraps a component and performs a shallow comparison of props. If props haven't changed, the component skips re-rendering.

```tsx
const MyComponent = React.memo(({ value }: { value: number }) => <div>{value}</div>);
```

---

### Q153. What is the difference between shallow and deep comparison?
**Answer:** Shallow comparison checks whether primitive values are equal and whether object/array references are the same. Deep comparison recursively checks all properties. React uses shallow comparison for `React.memo` and `PureComponent`, which is why stable references matter.

---

### Q154. When should you use `useMemo` and `useCallback`?
**Answer:** Use them only when you have a measurable performance problem. Premature optimization adds complexity. Use `useMemo` for expensive calculations; use `useCallback` when passing callbacks to memoized children. Profile first with React DevTools.

---

### Q155. What is React Profiler?
**Answer:** The React DevTools Profiler records rendering performance. It shows which components rendered, how long they took, and why they rendered. Use it to identify bottlenecks before optimizing.

---

### Q156. What is windowing/virtualization?
**Answer:** Windowing renders only the visible portion of a large list instead of all items. Libraries like `react-window` and `react-virtual` implement this pattern, drastically reducing DOM nodes and improving performance for long lists.

---

### Q157. How do you avoid unnecessary re-renders?
**Answer:**
- Use `React.memo` for pure components
- Use `useCallback`/`useMemo` for stable references
- Split components so only the parts that need to re-render are subscribed to state
- Use selectors in Redux to select minimal state slices
- Avoid creating new objects/arrays in render

---

### Q158. What is `Concurrent Mode` in React 18?
**Answer:** Concurrent Mode enables React to render multiple versions of the UI simultaneously. It allows React to pause, interrupt, and resume rendering, making apps feel more responsive. It's enabled by using `createRoot` instead of `render`.

---

### Q159. What is tree shaking?
**Answer:** Tree shaking is a build optimization that removes unused code (dead code) from the final bundle. It relies on ES modules' static import/export syntax. Ensure libraries support ES modules for tree shaking to work.

---

### Q160. How does image optimization affect React performance?
**Answer:** Use appropriately sized images, modern formats (WebP, AVIF), lazy loading (`loading="lazy"`), and CDNs. In Next.js, the `<Image>` component handles this automatically.

---

### Q161. What is `startTransition` and how does it improve UX?
**Answer:** `startTransition` marks updates as non-urgent, allowing React to prioritize more urgent updates (like user input). The deferred update renders in the background without blocking the UI.

---

### Q162. How do you profile React performance?
**Answer:**
1. Open React DevTools → Profiler tab
2. Click record, interact with the app, stop recording
3. Analyze the flame graph to see render times and reasons
4. Look for components with long render times or unnecessary renders

---

### Q163. What is bundle analyzer and why use it?
**Answer:** Tools like `webpack-bundle-analyzer` or `vite-bundle-visualizer` create a visual map of your bundle's contents. They help identify large dependencies that could be replaced, lazy-loaded, or removed.

---

### Q164. What are web workers and how can they help React apps?
**Answer:** Web workers run JavaScript in a background thread, off the main thread. Heavy computations can be offloaded to prevent UI freezes. Libraries like `comlink` simplify communication between the main thread and workers.

---

### Q165. What is `Suspense` for data fetching?
**Answer:** In React 18+, Suspense can wrap data-fetching that integrates with the Suspense model (via libraries like React Query or `use()`). While data loads, Suspense shows a fallback, enabling declarative loading states without `isLoading` checks.

---

## Testing

### Q166. What is React Testing Library?
**Answer:** React Testing Library (RTL) is a testing utility that encourages testing components as a user would interact with them — by querying by accessible roles, labels, and text — rather than by implementation details.

---

### Q167. What is the difference between unit, integration, and E2E tests?
**Answer:**
- **Unit tests** test a single function or component in isolation
- **Integration tests** test multiple components working together
- **E2E (end-to-end) tests** simulate real user flows through the entire app (e.g., Cypress, Playwright)

---

### Q168. How do you render a component in a test?
**Answer:**

```tsx
import { render, screen } from '@testing-library/react';
import Button from './Button';

test('renders button label', () => {
  render(<Button label="Click me" onClick={() => {}} />);
  expect(screen.getByRole('button', { name: 'Click me' })).toBeInTheDocument();
});
```

---

### Q169. What is `screen` in React Testing Library?
**Answer:** `screen` is a convenience object that provides all query methods bound to `document.body`. It's the recommended way to query the DOM in tests — more readable than destructuring from `render`.

---

### Q170. What are the different query types in RTL?
**Answer:**
- `getBy*` — returns element or throws if not found
- `queryBy*` — returns element or `null` (for asserting absence)
- `findBy*` — async, returns a promise (for async elements)
- Variants include `ByRole`, `ByText`, `ByLabelText`, `ByPlaceholderText`, `ByTestId`, `ByAltText`

---

### Q171. How do you fire events in tests?
**Answer:** Use `userEvent` from `@testing-library/user-event` (preferred, simulates real user behavior) or `fireEvent` (lower level).

```tsx
import userEvent from '@testing-library/user-event';

test('increments counter on click', async () => {
  const user = userEvent.setup();
  render(<Counter />);
  await user.click(screen.getByRole('button', { name: /increment/i }));
  expect(screen.getByText('1')).toBeInTheDocument();
});
```

---

### Q172. How do you mock API calls in tests?
**Answer:** Use `msw` (Mock Service Worker) to intercept network requests at the service worker level, providing realistic API mocking without modifying application code.

```tsx
const server = setupServer(
  rest.get('/api/user', (req, res, ctx) => {
    return res(ctx.json({ name: 'Alice' }));
  })
);
```

---

### Q173. How do you test custom hooks?
**Answer:** Use `renderHook` from `@testing-library/react`.

```tsx
import { renderHook, act } from '@testing-library/react';
const { result } = renderHook(() => useCounter(0));
expect(result.current.count).toBe(0);
act(() => result.current.increment());
expect(result.current.count).toBe(1);
```

---

### Q174. What is Jest and how is it used with React?
**Answer:** Jest is a JavaScript testing framework that provides a test runner, assertion library, and mocking capabilities. Create React App and Vite both have Jest (or Vitest) pre-configured. It works seamlessly with `@testing-library/react`.

---

### Q175. What is Vitest?
**Answer:** Vitest is a Vite-native testing framework compatible with Jest's API. It's faster than Jest for Vite projects due to shared configuration and native ES module support.

---

### Q176. What is snapshot testing?
**Answer:** Snapshot tests serialize a component's output and save it to a file. Future runs compare against this snapshot, failing if output changes. Useful for catching unintended UI changes, but can become brittle if overused.

```tsx
test('matches snapshot', () => {
  const { asFragment } = render(<Button label="OK" />);
  expect(asFragment()).toMatchSnapshot();
});
```

---

### Q177. How do you test context-dependent components?
**Answer:** Wrap the component with the context provider in the test render.

```tsx
render(
  <ThemeProvider theme="dark">
    <ThemedButton />
  </ThemeProvider>
);
```

Or create a custom `renderWithProviders` utility.

---

### Q178. What is `waitFor` in React Testing Library?
**Answer:** `waitFor` retries an assertion until it passes or times out. Use it for async operations that update the DOM.

```tsx
await waitFor(() => {
  expect(screen.getByText('Data loaded')).toBeInTheDocument();
});
```

---

### Q179. How do you test TypeScript-specific behavior?
**Answer:** For type-level testing, use `tsd` or `expect-type` packages to assert types without running code. For component testing, TypeScript catches type errors at compile time, complementing runtime tests.

```tsx
import { expectType } from 'tsd';
expectType<string>(identity('hello'));
```

---

### Q180. What is the test pyramid and how does it apply to React?
**Answer:** The test pyramid recommends many unit tests, fewer integration tests, and even fewer E2E tests. For React:
- Many: hook unit tests, pure utility function tests
- Some: component integration tests with RTL
- Few: Cypress/Playwright E2E tests for critical user flows

---

## Advanced Patterns

### Q181. What is the Compound Component pattern?
**Answer:** Compound components share implicit state between a parent and its children, providing a flexible API while hiding implementation details.

```tsx
function Tabs({ children }: { children: React.ReactNode }) {
  const [active, setActive] = useState(0);
  return <TabsContext.Provider value={{ active, setActive }}>{children}</TabsContext.Provider>;
}

Tabs.Tab = function Tab({ index, label }: { index: number; label: string }) {
  const { active, setActive } = useContext(TabsContext)!;
  return <button onClick={() => setActive(index)} className={active === index ? 'active' : ''}>{label}</button>;
};
```

---

### Q182. What is the Observer/Pub-Sub pattern in React?
**Answer:** Components can subscribe to an event emitter (external to React) and react to published events. Useful for cross-cutting concerns like toast notifications or analytics events, where Context would cause too many re-renders.

---

### Q183. What is the Container/Presentational pattern?
**Answer:** Separate components into containers (manage logic, data fetching, state) and presentational components (pure UI, driven by props). This improves reusability and testability. Modern hooks have reduced the need for this strict separation.

---

### Q184. What is the Facade pattern in React?
**Answer:** A facade component hides the complexity of multiple child components and provides a simplified API. For example, a `DateRangePicker` that composes two `DatePicker` components and manages their coordination internally.

---

### Q185. What is the Strategy pattern applied to React?
**Answer:** The strategy pattern passes a behavior (function or component) as a prop, allowing runtime selection of algorithms. The render prop and HOC patterns are implementations of this idea.

---

### Q186. What is code colocating in React?
**Answer:** Colocation means keeping state, logic, and styles as close as possible to where they're used. Avoid lifting state unnecessarily. This improves code readability, reduces props drilling, and makes refactoring easier.

---

### Q187. What are React Server Components (RSC)?
**Answer:** React Server Components (introduced in React 18, adopted by Next.js 13+) render on the server, have zero bundle size impact, can access server resources directly (databases, file system), but cannot use hooks or browser APIs. They're distinguished from client components with the `'use client'` directive.

---

### Q188. What is the `'use client'` directive?
**Answer:** `'use client'` marks a component and its imports as client-side code in frameworks using React Server Components (e.g., Next.js). Without it, components are server components by default. Client components can use hooks and browser APIs.

---

### Q189. What is streaming SSR?
**Answer:** Streaming SSR allows the server to progressively send HTML to the browser as it's generated, rather than waiting for the entire page. Combined with Suspense, React can send a shell immediately and stream in Suspense boundaries as their data resolves.

---

### Q190. What is Hydration in React?
**Answer:** Hydration is the process where React attaches event handlers to server-rendered HTML. Instead of re-rendering the whole page, React walks the existing DOM and makes it interactive. Hydration errors occur when the server and client render different HTML.

---

### Q191. What is `use server` in React/Next.js?
**Answer:** `'use server'` marks a function as a Server Action — an async function that runs on the server, callable from client components. It enables form submissions and mutations without creating API routes.

---

### Q192. What is the Strangler Fig pattern for React migration?
**Answer:** The Strangler Fig pattern involves incrementally migrating a legacy app to React by introducing new React components/pages alongside old code. Over time, legacy parts are "strangled" out as React components take over.

---

### Q193. How do you handle internationalization (i18n) in React?
**Answer:** Use libraries like `react-i18next` or `react-intl`. They provide hooks and context to translate strings, format dates/numbers, and switch locales.

```tsx
const { t } = useTranslation();
return <h1>{t('welcome.title')}</h1>;
```

---

### Q194. What are micro-frontends in React?
**Answer:** Micro-frontends decompose a frontend app into independently deployed features owned by different teams. React apps can be embedded as micro-frontends using module federation (Webpack), web components, or iframe-based approaches.

---

### Q195. What is the Builder pattern in component design?
**Answer:** The Builder pattern creates complex objects step by step. In React, a fluent API can chain configuration methods before building a component or data structure. Less common in React, but useful for complex form builders or query builders.

---

### Q196. How do you handle accessibility (a11y) in React?
**Answer:**
- Use semantic HTML elements
- Add ARIA attributes when semantic elements aren't sufficient (`aria-label`, `aria-expanded`, `role`)
- Manage focus with `useRef` for modals and dialogs
- Use `useId` for linking labels to inputs
- Test with tools like `axe-core`, `eslint-plugin-jsx-a11y`, and screen readers

---

### Q197. What is the difference between CSR, SSR, SSG, and ISR?
**Answer:**
- **CSR** (Client-Side Rendering): Browser downloads JS and renders in the browser. Slow initial load, fast subsequent navigation.
- **SSR** (Server-Side Rendering): HTML generated per request on the server. Good for SEO and initial load.
- **SSG** (Static Site Generation): HTML generated at build time. Fastest, but data can be stale.
- **ISR** (Incremental Static Regeneration, Next.js): Static pages regenerated in the background at intervals. Combines SSG speed with fresh data.

---

### Q198. What is the Module Federation pattern?
**Answer:** Module Federation (a Webpack 5 feature) allows multiple separate builds to share code at runtime, enabling micro-frontend architectures. One app can dynamically load components from another deployed app without rebuilding.

---

### Q199. How do you implement feature flags in React?
**Answer:** Feature flags (toggles) enable/disable features without code deployments.

```tsx
function Feature({ flag, children }: { flag: string; children: React.ReactNode }) {
  const isEnabled = useFeatureFlag(flag);
  return isEnabled ? <>{children}</> : null;
}

// Usage
<Feature flag="new-dashboard"><NewDashboard /></Feature>
```

Use services like LaunchDarkly, Unleash, or custom Context-based solutions.

---

### Q200. What is the future of React?
**Answer:** The React ecosystem is evolving toward:
- **React Compiler** (formerly React Forget) — auto-memoization, removing need for `useMemo`/`useCallback`
- **React Server Components** — seamless server/client boundary
- **Server Actions** — direct server mutations from components
- **`use` hook** — simplified async data reading with Suspense
- **Concurrent features** — `useTransition`, `useDeferredValue` becoming standard patterns
- **Better TypeScript integration** — improved generics and type inference

---

## Quick Reference

| Topic | Key Concepts |
|-------|-------------|
| State | `useState`, `useReducer`, Redux, Zustand |
| Side Effects | `useEffect`, `useLayoutEffect`, cleanup |
| Performance | `React.memo`, `useMemo`, `useCallback`, code splitting |
| TypeScript | Generics, utility types, discriminated unions |
| Patterns | HOC, render props, compound components |
| Testing | RTL, Jest/Vitest, `renderHook`, MSW |
| Modern React | Concurrent Mode, RSC, Server Actions |

---
