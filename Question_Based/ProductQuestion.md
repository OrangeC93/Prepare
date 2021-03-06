# Product
#### 1. Important parameters in RF?
- number of trees
- minimum number of events per tree
- number of features randomly sampled at each split

#### 2. A related to B, we managed to move A in a forced way, but B doesn't change, so A wasn't causing B.
- make the proxy for A.
- select variabes that can relate to that A.
- build a model to predict, select the important ones.
- figure out how to positively effect those variables.

#### 3. Metrics drop in sudden?
- technicsal problem (goes to 0)
  - bug in UI.
  - bug in action to store data into db.
  - bug in query to pull data from db.
  - bug in dashboard to show metrics.
- sudden change in user behavior
  - user segment by various features to dig into which part has problem.
  
#### 4. Metrics is down?
Whenever there's a metric is down, start by breaking it down into its components, make assumption, analyze them independently (build a model to see the reasons).
- numerator
- denominator

#### 5. How to think of adding a New Feature?
A common question to ask whether it would be a good idea to implement a new feature, which is a big part of DS job to look at the data, based on findings, suggets and validate new features/product idea. We need to consider it in the following 3 steps.
- potential influence to business: if feature is successful, would it be a good thing for site, can benefit KPI metrics?
  - eg. for FB, engagement is its final goal!
- demand: find a proxy for the demand of that feature in current data.
  - eg. if many users are already performing some sort of activity on your site, which you can get from using NLP to extract sentiments about this feature.
- test until implement.
  - define goal
  - clarify question
  - define a few metrics to measure the goal
  
#### 6. Long term metrics?
- find a short term proxy for the long term metric, this's the point of this as well as other similar questions about long term user engagement, retaining users, or estimating customer lifetime value, 
- eg. estimating subscription retention rate, which is usually the percent of users unsubscribe within 12 months, so we could collecting the customer 12 month ago and see whether they unsubscribe it in 12 month, label them 1 or 0, build model with variables you think might influence the result, and the results shows a short term metric has big influence on that long term metrics, so do testing on that short term metrics instead of the long term metric.

#### 7. A/B test by market?

#### 8. Metrics for any new product launched
Engagement and retention are two sides of the same coin, retained users are the most engaged ones in the high majority of
cases.
- ability to acquire new users
- ability to retain current users

#### 9. Novelty effect
- definition: users use more a product whenit is new. 
- how its effect: a good proxy to check for novelty effect is: only consider users for which it was the first experience on the site. First time users are obviously not affected by novelty effect.

#### 10. Insrumental variable vs Proxy variable
- An instrumental variable is used to help estimate a causal effect (or to alleviate measurement error). 
  - instrumental variablemust affect the independent variable of interest.
  - only affect the dependent variable through the independent variable of interest, called an exclusion restriction.
- A proxy variable is a variable you use because you think it is correlated with the variable you are really interested in, but have no (or poor) measurement of.

#### 11. We're trying to predict Y and how to find out whether X is a discriminant variable or not? If not, what are the actual discriminant variables？
Use tree to whether X or other variable are the discriminant variable to Y. 
- If the tree doesn't significantly use the X variable, you are sure that it doesn't really matter. In this
case, the tree would split on, for instance, combinations of other variables since that's where most of the
information is. If X is acting as a proxy for those variables, for sure the tree would choose those other
variables first, as they would allow for a better classification. After those splits, there is no information left to
extract from X so the tree won't touch it. It is important that you choose a model that works well with
correlated variables, like trees.
- In other case, if the tree does significantly use X variable, the next step is the find the reason for why it's the discriminant variable.

#### 12. What are the drawbacks of using supervised machine learning for fraud?
The drawbacks of supervised machine learning in general, especially, when you have extremely low signal-to-noise ratio.
- the high majority of events is legitimate, therefore, our model tends to predict everything as non-fraud, achieving very high classification accuracy, which is not useful.
  - changing the model internal loss to penalize more false negatives
    - the cost of a false negeative = the cost of a fraud
    - the cost of a false positive = the cost of blockign a legitimate customer 
  - using an extremely aggressive cut-off for classification (everything above 0.1 is classified as fraud)
  - reweighing the training events.
