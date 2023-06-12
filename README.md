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

## Results

- Data Preprocessing

    - What variable(s) are the target(s) for your model?
        - The target variable is the `IS_SUCCESSFUL` column in the dataset which has either a `1` or a `0` meaning successful use of funding or not respectively.

    - What variable(s) are the features for your model?
        - After dropping the `IS_SUCCESSFUL`, `EIN`, and `NAME` columns, the remaining columns serve as variables/features in the subsequent models.

    - What variable(s) should be removed from the input data because they are neither targets nor features?
        - Specific variables prior to putting the data through a hot-ended filter are not removed from the dataset outside of `EIN`, and `NAME` columns meantioned earlier. Lasso and Ridge tests were compared to a PCA text to reduce the data size while retaining as much of the explained variance as possible. Ultimately these two methods do similar preprocessing to the data so both were used together in conjunction with setting boundries using ~1.5x the interquartile range of the values in the `ASK_AMT` column to preprocess the data.

        ![IQR](/data_pictures/ASK_AMT_spread.png)

- Compiling, Training, and Evaluating the Model

    - How many neurons, layers, and activation functions did you select for your neural network model, and why?
        - While preprocessing, I maintained using the default prescribed model parameters of 80 neurons on the initial layer, 30 for the 1 hidden layer, and 1 for the output layer using `relu`, `relu`, `sigmoid` respectively. 
        - To find the optimal selection of neurons, layers, and activation functions, I used Keras Tuner in two phases: 
        - 1. limiting down and eliminating activation functions that did not improve the model.
            - The tuner was allowed to select any activation function for the first and hidden layers. The output layer was always 1 neuron with a `sigmoid` activation function. `leaky_relu`, `tanh`, `selu`, `elu`, & `sigmoid` were all eliminated from the pool based off the tuner trials. `relu` was eliminated from the initial layer but not from the hidden layers.
        - 2. test and find the optimal layout of neurons and hidden layers. 
            - Finally, the tuner was allowed to trial test to find the optimal layout from 3-8 hidden layers and 30-80 neurons per layer. Any further hidden layers did not result in any benefit and any further neurons contributed to a model that required too much computing power. The chosen layout for the model was the one with the highest `val_accuaracy` and can be found in the ipynb or just below:

            ``` 
            initial activation: tanh
            first_units: 80
            num_layers: 3
            layer_activation_0: tanh
            units_0: 50
            layer_activation_1: relu
            units_1: 55
            layer_activation_2: relu
            units_2: 75
            layer_activation_3: tanh
            units_3: 30
            ``` 

    - Were you able to achieve the target model performance?
        - The target model performance was hit in the preprocessing. Any neuron, layer, or activation function modification caused minimal impact past 75% `val_accuracy`.

    - What steps did you take in your attempts to increase model performance?
        - methods used include:
        1. Lasso and Ridge tests to drop low impact columns.

        ![Dropped_Columns](/data_pictures/droppedcolumns_model.png)

        2. PCA the data to reduce scaled data columns down to 35 columns for 100% explained variance, 20 columns for 70% explained variance with a model performance boost.

        ![PCA](/data_pictures/pca_model.png)

        3. Oversampling by comparing the use of RandomOverSampler and SMOTE oversampler. SMOTE in this case caused more inaccuracy in the validation test. It is possible that the synthetic samples are not accurately respresenting the actual data. RandomOverSampler copying samples did increase the accuracy marginally.

        ![Oversampled](/data_pictures/oversampled_model.png)

        4. Altering the model's activation function, layer count, and per layer neuron count via Keras Tuner.

        ![OptimizedNN](/data_pictures/optimizednn_model.png)

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