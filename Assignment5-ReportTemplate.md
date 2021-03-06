<table>
  <tr>
   <td><strong>Group # : 07</strong>
   </td>
   <td><strong>Student Names</strong>
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>Graydon Benson
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>Eli St. James
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>Maxwell Botham
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>Seher Dawar
   </td>
  </tr>
</table>



# Introduction

This lab was an enlightening foray into analysis of integration test data using reliability assessment tools. We used two ways to assess failure data: 

1. Reliability growth testing
2. Reliability assessment using Reliability Demonstration Chart (RDC)

By modifying the given failure data, we were able to come up with models and plots that are useful tools in calculating the MTTF, failure intensity and reliability of the software under test. 

Each section of the lab increased the group’s understanding of reliability analysis and measurement and how reliability certification can be used to determine the quality of a software. Finally, it helped us understand when testing is required for a software in a development process.


# Reliability Growth Testing


## Result of model comparison (selecting top two models)

We initially started by testing each type of model in C-SFRAT with the covariates of E, F, and C. We selected the ‘top model’ using a combination of the critic mean and median, as well as the highest value of logarithmic likelihoods. We were aiming to select a model with the critic mean and median of 1, or as close to 1 as possible. We ran the simulation with equal metric weights. After careful analysis, we selected the models DW3 on F, and IFRGSB on E and F, as they both had mean, as well as median critics very close to 1, and the two highest log-likelihoods.


