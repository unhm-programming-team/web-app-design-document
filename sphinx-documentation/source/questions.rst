Design Decision Questions
=========================

We have to make some decisions about some of the app requirements.


Which subreddits?
-----------------

We could maintain a table with a list of subreddits. Subreddit names could be the primary key. Columns include content categorizations which become link tags in the  final website feed.

We could append to this table over time, but should find 20 or so subreddits to start with.

E.g.:


.. list-table:: Proposed table structure for subreddit targets
   :widths: 25 25 50
   :header-rows: 1

   * - subreddit (primary key)
     - language
     - content type
   * - r/programming
     - all
     - general
   * - r/python
     - python
     - language
   * - r/gamedev
     - all
     - game development
   * - r/raspberry_pi
     - python
     - hardware
   * - r/webdev
     - all
     - web

How do we define what is "popular" content?
-------------------------------------------

This could be as simple as finding the top posts of the day in each subreddit.

A better solution could involve taking the top X posts in each subreddit for the day and finding the standard deviation, defining popular content as the posts with the highest positive standard deviation. So content with 200 upvotes in a subreddit where top posts typically get 10 upvotes would rank higher than one with 1,000 upvotes in a subreddit where top posts typically get 800 upvotes.

If we include the posts which are not eligible for our feed/database (like text posts) for calculating the standard deviation for each subreddit, we will wind up with a different selection of content than if we just pulled top eligible posts for the day from each subreddit.

How often will we scrape?
-------------------------

Probably depend on how the API access works. Probably should refresh the website content at least daily, so I would think minimum once daily API call.

What scraped data will we store?
--------------------------------

At a minimum, we need URL and standard deviation (if we go that route). Whether we need to store actual upvotes for tracking of stdev over time I suppose depends on how much data we can get per API call, and whether we can generate the standard deviation at the time we make the api call, and whether we need that information, before we put it in the database.

I suggest we store the underlying article information, NOT the reddit information, unless we have to. The upvote data would calculated into a standard deviation score. The subreddit source would be displayed as the corresponding language/content type from the subreddit table.

I think it would be good if there is no apparent connection to reddit - the web app is not regurgitating reddit, it's using reddit to identify quality articles of interest and then display THOSE articles, not reddit posts.

Is this a public web app or do we need some type of user registration?
----------------------------------------------------------------------

I suggest it be public with no registration off the bat. Maybe that's a feature we add in version 2 along with saved customizations of feed, comments, etc.

How are we going to organize our scraped data into a database?
--------------------------------------------------------------

I think we need to figure out what we will be getting from the API before we can definitively answer this question, but maybe something like this:


.. list-table:: Proposed .CSV structure for subreddit targets
   :widths: 10 10 25 25 25 25 25 25
   :header-rows: 1

   * - postId (primary key)
     - subreddit (Foreign Key)
     - title
     - author
     - linkType
     - url
     - date
     - stdev
   * - 0001
     - r/webdev
     - A 3D Hover Effect Using CSS Transforms
     - Ryan Finni
     - article
     - https://letsbuildui.dev/articles/a-3d-hover-effect-using-css-transforms
     - 6/20/2021
     - -1.5
   * - 0002
     - r/python
     - Top 3 Most Important Fourier Transforms in Python
     - Mr. P Solver
     - video
     - https://www.youtube.com/watch?v=GKsCWivmlHg
     - 7/19/2021
     - 1.5

