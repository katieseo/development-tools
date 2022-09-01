## React Typescript

right click > peek definition | go to definition

```
“always use interface for public API’s definition when authoring a library or 3rd-party ambient type definitions.”
“consider using type for your React Component Props and State, because it is more constrained.”
```
```
interface car { door: number; color: string; }

interface car { make: string; }

interface car is now { door: number; color: string; make: string; }

---

type car = { color:string }

type car = { make: string; }

type car is now { make:string }
```

#### props
array with obj
```javascript
type PropsT = {
    names: {
        first: string
        last: string
    }[]
}

const Part = (props:PropsT) => {
    return (
        <div>{props.names.map(n => n.first)}</div>
    )
}
const App = () => {
    return <Part names={[{first: 'Leanne', last: 'N'}, {first: 'Katie', last: 'S'}]} />
}
```
click event
```javascript
type Prop = {
  handleClick: (event:React.MouseEvent<HTMLButtonElement>, name:number) => void
}

const Button = (props: Prop) => {
  return <button onClick={(event)=>props.handleClick(event, 83)}>Click</button>
}


export default function App() {
  return (
    <div className="App">
      <Button handleClick={(event, id)=>console.log(event.type, id)} />
    </div>
  );
}
```
change event
```javascript
interface Props {
  .
  .
  .
  handleChange: (event: React.ChangeEvent<HTMLInputElement>) => void;
}

<input onChange={handleChange} />. <--- hover over onChange and copy (event: React.ChangeEvent<HTMLInputElement>)

```
```javascript
type Prop = {
  value: string
  handleChange: (event: React.ChangeEvent<HTMLInputElement>)=>void
}

const Input = (props:Prop) => {
  return <input type="text" value={props.value} onChange={props.handleChange} />
}


export default function App() {
  return (
    <div className="App">
      <Input value="" handleChange={(event)=>console.log(event.target)} />
    </div>
  );
}
```
CSSProperties
```javascript
type Prop = {
  styles?: React.CSSProperties
}

const Main = (props:Prop) => {
  return <div style={props.styles}></div>
}


export default function App() {
  return (
    <Main styles={{background: 'black', padding: '1rem'}} />
  );
}
```
Example.types.ts

> export type ExampleProps = {}

Example.tsx

> import {ExampleProps} from './Example.types'


#### hooks
useState (TS can infer the state type based on the initial value)
```javascript
const [count, setCount] = useState<number | null>(3);


interface TextNode {
  text:string
}  

const [count, setCount] = useState<TextNode>({text: 'hey'});
```

useRef
```javascript
const inputRef = useRef<>();
<input ref={} />   <-- make {} empty, hover over ref and find <HTMLInputElement>, copy into useRef


const inputRef = useRef<HTMLInputElement>(null);
<input ref={inputRef} />

```

useReducer
```javascript
type Actions = 
  | {type: 'add'; text: string;}
  | {type: 'remove'; idx: number;};

interface Todo {
  text: string;
  complete: boolean;
}

type State = Todo[];

cosnt TodoReducer= (state:State, action:Actions) => {
  switch (action.type) {
    case "add":
      return [...state, {text: action.text, complete: false}];
    case "remove":
      return state.filter((_, i) => action.idx !== i);  
    default:
      return state;
  }
}

export const ReducerEx: React.FC = () => {
  const [todos, dispatch] = useReducer(TodoReducer, []);
}

```
```
type AppState = {};
type Action =
  | { type: "SET_ONE"; payload: string }
  | { type: "SET_TWO"; payload: number };

export function reducer(state: AppState, action: Action): AppState {
  switch (action.type) {
    case "SET_ONE":
      return {
        ...state,
        one: action.payload // `payload` is string
      };
    case "SET_TWO":
      return {
        ...state,
        two: action.payload // `payload` is number
      };
    default:
      return state;
  }
}
```

#### render props
```javascript
type Props = {
  children?: React.ReactNode;
};

export const Main = ({ children }: Props) => {
  return (
    <>
      {children}
    </>
  );
};
```
```javascript
interface Props {
  children: (
    count: number, 
    setCount: React.Dispatch<React.SetStateAction<number>>    <---- hover setCount and copy
  ) => JSX.Element | null;
}

export const Counter: React.FC<Props> = ({children}) => {
  const [count, setCount] = useState(0);
  
  return <div>{children(count, setCount)}</div>
}

in App

<Counter>
  {(count, setCount) => (
    <>
      {count}
      <button onClick=>{() => setCount(count+1)} + </button>}
    </>
  )}
</Counter>
```
OR
```javascript
interface Props {
  children: (data: {      <----data:
    count: number, 
    setCount: React.Dispatch<React.SetStateAction<number>> 
  }) => JSX.Element | null;
}

return <div>{children({count, setCount})}</div>


in App

<Counter>
  {({count, setCount}) => (  <----destructure
    <>
      {count}
      <button onClick=>{() => setCount(count+1)} + </button>}
    </>
  )}
</Counter>
```

