## Cypress vs Selenium

Pros:
- Complete framework(selenium is a library)
- more faster than selenium (executes in browser instead of webdriver)
- more stable
- not require solid programming skills
- test and mock APIs
- don't need test environment

Cons:
- no IE and Safari support
- asynchronous code
- no mobile
- single domain and single tab
- not friendly with iFrames

## Preparation of development environment

- install Chrome Browser
- install node.js
- install git
- install IDE

## Cypress installation

**npm install cypress --save-dev**

*--save-dev* - add package to dev dependency

**npx cypress open**

## Cypress Configuration

**cypress.json**
```
{
  "baseUrl": "http://localhost:4200",
  "ignoreTestFiles": "**/examples/*", //do not include example tests
  "viewportHeight": 1080,  //default properties have less resolution then the most of screens
  "viewportWidth": 1920,
}
```

## Creating a playground file

**firstTest.spec.js**
```
/// <reference types="cypress"/> //helps IDE to find functions

describe('Our first suite', () => {

  describe('Our suite section', () => { //nested describe
    beforeEach('code for every test', () = {
      // repetitive code
    })
    
    it('some test name', () => {

    })
  })
  
  it('first test', () => {
  
  })
  
  it('second test', () => {
  
  })
})

describe('Our second suite', () => { // we can provide many describe in one file
  it('first test', () => {
  
  })
})

//context() - equal to describe

```
