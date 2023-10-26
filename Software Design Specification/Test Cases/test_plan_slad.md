# Unit Testing (Ingredient Class)

## Testing the Operations for the Ingredient Class

### Ingredient.setIngredientType()

#### Test for Blank Input, Invalid Input, Valid Input. (Predefined types: meat, seafood, fruit, vegetable, spice, sauce)
  - Empty Input: setIngredientType(“ empty “)
  - Invalid Input: setIngredientType(“Car”)
  - Valid Input: setIngredientType(“Meat”)

### Ingredient.setIngredientName()

#### Test for Blank Input, Invalid Input, Valid Input. (Names based on Data Set. Examples: “Sugar”, “Chicken, “Apple”, “Onion”)
  - Empty Input: setIngredientName(“ empty “)
  - Invalid Input: setIngredientName(“ Car ”)
  - Valid Input: setIngredientName(“Honey”)

### Ingredient.setIngredientPrice()

#### Test for Blank Input, Invalid Input, Valid Input. (Prices set are compared to prices found in store databases)
  - Empty Input: setIngredientPrice(“ empty “)
  - Invalid Input: setIngredientPrice(“ -$100 ”)
  - Valid Input: setIngredientPrice(“$5.99”)

### Ingredient.setIngredientCount()

#### Test for Blank Input, Invalid Input, Valid Input.
  - Empty Input: setIngredientCount(“ empty “)
  - Invalid Input: setIngredientCount(“ a ”), Invalid Input: setIngredientName(“ -2 ”)
  - Valid Input: setIngredientName(“4”)

## Functional Testing (Adding a Recipe)

### Creating Recipe Object based on Ingredient Objects and Direction
As a user, I want to be able to add recipes to my account. The software will be paying

Every Ingredient Entered should be of valid parameters
Recipe price should be equal to the sum of all ingredients
Recipe price should be placed onto the Budget Object for User
RecipeID should be Unique












Structural Testing (Login FlowChart)



