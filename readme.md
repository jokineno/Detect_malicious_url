### Assignment
- In this assignment you as a HoxHunt Data Science Hunter are given the task to extract interesting features from a possible malicious indicator of compromise, more specifically in this case from a given potentially malicious URL.

#### What we expect
Investigate potential features you could extract from the given URL and implement extractors for the ones that interest you the most. Below example code extracts one feature but does not store it very efficiently (just console logs it). Implement sensible data structure using some known data structure library to store the features per URL. Also consider how would you approach error handling if one feature extractor fails?

Be prepared to discuss questions such as: what features could indicate the malicousness of a given URL? What goes in to the thinking of the attacker when they are choosing a site for an attack? What would you develop next?


### What I'm going to do:
- Build feature extractors which could predict a risk of web page being malicious
- Handle errors in extracting
- Build a small dataset based on features


## FEATURES
- Features below are based on findings in provided papers and my own interest, bias and preconceptions about them. 
- The most of the papers discovered that combining features from different categories gives the best prediction accuracy [1,2]. 
    - Single best predicting category is URL based features [1]
    - Also all features are language independent
    

### Categories
1.  URL based features
    - protocol: http or https
        - There are more and more phishing sites which are using https [3].
    - count of dots in fqdn (Fully Qualified Domain Name): integer 
    - length of url: integer
    - length of Fully Qualified Domain Name (FQDN): integer
        - if Top Level Domain is used then FQDNs are typically longer [1]
    - number of redirections: integer
        - Many redirection can be used to evade the blacklists.
2. Blacklists
    - is ip on blacklist: Boolean
    - is ip proxy: lookup Boolean
3. WHOIS based features
    - age of the domain: Integer
        - The most of the phishing sites are alive a really short period of time
    - ownership period: start and end
    - country: String
        - Try to find out if IP comes from a 'high-risk' country.
4. Content based features
    - Number of input fields and text fields: Integer
    
5. Whitelist
    - Works as lookup table: Boolean


### Storing data

- Data Structure: DataFrame by Pandas, permanent storing can be done by saving in .csv format with df.to_csv() function.
- Handling NULLs, NoneTypes, NaNs: At this point if extractor cannot find any value code will save it to a DataFrame. It's also a valuable information. 
- Later, in model building stage I have to preprocess data more carefully and also map categorical values into numerical.



# Questions:
1. What features could indicate the malicousness of a given URL?
2. What goes in to the thinking of the attacker when they are choosing a site for an attack?
3. What would you develop next?


## 1. Features indicating the maliciousness of a given URL?
- URL based features are the single best predictor [1]
- However, best results are achieved when combining features from different categories [1,2]. 

## 2. Attackers thinking
- Typical example of phishing site is web page which mimics a target page. At its best the phishing site looks and feels the same and a user may not detect that. 
- Attackers' main goal is to get a user to a fake page and make a user to think this is the real target page. 
- Fake sites are usually alive for a short time so typically target sites are the ones with high traffic.
- The power of phishes come from statistical probability -> phishers may send thousands or tens of thousands of emails and some of them will be tricked! 
- Also URLs can be made to look similar to a target: for example paypal.com vs. paypaI.com
- Basically, the weakest link in the security chain is a human. Attacker tries to trick users cognition. 


## 3. What next? 
- In general: read more articles, learn more, understand better -> faster decision making. 
### High quality of data
- Extract more features: based on the most of the papers the multi-criteria methods: combine features from different categories works the best in predicting if a web page is a phishing site or not.
- I liked the idea of "Know your phish" [1] where all features are language independent. I would focus on those features.

### API
- Use another API such as www.whois.net in order to get more and more detailed informations. 
    - In this task input.payapi was a good one since it does not require authentication and code can be run by anyone without authentication. 
- Combine information from different APIs -> higher level of certainty of a quality of data
    - For example: double check blacklists. 

### Preprocess data
- For example:
    - mapping categorical values into numerical such as http/https -> 0/1
    - handle missing values by estimating or removing: this is a data set size dependent. 

### Define metrics: 
- For example: compare accuracy of the model to blacklist systems such as Google SafeBrowsing or PhishTank


### Build a model
- Build a classifier: for example Random Forest gave good results in detecting phishing site [2]
- Try out different models and compare their performance. 



# References

- [1] Marchal, Samuel & Saari, Kalle & Singh, Nidhi & Asokan, N.. (2016). Know Your Phish: Novel Techniques for Detecting Phishing Sites and Their Targets. 323-333. 10.1109/ICDCS.2016.10. Also:https://arxiv.org/pdf/1510.06501.pdf
- [2] A. Aggarwal, A. Rajadesingan, and P. Kumaraguru, “PhishAri: Automatic Realtime Phishing Detection on Twitter.” 2013. Also https://arxiv.org/pdf/1301.6899.pdf
- [3] HoxHunt, Also: https://www.hoxhunt.com/blog/statistics-showing-5-phishing-trends-for-2019/ 
