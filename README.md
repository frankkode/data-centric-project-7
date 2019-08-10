# [Data Driven Recipe Application](https://paraic-cooney-recipe-app.herokuapp.com/)
This is an interactive web application with both front-end & back-end functionality.  It constitutes the third of my four projects which form the assessment 
basis of my Full-Stack Web Development course with The Code Institute.
The purpose of this project is to allow users to interact with a back-end database of recipes using an appealing & intuitive front-end.  The application
allows users to upload & save their own recipes while also searching & browsing recipes submitted by others & bookmarking them for later recall.
The application also serves the purpose of allowing the administrator to attract revenue, either through promotion of their own recipes, through the inclusion
of product placement, or through a pay for promotion service where individuals pay a fee to have their recipes promoted.

## Notice to the administrator
Currently this application is designed for five promoted recipes.  To display a recipe in the promoted carousel on the Browse page give the recipe the field
of "promoted" in the Mongo database & a value of "on" ("promoted":"on").  Should this be applied to more than five recipes it will not break the code
& all recipes will display in the carousel.
To enable a promoted recipe to be displayed on the Search Results page the recipe must be given a field of "promoted_key" & unique value between
one & five ("promoted_key":"3").  If you wish to add more promoted recipes change the range of the random number generator (line 84 of the app.py 
file) & assign corresponding unique values to the promoted recipes (eg "promoted_key":"8" , "promoted_key":"12", etc).

## UX
The design goal of this project was to make the front-end as appealing & intuitively interactive as possible while also providing a warm colourful 
display which serves to stimulate the users senses.  An orange theme was chosen along with a visually rich background image of fresh ingredients along
with a linen background image to add a rustic feel.

For the application administrator I also wanted to be able to give them the ability to integrate promoted recipes (which serve as the revenue stream for the application)
without taking away from the user-generated feel & rendering the application overly-commercial.  To do so I incorporated a promoted recipe carousel in the 
browse section & also included a random promoted recipe underneath any search results.

The application was also design using a mobile-first approach to ensure responsive design & assist in search engine optimisation.

## Technologies Used
1. HTML
2. CSS
3. Bootstrap
4. Materialize
5. Javascript
6. jQuery
7. Python
8. Flask
9. Jinja2
9. MongoDB

## Features
**Feature 1 - Recipe Page**
The My Recipes page (which can be accessed from the button in the nav of the same name or by clicking the application logo in the center of the nav) utilises
the jinja templating language to loop through & render each recipe written by the user.  It does so by passing in all recipes as an argument when
rendering the url & then, through use of a jinja if statement rendering only those recipes who's author matches that of the username entered at the 
start of the session.
The page then uses the same method to render all recipes which the user has bookmarker by checking if the bookmarkers field contains the substring
matching the username.

**Feature 2 - Browse Functionality**
The Browse functionality can be accessed via the Browse tab.  It displays a number of carousels consisting of thumbnail images & recipe names. Upon
click the individual recipe is rendered on a separate tab.  The purpose of this section is to allow the user to browse many recipes visually & by
category in order to quickly process & narrow many recipes (as opposed to a list detailing all recipes or recipes details).
At the top of the Browse page is the promoted carousel which is to be utilised by the application administrator as outlined above in the UX section.

**Feature 3 - Upload Recipe Functionality**
As this application is to be comprised primarily of user generated content it was imperative that the user be given the ability to upload their own recipes.
To achieve this a form has been created on the Upload Recipe page.  The form was designed to incorporate not only textboxes but also checkboxes &
a slider to give some variety to the user.  The author name field is populated automatically.

**Feature 4 - Search Functionality**
The search feature allows the user to search based on a number of parameters (recipe name, ingredients, & author).  This feature will become more
relevant with a greater number of user generated content.  The functionality was designed to be as minimalistic as possible.

**Feature - Bookmark add/remove buttons**
The add/remove bookmark feature was implemented to give users the ability to save recipes which have been generated by other users.

**Feature - Delete buttons**
The delete feature allows users to delete recipes they themselves authored.

### Features Left to Implement
**Additional search capabilities**
I future I would like to impliment the ability to search by additional parameters such as difficulty & category.  I would also like to include
the ability to search by multiple criteria & a search function on the My Recipes page to allow the user to search through their own recipes (bookmarked
& authored).
Additionally I would like to remove the case sensitivity of the search by manipulating the search input.

**Update authored recipes**
I would like to grant the user the ability to update their own authored recipes.

**Photo upload**
I would like to give the user the ability to upload a photo file locally as opposed to a photo-url.

**User feedback**
I would like to impliment either a comment section or ratings system to allow other users to provide feedback on recipes.  Once this is implemented
I would then include it as a search parameter.

