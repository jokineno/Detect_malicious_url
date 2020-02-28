### Assignment
- In this assignment you as a HoxHunt Data Science Hunter are given the task to extract interesting features from a possible malicious indicator of compromise, more specifically in this case from a given potentially malicious URL.

#### What we expect
Investigate potential features you could extract from the given URL and implement extractors for the ones that interest you the most. Below example code extracts one feature but does not store it very efficiently (just console logs it). Implement sensible data structure using some known data structure library to store the features per URL. Also consider how would you approach error handling if one feature extractor fails?

Be prepared to discuss questions such as: what features could indicate the malicousness of a given URL? What goes in to the thinking of the attacker when they are choosing a site for an attack? What would you develop next?


### What I'm going to do:
- Build feature extractors and a data set of a few features
- There are several feature categories that can be used to detect a phishing web page with a high accuracy [1] 
- The single best category is URL based category in which predictor features are parts or elements of URL. However, if URL based features do not give a high predicing accuracy, then the features from other categories in interaction with url features gives the best performance. 
- In this task I'm going to use only features than be extracted with URL such as age, since they are the most interesting to me and also they can be extracted easily by a few lines of code and a web browser without any access to third party services. Also that was specifically given in a task instructions.
- Also multi-criteria methods have been proved to be most efficient [1]. 


## FEATURES
- Features below are based on findings in provided papers and my own interest, bias and preconceptions about them. 
- The most of the papers discovered that combining features from different categories gives the best prediction accuracy. 
    - Single best predictor is URL based features -> more features
    - Combined with other features the dataset will be more powerful in classifying.
    - Also all features are language independent


### Categories
1.  URL based features
    - protocol: http or https ok
    - count of dots: integer ok
    - count of level domains: integer
    - length of url: integer ok
    - length of Fully Qualified Domain Name (FQDN): integer ok
    - number of redirections: integer
2. Blacklists
    - is ip on blacklist: Boolean
    - is ip proxy: lookup Boolean
3. WHOIS based features
    - age of the domain: Integer
    - ownership period: start and end
    - country: String -> there are high risk countries and later I can use that information to classify the danger of the IP.
4. Content based features
    - Number of input fields and text fields: Integer
    
5. Whitelist
    - Works as lookup table: Boolean


### Storing data

- Data Structure: DataFrame by Pandas, permanent storing can be done by saving in .csv format
- Handling NULLs, NoneTypes, NaNs: At this point if extractor cannot find any value I will save it to a DataFrame. It's also a valuable information. Later, in model building stage I have to preprocess data more carefully and also map categorical values into numerical.



# What next? 
### Data
- Extract more features: based on the most of the papers the multi-criteria methods: combine features from different categories works the best. 
- Get more data but focus on feature categories instead of amount of data
  

###  Metrics: 
- Compare my results to existing blacklists systems such as Google safebrowsing and PhishTank.
- Compare results with a test set and validate the model

### Modeling
- Build a classifier: based on Twitter paper the Random Forest Classifier achieved good results
- Based on two papers combining feature categories gives the best results -> interaction of features! 
- Map values suitable for ML model




# References

- [1] Marchal, Samuel & Saari, Kalle & Singh, Nidhi & Asokan, N.. (2016). Know Your Phish: Novel Techniques for Detecting Phishing Sites and Their Targets. 323-333. 10.1109/ICDCS.2016.10. Also:https://arxiv.org/pdf/1510.06501.pdf
- [2] A. Aggarwal, A. Rajadesingan, and P. Kumaraguru, “PhishAri: Automatic Realtime Phishing Detection on Twitter.” 2013. Also https://arxiv.org/pdf/1301.6899.pdf


