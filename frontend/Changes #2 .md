# For Awaze Backend Developer

## Brief Explanation of the Platform 






- I wrote all this stuff to give you the basic understanding of the whole platform and what you'll be working on. Chill.

Please Read Everything Carefully.






- Main concept is creating a central platform that restaurants can contain their digital menus, while tracking their analytics that come from it for marketing purposes. Now I'm going to specificallyy tell you what we want the platform to have as the background process. 

- There are 2 packages that the platform will be offering for the restaurants. Currently being the basic plan and premium plan. 


# Dashboards
- We need a *auth.vue* file that the dashboard will lead both of them to if they're not logged in. 
- We need a *dashboard.vue* system that both can access, but depending on their subscribed plan, we want the dashboard to show and not show the features i am going to list you below. **(The current one we worked on as "Dashboard.vue" needs to be changed to "SuperAdmin.vue")**
- Let's work on a new page called *dashboard.vue* that will replace both basic.vue and premium.vue pages. Only needs adjustment of already made pages.


# Basic Plan users can access (Only these!)
- Loging in
- *Sub-Page #1 (Status)*: A Dashboard that will show explicitly how many Foods/Drinks and Categories they have, Average Food/Drink Prices, ...
- *Sub-Page #2 (Items )* A Sub=Page where the restaurant owner can See a list of each of the foods they have, see the details by opening the foods/drinks and are able to edit every information on everything they can see, delete foods, add foods,...
- *Sub-Page #3 (Notification)* A Sub=Page where the new activity in the restaurant is shown. (Consists mainly of comments made on their foods with the information of what food it's commented on, name of commentor and what it commented, what time they commented and what rating they gave.)
- *Sub-Page #4 (Settings)* A Sub=Page where the restaurant owner can Access their whole identity (Name of the restaurant, Location, phone no., description, image) and can edit everything, See and Edit their information. Can also Log out from their account in this page. Also have an Upgrade your plan button.

- Understand that: This plan doesn't support ordering, Room integration, having Branches, Accessing Games. Any button that lead to this options need to be shown *neither* in their dashboards *nor* their menus.

# Premium plan users can access
- Loging in and everything the basic ones have, plus these additional sub-pages and additional information on the previous pages.

Additions on Previous Pages:
- On *Sub-Page #1 (Status)*: Add the amount of tables they have and the amount of rooms they have.
- On *Sub-Page #4 (Settings)*: Add 3 toggle buttons, one for **Ordering Option** (creating multiple Tables with their own qr codes.), and second one for **Room integration**(Creating multiple rooms with their own qr codes.) Third one for **Restaurant branch** (Restaurants that have branches). Then remove the Upgrade your plan button. 
  Details about these 3 is listed below in the features section.

Additional Pages:
- *Sub-Page #5 (Analytics)*: A Dashboard that will show Total number of QR Scans / Page Views, Total number of orders, Daily / Weekly amount of visitors, Average amount of views per day, Average amount of orders per day, most viewed foods / Categories.
  - List of top 10 foods that has been viewed the most(with how many views they got, how many orders they got, Average minutes/seconds they have been viewed) 
  - Peak working times: in what time range the views are mostly high.
- *Sub-Page #6 (Subscription)*: A Sub-page that they can see what plan they are on (Current Plan), Subscription Price, Payment status, next payment, Payment history, Upgrade / Downgrade options.









# Super Admin Dashboard
- We need an ultimate platform dashboard that have access to anything going on in the platform database.
- We need to access the dashboard of any restaurant in the restaurant without needing to know their passwords. (The dashboard will also check if the request is coming from this page before leading it to the *login.vue* page.) This will happen when the restaurant is clicked from the restaurants list in the "Restaurants" Sub-page.
- Needs it's own Login system so that no one other than the owners can access it.

Sub-pages:
- Dashboard
- Restaurants
- Revenue
- Management
- Settings

- **We have already done most of this. I'll put further informations when needed, just to not kill time for now. This just needs a little more adjustment.**









