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

|POS|avg score|avg length|W|E|
|---|---|---|---|---|
|adj|0.39|5.7|0.45|0.16|
|noun|0.30|10.1|0.52|0.10|
|adv|0.32|7.2|0.60|0.20|
|verb|0.31|5.9|0.58|0.17

# Example Project

Write a 3-4 sentence abstract. It should introduce the problem and your approach. You may also want some numbers like 35 mAP and 78% accuracy. You can use this example README for your project, you can use a different ordering for your website, or you can make a totally different website altogether!

VIDEO GOES HERE (probably): Record a 2-3 minute long video presenting your work. One option - take all your figures/example images/charts that you made for your website and put them in a slide deck, then record the video over zoom or some other recording platform (screen record using Quicktime on Mac OS works well). The video doesn't have to be particularly well produced or anything.

## Introduction

In a year of global pandemic, data driven to methods for prevention of infectious diseases have been under the spotlight. In this context, 'track & trace' technologies have justifiably received a lot of attention as they promise to contain outbreaks by tracing chains of infections to identify spreaders and individuals under risk. Taking a step further, accurate predictive models would be able to spot and help isolate individuals who are infected without a clinical diagnosis, hence stopping chains of infections right on their track even before they start to accelerate.

Accordingly, this project was motivated by the task of making daily predictions regarding influenza onset for a cohort of flu study participants using a mixture of demographic data, medical history, daily surveys and activity data derived from Fitbit smart watches. As will be described in sections below, this involved a) defining a realistic and meaningful prediction task which would mimic the conditions under which a real-time flu prediction framework could potentially operate, b) comparing how traditional machine learning methods performed against neural network based approaches for the task at hand and c) effect of different hyper parameters and also different data modalities to the overall performance. 

## Related Work

Data derived from wearable devices, especially markers such as heart rate, sleep status and step count, has recently gained more traction as potential predictors of influenza like illness (ILI) onset. On a population level, weekly variations in wearable data were shown to correspond to ILI rates for the same population [[1](https://www.thelancet.com/journals/landig/article/PIIS2589-7500(19)30222-5/fulltext)]. Although largely correlational and lacking the individual level resolution that would deliver actionable predictions, this work demonstrates the potential of wearable data for influenza surveillance. Similar work focusing on individual level predictions for COVID-19 symptoms and COVID-19 related hospitalizations has leveraged neural networks to model wearable device data and demonstrated that heart rate, as well as respiration rate, as measured by Fitbit devices were elevated with disease onset[2]. Another very recent paper using wearable data for ILI symptom prediction, involving both influenza and COVID-19, employed both neural nets and traditional machine learning modeling but most importantly highlighted the importance of defining a realistic learning task and setting by demonstrating the difference in performance between retrospective and prospective training schemes [3].

Inspired by such previous work, this project seeks to validate the usefulness of wearable data, as measured by the performance boost it provides for prediction of ILI related outcomes , while maintaining a realistic, and potentially deployable, modeling framework.

## Approach

How did you decide to solve the problem? What network architecture did you use? What data? Lots of details here about all the things you did. This section describes almost your whole project.

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

During the observation phase, which include all data for the first t weeks of the study period, the models were first trained on randomly cropped time windows, the aforementioned n day windows. Subsequently, the deployment phase includes all data beyond the first t weeks of the study period and models make daily predictions while progressing through this phase. In other words, the model first predicts a flu positivity on day d, then uses that day to train further before repeating the same procedure with day d+1 and so on, following a 'train and advance' approach. In this way, future data is never made available to the model similar to a realistic setting where a deployed model could only use previous days' data. The timeline used to split data set into observation and deployment, i.e. train, validation and test, is illustrated below:

## Results

How did you evaluate your approach? How well did you do? What are you comparing to? Maybe you want ablation studies or comparisons of different methods.

You may want some qualitative results and quantitative results. Example images/text/whatever are good. Charts are also good. Maybe loss curves or AUC charts. Whatever makes sense for your evaluation.

## Discussion

You can talk about your results and the stuff you've learned here if you want. Or discuss other things. Really whatever you want, it's your project.