## Plots for failure rate and reliability of the SUT for the test data provided  
![unnamed](https://user-images.githubusercontent.com/62816561/162062867-fd8e7dd6-5d8d-48fd-83b0-ce423c0e2ae9.png)
![unnamed (1)](https://user-images.githubusercontent.com/62816561/162062893-45276e71-d47e-460d-8f7e-dff1a72d2ae6.png)
![unnamed (2)](https://user-images.githubusercontent.com/62816561/162062903-daf014e4-9a54-4c24-a555-e66394bcc05f.png)

This graph alone wasn’t enough for us to determine reliability growth. We have to use the predictive tool.

![unnamed (3)](https://user-images.githubusercontent.com/62816561/162063043-6ed877ac-ed71-4e61-a152-29197bca2c9e.png)

Adding more predictive intervals allowed us to evaluate whether failures were plateauing (becoming less and less). 

![unnamed (4)](https://user-images.githubusercontent.com/62816561/162063085-af170f1d-a5eb-4643-9830-eac920391f98.png)

Here it is again with 99 intervals predicted. 

![unnamed (5)](https://user-images.githubusercontent.com/62816561/162063109-9a63582c-6cd2-45d2-b5ca-4cde9e3a9afd.png)

## A discussion on decision making given a target failure rate 

The target failure of 0.5 (as shown in our graphs) makes the system reasonable to accept. When we tested at rates of 1.0 or higher, the failure intensity rate became too large to accept from the prediction. At the end of the testing interval the graph begins to plateau (after 31 intervals). This shows the user that software is becoming more stable, as there are less failures as time goes on. We are unsure of the reason this is happening, but assume that there is most likely an active development team tracking and terminating bugs, then redeploying the software.


## A discussion on the advantages and disadvantages of reliability growth analysis

Reliability growth analysis excels in estimating and predicting the improvement of system reliability as a function of the amount of system testing that is carried out. It cannot, however, conclude on its own when the failure rate is sufficient to accept the software. We can use RGA to help determine if software is ready given appropriate data and targets. 


# RDC Testing

**3 plots for MTTFmin, twice and half of it for your test data**

![unnamed (11)](https://user-images.githubusercontent.com/62816561/162063336-e0cc62bb-98f2-4e24-b693-a8e4da33db16.png)

## Explain your evaluation and justification of how you decide the MTTFmin

There were a few steps we took to find MTTFmin for the test data provided. Firstly, we mapped the data given (failures per time interval, 31 lines), into the time interval between failures, 92 lines. This new form of failure data allowed us to input it to SRTAT for computation and graphing. 

Secondly, we calculated the average failures per second (assuming that each ‘time unit’ in the original dataset was 1 second) and ended up with the value of 2.967 failures per second. Then we changed the values for the Discrimination Ratio (γ), Consumer Risk (β) and Developer Risk (α) to try out a few what-if scenarios for our SUT. We tried these scenarios:



1. Discrimination Ratio = 2, Consumer Risk = 0.1, Developer Risk = 0.1

![unnamed (6)](https://user-images.githubusercontent.com/62816561/162063377-fcb30de2-dca1-45b4-b675-7afb53067807.png)


2. Discrimination Ratio = 1.2, Consumer Risk = 0.1, Developer Risk = 0.1

![unnamed (7)](https://user-images.githubusercontent.com/62816561/162063403-d4da0188-a92b-4892-aaa4-d8755d6de58b.png)


3. Discrimination Ratio = 2, Consumer Risk = 0.3, Developer Risk = 0.2

![unnamed (8)](https://user-images.githubusercontent.com/62816561/162063418-bbb0e532-8451-41f7-aab7-9b1243d4f912.png)


4. Discrimination Ratio = 2, Consumer Risk = 0.2, Developer Risk = 0.5

![unnamed (9)](https://user-images.githubusercontent.com/62816561/162063434-1e909a28-d1b7-4163-8eb9-54ed45c6c032.png)


Finally, we used these values Discrimination Ratio = 2, Consumer Risk = 0.44, Developer Risk = 0.1 to come up with our MTTFmin = 1.4835. We decided that the MTTFmin will be the value where the blue failure data point crosses into the Accept region. 

![unnamed (10)](https://user-images.githubusercontent.com/62816561/162063449-9101945a-7519-4274-987f-c88ab6d0b9a0.png)

One thing to note is that we were confused why SRTAT only provided us with one ‘Failure Data’ point. We tried inputting our data in multiple ways, including ‘Time Between Failures’ (final), and ‘Failure Count’. No matter which way we mapped and inputted the data to SRTAT, we were only able to produce one blue failure point. We would also like to note that we tried to use the RDC excel sheet provided, to no avail. We weren’t able to produce the correct amount of data points in the RDC demo chart, even after excessive reading of the user guides (both the PDF provided and the section of the sheet called ‘Mode D’emploi’.)


## A discussion on the advantages and disadvantages of RDC

**Advantages:**



* RDC analysis is a very versatile, time and cost efficient way of analyzing the reliability of a system.
* Experimenting with different values of confidence levels and MTTF (what-if scenarios) is doable

**Disadvantages**



* A disadvantage of the RDC is that it cannot be used to calculate the exact quantitative value for the reliability (or availability) of the system under study
* RDC can only indicate that the SUT is acceptable or not


# Comparison of Results

In reliability growth analysis, we determined that the software is acceptable at failure intensity of** 0.5** and below, whereas RDC shows that the software is acceptable at **1.4385.** While both results are different, they indicate the software can be successfully accepted.


# Discussion on Similarity and Differences of the Two Techniques

**Similarity**



* Reliability Growth Analysis and Reliability Demonstration Chart are both system reliability techniques that allow important decisions to be made about the SUT and its testing. 
* Both Reliability Growth Analysis and Reliability Demonstration Chart are based upon inter failure times and target failure rate or MTTF.

**Differences**



* Reliability Growth Analysis is also based on failure count while the Reliability Demonstration Chart is not. 
* Reliability Growth Analysis uses basic exponential and logarithmic Poisson models to estimate λ\λF ratios and see the trend in failure intensity. On the other hand Reliability Demonstration Chart uses Discrimination Ratio (γ), Consumer Risk (β) and Developer Risk (α) to determine the acceptibnility of the SUT.


# How the team work/effort was divided and managed?

The assignment was divided into two parts - Reliability Growth Analysis and Reliability Demonstration Chart. We split up into pairs and each pair covered one part. Then we explained our results to the other pair and rechecked each other’s results. Rechecking each other's results gave us a lot of insight that made writing the discussions easier. 


# Difficulties encountered, challenges overcome, and lessons learned

The assignment was interesting but we had a few challenges along the way. It was quite hard to figure out how to make the RDC Excel sheet take in more than 16 values. Additionally, modifying the data to fit the tools was quite unintuitive and we ended up spending many hours trying to get everything to run. Finally, the SRTAT RDC only showed one failure data point and we had to make assumptions to calculate the MTTFmin. For the Reliability Growth Analysis, CSFRAT was quite slow and doing the MTTF/MTBF calculation was quite complicated.


# Comments/feedback on the lab itself

While the lab was quite useful to our understanding of reliability analysis, we wish the data was better formatted and suited to the software we were running it on. Additionally, we would have liked the SRTAT software to come with a manual. Finally, it would have been great if the CASRE suite ran . 
