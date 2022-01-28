# Gilded Rose Requirements Specification

Hi and welcome to team Gilded Rose. As you know, we are a small inn with a prime location in a
prominent city ran by a friendly innkeeper named Allison. We also buy and sell only the finest goods.
Unfortunately, our goods are constantly degrading in quality as they approach their sell by date. We
have a system in place that updates our inventory for us. It was developed by a no-nonsense type named
Leeroy, who has moved on to new adventures. Your task is to add the new feature to our system so that
we can begin selling a new category of items. First an introduction to our system:

	- All items have a SellIn value which denotes the number of days we have to sell the item
	- All items have a Quality value which denotes how valuable the item is
	- At the end of each day our system lowers both values for every item

Pretty simple, right? Well this is where it gets interesting:

	- Once the sell by date has passed, Quality degrades twice as fast
	- The Quality of an item is never negative
	- "Aged Brie" actually increases in Quality the older it gets
	- The Quality of an item is never more than 50
	- "Sulfuras", being a legendary item, never has to be sold or decreases in Quality
	- "Backstage passes", like aged brie, increases in Quality as its SellIn value approaches;
	Quality increases by 2 when there are 10 days or less and by 3 when there are 5 days or less but
	Quality drops to 0 after the concert

We have recently signed a supplier of conjured items. This requires an update to our system:

	"Conjured" items degrade in Quality twice as fast as normal items

Feel free to make any changes to the UpdateQuality method and add any new code as long as everything
still works correctly. However, do not alter the Item class or Items property as those belong to the
goblin in the corner who will insta-rage and one-shot you as he doesn't believe in shared code
ownership (you can make the UpdateQuality method and Items property static if you like, we'll cover
for you).

Just for clarification, an item can never have its Quality increase above 50, however "Sulfuras" is a
legendary item and as such its Quality is 80 and it never alters.

# Starting Code
- We inherit the GuildedRose class with the updateQuality method, which is a bit difficult to work with as it is
- You've been asked to add a feature for a new kind of item 
  - The way this handles the different kinds of items is quite complex 
  - Before we can change this code and add the new feature we need to refactor it 
  - Before we can refactor it we need some tests
- It comes with a starting test which fails, so the first thing to do is to get the test to pass

# Steps Taken
- The quickest way to get the test to pass is to match the name that we've constructed it with
- When this test is green it shows that we have setup the IDE correctly 
- Rename Test Method
- The supplied test is not a very strong test, which we want to improve considerably before relying on it for refactoring 
- Convert to use approval test framework  (https://github.com/approvals/ApprovalTests.Java)
  - Running the test for the first time will fail because we don't have an approved result to compare against
  - mv received.txt to approved.txt to approve changes
- Rewrite Test to use combination approvals 

# Coverage
- Coverage is not the only mesure of how well you're testing the code
- Another more stringent method is to subject your testsuite to something called _mutation testing_, for which there are tools 
- To do this by hand, you start with a test that will pass and then you make a small change in the code, i.e. a _mutation_ to the code
- then you run the tests again and see whether they actually catch the error that you've introduced 
- You can do a few manual mutations to check the quality of your tests
- If you manage to change the code and the test still passes, you have a problem 

# Refactoring
- the updateQuality method is the target of the refactoring
- it's quite tangled conditional logic that's looking at the name of the item and deciding how to update stuff 
- the task we have is to add a new class of item: _ConjuredItems_
- it would be really useful if all the logic to do with a particular type of item were grouped together in this methods, so that it would be obvious what to do when you're adding a new kind of item 
- That's often the way with refactorings: _you're trying to make the code look so that the new feature is really easy to add_

## Refactoring Steps
1. Replace `items[i]` with a local variable 
2. pull out the body of the for loop into its own method 
3. inline the `item` variable again in `updateQuality()`
4. Now `doUpdateQuality()` is the target of the refactoring 
5. lift-up-conditional refactoring
  - first identify the conditional to lift up, in this case all the code related to "Aged Brie"
  - when we duplicate all the code into both branches of the if/else we should have some dead code to delete, which the coverage tool will help us find
 
## Replace Conditional with Polymorphism
- Makes code easier to handle and easier to add new classes of Item without having to go into the `doUpdateQuality()` method
- This method currently doesn't obey the open-closed principle
- We'd like to be able to add Conjured Items without modifying existing code 
- We want to create some subclasses on Item and move the code to do with a particular type of item into the relevant subclass 
  - this will make it easier to add a new class of Item 
  - you just need a new subclass of Item without changing the existing code everytime you need a new sort 
- Given we have a requirement for a new class of Item, it's kind of reasonable to think this might be a frequent occurence 
- We just need to change the Item class minimally with a factory method and some subclasses
### Steps
1. Create relevant subclasses 
2. Push down parts of the switch statement logic into each subclass 