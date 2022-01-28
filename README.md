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