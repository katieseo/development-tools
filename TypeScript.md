## React Typescript

TIP: vs code - right click > peek definition | go to definition

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

interface AuthUser {
  name: string
  email: string
}  

const [user, setUser] = useState<AuthUser | null>(null);
.
.
.
<p>{user?.name}

OR -----
const [user, setUser] = useState<AuthUser>({} as AuthUser);

```

useRef
```javascript
const inputRef = useRef<HTMLInputElement>(null)

useEffect(()=>{
    inputRef.current?.focus()
},[])

return (
    <div className="App">
      <input type="text" ref={inputRef}/>
    </div>
);


OR

const inputRef = useRef<HTMLInputElement>(null!).  <--- null!
inputRef.current.focus()    <--- don't need ?

```
```javascript
mutable case:

useRef<number | null>(null)

if (intervalRef.current) window.clearInterval(intervalRef.current)

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
type State = {count: number, name: string}

  type Action = 
    | {type: 'INCREMENT'; payload: number}
    | {type: 'SETNAME'}
 
  const initialState = {count: 0, name: ''};

  const reducer = (state: State, action:Action) => {
    switch(action.type){
      case 'INCREMENT':
        return {...state, count: state.count + action.payload}
      case 'SETNAME':
        return {...state, name: 'Leanne'}
      default:
        return state
    }
  }

  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div className="App">
      {state.count} {state.name}
      <button onClick={()=>dispatch({type:"INCREMENT", payload: 10})}>increment count</button>
      <button onClick={()=>dispatch({type: "SETNAME"})}>set name</button>
    </div>
  );
```
Context API
```javascript

===== ThemeContext.tsx

import { createContext } from "react";

const theme = {
  primary: {
    main: 'black',
    text: 'white'
  },
  secondary: {
    main: 'white',
    text: 'black'
  }
}

type ThemeContextProvider = {
  children: React.ReactNode;
};

export const ThemeContext = createContext(theme);

export const ThemeContextProvider = ({ children }: ThemeContextProvider) => {
  return (
    <ThemeContext.Provider value={theme}>{children}</ThemeContext.Provider>
  );
};


===== App.tsx

<ThemeContextProvider>
    <Box />
</ThemeContextProvider>


===== Box.tsx

import {useContext} from 'react'
import {ThemeContext} from './ThemeContext'

export const Box = () => {
  const theme = useContext(ThemeContext)
  return <div style={{background: theme.primary.main, color: theme.primary.text}}>Theme</div>
}

```
```javascript
===== USerContext.tsx
import {useState, createContext} from 'react';

export type AuthUser = {
  name: string
  email: string
}

type UserContextType = {
  user: AuthUser | null
  setUser: React.Dispatch<React.SetStateAction<AuthUser | null>>
}

type UserContextProviderProp = {
  children: React.ReactNode
}

export const UserContext = createContext<UserContextType | null>(null)
// OR = createContext({} as UserContextType) --> don't need to check if userContext?

export const UserContextProvider = ({children}:UserContextProviderProp) => {
  const [user, setUser] = useState<AuthUser | null>(null)

  return (
    <UserContext.Provider value={{user, setUser}} >
      {children}
    </UserContext.Provider>
  )
}

===== App.tsx

<UserContextProvider>
    <User />
</UserContextProvider>


===== User.tsx

import {useContext} from 'react'
import {UserContext} from './UserContext'

export const User = () => {
  const userContext = useContext(UserContext)
  const handleLogin = () => {
    if (userContext) {
      userContext.setUser({
        name: 'Leanne',
        email:'l@l.com'
      })
    }
  }

  return (
    <div>
      <button onClick={handleLogin}>Login</button>
      name: {userContext?.user?.name}
    </div>
  )
}

```

#### Component Prop
```javascript
==== App.tsx
<Private isLoggedIn={true} component={Profile}  />

==== Profile.tsx
export type ProfileProps = {
  name: string
}

export const Profile = ({name}:ProfileProps) => {
  return <div>{name}</div>
}

==== Private.tsx
import {Login} from './Login'
import {ProfileProps} from './Profile' <--- import type for name prop to pass

type Props = {
  isLoggedIn: boolean
  component: React.ComponentType<ProfileProps> <--- add it here for name prop
}

export const Private = ({isLoggedIn, component:Component}:Props) => {
  return (
    isLoggedIn ? <Component name="Leanne" /> : <Login />
  )
}

```
#### Generic Props
```javascript
=== App.tsx
<List items={[1,2,3]} handleClick={(item) => console.log(item)} />

=== List.tsx

type Props<T> = {
  items: T[]
  handleClick: (value:T) => void
}

export const List = <T extends {}>({items, handleClick}:Props<T>) => {
  return (
    <div>
      {items.map(item => (
        <div onClick={()=>handleClick(item)}>{item}</div>
      ))}
    </div>
  )
}

*examples:
<T extends string | number>
<T extends {id: number}>

```

#### Restricting Props
```javascript
type RandomNumber = {
    value: number
}

type PositiveNumber = RandomNumber & {
    isPositive: boolean
    isNegative?: never
    isZero?: never
}

type RandomNumberProps = PositiveNumber | NegativeNumber | Zero

```

#### Template Literals and Exclude

```javascript
type HorizontalPosition = "Left" | "center" | "right"
type VerticalPosition = "Top" | "center" | "bottom"

type ToastProps = {
    position: `${HorizontalPosition}-${VerticalPosition}`
}

export const Toast = ({position}:ToastProps) => {
    return <div>{position}</div>
}

*** exclude 'center-center'
*** position: Exclude<`${HorizontalPosition}-${VerticalPosition}`, 'center-center'> | 'center'

```
#### Wrapping HTML Elements
```javascript

type ButtonProps = {
    variant: 'primary' | 'secondary'
    children: string
} & Omit<React.ComponentProps<'button'>, 'children'>

export const CustomButton = ({variant, children, ...rest}:ButtonProps) => {
    return (
        <button className={`button-${variant}`} {...rest}>
            {children}
        </button>
    )
}

export const ParentPage = () => {
    return (
        <CustomButton variant='primary' onClick={()=>console.log('clicked')>Primary Button</CustomButton>}
    )
}

```
```
type InputProps = React.ComponentProps<'input'>

export const CustomInput = (props: InputProps) => {
    return <input {...props} />
}

```


#### Extracting a Components Prop Types
```javascript
import { Greet } from '../Greet'

exrpot const CustomComponent = (props: React.ComponentProps<typeof Greet>)=> {
   return ...
}

```

#### Polymorphic Components

```javascript
type TextOwnProps<E extends React.ElementType> = {
  size?:'sm' | 'md' | 'lg'
  children: React.ReactNode
  as?: E
}

type TextProps<E extends React.ElementType> = TextOwnProps<E> & Omit<React.ComponentProps<E>, keyof TextOwnProps<E>>

export const Text = <E extends React.ElementType = 'div'>({size, children, as}:TextProps<E>) => {
  const Component = as || 'div'

  return (
    <Component className={`class-width-${size}`}>{children}</Component>
  )
}

export const ParentPage = () => {
  return (
    <div>
      <Text as='h1' size='sm'>H1</Text>
      <Text as='label' size='sm' htmlFor="username">Label</Text>
    </div>
  )
}

```




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

