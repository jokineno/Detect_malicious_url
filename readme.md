## Assignment
- In this assignment you as a HoxHunt Data Science Hunter are given the task to extract interesting features from a possible malicious indicator of compromise, more specifically in this case from a given potentially malicious URL.

#### What we expect
Investigate potential features you could extract from the given URL and implement extractors for the ones that interest you the most. Below example code extracts one feature but does not store it very efficiently (just console logs it). Implement sensible data structure using some known data structure library to store the features per URL. Also consider how would you approach error handling if one feature extractor fails?

Be prepared to discuss questions such as: what features could indicate the malicousness of a given URL? What goes in to the thinking of the attacker when they are choosing a site for an attack? What would you develop next?

#### What we don't expect
Implement a humangous set of features.
Implement any kind of actual predicition models that uses the features to give predictions on malicousness at this stage 


### What I'm going to do:
- Build feature extractors, which could predict a risk of web page being malicious
- Extract 10-15 features. 
- Handle errors in extracting process: return NaN if error occurs, also save the error message. 
    - Store data in all cases -> NaNs are valuable information as well at this point.
- Build a small dataset based on features and save it into .csv.

### What I'm NOT going to do
- Not too many features!
- No modeling at this point! 


## Features
- Predicting features below are based on findings in provided papers and my own interest, bias and preconceptions about them. 
- The most of the papers discovered that combining features from different categories gives the best prediction accuracy [1,2]. 
    - The single best predicting category is URL based features [1]
    - All features are language independent: no need to model a language detectors and independent models. 
    - I will combine features from different categories. 
    

### Categories: URL based, BlackLists, WHOIS based
1.  URL based features
    - protocol: HTTP or HTTPS
        - There are more and more phishing sites which are using https [3].
    - a count of dots in FQDN (Fully Qualified Domain Name): integer
        - There are more dots in FQDN in phishing sited [2]
    - a length of url: integer
        - This seems to be a trivial but interesting and never returns Null. 
    - a length of Fully Qualified Domain Name (FQDN): integer
        - Phishing site domains are typically shorter [2]
    - a number of redirections: integer
        - Many redirection can be used to evade the blacklists.
2. Blacklists
    - is ip on blacklist: Boolean
    - is ip proxy: lookup Boolean
3. Domain history
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
- Data Structure: DataFrame by Pandas
    - Permanent storing in .csv format -> can be done with df.to_csv() function provided by Pandas .
- Handling NULLs, NoneTypes, NaNs: At this point, if extractor cannot find any value code will save it to a DataFrame. 
- Later, in model building stage I have to preprocess data more carefully and also map categorical values into numerical.



# Questions:
1. What features could indicate the malicousness of a given URL?
2. What goes in to the thinking of the attacker when they are choosing a site for an attack?
3. What would you develop next?


## 1. Features indicating the maliciousness of a given URL?
- The best results are achieved when combining features from different categories [1,2]. 
- URL based features are the single best predictor [1]
    - Phishing site URLs might look vague, they can have many redirections or emails included. 
    - Protocol
        - HTTP or HTTPS -> HTTPS is more secure.
        - However, in last years there have been more and more phishing sites with https protocol. 
- Examples of other feature categories
    - Content based
    - Background might be just a screenshot of the target page. 
        - Not a lot of text content in the page. 
        - Are the same words used in content and in domain. 
     - Keyterm extraction
         - Usage of starting and landing main level domain (MLD)
            - MLD name typically appears in several sources extracted from target pages where as in phishing pages not [1].
         - Registered Domain Name (RDN) usage [1]
            - Similarity between different elements: starting, landing url, redirection chain, loaded content and HREF links. 
            - Phishing pages uses longer redirection chains and there's more variance in rdn usage compared to real pages [1]
        



## 2. Attackers' flow of thought: Make Things Look and Feel the Same!
- There are many types of ways to phish information. 
    - Phone calls: Nearly 50% of phone calls will be scams [3]
    - Impersonating techiques: DNS spoofing, email spoofing, social engineering [1]

### Social Engineering: trick user's cognition! 
- The weakest link in a security chain is a human. An attacker tries to trick a user's cognition. 
- The most of the attacks start with an email and people have hard time to detect a real from a fake one [4]. 
- Typical example of phishing site is web page which mimics a target page. At its best the phishing site looks and feels the same and a user may not detect that. 
- Attackers' main goal is to drive a user to a fake page and make the user to think this is the real target page. In this case user might get tricked to hand over sensitive information: personal information, credit card number etc. 
- Fake sites are usually alive for a short time. Typically target sites are the ones (enterprises) with a high traffic [3].
    - The power of phishes come from statistical probability -> phishers send thousands or tens of thousands of emails and some of people will buy it! 
- Also URLs can be made to look similar to a target: for example paypal.com vs. paypaI.com



## 3. What next? 
- In general, I will: read more articles, learn more, understand more -> faster decision making. 
- I would gather more data from different sources, preprocess it, define metrics for model performance and then try out different models and see which performs the best. 

### Achieve a higher quality of data
- Motto: garbage in, garbage out / gold in, gold out
- Extract more features: based on the most of the provided articles the multi-criteria methods combining features from different categories works the best in predicting a phishing page [1,2]. 
- I liked the idea of "Know Your Phish" [1] where all features are language independent. I would focus on those features.

### Combine APIs
- Use another API such as www.whois.net in order to get more and more detailed informations. 
    - In this task input.payapi was a good one since it does not require authentication and code can be run by anyone without authentication. 
- Combine information from different APIs -> higher level of certainty of a quality of data
    - For example: double check blacklists. 

### Preprocess data
- For example:
    - Mapping categorical values into numericals such as HTTP/HTTPS -> 0/1
    - Handle missing values: estimate or delete: this depends on a size of a dataset

### Define metrics: 
- How I measure the accuracy of my model: train a model, validate it and test it with a test set. 
- Get sample URLs from PhishTank for example. 

### Build a model
- Build a classifier: for example Random Forest performed well in detecting phishing sites [2]
- Try out different models and compare their performance. 


# References

- [1] Marchal, Samuel & Saari, Kalle & Singh, Nidhi & Asokan, N.. (2016). Know Your Phish: Novel Techniques for Detecting Phishing Sites and Their Targets. 323-333. 10.1109/ICDCS.2016.10. Also:https://arxiv.org/pdf/1510.06501.pdf
- [2] A. Aggarwal, A. Rajadesingan, and P. Kumaraguru, “PhishAri: Automatic Realtime Phishing Detection on Twitter.” 2013. Also https://arxiv.org/pdf/1301.6899.pdf
- [3] HoxHunt Blog 1, Also: https://www.hoxhunt.com/blog/statistics-showing-5-phishing-trends-for-2019/ 
- [4] HoxHunt Blog 2, Also: https://www.hoxhunt.com/blog/phishing-attacks-and-scams-in-2019-and-beyond/
