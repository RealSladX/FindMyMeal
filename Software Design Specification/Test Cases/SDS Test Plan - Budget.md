**Unit Testing (Budget Class)**

Testing the Operations for the Budget Class

Budget.increaseBudget()

Test for Blank Input, Invalid Input, Valid Input. (Any positive double between 0 and 9,999,999)


* Empty Input: increaseBudget(“ “)
* Invalid Input: increaseBudget(“abcd123”)
* Valid Input: increaseBudget(“100.99”)

Budget.decreaseBudget()

Test for Blank Input, Invalid Input, Valid Input. (Any negative double between the Budget and 0)

* Empty Input: decreaseBudget(“ “)
* Invalid Input: decreaseBudgetN(“-catmonkey”)
* Valid Input: increaseBudget(“-99.99”)

**Functional Testing (Adding a Planner)**

Adding a Planner that has a set budget and directly compares it to the Recipe price

As a user, I want to be able to create a planner that has a set budget that restricts me from going over as I shop and create recipes. The software will accumulate the price of every Ingredient entered and the Recipe price should be equal to the sum of all ingredients Recipe price will be compared to the Budget Object and check if the Recipe is below Budget.

**System Testing (Budget)**

<p id=“gdcalert1” ><span style=“color: red; font-weight: bold”>>>>>>  gd2md-html alert: inline image link here (to images/image1.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href=“#”>Back to top</a>)(<a href=“#gdcalert2”>Next alert</a>)<br><span style=“color: red; font-weight: bold”>>>>>> </span></p>