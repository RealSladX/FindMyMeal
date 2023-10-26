
# System Testing

## User Features

### 1. Craftable Recipes

As a user, I want to know what recipes I can make with the ingredients I have on hand so I can better decide what meal I want to prepare.

**Given**: a predefined recipe book and populated pantry containing a set of ingredients
**When**: the user navigates to the craftable recipes feature
**Then**: they should see a list of recipes able to be made with the available ingredients

### 2. Shopping Lists

As a user, I want to know what ingredients (if any) I'm missing in order to create a given recipe or collection of recipes so I know what groceries I need to buy.

**Given**: the user is viewing the details of a recipe or a list of recipes in the recipe book
**When**: the user selects one or more recipes from a list and presses a button to plan these recipes, or presses a corresponding button on an individual recipe page
**Then**: the recipe(s) are added to the meal plan

**Given**: the user is viewing the meal plan
**When**: the user presses a button to generate a shopping list
**Then**: the user will see a list of ingredients and quantities needed to create the planned recipes, less the amounts already on hand

### 3. Inventory Tracking

As a user, I want the software to keep track of when I use ingredients that are part of known recipes so I don't have to update the on-hand quantities manually every time I create a meal.

**Given**: the user is viewing the details of a recipe
**When**: the user presses a button to create this recipe
**Then**: the quantity of each ingredient in the pantry are reduced by the amount defined in the recipe, or a multiple (of servings) thereof AND removed from the panty completely if the new quantity is zero

**Given**: the user is viewing the shopping list
**When**: the user presses a button to "do shopping"
**Then**: the user is able to change the quantities of each ingredient in the list and unselect certain ingredients (to account for the fact that ingredients might not be bought - or sold - in the exact amount needed)
**When**: the user presses a button to "add shopping to pantry"
**Then**: the quantity of each ingredient in the pantry is increased by the corresponding amount in the shopping list

# Integration Testing

## Non-volatile Storage

**Given**: a prepopulated pantry or recipe book object and an injectable storage provider
**When**: the object is saved and a new object of the same class is instantiated using the same storage provider
**Then**: the two objects should be deeply equal

# Unit Testing

## Recipe.isCraftable(Pantry p)

**Given**: a pantry object containing a list of available ingredients
**When**: the pantry is empty
**Then**: the recipe is not craftable (return false)
**When**: the pantry contains all required ingredients in one less than the required quantity
**Then**: the recipe is not craftable
**When**: the recipe contains all but one of the required ingredients in sufficient quantity, and one ingredient is absent
**Then**: the recipe is not craftable
**When**: the pantry contains all required ingredients in the exact quantities required
**Then**: the recipe is craftable
**When**: the pantry contains all required ingredients in the maximum storable value of their datatype
**Then**: the recipe is craftable

## RecipeBook.getCraftableRecipes(Pantry p)

**Given**: a pantry object containing a list of available ingredients
**When**: the pantry is empty
**Then**: method returns an empty set
**Given**: the pantry contains sufficient ingredients to craft some but not all recipes in RecipeBook
**Then**: method returns a list of only the craftable recipes
**Given**: the pantry contains sufficient ingredients to craft any recipe in RecipeBook
**Then**: method returns a list contains all recipes in RecipeBook