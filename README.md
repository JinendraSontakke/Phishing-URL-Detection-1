# Phishing URL Detection

As the internet becomes a major mode for economic transactions and communications, online trust and cybercrimes have increasingly become an important area of study. The purpose of this project is to help individuals identify phishing URLs in order to provide safer practices online. The deployment of the Streamlit application allows users to verify the authenticity of URLs themselves.


### Table of Contents:

- Part I: Executive Summary
- Part II: Data Dictionary
- Part III: Conclusion

## Part I: Executive Summary

Phishing is a form of cybercrime in which a target is contacted via email, telephone, or text message by an attacker disguising as a reputable entity or person. The attacker then lures individuals to counterfeit websites to trick recipients into providing sensitive data. According to the FBI, phishing incidents nearly doubled in frequency, from 114,702 incidents in 2019, to 241,324 incidents in 2020.


The following sections are supported by the respective numbered Jupyter Notebooks:

### [01: Data Collection & Cleaning](https://github.com/ksylvia16/Phishing-URL-Detection/blob/fcb3072ea80edf49a117722be3d80ee97c098707/code/01_Data_Collection_%26_Cleaning.ipynb)

This project initially used just one dataset of 96,005 URLs-- about 50% legitimate URLs and 50% phishing URLs. While the model created was able to perform with an 91% accuracy on the testing data, model deployment seemed to have its own pitfalls. Simple websites such as www.google.com were classified as phishing. When taking a closer look at our dataset, it was evident that legitimate URL samples did not include short, simple URLs. An additional dataset is merged with the original to improve our model upon deployment. Phishing URLs were pulled from websites such as PhishTank and OpenPhish and legitimate URLs were pulled from websites such as Alexa and Common Crawl. 

Data cleaning included dropping null values (URLs that did not distinguish if legitimate or phishing), dropping unnecessary columns, changing dtypes, and adding a protocol to URLs without one. One dataset does not include a protocol (such as 'http://') in the provided URLs. In order for a future use of urlparse to work efficiently on the concatenated DataFrame, all URLs must include a protocol. It is important to note that features extracted from the protocol were not used in the model, but simply aided in the split of different URL parts. After combining the two DataFrames, duplicate URLs were dropped. A total of 545,895 instances were used.


### [02: Pre-Processing](https://github.com/ksylvia16/Phishing-URL-Detection/blob/fcb3072ea80edf49a117722be3d80ee97c098707/code/02_Pre-Processing.ipynb)

Feature engineering was a significant part of the Pre-Processing step. Using a function from urllib library, protocol, domain, path, query, and fragment were extracted from the URL and respective columns were created. The protocol column was dropped as more sophisticated phishing URLs are created with a protocol of https://. This can be misleading as the website is not, in fact, secure. Protocol was emitted from the model to prevent bias towards the different protocols. Length of URL, domain, path, query, and fragment are extracted as well as the quantity of specific characters in each part of the URL.


### [03: Model Selection and Evaluation](https://github.com/ksylvia16/Phishing-URL-Detection/blob/fcb3072ea80edf49a117722be3d80ee97c098707/code/03_Model_Selection_%26_Evaluation.ipynb)

First, we divide our data into a Train and Test vector to begin modeling. Nine models were examined:

- Stochastic Gradient Descent Classifier
- Logistic Regression
- AdaBoost
- Gradient Boost
- Decision Tree Classifier
- Bagging Classifier
- K-Nearest Neighbors Classifier
- Extra Trees Classifier
- Random Forest Classifier

Once the best model was determined, hyperparameter tuning using `GridSearchCV` and `RandomizedSearchCV` continued to optimize our final model. Random Forest Classifier performed best with a 94.5% testing score.

### [Model Deployment - Streamlit Application](https://github.com/ksylvia16/Phishing-URL-Detection/blob/fcb3072ea80edf49a117722be3d80ee97c098707/code/app.py)

Finally, the model is deployed through a Streamlit application. The application is designed for any individual to enter a URL, press a button, and the model will predict if the URL is a phishing or legitimate URL.

## Part II: Data Dictionary

Like mentioned above, the project initially used just one dataset of 96,005 URLs- about 50% legitimate URLs and 50% phishing URLs. An additional dataset was needed to improve our model upon deployment.

Data Sources: 

- [S. Marchal, J. Francois, R. State, and T. Engel. PhishStorm: Detecting Phishing with Streaming Analytics. IEEE Transactions on Network and Service Management (TNSM), 11(4):458-471, 2014]('https://research.aalto.fi/en/datasets/phishstorm-phishing-legitimate-url-dataset#:~:text=The%20dataset%20contains%2096%2C018%20URLs,%3A%20legitimate%20%2F%201%3Aphishing')
- [Siddharth Kumar. Malicious And Benign URLs (Kaggle)]('https://www.kaggle.com/siddharthkumar25/malicious-and-benign-urls')

Below is a list of relevant features included in `url_updated.csv` as they relate to the final predictive model:

