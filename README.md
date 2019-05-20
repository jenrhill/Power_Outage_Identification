# Power Outage Identification

### Problem Statement 

Utilities’ Outage Management Systems (OMS) has been rolling out new smart grid technologies to supplement traditional methods for detecting and reporting on power outages. However, the new technologies won’t be completely rolled out until 2030.[1](http://people.stern.nyu.edu/kbauman/research/papers/2015_KBauman_WITS.pdf) We will build a tool to help supplement these efforts while the new technologies are being rolled out. Our tool will use Twitter combined with weather data to help identify and report on likely locations of legitimate power outages using Natural Language Processing. Our metric to evaluate will be cosine similarity.

**Team Members**

- [Noah Christiansen](https://www.linkedin.com/in/noah-christiansen/)
- [Jen Hill](https://www.linkedin.com/in/jenhill/)
- [Vonn Napoleon Johnson](https://www.linkedin.com/in/johnsovo/)

This project was completed in cooperation with [General Assembly](https://generalassemb.ly) in April 2019.

---

### Project Files

Here is the workflow order to follow when running through the notebooks, which can be found in the [code folder](./code):

- [1_Data_Collection_and_EDA.ipynb](./code/1_Data_Collection_and_EDA.ipynb)
- [2_Preprocessing_and_Word2Vec_Model.ipynb](./code/2_Preprocessing_and_Word2Vec_Model.ipynb)
- [3_Outage_Map_and_Next_Steps.ipynb](./code/3_Outage_Map_and_Next_Steps.ipynb)

---

### Executive Summary

Our approach was to first decide what social media platforms to pull from. We found Twitter to be the most viable platform in terms of data we would have access to, and with how quickly news gets shared on the platform. We used TwitterScraper for targeted keywords based on location to scrape 5 years of data across 12 cities. We chose our keywords based on how energy companies and consumers speak about power online, making sure to watch for potential misclassifications (ex: Blackout is a video game and Power is a TV show). The cities we chose were based on population, with a few added in because they have high yearly outage rates.

From initial tweet review, we hypothesized that weather was the biggest influencer of a power outage. We used the NOAA API wrapper to pull historical weather data by date for our select cities. Not all weather stations pull in the same data, so we focused on what was consistently collected across all states, high temp, low temp, and precipitation. We converted these numbers into words based on ranges of temperature and precipitation in order to be able to append the words to our tweets. We knew we wanted to use Natural Language Processing and needed words instead of numbers to do that.

EDA helped us not only set up a preprocessing plan for our model, but also and approach with setting word lists to compare our tweets to for misclassification. One example was that “Internet outage” showed up high on the frequency count list, meaning we had scraped some telecommunication outages along with power outages. For preprocessing, we set all the characters to lowercase and removed punctuation and stopwords. For modeling, we chose to use Word2Vec because of the way it focuses on the relationship of words and gives weight to that value. It brings context of word choices into play, which will give us a better understanding of the group of words used in a tweet to talk about a power outage.

While machines have no problem understanding high dimensional space, after using Word2Vec, we needed to convert our data back into two-dimensional space using a t-SNE model in order to examine and understand the data. Then we used cosine similarity to compare our words to targeted keyword lists to determine if the words within each tweet were related to a legitimate power outage or not. Cosine similarity score results were manually reviewed to find misclassifications and tune our model. 

---

### Conclusion and Next Steps

We were able to create a prototype that can classify a tweet as being a legitimate power outage. However, there are some limitations to it that would need to be addressed before rolling it out to classify more data. Because we had to use a Twitter scraper instead of Twitter’s API, our location data is not as exact as it could be. And even if we could use the API, not all users list location data on their Twitter profile out of privacy concerns. Plus, the evaluation process for accuracy is a manual one that rolling out countrywide would demand resources and time for review.

However, we do see great possibilities in a more widespread rollout, based on live data. Had we more time, the next steps we would have taken and recommend considering are the following:

1. Test the model on live data: tweets from Twitter’s API and weather data from Dark Sky’s API. 

2. Use K-means clustering to group power outage findings to better be able to confirm an entire area is without power. Sectioning out clusters by region and by weather would be interesting to look at as well. We only looked at the weather options that were consistently available for all our select cities, but looking into more detail on things like wind speed would be beneficial here.

3. Explore other dimensionality/data reduction methods besides t-SNE, such as using principal component analysis before data preprocessing.


---

### Source Documentation

- [Using Social Sensors for Detecting Power Outages in the Electrical Utility Industry](http://people.stern.nyu.edu/kbauman/research/papers/2015_KBauman_WITS.pdf) 
- [Blackout Tracker Annual Reports](https://switchon.eaton.com/plug/blackout-tracker) - We reviewed reports from 2014-2018
- [The Top 10 Largest U.S. Cities by Population](https://www.flickr.com/places/info/1)
- [Wikipedia: List for 100 K to 1000 K Temperatures](https://en.wikipedia.org/wiki/Orders_of_magnitude_(temperature)#Detailed_list_for_100_K_to_1000_K) 
- [Wikipedia: Rain Intensity](https://en.wikipedia.org/wiki/Rain#Intensity)