## Predicting high salaries from Indeed.com posting
### Executive Summary
---

**Overview**

Using information taken from [uk.indeed.com](https://uk.indeed.com) a predictive model has been developed to identify job postings likely to offer high salaries. The key features of a job posting likely to influence whether or not it is high salary are:

- whether the job is based in London
- whether the job title is either for a lead or senior data role
- whether the employing organisation has University in its name

The model was tested on results from Indeed.com on 9th September 2021. The model succesfully identifed jsdkadjis jobs as offering high salaries, with no results incorrectly identified as having a high salary.

Indeed.com have many posts which do not contain salary information. With this model it would be possible those with a high likelihood of offering high salaries.

The criterion for a high salary was set as greater than the median which, for the data collected was £60,000.

---
**Methodology**

*Webscraping*

Indeed.com allow users to search for jobs with specific key words and within a specific region. For this exercise, we were interested jobs matching the desciption of 'Data Scientist' offering greater than £20,000. To gather information for all jobs matching this description in a city such as London would take a considerable amount of time. Instead of doing this manually, we are able to capture specific information on each webpage based on the underlying HTML code for the page. Because of the speed at which we are able to gather information, we can loop over multiple search pages from multiple cities.

The information gathered included job title, company name and location. For the purpose of building the model, the salary information was also taken, where available. This was done for 16 major UK cities including London.

<br>*Data Cleansing*

All available results for the selected cities were scraped on 5th September and 7th September. This provided 330 unique job results with salary information from which to build and test the model. Information from Indeed.com isn't necessarily consistent, with some locations being provided as a city, some as a region, and others included post codes. The data was cleaned to provide consistent groupings of locations. Information with regards to whether the job allowed for remote working was also extracted from the location and put into binary variables (specifiying whether or not remote working was part of the location information).

To make use of the text information in the job title and company names, combinations of words and individual words are considered in terms of whether their presence in a job posting is indicative of a high salary of not. Any improvement in consistency within the text fields will improve the effectiveness of the model. For this reason the job titles were cleaned to remove symbols and trim down words.

<br>*Random Forest Predictive Model*

A decision tree is a combination of simple yes/no questions regarding each observation. From these questions the model calculates a probability for the observation being each of potential outcomes. A random forest model takes a large number of different decision trees each based on a different set of questions. After applying applying each of these individual decision trees to an observation, the random forest will base its prediction based on the aggregation of each of the individual outputs into a single probability. The benefit of a random forest is that it avoids overfitting - a situation where the model performs well for the data it's created from, but deals poorly when trying predict for data it hasn't seen before.

The default for the random forest model is to mmake the decision of whether an observation is high salary is based on whether the probability is greater than 0.5. Because we wish to have more certainty over those predicted as high value, this threshold was raised to 0.8. So each of the high salary predictions are based on probabilities greater than 0.8.

---
**Limitations**

1. Because of the requirement to raise the certainty of the high salary predictions, the overall quality of model is degraded. There will be an increase in the number of jobs with high salaries not flagged as such.
2. The model was created based on an initial set of data in a point in time. As the job market changes the data the model was built on may no longer be representative, leading to incorrect predictions. The most obvious example of this would be the threshold of £60,000 separating low and high salaries. As the average salary increases, this threshold would need to change.
3. While random forest models are able to provide an indication of which features are most important in the decision of whether an observation has a high salary or not, it is not useful in providing information on whether the feauture will increase or decrease the liklihood of a high salary.

---
