## React Typescript
#### props
```
interface Person {
  fName: string;
  lName: string;
}

interface Props {
  text? : string;
  fn: (name: string) => void;
  obj: {
    title: string
  }
  person: Person
}

export const Comp: React.FC<Props> = () => {
  return ()
}
```
#### hooks
useState
```
const [count, setCount] = useState<number | null>(3);


interface TextNode {
  text:string
}  

const [count, setCount] = useState<TextNode>({text: 'hey'});
```

useRef
```
const inputRef = useRef<>();
<input ref={} />   <-- make {} empty, hover over ref and find <HTMLInputElement>, copy into useRef


const inputRef = useRef<HTMLInputElement>(null);
<input ref={inputRef} />

```

useReducer
```
type State = Todo[];

cosnt TodoReducer= (state:State, action:Action) => {
  switch (action.type) {
    case "add":
      return [...state, {text: action.text, complete: false}];
    default:
      return state;
  }
}

export const ReducerEx: React.FC = () => {
  const [todos, dispatch] = useReducer(TodoReducer, []);
}

```

#### event
```
interface Props {
  .
  .
  .
  handleChange: (event: React.ChangeEvent<HTMLInputElement>) => void;
}

<input onChcnage={handleChange} />. <--- hover over onChange and copy (event: React.ChangeEvent<HTMLInputElement>)

```



https://www.sitepoint.com/react-with-typescript-best-practices/

[Components] 

// Written as a function declaration
function Heading(): React.ReactNode {
  return <h1>My Website Heading</h1>
}

// Written as a function expression
const OtherHeading: React.FC = () => <h1>My Website Heading</h1>

[Props] 

interface Props {
  name: string;
  color: string;
}

type OtherProps = {
  name: string;
  color: string;
}

// Notice here we're using the function declaration with the interface Props
function Heading({ name, color }: Props): React.ReactNode {
  return <h1>My Website Heading</h1>
}

// Notice here we're using the function expression with the type OtherProps
const OtherHeading: React.FC<OtherProps> = ({ name, color }) =>
  <h1>My Website Heading</h1>

*
“always use interface for public API’s definition when authoring a library or 3rd-party ambient type definitions.”
“consider using type for your React Component Props and State, because it is more constrained.”

* Other example

type Props = {
  /** color to use for the background */
  color?: string;
  /** standard children prop: accepts any valid React Node */
  children: React.ReactNode;
  /** callback function passed to the onClick handler*/
  onClick: ()  => void;
}

const Button: React.FC<Props> = ({ children, color = 'tomato', onClick }) => {
   return <button style={{ backgroundColor: color }} onClick={onClick}>{children}</button>
}


[Hooks] 
type User = {
  email: string;
  id: string;
}

// the generic is the < >
// the union is the User | null
// together, TypeScript knows, "Ah, user can be User or null".
const [user, setUser] = useState<User | null>(null);


**

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



********* Typescipt Generics

///**** Props, useState */

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


///**** JSX generic */

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


Basics ============================================================================


https://2ality.com/2018/04/type-notation-typescript.html
https://www.digitalocean.com/community/tutorials/how-to-use-generics-in-typescript
https://www.typescriptlang.org/play/


* Type annotations
  type annotation: type expression
  let x: number;


* Type inference
  TypeScript can often infer types


* Specifying types
Static types for JavaScript’s dynamic types:
  -undefined, null
  -boolean, number, string
  -symbol
  -object
TypeScript-specific types:
  -Array (not technically a type in JS)
  -any (the type of all values)
  -Etc.
  
  
* Type aliases
  type Age = number;
  const age: Age = 82;


* Arrays
-lists
    1. let arr: number[] = []; (empty Array, so need help)
    2. let arr: Array<number> = [];
-tuples (The length of the Array is fixed)
    let point: [number, number] = [7, 5]; (otherwise infer as list type)
    
    // %inferred-type: [string, number][]
    const entries = Object.entries({ a: 1, b: 2 });      // [[ 'a', 1 ], [ 'b', 2 ]]);


* Function types

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

* Union types
    function getScore(numberOrString: number|string): number {
    }
    
    function stringify123( callback: null | ((num: number) => string) ) {
    }
        

* Typing objects
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
    

* Type variables and generic types

