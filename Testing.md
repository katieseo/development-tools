Wallaby.js (VS code extension)

https://jestjs.io/docs/expect

https://testing-library.com/docs/react-testing-library/intro/
Core API / Queries / ByRole

https://www.w3.org/TR/html-aria/#docconformance
Implicit ARIA semantics


- React Testing Library
- JEST
- Test-driven development (TDD)


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


## React Testing

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
