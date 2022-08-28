- React Testing Library
- JEST
- Test-driven development (TDD)

## Resources
**Wallaby.js** (VS code extension)

    - cmd+shift+p > Wallaby.js start
    
    - right click > Wallaby.js Line Tests > Launch Coverage & Test Explorer

- Reaching DOM elem & Interect DOM elem:

https://testing-library.com/docs/react-testing-library/intro/
(Core API / Queries / ByRole)

- Test:

https://jestjs.io/docs/expect

- Implicit ARIA semantics:

https://www.w3.org/TR/html-aria/#docconformance


## React Testing
#### Basic Examples

```
import { render, screen } from '@testing-library/react';
import App from './App';

test('renders learn react link', () => {
  render(<App />);
  const elem = screen.getByText(/learn react/i);
  expect(elem).toBeInTheDocument();
});
```
```
onClick={handleClick}

fireEvent.click(buttonEl);
```
```
onChange={handleChange}

const testValue = "test";
fireEvent.change(idElem, { target: { value: testValue } });     <---input value
expect(idElem.value).toBe(testValue);
```

#### Queries
```
<a data-testid='my-test-id'>mylink</a>

const elem = screen.getByTestId('my-test-id')
const el = screen.getByText('mylink') or /^mylink$/i
```
```
<li></li>
<li></li>
<li></li>

const elem = screen.getAllByRole('listitem');
expect(elem).toHaveLength(3);
expect(elem.length).toBe(3);
expect(elem.length).toEqual(3);
```

```
<span title="sum">{3+2}</span>

const elem = screen.getByTitle('sum');
expect(elem.textContent).toBe('5');
```

#### more matchers
```
.toBeGreaterThanOrEqual(3.5);
.toBeLessThanOrEqual(4.5);
```
```
.toBeDisabled();
not.toHaveTextContent(/loading/i);
```
```
style={{visibility: 'hidden'}}

expect(errorMsgElem).not.toBeVisible();
```

#### useState
```
when state is coming from parent component:

render(<App><Comp /></App>)
```

#### useReducer
test initial state, reducers, and actions
```
export const SUCCESS = {
  type: "SUCCESS",
};

expect(myReducer(initialState, SUCCESS)).toEqual({ myState: true });
```
then test the component
```
expect(valueEl.textContent).toBe("False");
fireEvent.click(buttonEl);
expect(valueEl.textContent).toBe("True");
```

#### useContext
```
render(<App />)
```
This will work since other's are both child components of <App /> they are rendered automatically.
Don't need this code
```
render(
      <App>
        <Context.Provider>
          <Comp />
        </Context.Provider>
      </App>
    );
```

#### Controlled component Forms
```
<label htmlFor="username">Username:</label>
<input type="text" id="username" placeholder="username" value={username} onChange={handleChange}/>

const inputEl = screen.getByLabelText(/Username/i);
const testValue = "test";

fireEvent.change(inputEl, { target: { value: testValue } });
```
```
<form data-testid="form" onSubmit={handleSubmit}>

fireEvent.submit(formEl, {target: {text1: {value: 'Text' } } })
```

#### useEffect and API requests with axios

create :file_folder and file: __ mocks __/axios.js

In component file
```
const handleClick = async () => {
    const { data } = await axios.get(
      "https://jsonplaceholder.typicode.com/users/1"
    );
    setUser(data);
  };
```


In filename.test.js
```
import Comp from "./Comp";
import { fireEvent, render, screen, waitFor } from "@testing-library/react";

jest.mock("axios", () => ({
  __esModule: true,
  default: {
    get: () => ({
      data: { id: 1, name: "Leanne" },
    }),
  },
}));

describe("comp test", () => {
  test("should return username", async () => {
    render(<Comp />);
    const buttonEl = screen.getByRole("button");
    const usernameEl = screen.getByText(/^Username/i);
    fireEvent.click(buttonEl);
    await waitFor(() =>
      expect(usernameEl.textContent).toBe("Username: Leanne")
    );
  });
});
```
```
text("", async () => {
    .
    .
    const userText = await screen.findByText("Jane") **not getByText
    expect(userText).toBeInTheDocument();
})
```

## Jest
#### Mock Function
```
test("should add 1,2 and give result 3", () => {
  const v1 = 1;
  const v2 = 2;
  const mockFn = jest.fn().mockImplementation((a, b) => a + b);

  expect(mockFn(v1, v2)).toBe(3);
});
```
```
test("should return 3", () => {
  const mockFn = jest.fn().mockReturnValue(3);
  expect(mockFn()).toBe(3);
});
```


#### npm test -- --coverage

> Always active code coverage (package.json)
```
"scripts": {
    "test": "jest"
},
"jest": {
  "collectCoverage": true
},
```
or
```
"scripts": {
    "test": "jest --coverage"
},
```
or
```
"scripts": {
    "test": "jest"
},
"jest": {
  "collectCoverage": true,
  "coverageReporters": ["html"]
},
```
