# Scraping_Analyst
### Tasks
* Scrape Thanksgiving/Fall Items from websites (Walmart, ~~Target~~, Kroger, Publix, ~~Hobby Lobby~~, ~~Amazon~~)
  * ~~Name~~
  * ~~Cost~~
  * ~~Rating~~
  * ~~Number of reviews~~
  * ~~Category~~
  * ~~Sub Category~~
  * ~~Brand?~~
* ~~Clean data (data types, combine or merge maybe)~~
  * ~~Optional Change names, more accurate categories~~
* Create graphs comparing the data found
  * See which companies have better-reviewed decor
  * Better cost
  * Variety
  * Perhaps a boxplot of the cost of each category either all together or per company
* ~~Perhaps load it into a database~~

## Introduction    
This project aimed to go through the ETL process steps and create visualizations based on the data extracted from each company on holiday decorations. This is done to compare companies' differences and see how the store fares. 
**Tools**
* Selenium
* Beautiful Soup
* MySQL
* Matplotlib
* Seaborn
* Pandas
* NumPy

## ETL
Most of the project resides in this process. The way the holiday decorations' data was collected was through web scraping with Selenium and Beautiful Soup. Selenium was used to navigate the website and move to the next page until all the items were collected. Beautiful Soup was used to extract the item information like name, cost, rating, etc.    
#### Walmart
To begin this whole process, I started with Walmart. However, I learned after many attempts that Walmart has very good antibot detection. Their bot detection can sometimes be instant and it is far too much work to bypass this along with the human CAPTCHA test. From the research I made, I learned that it is better to move on from Walmart as the ways to bypass it no longer work.     
#### Amazon
With scraping Amazon, this mainly went smoothly. I say mainly as towards the end, I would temporarily be paused from scraping the website. This fortunately was after I had completed what I intended at the time. I found Amazon to be the simplest when scraping for the item description, but most annoying to navigate the website. The reason is that the HTML for browsing the items on Amazon was buried under multiple attributes with similar values or hidden deep inside a container. After careful searching, I was able to find the links that I needed to go to each item. Scraping the item information was rather straightforward as the data I required didn't share attribute values. Once all of that was collected, I was able to make it into a CSV file and move on to the next website.     
#### Target
Target scraping was rather smooth, the only complication that was run into was that not all of the items on the page would load unless you scrolled down. This also included the next page button. To solve this, I had to create a script to slowly scroll through the webpage since scrolling down instantly would cause the webpage not to load completely. With that solved it was on to the next part which was scraping the item data. The only issue I ran into here was that the brand for the item sometimes had extra text that was irrelevant. I had to make sure that the script removed all those parts while keeping the brand name intact. Besides that, everything went well.
#### Hobby Lobby
