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


## Results

How did you evaluate your approach? How well did you do? What are you comparing to? Maybe you want ablation studies or comparisons of different methods.

You may want some qualitative results and quantitative results. Example images/text/whatever are good. Charts are also good. Maybe loss curves or AUC charts. Whatever makes sense for your evaluation.

## Discussion

You can talk about your results and the stuff you've learned here if you want. Or discuss other things. Really whatever you want, it's your project.
