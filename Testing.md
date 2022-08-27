- React Testing Library
- JEST
- Test-driven development (TDD)

## Resources
**Wallaby.js** (VS code extension)

    - cmd+shift+p > Wallaby.js start
    
    - right click > Wallaby.js Line Tests > Launch Coverage & Test Explorer

Reaching DOM elem & Interect DOM elem:
https://testing-library.com/docs/react-testing-library/intro/
(Core API / Queries / ByRole)

Test:
https://jestjs.io/docs/expect

https://www.w3.org/TR/html-aria/#docconformance
Implicit ARIA semantics


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
<a data-testid='my-test-id'>

const elem = screen.getByTestId('my-test-id')
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

#### More
```
style={{visibility: 'hidden'}}

expect(errorMsgElem).not.toBeVisible();
```

```
onChange={handleChange}

const testValue = "test";
fireEvent.change(idElem, { target: { value: testValue } });
expect(idElem.value).toBe(testValue);
```
```
onChange={handleChange}

const testValue = "test";
fireEvent.change(idElem, { target: { value: testValue } });
expect(idElem.value).toBe(testValue);
```

## Jest
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
