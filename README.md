# job_satisfaction_neural_network

- - -

## Overview of the Analysis and Neural Network

- Purpose
    - The purpose of the neural network is to predict happiness on an objective basis using how a person feels regarding the finer details of role and organization. We would like to increase the size of the dataset from just the reviews from Glassdoor to include factors that can effect the organiztion and an individuals' mood that can be found in the World Happiness Report (WHR).

- What is being predicted?
    - Overall_rating is the column being predicted. The scale is subjective as a 1-5 rating can be effected by different aspects in a person's professional life. We hope to remove as much bias from individual instances as possible and create a model that can guage overall satisfaction from a more holistic approach.

- Goal
    To create a model that is capable of guaging mood in an organization based off more simple review questions. Organizations don't often get complete exit interviews and often do not consider the mood of exisiting employees. Glassdoor collects data with the incentive for users to access information posted form other users. This will give a more complete approach to guaging mood vs standard practices.

- Measuring
    After the model is built, it will be tested on a subset of testing data and the rating with the greatest confidence will be selected as the "Predicted_Overall_Rating" and that will be compared to the user input overall rating. The delta will be added up and averaged for the number of samples to get the Square Root of the Mean Squared Error.

## Data Preprocessing & Results

1. To reduce the dataset to working componets, the data was split into 4 smaller CSVs and hosted on DropBox as a static database with a manufactured key used to connect them together when a query is made.

2. For this update, `date_review`, `job_title`, `location` are dropped. Location will be cleaned in a later update to this report for the WHR to be joined.

3. `firm` and `current` get any instances with a `value_count()` lower than the cut off changed to "other."

4. All preprocessing cleaning methods were rounded down to hot-end processing string data with NaN values either removed or masked with the column's mean.
    - For the purpose of this update, NaN values were dropped as the cleaning process for masking NaN values was longer than Google Colab's non-interact timer.

5. User input string data from `headline`, `pros`, and `cons` is now loaded separately to go through processing.
    - All columns corresponding to the dropped NaN values from the previous DataFrame are dropped.
    - For each possible rating and each attribute for each rating, a collection of tokenized words are collected as will as their counts in a new DataFrame. This shows the top 500 words for each attribute rating combination.

    ![image1](/pictures/Screenshot%202023-06-12%20155707.png)

    - Included in the data processing but not used for this update of the report is a droppinf of all duplicate words from this large dataset showing only unique values. In the next update, this will be removed in favor of using the count of values as a weight for the neural network.

6. The actual dataset with user input string values is put through a function that tokenizes the text and records matches with the columns of popular words in each attribute and rating. This count is appended to a DataFrame that will be merged with the actual dataset. The purpose of this is to objectify subjective writing to improve the neural network's performance from using the Natural Language Toolkit.

## Summary



- - -

### Background

Glassdoor takes in reviews from current and former employees on the organization that they work or worked for. Items like salary, approval of the CEO, and overall rating are only some of the items of information that Glassdoor requests to collect. "The World Happiness Report is a publication of the Sustainable Development Solutions Network, powered by the Gallup World Poll data. The World Happiness Report reflects a worldwide demand for more attention to happiness and well-being as criteria for government policy. It reviews the state of happiness in the world today and shows how the science of happiness explains personal and national variations in happiness."

One of the objectives of this research project is to include happiness based off location from the World Happiness Report (WHR) in conjuction with user reviews of how satisfied they are working for or having worked for organizations that operate in the country they reside. This will allow intangibles from the WHR to be used when answering the question: "What really makes employees happy?"

Data points from the Glassdoor Reviews CSV are as follows:

    - `firm`— Organization name
    - `date_review`— Date review was posted
    - `job_title`
    - `current`— If reviewer is a current employee of ex employee and duration
    - `location`—
    - `overall_rating`— Rating from 1 - 5
    - `work_life_balance`— Rating from 1 - 5
    - `culture_values`— Values of the organization in alignment with employees. Rating from 1 - 5
    - `diversity_inclusion`— Rating from 1 - 5
    - `career_opp`— Opportunities for growth. Rating from 1 - 5
    - `comp_benefits`— Overall Benefits including pay. Rating from 1 - 5
    - `senior_mgmt`— Rating from 1 - 5
    - `recommend`— rated with v - Positive, r - Mild, x - Negative, o - No opinion
    - `ceo_approv`— rated with v - Positive, r - Mild, x - Negative, o - No opinion
    - `outlook`— rated with v - Positive, r - Mild, x - Negative, o - No opinion
    - `headline`— User input title of descriptive review
    - `pros`— User input
    - `cons`— User input


Data points from the WHR CSV can be found on the WHR website. Link to the [Statistical Appendix](https://happiness-report.s3.amazonaws.com/2023/WHR+23_Statistical_Appendix.pdf)
    
- - -

## References

Glassdoor Job Reviews. This large dataset contains job descriptions and rankings among various criteria such as work-life balance, income, culture, etc. The data covers the various industry in the UK. Great dataset for multidimensional sentiment analysis. [Kaggle](https://www.kaggle.com/datasets/davidgauthier/glassdoor-job-reviews).

World Happiness Report 2023. The World Happiness Report is a landmark survey of the state of global happiness. [https://worldhappiness.report/](https://worldhappiness.report/)