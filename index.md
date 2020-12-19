## FluShot: Predicting flu onset using data from wearable devices

You can use the [editor on GitHub](https://github.com/melihyilmaz/CSE490G1_FinalProject/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

# Example Project

**Absract:** Write a 3-4 sentence abstract. It should introduce the problem and your approach. You may also want some numbers like 35 mAP and 78% accuracy. You can use this example README for your project, you can use a different ordering for your website, or you can make a totally different website altogether!

[PROJECT VIDEO](https://drive.google.com/drive/folders/1peh_0dNibL9ziWJkzX4oK5BzMr4GiCE8?usp=sharing)

## Table of Contents
- [Introduction](#introduction)
- [Related Work](#related-work)
- [Methods](#methods)
- [Results](#results)
- [Discussion](#discussion)
- [References](#references)

## Introduction

In a year of global pandemic, data driven to methods for prevention of infectious diseases have been under the spotlight. In this context, 'track & trace' technologies have justifiably received a lot of attention as they promise to contain outbreaks by tracing chains of infections to identify spreaders and individuals under risk. Taking a step further, accurate predictive models would be able to spot and help isolate individuals who are infected without a clinical diagnosis, hence stopping chains of infections right on their track even before they start to accelerate.

Accordingly, this project was motivated by the task of making daily predictions regarding influenza onset for a cohort of flu study participants using a mixture of demographic data, medical history, daily surveys and activity data derived from Fitbit smartwatches. As will be described in sections below, this involved a) defining a realistic and meaningful prediction task which would mimic the conditions under which a real-time flu prediction framework could potentially operate, b) comparing how traditional machine learning methods performed against neural network based approaches for the task at hand and c) effect of different hyper parameters and also different data modalities to the overall performance.

## Related Work

Data derived from wearable devices, especially markers such as heart rate, sleep status and step count, has recently gained more traction as potential predictors of influenza like illness (ILI) onset. On a population level, weekly variations in wearable data were shown to correspond to ILI rates for the same population [[1](https://www.thelancet.com/journals/landig/article/PIIS2589-7500(19)30222-5/fulltext)]. Although largely correlational and lacking the individual level resolution that would deliver actionable predictions, this work demonstrates the potential of wearable data for influenza surveillance. Similar work focusing on individual level predictions for COVID-19 symptoms and COVID-19 related hospitalizations has leveraged neural networks to model wearable device data and demonstrated that heart rate, as well as respiration rate, as measured by Fitbit devices were elevated with disease onset[[2](https://www.nature.com/articles/s41746-020-00363-7)]. Another very recent paper using wearable data for ILI symptom prediction, involving both influenza and COVID-19, employed both neural nets and traditional machine learning modeling but most importantly highlighted the importance of defining a realistic learning task and setting by demonstrating the difference in performance between retrospective and prospective training schemes [[3](https://drive.google.com/file/d/1SEvMyBovlbPTVpPx-BZlRf3sUoQ9oKW1/view)].

Inspired by such previous work, this project seeks to validate the usefulness of wearable data, as measured by the performance boost it provides for prediction of ILI related outcomes , while maintaining a realistic, and potentially deployable, modeling framework.

## Methods

### Data

For this project, a private data set compiled by Evidation Health, a digital health company, as a part of the Homekit2020 flu study was used. Some fast facts about the study cohort can be found below:

- Around 5200 participants
- Age 18 years or older
- US residents
- Own wearable Fitbit
- Willing to complete daily surveys on the mobile app
- Followed for 4 months, January - May 2020

Study participants were follow through the flu season of 2020 and after enrolling in the study, were provided with at home flu test kits. They were told to use the kits soon as they were instructed to do so by the study app, the main interface in the whole study with which the participants interact. The study procedure was roughly as following:

1) Baseline questionnaire at the beginning
2) Home test kits delivered to participants
3) Daily one question survey through mobile app
4) If participant has symptoms, more survey questions
5) A certain combination of symptoms (e.g. has cough and muscle ache) trigger test kit
6) Tests sent to lab for diagnosis

For each participant, four different sources of data were available:

- Baseline Questionnaire (baselines)
- Daily Surveys (survey data)
- Fitbit data (activity data)
- Lab results

Baseline Questionnaire, referred to as baselines from here on, included demographic information, medical history, drug usage history as well as some lifestyle and wellbeing questions that participants respond to at the beginning of the study.

Daily survey, referred to as survey data from here on, normally consist of a single question that inquiring whether the participant had any ILI symptoms in the past 24 hours. If the answer is yes, more questions follow soliciting information about the nature of symptoms, medication usage if any and some quality of life questions.

Fitbit data, referred to as activity data from here on, includes heart rate data, sleep related features (nap count, minutes in bed, minutes of light sleep vs deep sleep etc.), physical activity related features (step counts, calories burned, minutes of intense physical activity etc.)

Lab results indicate whether a self-administered test kit is flu positive. Three different ILIs, namely Flu A, Flu B and RSV, constitute the definition of flu positive in this project. The actual date of diagnosis was selected as the date corresponding test kit was triggered.

### Prediction task and training setup

The prediction task was formulated as a binary classification task where a flu diagnosis would be predicted for each day during the flu season, given baselines, survey data and activity data. Beyond their date of diagnosis, flu positive participants were excluded from the rest of the training procedure. To classify each day, a fixed time window of n days were used (e.g. data from Monday to Friday was used to for a given participant to predict if the participant would receive diagnosis on Friday).

A prospective training scheme was used to train models under a more realistic framework which mimic a potential deployment setting. To this end, the data was split temporally into two parts:

- Observation Phase (training)
- Deployment Phase (validation and testing)

During the observation phase, which include all data for the first t weeks of the study period, the models were first trained on randomly cropped time windows, the aforementioned n day windows. Subsequently, the deployment phase includes all data beyond the first t weeks of the study period and models make daily predictions while progressing through this phase. In other words, the model first predicts a flu positivity on day d, then uses that day to train further before repeating the same procedure with day d+1 and so on, following a 'train and advance' approach. In this way, future data is never made available to the model similar to a realistic setting where a deployed model could only use previous days' data. Also, the first 2 weeks of the deployment phase were used for validation and hyper-parameter selection, whereas the rest was only used at test time. The timeline used to split data set into observation and deployment, i.e. train, validation and test, is illustrated below:

![data_timeline](https://github.com/melihyilmaz/CSE490G1_FinalProject/blob/gh-pages/data_timeline.png)


### Models

Three different modeling, of increasing complexity, were employed, here on referred to as model 1, 2 and 3.

Model 1 consists of a single machine learning classifier, namely an XGBoost, which predicts the label for a given time window. Baselines, survey data and activity data are all concatenated and subsequently passed to the model. Model 1 acts as a simple baseline which doesn't exploit the temporal relationship in the time series data.

Model 2 consists of 2 neural networks serving as feature extractors for survey and activity data respectively, as well as an XGBoost end classifier. Instead of feeding the time series data straight to the end classifier, this approach focuses on learning fixed length neural representations from them, which would condense their predictive potential while exploiting the relationships among features. The proxy task used to learn representations of both survey and activity data is the main task itself, i.e. predicting the flu diagnosis at the end of a given time window. Feature extractors, one using only survey data and the other only activity data, are trained for this task and the output of the penultimate layer of these neural nets are passed on to the end classifier. Representations survey data, activity data and baselines are concatenated before being fed to the end classifier. Ultimately, the end classifier predicts the label of the given time window. You can find schematic for model 2 below:

![model2&3](https://github.com/melihyilmaz/CSE490G1_FinalProject/blob/gh-pages/model2%263.png)

Two different network architectures were employed as feature extractors the experiments. Firstly, a one dimensional convolutional neural network (1-D CNN), also called a temporal CNN, was used. Consisting of 3 1-D convolutional layers and a final fully connected layer, where batch normalization is applied following each convolutional layer and dropout right before the fully connected. Secondly, a recurrent neural network (RNN) based architecture featuring a bi-directional LSTM and a subsequent fully connected layer was used. In both cases, the input of fully connected layers, i.e. the output of the penultimate layer, were used taken to be learned representations. Selecting these architectures for feature extractors allowed the model to learn temporal dependencies among the time series data.

Model 3 closely resembles model 2 in terms of its feature extractors but, instead of an XGBoost, features two fully connected layers as the end classifier which allow for the end-to-end training of the whole framework. Similar to what happens in model 2, feature extractors are first separately trained (this step can be called pre-training). Later, both feature extractors and the end classifier are trained in an end-to-end manner where updates are propagated all the way from the end to the beginning (this step can be called fine-tuning).

### Preprocessing and hyper-parameters

Below steps were taken to preprocess the data:

- Categorical features were one-hot encoded
- Missing days in the activity data were imputed, separate binary features were added to encode missingness (e.g. missing_heartrate = 1 if heart rate missing, otherwise 0)
- Days of the week were cyclically encoded with help of sine and cosine functions, i.e. ensuring that representation of Monday followed Sunday.
- Time windows for which a third of days are missing (e.g. 3 days missing when time window size = 9) are discarded
- Data was standardized prior to training. Mean and variance parameters of the training data were used to scale validation and test data.
- Principal Component Analysis (PCA) was applied to the baseline data where resulting features explained 0.95 of variance in the Data

Window size (# of days) was treated as a hyper-parameter and a set of values [5, 10, 15, 20, 30] were experimented with. To tackle the class imbalance in the data set (flu positive time windows constituted less than 1% of the entire data set), class weights inversely related to class frequency were used during modeling, driving the model to calculate a greater loss value for misclassifications of the minority class. Architecture of the feature extractors was also treated as a hyper parameter (CNN vs LSTM). Learned representations were of fixed size 128 in both cases (the number was obtained with hyper-parameter search).

Binary cross entropy was used as the loss function befitting binary classification and Adam was the optimizer of choice. ReLU activations were used with convolutional layers. For XGBoost, the number of estimators and maximum tree depth was found with hyper-parameter search and the results section only indicate the best performance obtained. Feature extractors were initially trained with early stopping in case validation loss (0.15 of the training data was held out, not to be confused with deployment phase data) stopped increasing for a consecutive 5 epochs. A batch size of 32 was used.

## Results

Results for different models can be found below (window size = 15, positive class is flu diagnosis):

|Model name|AUC|Precision|Recall|F-1 Score|
|---|---|---|---|---|
|Model 1 (XGBoost)|0.729|0.317|0.425|0.363|
|Model 2 (w/ CNN)|0.883|0.379|0.501|0.431|
|Model 2 (w/ LSTM)|0.902|0.406|0.526|0.458|
|Model 3 (w/ CNN)|0.939|0.441|0.553|0.491|
|Model 3 (w/ LSTM)|0.953|0.462|0.579|0.513

As seen above, model 3 which leverages both representation learning and end-to-end training performs the best among all three. Model 2 also seems to be better than model 1, highlighting the advantage of exploiting the temporal dependencies in the data, in addition to demonstrating the superiority of neural networks compared to traditional machine learning approaches for the task at hand. LSTM emerges as the marginally better performer for both model 2 and 3 in the face of CNN.

Results for different input data can be found below (model 3 with LSTM was used in all instances):

|Input data|AUC|Precision|Recall|F-1 Score|
|---|---|---|---|---|
|Baselines+Survey|0.875|0.368|0.492|0.421|
|Baselines+Survey+Activity|0.953|0.462|0.579|0.513|

Addressing the question regarding usefulness of wearable data raised earlier, these results attest that predictive performance improves considerably when model uses not only baselines and survey data but also activity data, as reaffirming the claims of previous relevant work.

Results for experiments with different window sizes can be found below (model 3 with LSTM was used in all instances):

|Window size (# of days)|AUC|Precision|Recall|F-1 Score|
|---|---|---|---|---|
|5|0.857|0.358|0.471|0.407|
|10|0.932|0.436|0.548|0.486||
|15|0.953|0.462|0.579|0.513|
|20|0.894|0.403|0.509|0.449|
|30|0.808|0.342|0.459|0.391

A window size of 15 seems to be the happy optimal, where performance degrades with increasing window sizes


## Discussion

The project essentially accomplishes what it has set out to do, mainly achieving superior performance compared to non-neural baselines and demonstrating the usefulness of wearable data, but there's certainly a lot of room for improvement. The immense class imbalance in the data has been a formidable challenge and methods other than class weighting can be employed to alleviate it. Also, a better understanding of the precision/recall trade-off, made more pressing by the imbalance, is necessary as medical applications are usually more sensitive to model behavior in that regard (e.g. higher recall for flu positivity might be desired at the expense of lower precision).

On the other hand, formulating a better proxy task with which feature extractors would trained makes for important future work. The main task itself passes as an acceptable proxy task but interactions between different features remain unexploited. Using what is called a self-supervised training scheme, some inherent relations within features can be used in the absence of labels. For example, recent work has used the self-supervised task of predicting heart rate from step counts to learn representations of activity data, which day later use predict different outcomes [[4](https://arxiv.org/pdf/2011.04601.pdf)]. A similar, or even more creative, proxy task could enrich the learned neural representations for the main task of flu prediction.

## References

[[1](https://www.thelancet.com/journals/landig/article/PIIS2589-7500(19)30222-5/fulltext)] 

[[2](https://www.nature.com/articles/s41746-020-00363-7)]

[[3](https://drive.google.com/file/d/1SEvMyBovlbPTVpPx-BZlRf3sUoQ9oKW1/view)]

[[4](https://arxiv.org/pdf/2011.04601.pdf)]