## Data Structure
The back end of this application interacts with two MongoDB collections - categories & recipes.

### Categories
![Categories screenshot](https://i.imgur.com/bvAXjTR.jpg)

The categories collection is comprised of sixteen entries.  At the front-end this collection controls the carousels on the Browse page & the
checkboxes available for selection on the Upload page.

### Recipes
![Recipes screenshot](https://i.imgur.com/MXHmf9k.jpg)

The recipes collection is comprised of many fields which are not present for each recipe instance.  The majority of the fields shown in the above
will be present in every entry due to the fact that they are require or automatically populated during the upload phase.  Certain instances of
recipes may have additional entries for the sixteen previously mentioned categories, & may or may not contain the bookermarks & promoted/promoted_key
fields.

## Testing

All links have been manually tested to ensure that they are pointing to the correct destination.

All buttons (add/remove bookmark, delete authored recipe, upload, & search) have all been tested manually.  All have been testing by cross-referencing
the back end database & manually confirming that;
- All search results that should be present are present
- The username has been added to or removed from the bookmarkers field without removing the other entries in that field.
- That the recipe selected for deletion has been removed from the database.
- That the recipe uploaded has been added to the database with all selected fields & information included.

This application was tested across multiple browsers (Chrome, Safari, Internet Explorer, FireFox) and on multiple mobile devices to ensure compatibility 
and responsiveness.  This was done using Chrome's devloper tools & also an online resource (https://responsivedesignchecker.com/).
The collapsable navbar was also test at this stage.

All pages were tested to ensure that they were rendering correctly.  The front-end was cross-referenced with the back-end to ensure that all
correct data was being passed through to the front-end & rendered correctly, for example the My Recipes page was tested to ensure that all
recipes that should be present (both authored & bookmarked) are present.  
It was during this phase that I noticed a bug whereby the first carousel was being rendered & while the other's were not.  This bug was also present
on the My Recipes page whereby the 'Bookmarked' section would not render correctly (in both cases empty containers would render but they would
not be populated with data).
The cause of this bug was that I could not apply multiple separate Jinja for loops to the same dataset (which was passed through the Python route).
In order to apply multiple Jinja for loops I had to pass the same data into the render_template twice as Jinja only allows an argument to be used once;
return render_template("myrecipes.html", recipes=mongo.db.recipes.find(), username=username, recipes2=mongo.db.recipes.find())
This is also evident in the /browse_recipes route.

A manual test was run to ensure that the correct pages were rendering based on the presence of recipes written by the user or bookmarks for the
user.  There are four paths that may be taken (note all usernames used in testing were appropriate at the time of testing);
1. Both authored recipes & bookmarks present for user.  This was tested by logging in using username 'paraiccooney' & successfully rendered 'myrecipes.html'.
2. Authored recipes present but no bookmarks for user.  This was tested by logging in using username 'Bord Bia' & successfully rendered 'no_bookmarks.html'.
3. Bookmarks present but no authored recipes for user.  This was tested by logging in using username 'Paul Simon' & successfully rendered 'no_authored.html'.
4. No bookmarks or authored recipes by user.  This was tested by logging in using username 'Michael Harris' & successfully rendered 'no_recipes.html'.

A similar test was run as that described above was run to ensure the correct page was being rendered based on the presence of search results.

Finally all Javascript code was run through JS Hint (https://jshint.com/) to flag any potential bugs. None were returned.  The app.py file was checked
for bugs using https://pythonbuddy.com/

## Deployment
This application is hosted & deployed using Herouku & is located at ( https://paraic-cooney-recipe-app.herokuapp.com/ ). 
The application will update automatically with new commits as the master origin has been updated to Heroku.

In order to run this application locally you can clone this repository directly into your personal editor using the following command;
git clone (https://git.heroku.com/paraic-cooney-recipe-app.git)
In order for this application to be deployed correctly the landing page must be named index.html.  All html pages which are to be rendered also must be kept
in a folder named 'templates' which is to be located in a folder labeled 'static'.  These requirements arise from the use of the Jinja templating
language.

To cut ties with this repository use the following command;
git remote rm origin

## Credits
### Content
The intial content of this application was taken from multiple online sources;
https://www.bbcgoodfood.com

https://www.bordbia.ie

https://www.goodfoodireland.ie

https://hurrythefoodup.com

### Media
All media was taken from the corresponing recipes on the above sites or through a Google image search.

## Acknowledgements
The Code Institute community on Slack, my project mentor Aaron Sinnott, & the online tutor support were of great assistance in building this project. 

Stack Overflow was of great use & many contributors had answered questions similar to those I held at separate points throughout the project.

Startup modal credits - https://www.tutorialrepublic.com/faq/how-to-launch-bootstrap-modal-on-page-load.php