## Generics

#### Props, useState
```
interface Props {
    name: string;
}

const Hello:React.FC<Props> = (props) => {
    const [person] = useState<{age: number | null}>({age: 0})

    return <div>Hello {props.name}!</div>
}

const Main = () => {
    return <Hello name="John" />
}
```

#### JSX generic
```
interface FormProps<T> {
    values: T;
    children: (values: T) => JSX.Element
}

const Form = <T extends {}>({values, children}: FormProps<T>) => {
    return children(values)
}

const App = () => {
return (
    <>
        <Form<{name: string | null}> values={{name:'John'}}>
            {(values) => <div>{values.name}</div>}
        </Form>
    </>
)}
````

## Typescript basics


https://2ality.com/2018/04/type-notation-typescript.html

https://www.digitalocean.com/community/tutorials/how-to-use-generics-in-typescript

https://www.typescriptlang.org/play/


#### Type annotations
  type annotation: type expression
  let x: number;


#### Type inference
  TypeScript can often infer types


#### Specifying types
Static types for JavaScript’s dynamic types:
  -undefined, null
  -boolean, number, string
  -symbol
  -object
TypeScript-specific types:
  -Array (not technically a type in JS)
  -any (the type of all values)
  -Etc.
  
  
#### Type aliases
  type Age = number;
  const age: Age = 82;


#### Arrays
-lists
    1. let arr: number[] = []; (empty Array, so need help)
    2. let arr: Array<number> = [];
-tuples (The length of the Array is fixed)
    let point: [number, number] = [7, 5]; (otherwise infer as list type)
    
    // %inferred-type: [string, number][]
    const entries = Object.entries({ a: 1, b: 2 });      // [[ 'a', 1 ], [ 'b', 2 ]]);

#### Function types

    // we don’t need a type annotation here
    // %inferred-type: (num: number) => string
    const func = (num: number) => String(num);

    * complicated example
    
    function stringify123(callback: (num: number) => string) {
      return callback(123);
    }
    
    * It’s recommended to annotate all parameters of a function 
      (except for callbacks where more type information is available).
      
      We can also specify the result type:
      TypeScript is good at inferring result types, 
      but specifying them explicitly is occasionally useful.
      
        function stringify123(callback: (num: number) => string): string {
          return callback(123);
        }
    
    * :void - It tells TypeScript that the function always returns undefined 
      function f2(): void { }
      
    * ?: optional parameters
    
    * parameter default values
      Default values make parameters optional. 
      We can usually omit type annotations, because TypeScript can infer the types.
    
      If we wanted to add type annotations, that would look as follows.

        function createPoint(x:number = 0, y:number = 0): [number, number] {
          return [x, y];
        }
        
    * Rest parameters must be arrays
      function joinNumbers(...nums: number[]): string {
        return nums.join('-');
      }
      
      joinNumbers(1, 2, 3)

#### Union types
    function getScore(numberOrString: number|string): number {
    }
    
    function stringify123( callback: null | ((num: number) => string) ) {
    }
        

#### Typing objects
- Records: A fixed number of properties that are known at development time. Each property can have a different type.
- Dictionaries: An arbitrary number of properties whose names are not known at development time. One type per kind of key (mainly: string, symbol).

    * Typing objects-as-records via interfaces
    
    interface Point {
      x: number;
      y: number;
    }
    
    OR
    
    interface Point {
      x: number,
      y: number,
    }
    
    TypeScript’s type system works structurally
    
    function pointToString(pt: Point) {
      return `(${pt.x}, ${pt.y})`;
    }
    
    pointToString({x: 5, y: 7}) // compatible structure
    
    /////
    
    * Object literal types (anonymous interfaces)
    
    type Point = {
      x: number;
      y: number;
    };
    
    object literal types can be used inline:
    
    function pointToString(pt: {x: number, y: number}) {
      return `(${pt.x}, ${pt.y})`;
    }
    
    ////
    
    * ?: optional properties
    
    interface Person {
      name: string;
      company?: string
    }
    
    ////
    
    * Interfaces can also contain methods

#### Type variables and generic types

