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
   
//**The most recommended way by cypress
  ``cy.get('[data-cy="inputEmail1"]');``**
  
//The child element of some element
  ``cy.get('nav nb-select');``

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

## Invoke command
for using jquery method from cypress object
can be useful for:
- **getting attributes values**
```
cy.get('[for="exampleInputEmail1"]').invoke('text').then(text => {
  expect(text).to.equal('Email address');
})

cy.contains('nb-card', 'basic form)
  .find('nb-checkbox')
  .click()
  .find('.custom-checkbox')
  .invoke('attr','class')
  //.should('contain','checked')
  .then(classValue => {
    expect(classValues).to.contain('checked')
  })
```
- **getting value for an input field**
```
cy.contains('nb-card', 'Common Datepicker').find('input').then( input => {
  cy.wrap(input).click()
  cy.get('nb-calendar-day-picker').contains('17').click()
  cy.wrap(input).invoke('prop', 'value').should('contain', 'Dec 17, 2019')
  
})
```
## Radiobuttons and checkboxes
command: **check()** // only check checkbox, but not uncheck if it's checked
for uncheck use **click()** method
**first()** // for array of elements
```
cy.contains('nb-card', "Using the Grid").find('[type="radio"]').then(radioButtons => {
  cy.wrap(radioButtons)
    .first()
    .check({force: true})   // {force: true} disable cypress default check for element to be visible
    .should('be.checked')
    
  cy.wrap(radioButtons).eq(1).check({force: true}) // check the second radio button
  cy.wrap(radioButtons).eq(0).should('not.be.checked')
  cy.wrap(radioButtons).eq(2).should('be.disabled')
})
```
## Lists and dropdowns 

```
cy.get('options-list').contains('Dark').click()

// check if have proper css style
cy.get('nb-layout-header nav').should('have.css', 'background-color', 'rgb(43,43,69)')

// check if have proper text content
`cy.get('nav nb-select').should('contain', 'Dark')
```
**check all options in dropdown menu**
```
cy.get('nav nb-select').then( dropdown => {
  cy.wrap(dropdown).click()
  cy.get('.options-list nb-options').each((listItem, index) => {
    const itemText = listItem.text().trim() // trim() for removing trailing backspaces
    
    cy.wrap(listItem).click()
    cy.wrap.(dropdown).should('contain', itemText)
    
    if (index < 3) {
      cy.wrap(dropdown).click()
    }
  })
})
```
**select()** command
- works only when your tag name is **select**
- option can be selected not only by a text, but also by value
