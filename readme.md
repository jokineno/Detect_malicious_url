## Description
- In this assignment I am extracting a small set of features that could be a sign of a malicious URL. 


### What I'm going to do:
- Build feature extractors, which could predict a risk of web page being malicious
- Extract 10-15 features. 
    - Use APIs: I will use APIs that does not require an API key or another authentication.
    - Use Beautiful Soup library for web scraping
- Handle errors in an extracting process: if error occurs -> return NaN, also save the error message.
    - Store data even if feature extraction fails -> NaNs are valuable information as well at this point.
- Build a small dataset based on features and save it into .csv.

### What I'm NOT going to do
- Not too many features. 
- No modeling at this point. 


## Features
- Predicting features below are based on findings in provided papers and my own interest, bias and preconceptions about them. 
- The most of the papers discovered that combining features from different categories gives the best prediction accuracy [1,2]. 
    - The single best predicting category is URL based features [1]
    - All features are language independent: no need to model a language detectors and independent models. 
    - I will combine features from different categories. 
    

### Categories: URL based, BlackLists, WHOIS based
1.  URL based features
    - Protocol: HTTP or HTTPS
        - There are more and more phishing sites which are using https [3].
    - a number of dots in FQDN (Fully Qualified Domain Name): integer
        - There are more dots (subdomains) in FQDN in phishing sited [2]
    - a length of URL: integer
        - This seems to be a trivial but interesting and never returns Null. 
    - a length of Fully Qualified Domain Name (FQDN): integer
        - Phishing site domains are typically shorter [2]
    - a number of redirections: integer
        - Many redirection can be used to evade the blacklists.
2. Blacklists
    - is ip on a blacklist: Boolean
         - Use IPSUM list: see https://raw.githubusercontent.com/stamparm/ipsum/master/ipsum.txt
    - is ip a proxy: lookup Boolean
        - Use https://input.payapi.io
3. Domain history
    - An age of a domain: Integer
        - The most of phishing sites are alive a really short period of time
    - How many days to expire: Integer
         - Phishing sites are not meant to stay alive for a long period of time. 
    - Country: String
        - Try to find out if an IP comes from a 'high-risk' country.
        - See https://payapi.io/apidoc/#api-Fraud-isIpTop10HighRiskCountry
4. Content based features
    - a number of HREFs in a html source code
        - Trying to figure out if phishing sites has a different amount of outgoing links. 
 

### Storing data
- Data Structure: DataFrame by Pandas
    - Permanent storing in .csv format -> can be done with df.to_csv() function provided by Pandas .

# Questions:
1. What features could indicate the malicousness of a given URL?
2. What goes in to the thinking of the attacker when they are choosing a site for an attack?
3. What would you develop next?

## 1. Features indicating the maliciousness of a given URL?
- URL based features are the single best predictor [1]
    - Different elements in URL (string): see above "Categories"
    - Protocol
        - HTTP or HTTPS -> HTTPS is more secure.
            - However, in last years there have been more and more phishing pages with https protocol [3]. 
- Content based features: 
    - An amount of text inputs, links
        - Use of keyterms in a site: how many times title appears on a page attributes. 
    - RDN usage
        - Compare a similarity of starting and landing url
    - A number of redirections
        - Phishing site evade blacklists by having a longer redirection chain. 

## 2. Attackers' line of thought: Make Things Look and Feel the Same!
- There are many types of ways to phish information. 
    - Phone calls: Nearly 50% of phone calls will be scams [3]
    - Impersonating techiques: DNS spoofing, email spoofing, social engineering [1]

### Social Engineering: trick user's cognition! 
- The weakest link in a security chain is a human. An attacker tries to trick a user's cognition. 
- The most of the attacks start with an email and people have hard time to detect a real from a fake one [4]. 
- A typical example of a phishing site is a web page which mimics a target page. At its best the phishing site looks and feels the same and a user may not detect that. 
- Attackers' main goal is to drive a user to a fake page and make the user to think this is the real target page. In this case user might get tricked to hand over sensitive information: personal information, credit card number etc. 
- Fake sites are usually alive for a short time. Typically target sites are the ones (enterprises) with a high traffic [3].
    - The power of phishes come from statistical probability -> phishers send multiple emails and some of people will buy it! 
- Also URLs can be made to look similar to a target: for example paypal.com vs. paypaI.com

## 3. What next? 
- In general: read more articles, learn more, understand more -> faster decision making. 
- I would gather more data from different sources, preprocess it, define metrics for a model performance and then try out different models and see which performs the best. 

### Achieve a higher quality of data
- Motto: garbage in, garbage out / gold in, gold out
- At the moment a processed dataset has many None values. 
    - I should discover more features that returns numerical or categorical value and are significant also. 
- Extract more features: based on the most of the provided articles the multi-criteria methods combining features from different categories works the best in predicting a phishing page [1,2]. 
- I liked the idea of "Know Your Phish" [1] where all features are language independent. I would focus on those features.

### Intuition over the final data set.
- Draw histograms, plots, find out correlations between features in order to get a intuition what is happening in the data. 

### Combine APIs
- Combine information from several APIs or find an API that already does that: make sure that the data quality is ok! 
    - In this task input.payapi was a good one, since it does not require authentication and is free to use. 
- Problems in a single API: some of the URLs I used in the project were found from Phishtank's blacklist but were not listed on other blacklists such as "Ipsum" or "input.payapi". 

### Preprocess data
- Mapping categorical values into numericals such as HTTP/HTTPS -> 0/1
- Handle missing values: estimate or delete: this depends on a size of a dataset

### Define metrics: 
- How I measure the accuracy of the model: train a model, validate it and test it with a test set. 
- Compare a model performance to phishing databases: can the model detect the ones that are already blacklisted. 

### Build a model
- Build a classifier: for example Random Forest performed well in detecting phishing sites [2]
- Try out different models and compare their performance. 

# References
- [1] Marchal, Samuel & Saari, Kalle & Singh, Nidhi & Asokan, N.. (2016). Know Your Phish: Novel Techniques for Detecting Phishing Sites and Their Targets. 323-333. 10.1109/ICDCS.2016.10. Also:https://arxiv.org/pdf/1510.06501.pdf
- [2] A. Aggarwal, A. Rajadesingan, and P. Kumaraguru, “PhishAri: Automatic Realtime Phishing Detection on Twitter.” 2013. Also https://arxiv.org/pdf/1301.6899.pdf
- [3] HoxHunt Blog 1, Also: https://www.hoxhunt.com/blog/statistics-showing-5-phishing-trends-for-2019/ 
- [4] HoxHunt Blog 2, Also: https://www.hoxhunt.com/blog/phishing-attacks-and-scams-in-2019-and-beyond/