|Feature|Type|Dataset|Description|
|---|---|---|---|
|url_length|int|url_updated.csv|Length of URL|
|qty_dot_url|int|url_updated.csv|Quantity of (.) in URL|
|qty_hyphen_url|int|url_updated.csv|Quantity of (-) in URL|
|qty_slash_url|int|url_updated.csv|Quantity of (/) in URL|
|qty_questionmark_url|int|url_updated.csv|Quantity of (?) in URL|
|qty_equal_url|int|url_updated.csv|Quantity of (=) in URL|
|qty_at_url|int|url_updated.csv|Quantity of (@) in URL|
|qty_and_url|int|url_updated.csv|Quantity of (&) in URL|
|qty_exclamation_url|int|url_updated.csv|Quantity of (!) in URL|
|qty_space_url|int|url_updated.csv|Quantity of ( ) in URL|
|qty_tilde_url|int|url_updated.csv|Quantity of (~) in URL|
|qty_comma_url|int|url_updated.csv|Quantity of (,) in URL|
|qty_plus_url|int|url_updated.csv|Quantity of (+) in URL|
|qty_asterisk_url|int|url_updated.csv|Quantity of (\*) in URL|
|qty_hashtag_url|int|url_updated.csv|Quantity of (#) in URL|
|qty_dollar_url|int|url_updated.csv|Quantity of (\\$) in URL|
|qty_percent_url|int|url_updated.csv|Quantity of (%) in URL|
|domain_length|int|url_updated.csv|Length of domain|
|qty_dot_domain|int|url_updated.csv|Quantity of (.) in domain|
|qty_hyphen_domain|int|url_updated.csv|Quantity of (-) in domain|
|path_length|int|url_updated.csv|Length of path|
|qty_dot_path|int|url_updated.csv|Quantity of (.) in path|
|qty_hyphen_path|int|url_updated.csv|Quantity of (-) in path|
|qty_slash_path|int|url_updated.csv|Quantity of (/) in path|
|qty_equal_path|int|url_updated.csv|Quantity of (=) in path|
|qty_at_path|int|url_updated.csv|Quantity of (@) in path|
|qty_and_path|int|url_updated.csv|Quantity of (&) in path|
|qty_exclamation_path|int|url_updated.csv|Quantity of (!) in path|
|qty_space_path|int|url_updated.csv|Quantity of ( ) in path|
|qty_tilde_path|int|url_updated.csv|Quantity of (~) in path|
|qty_comma_path|int|url_updated.csv|Quantity of (,) in path|
|qty_plus_path|int|url_updated.csv|Quantity of (+) in path|
|qty_asterisk_path|int|url_updated.csv|Quantity of (\*) in path|
|qty_dollar_path|int|url_updated.csv|Quantity of (\\$) in path|
|qty_percent_path|int|url_updated.csv|Quantity of (%) in path|
|query_length|int|url_updated.csv|Length of query|
|qty_dot_query|int|url_updated.csv|Quantity of (.) in query|
|qty_hyphen_query|int|url_updated.csv|Quantity of (-) in query|
|qty_slash_query|int|url_updated.csv|Quantity of (/) in query|
|qty_questionmark_query|int|url_updated.csv|Quantity of (?) in query|
|qty_equal_query|int|url_updated.csv|Quantity of (=) in query|
|qty_at_query|int|url_updated.csv|Quantity of (@) in query|
|qty_and_query|int|url_updated.csv|Quantity of (&) in query|
|qty_exclamation_query|int|url_updated.csv|Quantity of (!) in query|
|qty_space_query|int|url_updated.csv|Quantity of ( ) in query|
|qty_tilde_query|int|url_updated.csv|Quantity of (~) in query|
|qty_comma_query|int|url_updated.csv|Quantity of (,) in query|
|qty_plus_query|int|url_updated.csv|Quantity of (+) in query|
|qty_asterisk_query|int|url_updated.csv|Quantity of (\*) in query|
|qty_dollar_query|int|url_updated.csv|Quantity of (\\$) in query|
|qty_percent_query|int|url_updated.csv|Quantity of (%) in query|
|fragment_length|int|url_updated.csv|Length of fragment|
|qty_dot_fragment|int|url_updated.csv|Quantity of (.) in fragment|
|qty_hyphen_fragment|int|url_updated.csv|Quantity of (-) in fragment|
|qty_slash_fragment|int|url_updated.csv|Quantity of (/) in fragment|
|qty_questionmark_fragment|int|url_updated.csv|Quantity of (?) in fragment|
|qty_equal_fragment|int|url_updated.csv|Quantity of (=) in fragment|
|qty_and_fragment|int|url_updated.csv|Quantity of (&) in fragment|
|qty_exclamation_fragment|int|url_updated.csv|Quantity of (!) in fragment|
|qty_space_fragment|int|url_updated.csv|Quantity of ( ) in fragment|
|qty_comma_fragment|int|url_updated.csv|Quantity of (,) in fragment|
|qty_asterisk_fragment|int|url_updated.csv|Quantity of (\*) in fragment|
|qty_hashtag_fragment|int|url_updated.csv|Quantity of (#) in fragment|
|qty_dollar_fragment|int|url_updated.csv|Quantity of (\\$) in fragment|
|qty_percent_fragment|int|url_updated.csv|Quantity of (%) in fragment|

## Part III: Conclusion

Phishing threats are continuously evolving to become more complex. As a result, more users across the globe are falling victim to these attacks. Phishing awareness and detection becomes an increasingly important area of study and users should be concious of their online practices. Ultimately, this project was successful in crafting a model that performs well above the baseline when predicting whether a URL is legitimate or phishing. In order to ensure safe practices online, users should treat every email with skepticism and never click on a link without examining it first. With the addition of 'Fishing for Phishers' application, users can utilize this tool to take responsibility in verifying URLs.