# Menu Page
- We don't want a person who got into the website through a specific restaurant / specific branch of a restaurant to have access to any other restaurant or any other branch. We will be avoiding a "Back" button which will take them to all the list of them. You did the right thing showing them on the '/menu' page tho. we just don't want the back button inside 'menu/specific_restaurant'.

About Order:
- We don't want the order button on basic plan restaurants.
- We don't want the order button on
- We need an orders button on the menu page. where people can see the list that's ordered on their table. 









# Kitchen Admin Page
- Page only accessible if the ordering toggle is 'on' in the premium user's settings, where the workers in the kitchen can see who ordered, what and how much they ordered, when they ordered it, from where (table/room) they ordered it and their phone number. 

- They can also touch the buttons telling the one ordered that the food is being made and when the food is ready to be delivered. These informations will also be shown on the browser who ordered it.

- Need a login system that the kitchen workers of that specific restaurant only get in with, and lists all the orders coming from that specific menu page.









# Game Page
- A Page dedicated to showing a list of games which will be available to be played for individuals and groups. This page will be recommended on premium restaurant's pages as a button and also on a page that a person who ordered a food watches it saying "Play a game until the food is delivered to you."
- The list of the games will be decided real soon and we'll try to clone already made up games from github and integrate them with this platform.










# Features to keep mind on

- *Commenting*: Need a delete button on the dashboard.(In the food/drinks details, showing the list of their comments.)

- *Ordering*: a feature accessible for premium users only if they made the toggle on. It creates multiple tables with their own QR Codes and also makes them access the kitchen page. **Need to take phone number when asking inputs from ordering person for eligibilty and trust purposes.**
- We need the amount of tables to be shown in the analytics page. but the restaurant can't see the qr codes for the tables. Only the SuperAdmin can access any QR Code created through this platform.
- We don't want the whole information created to be deleted instantly if the toggle is turned off, so we want 2 things. 1. Ask twice if they're actually trying to delete the information created. 2. Just hide the information for some time if they want it back.

- *Room Integration*: also a feature accessible for premium users only if they made the toggle on, creates multiple rooms and their qr codes. 
- We also need the amount of rooms to be shown in the analytics page. but the restaurant can't see the qr codes for the rooms.

- *Restaurant Branches*: This is a new idea added! So when there are premium users that have multiple branches and want to integrate it in this platform, they cam turn on the toggle and have a branches feature. What this does is rather than creating the foods as 1 restaurant and containing them in there, It gives you a choice to either create a food list for each branch or do one list of foods and all the branches will take that list as theirs. Rather than just creating a list of foods/drinks, you first create a list of branches and the branches are the one's that contain the foods.
- This will also affect the analytics page of the premium users showing both analytics as a branch and also generally.









# Notes to take and Add

- If payment is not done for 2 months, the platform will suspend the restaurant.
- Menu checks if a restaurant is suspended or not before showing it on the page. If a restaurant is suspended, Menu page will show the Restaurant is suspended for payment issues.

- Only the SuperAdmin can access Any and Every QR Code created through this platform. No access for a QR Code is given to the restaurant owner.













# Misunderstanding on previous work
- Login is *not* needed only if the SuperAdmin is trying to access from the SuperAdmin Dashboard. If the one trying to access didn't come from the superadmin page restaurants section, it always needs to login.











## Next Instructions (Adjustments)

Organizing the code:
- Adjust the *auth.vue* file. I have already created the file in 'pages/admin'. Make the dashboard lead to it if the one trying to access is not logged in. (Cut the code from the basic or premium page (whichever you think is best) and put it there.) It also needs to ask what the plan is. (Inputs needed: *username*, *plan* (choice from basic / premium), *password*). If the information is correct, Lead to the dashboard which shows for their plan.
- Adjust the *dashboard.vue* file (basic/ Premium we will be working on now) accessed only for the restaurant logged in, with their specific information.
- **We don't want the SuperAdmin page edited!**

Actual backend:
- Make the dashboard.vue file show the features depending on the plan the user logged in.
- Connect the login info to the dashboard page we made.
- Make the dashboard actually pull the right information for the selected restaurant from the database.





# Notes to take 

- 

- Please Finish it Asap.