However, these techniques only work if you have many positve cases in absolute numbers in your training set, and to have many positive cases you need massive amount of data. 
- Also your training set if based on past events that have been identified as frauds. If you haven't caught them in the past, the model won't predict them as fraud, so it's a vicious circle.
- model cannot defense new forms of fraud, you're always one step behind.

We can use anomaly detection, we are not training the model but simply on different behavior.
So, the combination of both approaches: block uses if their pattern is similar to the past frauds.

#### 12. Two-Step authentication
- first approach
  - identify cost of false negativea nd values of true negatives
  - run a test where only a subset of users if required to go through the two-step process
  - apply values 
- second approach
  - segement users accordign to predicted probability of fraud
  - find which segments is profitable to implement the two-step process

#### 13. [Anomaly detection?](https://www.slideshare.net/streamanalytix/anomaly-detection-real-world-scenarios-approaches-and-live-implementation)
- taxonomy of anomaly detection
  - point anomaly
  - contextual anomaly
  - collective anomaly
- choice of algorithm
  - data has no labels
    - apply time-series anomly detection algorithms
    - apply k-means clustering
  - data has labels
    - when time-stamps are present: apply standard machine learning classification
    - when time-stamps are absent: apply sequence classification algorithms
- approaches to anomay detection
  - supervised(classification)
    - data skewness, lack of counter examples
  - semi supervised(novelty detection)
    - requires a nomal training dataset
  - unsupervised(clustering)
    - faces curse of dimensionality

#### 14. Missing value 
Missing values due to due the self-selection bias in most cases. 
eg. Uber trips without rider review, not filling out profile information on a social network.

#### 15. A/B test wins with significsant p-value, but you choose not to make the change
- changes has cost: human labor costs, customer service time, risk of bugs...
- inferenrial statistics: if the sample size is very large, it's extremely likely to get a significant p-value, even if the effect is very samll.

#### 16. Randomly split users?
- mentally take extreme cases. will these optionshave any effect on the control group?iIf the answer is yes (like it is obviousfor the Uber case), you can't just randomly split users. 
  - new product has a bug and it is unusable. 
  - new product is amazing and test users will use it 24/7, novelty effect
- eg. Uber with new UI, split users in same market, results will be influenced by competition, so test by market, so we need to match comparable markets in pair.

#### 17. What're the implications of curse of hight dimension from a product standpoint?
- it means every event is almost guaranteed to be an outlier on at least one dimension, if you imagine an n-dimensional space, where n is the number of features, all events will end up close to the border and the center will be empty.
- it means you're building a product that perfectly fits no one, which is a problem for ds to solve between statistical problem via personalization.

#### 18. A/B test where t-test independency assumptions are not met and check the implications?
- make sure test is done by randomly spliting users, test and control groups are not independent.
  - eg. test in social network, the behaviours by test group can be seen by control group.
- social network are affected by network effects.

#### 19. How find out if users put wrong/fake information 

#### 18. Issue with splitting a small dataset in training/test set?
If data set is too samll, the actual model will depend heavily on the random way in which you split the data.
- cross validation
- simulate sampling more data from the original distribution

#### 20. What is Churn?
A common revenue model for SaaS company is to charge a monthly subscription fee for access to their product. Company aim to continually increase the number of users paying for their product. One metric that's helpful for this goal is churn rate.

Churn rate is the percent of subscribers that have canceled within a certain period, usually a month. For user base to grow, the churn rate must be less than the new subscriber rate for the same period.

#### 21. Avoid vantiy metrics
Metrics that look good,but are useless. A good way to quickly identify if my metrics are useful is: imagine a bot is creating a bunch of fake accounts. Will my metrics go up? 
- In the example above, we are good. The engagement metric will drop and alert us that something not good is going on. 
- On the other hand, if you ONLY looked at new sign ups per day, you would be happy with the fake accounts. 
If they ask you for just one metric, make sure it is not a metric that a bot could improve.

