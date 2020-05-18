# Identifying COVID-19 through Deep Learning


<img src='images\corona_gif.gif' width="750" align="center">

## Introduction

##### **Disclaimer**

This work is meant to fulfill educational purposes. The hypothetical business case, and results of modeling are not meant to be considered as medical advise, and has not been approved by any professional or medical body.

As of May 4, 2020 COVID-19 has had devastating impacts on the world. To date, there are over 3.5 million confirmed cases in roughly a 5 month window of time. While there are several factors that have led to these disheartening statistics, one key element in this proliferation is travel. Due to the often asymptomatic nature of COVID-19 infections, infected individuals have been able to unknowingly spread the virus to over 200 countries and territories. 
In order to 'flatten the curve' my group would like to **propose a systematic approach to implementing non-invasive testing at key international airports that would identify and quarantine would be infected travelers**. Our proposal is simple, through effective risk mapping and assessment driven by data, high risk travelers' lungs are scanned with computed tomography (CT), and the resulting **x-ray images are assessed with a Convolutional Neural Network trained on detecting COVID-19**. Those travelers identified by the neural network as infected would then be further tested by medical professionals before being approved for travel. **This project focuses on the creation and optimization of this COVID-19 detecting CNN**.

## Objectives

There are two main objectives for this portion of the aforementioned proposal, which are as follows:
 1. Generate a Convolutional Neural Network (CNN) which detects the presence of COVID-19 in CT images with a **high** accuracy.
 2. Reveal potential indicators of COVID-19 through analysis of said CNN.

## Metrics for Evaluation

Our task is essentially a binary classification problem, where an x-ray image is considered input, and **our model produces an prediction of whether this image is of a COVID-19 infected lung, or is not**. With these objectives and framework in mind, the threshold of success mainly rests on the accuracy of our model. We will set the goal of our model attaining **at least an 80% accuracy**. With accuracy here being defined as:

$$\text{Accuracy} = \frac{\text{TP+TN}}{{\text{TP+TN+FP+FN}}$$

Then our model will be successful where:

$$\text{Accuracy} >= .80$$


In the case where our model predicts incorrectly, we **prefer a false positive more than a false negative**. In the case of a false positive, a traveler is determined as infected where they are not. In this case that person would be held for further testing, potentially missing their flight. However **in the case of a false negative, our model has determined that an image of lungs that are COVID-19 infected are not, and thus that traveler would continue on potentially infecting others**. Therefore we aim for a model that meets the most recent standards as defined by the FDA for Covid-19 tests:

"Tests should accurately identify at least 90% of positive cases (what epidemiologists call sensitivity) and 95% of negative cases (specificity)."

For our model's evaluation this is further defined as:


$$\text{Precision} = \frac{\text{Number of True Positives}}{\text{Number of Predicted Positives}}$$

  

$$\text{Recall} = \frac{\text{Number of True Positives}}{\text{Number of Actual Total Positives}}$$

Our model will achieve:

$$\text{Recall} >= .90$$


## The Dataset

The dataset used for our work consists of xray images of lungs tested negative and positive for Covid-19 from the IBM 2020 Call for Code Global Challenge, and had graciously been preprocessed by Casper Hansen, who's work can be found here: https://developer.ibm.com/articles/using-deep-learning-to-take-on-covid-19/ . The data itself was biased with 98.9% of the images being of covid-19 negative cases, and so the normal dataset was shortened to the first 99 instances:

<img src='images\dist.PNG'>

These images have unique pixel intensities and shapes related to their specific case:

<img src='images\tensorboard_img\pos_ex.png'>



### Modeling

After modeling with various CNN's the most promising involved transfer learning with VGG19, a 19 layer convolutional neural network created by the Oxford Visual Geometry Group for the ImageNet Large Scale Visual Recognition Challenge ILSVRC-2014 competition trained on 4 GPUs for 2–3 weeks.

Transfer learning as defined by Goodfellow et al in Deep Learning is a "Situation where what has been learned in one setting is exploited to improve generalization in another setting". By this definition, we used the weights gained from the Imagenet dataset, and the architecture of VGG19 to generalize for the covid-19 dataset:

<img src='images\tensorboard_img\full_vgg19.png'>

### Performance

Our performance was able to meet standards set by the FDA for approved Covid-19 testing for recall, and met our goal for accuracy:

<img src='images\tensorboard_img\vgg_success_matrix.PNG'>
          
<img src='images\tensorboard_img\vgg_success_report.PNG'>

### Visulizations

When visualizing the activation layers of the CNN, it was clear that noise coming from outside of the body space within the images was influencing the network's predictions. It appears more common that images of positive cases contained some labeling format not present in the negative cases, and therefore could allow the model to 'game' the assessment. Observe the top left corner in these images:

<img src='images\tensorboard_img\layer_images.png'>

### Conclusion

In conclusion, while the goal of creating visualizations that may be of use to the medical research community was achieved, we were also able to meet the FDA standards for Covid-19 testing. With such a small dataset to work with, there is most likely strong sampling bias. Our covid-19 samples are so few, that they are most likely from the same general location, and most certainly from the same country. With covid-19 being a pandemic, to model accurately we would need at least several hundred more cases to observe.

Until the time that increasing the dataset is possible, it will be most prudent to work with less complex models than Inception, which was not able to identify the covid-19 positive cases at all.