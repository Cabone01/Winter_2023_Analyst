# Christmas Decoration Dive
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
Most of the project resides in this process. The holiday decorations' data was collected through web scraping with Selenium and Beautiful Soup. Selenium was used to navigate the website and move to the next page until all the items were collected. Beautiful Soup was used to extract the item information like name, cost, rating, etc.    
### Extract
#### Walmart
To begin this whole process, I started with Walmart. However, I learned after many attempts that Walmart has very good antibot detection. Their bot detection can sometimes be instant and it is far too much work to bypass this along with the human CAPTCHA test. From the research I made, I learned that it is better to move on from Walmart as the ways to bypass it no longer work.     
#### Amazon
With scraping Amazon, this mainly went smoothly. I say mainly as towards the end, I would temporarily be paused from scraping the website. This fortunately was after I had completed what I intended at the time. I found Amazon to be the simplest when scraping for the item description, but most annoying to navigate the website. The reason is that the HTML for browsing the items on Amazon was buried under multiple attributes with similar values or hidden deep inside a container. After careful searching, I was able to find the links that I needed to go to each item. Scraping the item information was rather straightforward as the data I required didn't share attribute values. Once all of that was collected, I was able to make it into a CSV file and move on to the next website.     
![amazon_scrape_example](https://github.com/Cabone01/Winter_2023_Analyst/assets/89541481/ca8d6785-061d-4e9e-a896-35728ff91f80)    
In the above image, you can see the location of the data that was extracted from each of the products from Amazon. Initially, I did not have the actual price or the discount data extracted. If I were, I would simply extract the values shown next to and below the sale price value.    
#### Target
Target scraping was rather smooth, the only complication that was run into was that not all of the items on the page would load unless you scrolled down. This also included the next page button. To solve this, I had to create a script to slowly scroll through the webpage since scrolling down instantly would cause the webpage not to load completely. With that solved it was on to the next part which was scraping the item data. The only issue I ran into here was that the brand for the item sometimes had extra text that was irrelevant. I had to make sure that the script removed all those parts while keeping the brand name intact. Besides that, everything went well.    
![target_scrape_example](https://github.com/Cabone01/Winter_2023_Analyst/assets/89541481/4822f338-f7b5-4d69-96b2-9da7eac66da7)    
This image like the previous one shows what I extracted. You can see here that there is no rating value shown leaving just the stars. However, in the HTML code, the average rating of the product is used to determine how the stars are filled. So I simply used the value from that as the rating.    
#### Hobby Lobby
Scraping Hobby Lobby went as smoothly as Target. There was only one issue I had, which was that you were unable to see what the last page number was. This meant that I had to change my for loop into a while loop. The other scripts were all used for loops because I was able to know the final page's number. So for the while loop, it would only continue if the next page button was not disabled. Once that was done, everything else with scraping the website's products was straightforward.    
![hobby_lobby_scrape_example](https://github.com/Cabone01/Winter_2023_Analyst/assets/89541481/a0461cbe-3a4d-4e8e-b7b7-e82831830b28)    
Similarly to Target, I also used the value that determined the fill of the stars as the rating value. However, you would see a new issue occurred here where I did not have a discount value. Originally I was just gonna divide the two prices to get my percentage but I found that in the HTML code, the discount value was hidden with the sale price. So with that, I used that location when I needed to extract the discount data.    
#### At Home
At Home required without a doubt the most work out of these websites. Many issues that I had with the other websites except Hobby Lobby occurred here along with more. The first issue was that I had to use the scroll script to reveal all the items and the button. This was easy since I already had one working for Target. However, the At Home could not load the page nearly as fast as Target which led me to have to slow it down much more. The amount of time it took a page to load was inconsistent, which resulted me in having to put a longer timer for it to load. This led to At Home taking the most time for scraping since it also had the most items and pages out of all of them. It would take roughly 7 hours to get just the product's links while the rest took no more than 1 or 2 hours to do everything. This resulted in me having to split the work into 2 notebooks as a lot of time would be lost redoing that script. The next problem that occurred was bot detection. While scraping the product details, I would be halted from using the website entirely. I did notice though that closing and reopening the website a bunch of times did not appear to trigger the bot. So I had the product scraping script, opened a new webpage for each of the products, and closed it once it was done. This worked perfectly, which allowed me to finish scraping the website's products without issue.    
![at_home_scrape_example](https://github.com/Cabone01/Winter_2023_Analyst/assets/89541481/7abfbe69-a94e-4f68-a4f8-d789e85157df)    
Almost everything I needed was here except for the discount price. For this one, I was able to find the discount value separate from the elements the two prices were in. I am unsure why it was completely separate but it was correct so I was able to use it.    
### Transforming/Cleaning
If you look at the RAW datasets, you can see that the values were different between the other columns. For example, for the brand column I had one dataset have N/A values if there was no brand and another have an unknown as the value. This was intentional as I wanted to practice transforming the data into something more usable. You could argue that it would be better to be clean when the website was scraped. To that, I fully agree, but like I said it's for my practice. When cleaning the columns, the first thing I did was check for null values and the type of the columns. I would replace the null values with whatever was appropriate for the column. I then checked for the number of unique values in each column to check for duplicates using the name column since it should be all unique. Before I could change the price, discount, rating, and number of reviews columns into float or integers. I first had to remove the dollar sign, percent sign, and commas, and change any string value into a 0 or its correct value. Once that was done I changed the column's data type. After that, the data was cleaned and I loaded the clean dataframes into a new folder.    
### Loading
To complete the ETL cycle, I have to load the dataframes into a SQL database. The database I chose was MySQL since I knew that I could connect that to AWS later on. There is not much that has to be said here besides that I had to make the varchar values rather high. The reason was that some names or brands were very long. For the connection to the database, I have it as holiday decorations currently since I intend to add to this for several holidays. Then all the dataframes I had placed in a database I made called 2023_winter since they fall underneath that season except for Amazon. They were all able to load into the database without issue. This led to the completion of the ETL process.    
![Schema](https://github.com/Cabone01/Winter_2023_Analyst/assets/89541481/e6f7f0e9-a042-4f56-b9c4-023fe20d7901)    
Above is the schema I used for each of the datasets except Amazon which is missing the actual price and discount. This is mainly because as I went on to each website, I wanted to gather more data that could be useful.    
## Tableau
Here I will link the Tableau stories I made using the data I collected from the websites. Since I intended to work with Christmas Products, I did not create one for Amazon.    
    
**Links**
* [Target](https://public.tableau.com/app/profile/taco5815/viz/ChristmasDecorationAnalysisofTarget/ChristmasDecorationAnalysisofTarget)
* [Hobby Lobby](https://public.tableau.com/views/ChristmasDecorationAnalysisofHobbyLobby/ChristmasDecorationAnalysisofHobbyLobby)
* [At Home](https://public.tableau.com/app/profile/taco5815/viz/ChristmasDecorationAnalysisofAtHome/ChristmasDecorationAnalysisofAtHome)
