# A/B Testing Udacity Final Project
 A/B Testing With Python - Walkthrough Project in
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1Yj3FY-f0K5gRbbE967lB-dSXpx6ueD8w?usp=sharing)

## Experiment Overview: Free Trial Screener
At the time of this experiment, Udacity courses currently have two options on the course overview page: "start free trial", and "access course materials". If the student clicks "start free trial", they will be asked to enter their credit card information, and then they will be enrolled in a free trial for the paid version of the course. After 14 days, they will automatically be charged unless they cancel first. If the student clicks "access course materials", they will be able to view the videos and take the quizzes for free, but they will not receive coaching support or a verified certificate, and they will not submit their final project for feedback.
In the experiment, Udacity tested a change where if the student clicked "start free trial", they were asked how much time they had available to devote to the course. If the student indicated 5 or more hours per week, they would be taken through the checkout process as usual. If they indicated fewer than 5 hours per week, a message would appear indicating that Udacity courses usually require a greater time commitment for successful completion, and suggesting that the student might like to access the course materials for free. At this point, the student would have the option to continue enrolling in the free trial, or access the course materials for free instead. This screenshot shows what the experiment looks like ![Experiment Screenshot](https://github.com/naiborhujosua/Udacity-Project_A-B-Testing/blob/main/Final%20Project_%20Experiment%20Screenshot.png) The unit of diversion is a cookie, although if the student enrolls in the free trial, they are tracked by user-id from that point forward. The same user-id cannot enroll in the free trial twice. For users that do not enroll, their user-id is not tracked in the experiment, even if they were signed in when they visited the course overview page.

The main goal for this experiment is to make a better experience for the students in learning subjects through an online course and improve features such as coaches support so that students will be able to complete the course. 

### Hypothesis
Before delivering a data-driven decision, We have to make a hypothesis that we can test through a few experiments with enough statistical power and how to analyze and draw a conclusion. Because we are using A/B testing, We will decide 2 hyphothesis as follows :
* <strong>Null Hypothesis(H0)</strong> : There is no difference and significant change in reducing the early udacity course cancellation after making a few changes.
* <strong>Alternative Hypothesis(H1)</strong> :There is an effect after making some changes that reduce the number of frustrated students who left the free trial due to not having enough time without reducing the the remaining students who continue past free trial and complete the course. 



## Metric Choice
We choose invariant metric and evaluation metric for A/B testing. Invariant metric is used for those who are controlled that have same number of users across the two groups(population sizing), comparable distribution across segments and same number of trials while Evaluation metric is used for expecting a change and relevant to the business goals we want to achieve. We will Dmin which marks the minimum practically significant to the business. 
 We can check the detail of the metrics in the instructions [pdf](https://github.com/naiborhujosua/Udacity-Project_A-B-Testing/blob/main/Final%20Project%20Instructions.pdf) attached in this repository. 
<div>
 * <strong>Invariant Metrics</strong> 
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Metric Name</th>
      <th>Metric Formula</th>
      <th>DMin</th>
      <th>Notation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Number of Cookies in Course Overview Page</th>
      <td>Unique daily cookies on page</td>
      <td>3000 cookies</td>
      <td>Ck</td>
    </tr>
    <tr>
      <th>Number of Clicks on Free Trial Button</th>
      <td>Unique daily cookies who clicked</td>
      <td>240 clicks</td>
      <td>Cl</td>
    </tr>
    <tr>
      <th>Click-Through-Probability</th>
      <td>Cl/Ck</td>
      <td>0.01</td>
      <td>CTP</td>
    </tr>
  
  </tbody>
</table>
</div>
<div>
* <strong>Evaluation Metrics</strong> 
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Metric Name</th>
      <th>Metric Formula</th>
      <th>Dmin</th>
      <th>Notation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Gross Conversion</th>
      <th>enrolled/Cl</th>
      <td>0.01</td>
      <td>ConversionGross</td>
    </tr>
    <tr>
      <th>Retention</th>
      <td>paid/enrolled</td>
      <td>0.01</td>
      <td>Retention</td>
    </tr>
    <tr>
      <th>Net Conversion</th>
      <td>paid/Cl</td>
      <td>0.0075</td>
      <td>ConversionNet</td>
    </tr>
  
  </tbody>
</table>
</div>


## Measuring Variability 
<div>
  * <strong>Analytical Estimate of Standard Deviation </strong> 
  (Check The Google Collab for the calculation using the baseline conversion data)
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Evaluation Metric</th>
      <th>Standard Deviation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Retention</th>
      <td>0.0549</td>
    </tr>
    <tr>
      <th>Net Conversion</th>
      <td>0.0156</td>
    </tr>
    <tr>
      <th>Gross Conversion</th>
      <td>0.0202</td>
    </tr>
  </tbody>
</table>
</div>

In order to estimate variance analytically, we can assume metrics which are probabilities(p^) are binomially distributed. This assumption is only valid when the unit of diversion of the experiment is equal to the unit of analysis (the denominator of the metric formula). In the cases when this is not valid, the actual variance might be different and it is recommended to estimate it empirically. Because the unit of analysis(denominator)  and the unit of diversion of Gross Conversion and Net Conversion is equal, Thus, We can choose these two factors variance estimation. We can not choose retention because the unit of analysis and the unit of diversion is not equal.

## Sizing 
You can check the calculation on the [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1Yj3FY-f0K5gRbbE967lB-dSXpx6ueD8w?usp=sharing) using baseline conversion data and 5000 samples/40000. Pageviews for each evaluation metric to achieve data-driven(statistical power). I use [A/B testing Calculator](https://www.evanmiller.org/ab-testing/sample-size.html) for the calculation.

Gross Conversion
* Baseline Conversion: 20.625%
* Minimum Detectable Effect: 1%
* Alpha: 5%
* Beta: 20% -Sensitivity (1 - Beta): 80%
* Sample Size = 25,835 enrollments/group
* Number of groups = 2 (experiment and control)
* Total sample size = 51,670 enrollments
* Clicks/Pageview: 3200/40000 = 0.08 clicks/pageview
* Pageviews Required = 51,670/0.08 = 645,875

Retention
* Baseline Conversion: 53%
* Minimum Detectable Effect: 1%
* Alpha: 5%
* Beta: 20%
* Sensitivity (1 - Beta): 80%
* Sample size = 39,115 enrollments/group
* Number of groups = 2 (experiment and control)
* Total sample size = 78,230 enrollments
* Enrollments/pageview: 660/40000 = 0.0165 enrollments/pageview
* Pageviews = 78,230/0.0165 = 4,741,212

Net Conversion
* Baseline Conversion: 10.9313%
* Minimum Detectable Effect: 0.75%
* Alpha: 5% -Beta: 20%
* Sensitivity (1 - Beta): 80%
* Sample size = 27,413 enrollments/group
* Number of groups = 2 (experiment and control)
* Total sample size = 54,826 enrollments
* Enrollments/pageview: 3200/40000 = 0.08 clicks/pageview
* Pageviews = 54,826/0.08 = 685,325

## Duration and Exposure 
Based off the calculation on the 100% diversion of traffic given 40,000 page views per day, it will take ~ 119 for this trials. Thus, it is risky to do the experiments during 119 days because it will make higher frustrated students, lower conversion and retentiion and the inneficient use of coaching support. In terms of timing, It is more likely to try ~18 days experiment with 100 diversion and -35 days given 50% diversion for Gross Conversion and Net Conversion.


## Analysis 
* [Experiment Group](https://github.com/naiborhujosua/Udacity-Project_A-B-Testing/blob/main/Results_%20Experiment.csv)
* [Control Group](https://github.com/naiborhujosua/Udacity-Project_A-B-Testing/blob/main/Results%20_Control.csv)

## Sanity Checks
For the invariant metrics we assume equal diversion into experiment and control group. We will use 95% Confidence Interval to calculate the lower and upper bound as follows :

 Check [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1Yj3FY-f0K5gRbbE967lB-dSXpx6ueD8w?usp=sharing) for the calculation using the [baseline conversion data](https://github.com/naiborhujosua/Udacity-Project_A-B-Testing/blob/main/baseline_values.csv)
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Metric</th>
      <th>Expected Value</th>
      <th>Observed Value</th>
      <th>CI Lower Bound</th>
      <th>CI Upper Bound</th>
      <th>Result</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Number of clicks on "start free trial"</th>
      <td>0.5000</td>
      <td>0.5006</td>
      <td>0.4988</td>
      <th>0.5012</th>
      <td>Pass</td>
    </tr>
    <tr>
      <th>Number of Cookies</th>
      <td>0.5005</td>
      <td>0.5005</td>
      <th>0.4959</th>
      <td>0.5042</td>
      <td>Pass</td>
    </tr>
    <tr>
      <th>Click-through-probability</th>
      <td>0.0821</td>
      <th>0.0822</th>
      <td>0.0812</td>
      <td>0.0830</td>
      <td>Pass</td>
    </tr>
  </tbody>
</table>
</div>


## Check For Practical and Satistical Significance
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Metric</th>
      <th>Dmin</th>
      <th>Observed Difference</th>
      <th>CI Lower Bound</th>
      <th>CI Upper Bound</th>
      <th>Result</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Gross Conversion</th>
      <td>0.01</td>
      <td>-0.0205</td>
      <td>-0.0291</td>
      <th>-0.0120</th>
      <td>Statistically and Practically Significant</td>
    </tr>
    <tr>
      <th>Number of Cookies</th>
      <td>0.0075</td>
      <td>-0.0048</td>
      <th>-0.0116</th>
      <td>0.0019</td>
      <td>Neither Statistically nor Practically Significant</td>
    </tr>
    </tr>
  </tbody>
</table>
</div>

## Sign Tests Analysis
 Check [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1Yj3FY-f0K5gRbbE967lB-dSXpx6ueD8w?usp=sharing) for the calculation of <strong>P-value</strong>
<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th>Metric</th>
      <th>p-value for sign test</th>
      <th>Statistically Significant(alpha=5%)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Gross Conversion</th>
      <td>0.0026</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>Net Conversion</th>
      <td>0.6776</td>
      <td>No</td>
    </tr>
    </tr>
  </tbody>
</table>
</div>

## Make a Recommendation

The experiment were divided into control and experiment group where potential udacity students were diverted by cookie. The students were asked either to choose access free trial where they will be asked to enter credit card information and then they will be enrolled in a free trial for the paid version. After 14 days of free trial they will be automatically be charged unless they cancel first before the free tral ends or acess course material where they will be able to acess videos and take the quizzes for free but they will not receive coaching support or verified certificate and tehy will not submit final project for feedbacks. Based on this condition, we chose invarian metrics such as  number of cookies, number of clicks and click throuh probability and evaluation metrics such as gross conversion and net conversion because of the unit of diversion and unit of analysis is equal assuming the binomial distribution between these two metrics. Then, We set the null and alternative hypthosis in the first place and make an experiment if there is an effect (alternative hypothesis) or no effect(null hypothesis) by measuring them using statistical power such as statistical significant and practical significant threshold for each evaluation metrics. Analysis revealed that the expected equal distribution of cookies into control and experiment for each invariant metrics y 95 confidence interval depicted a difference in gross conversion at 95% CI where the p-value is smaller than the alpha(significant value) so that we can reject the null hypothesis. The gross conversion also satisfy the practical significant threshold through Dmin which is given for evaluation metrics. Nevertheles, we found that Conversion rate did not meet the statitically significant significant and practical thresholdd. Based off these considerations, that a statistically and practically significantdecresease in Gross Conversion was observed but with no significant in Net Conversion. This indicated that a decrease in enrollment not coupled to an increase instudents who are staying for the 14 days to trigger for payment. Based on this analysis, i reccoment not to launch the product and dig deeper to pursue other experiments. 







