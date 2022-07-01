## Types of locators

**cy.visit('/')** - start running tests

//by inner html text
  ``cy.contains('Forms').click();
    cy.contains('Form Layouts').click();
  ``

//by tag name
   ``cy.get('input');``
   
//by id
   ``cy.get('#inputEmail');``
   
//by class name
   ``cy.get('.input-full-width');``
   
//by attribute name
   ``cy.get('[placeholder]');``
   
//by attribute name and value
   ``cy.get('[placeholder="Email"]');``
   
//by class value
   ``cy.get('[class="input-full-width size-medium"]'); // we should provide entire value for class attribute``
   
//by tag name and attribute with value
   ``cy.get('input[placeholder="Email"]');``
   
//by two different attributes
   ``cy.get('[placeholder="Email"][fullwidth]');``
   
//by tag name, attribute with value, Id and class name
   ``cy.get('input[placeholder="Email"]#inputEmail.input-full-width');``
   
//The most recommended way by cypress
  ``cy.get('[data-cy="inputEmail1"]');``

## Finding web elements
```
cy.get('[data-cy="signInButton"]');
cy.contains('Sign in'); // how the text is written exectly as in the dom

cy.contains('[status="warning"]', 'Sign in') // have attribute and contains sign in

cy.get('#inputEmail3').parents('form').find('button'); // find() - search element inside parent
cy.get('#inputEmail3')
  .parents('form')
  .find('button')
  .should('contain', 'Sign in')  // verify something
  .parents('form')
  .find('nb-checkbox')
  .click()
  
cy.contains('nb-card', 'horizontal form').find('[type="email"]');
```

## saving subject of the command (then and wrap methods)

- **should()**
```
cy.contains('nb-card', 'using the grid').find('[for="inputEmail1"').should('contain', 'email')
```
***You can not save the result value of cy.contains() because cypress is asynchronous***
```
// does not work (selenium style
const firstForm = cy.contains('nb-card', 'using the grid')
firstForm.find('[for="inputEmail1"').should('contain', 'email')
```
- **then()**
- **expect()**
```
cy.contains('nb-card', 'using the grid').then(firstFrom => {
  const emailLabelFirst = firstForm.find('[for="inputEmail1"]').text()
  expect(emailLabelFirst).to.equal('Password')
}
```
when we call **then()** - parameters of this function(firstForm) become jquery object, not a cypress object anymore
Thats why we are using **expect()** method instead of **should()** and others functions
Qjuery object have not such functions like **click()** or **type()** but we can assign the result of **find** function to a variable

We can swith from Jquery object to cypress objects using function 
- **wrap()**
```
cy.wrap(firstFrom).find('[for="inputEmail1"').should('contain', 'email')
```
