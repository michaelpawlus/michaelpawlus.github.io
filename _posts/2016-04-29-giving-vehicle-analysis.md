---
layout: post
title: Giving Vehicle Analysis
author: Michael Pawlus
published: true
status: publish
draft: false
---
 
There was an interesting question on PRSPCT-L a few weeks ago:
 
> Do you know of or have experience with demographic modeling and data appends that help predict a donor’s preferred giving method (i.e., event, direct mail, online, etc.)? 
 
I thought I would take a minute to look into this.  I actually couldn't initially think of which variables to include aside from age.  My starting point was looking at those that made gift by check through the mail, online, over the phone or in person during this fiscal year.  I was evaluating the data at the household level so gifts from married households only count once.
 
I decided to not use prior giving at all though I think that is likely the best predictor.  That is, those that gave online last year will likely do so again this year.  I also think other giving stats like length of giving, average amount and total amount would also be helpful in making predictions.
 
However, my interpretation of the original question was that this should just be an analysis of demographic data and so that is the path that I took.
 
I eventually choose:
 
* age
* constituent type: company, individual, etc. 
* spouse constituent type: for those married (with an 'N' for those not married)
* a variable called type2 which is maybe not the greatest variable ever
    + type2 = 1 means married
    + type2 = 2 means not married and constituent is male
    + type2 = 3 means not married and constituent is female
    + type2 = 4 means company
    + type2 = 5 means foundation
    + type2 = 6 means missing information or other
* phone: do they have a phone number in our system or not
* email: do they have an email in our system or not
* mail: do they have a good mailing address in our system or not
* assigned: is this constituent assigned to a gift officer
* I added a 1 or 0 if they had any of the following no contact flags:
    + no contact at all
    + no contact by phone
    + no contact by email
    + no contact by mail
    + no personal contact
    + no solicitations
* I also added a code for geographic distance from the university:
    + 1 = in our tri-county area
    + 2 = in our state
    + 3 = everyone else
* Lastly, I added a count of the number of solicitations they received by type
 
My approach to problems like this is to get a decent sized set of data points together based on intuition like the ones described and then create a really quick model.  I will skip the modelling code here for now but it is in my [fundraising analytics](https://github.com/michaelpawlus/fundraising_analytics) folder along with the data set that I used.
 
This next part is just the report out from the model.  I cannot seem to hide it so just scroll through this to get to the plots.
 

{% highlight text %}
## [0]	val-mlogloss:1.573021	train-mlogloss:1.566801
## [1]	val-mlogloss:1.542564	train-mlogloss:1.529456
## [2]	val-mlogloss:1.518447	train-mlogloss:1.496815
## [3]	val-mlogloss:1.488326	train-mlogloss:1.461229
## [4]	val-mlogloss:1.462185	train-mlogloss:1.429590
## [5]	val-mlogloss:1.442493	train-mlogloss:1.403462
## [6]	val-mlogloss:1.418824	train-mlogloss:1.374059
## [7]	val-mlogloss:1.396360	train-mlogloss:1.346390
## [8]	val-mlogloss:1.376456	train-mlogloss:1.319052
## [9]	val-mlogloss:1.354986	train-mlogloss:1.293203
## [10]	val-mlogloss:1.337133	train-mlogloss:1.267126
## [11]	val-mlogloss:1.319295	train-mlogloss:1.243102
## [12]	val-mlogloss:1.306464	train-mlogloss:1.221627
## [13]	val-mlogloss:1.293017	train-mlogloss:1.201316
## [14]	val-mlogloss:1.276974	train-mlogloss:1.178625
## [15]	val-mlogloss:1.265909	train-mlogloss:1.160488
## [16]	val-mlogloss:1.254487	train-mlogloss:1.143027
## [17]	val-mlogloss:1.242049	train-mlogloss:1.124598
## [18]	val-mlogloss:1.229030	train-mlogloss:1.105606
## [19]	val-mlogloss:1.219191	train-mlogloss:1.089167
## [20]	val-mlogloss:1.207613	train-mlogloss:1.073157
## [21]	val-mlogloss:1.199207	train-mlogloss:1.059713
## [22]	val-mlogloss:1.191642	train-mlogloss:1.047457
## [23]	val-mlogloss:1.181021	train-mlogloss:1.032885
## [24]	val-mlogloss:1.171896	train-mlogloss:1.017604
## [25]	val-mlogloss:1.165301	train-mlogloss:1.005522
## [26]	val-mlogloss:1.158227	train-mlogloss:0.992049
## [27]	val-mlogloss:1.149824	train-mlogloss:0.978554
## [28]	val-mlogloss:1.143039	train-mlogloss:0.968725
## [29]	val-mlogloss:1.138274	train-mlogloss:0.958425
## [30]	val-mlogloss:1.130911	train-mlogloss:0.946467
## [31]	val-mlogloss:1.125098	train-mlogloss:0.935771
## [32]	val-mlogloss:1.119237	train-mlogloss:0.924547
## [33]	val-mlogloss:1.113337	train-mlogloss:0.914362
## [34]	val-mlogloss:1.108516	train-mlogloss:0.904058
## [35]	val-mlogloss:1.102971	train-mlogloss:0.894525
## [36]	val-mlogloss:1.097851	train-mlogloss:0.885303
## [37]	val-mlogloss:1.094775	train-mlogloss:0.878390
## [38]	val-mlogloss:1.089699	train-mlogloss:0.868678
## [39]	val-mlogloss:1.085921	train-mlogloss:0.861370
## [40]	val-mlogloss:1.082001	train-mlogloss:0.853485
## [41]	val-mlogloss:1.079273	train-mlogloss:0.845864
## [42]	val-mlogloss:1.075192	train-mlogloss:0.838520
## [43]	val-mlogloss:1.072200	train-mlogloss:0.831788
## [44]	val-mlogloss:1.070244	train-mlogloss:0.824490
## [45]	val-mlogloss:1.066154	train-mlogloss:0.816739
## [46]	val-mlogloss:1.063598	train-mlogloss:0.809148
## [47]	val-mlogloss:1.060212	train-mlogloss:0.800574
## [48]	val-mlogloss:1.057579	train-mlogloss:0.793868
## [49]	val-mlogloss:1.054171	train-mlogloss:0.787677
## [50]	val-mlogloss:1.052791	train-mlogloss:0.781321
## [51]	val-mlogloss:1.050228	train-mlogloss:0.774556
## [52]	val-mlogloss:1.048807	train-mlogloss:0.768752
## [53]	val-mlogloss:1.046279	train-mlogloss:0.762640
## [54]	val-mlogloss:1.044428	train-mlogloss:0.757771
## [55]	val-mlogloss:1.041930	train-mlogloss:0.752710
## [56]	val-mlogloss:1.039998	train-mlogloss:0.748384
## [57]	val-mlogloss:1.038791	train-mlogloss:0.742690
## [58]	val-mlogloss:1.038264	train-mlogloss:0.738483
## [59]	val-mlogloss:1.036402	train-mlogloss:0.732569
## [60]	val-mlogloss:1.034444	train-mlogloss:0.727580
## [61]	val-mlogloss:1.032156	train-mlogloss:0.722772
## [62]	val-mlogloss:1.030492	train-mlogloss:0.718089
## [63]	val-mlogloss:1.029601	train-mlogloss:0.713212
## [64]	val-mlogloss:1.028225	train-mlogloss:0.708559
## [65]	val-mlogloss:1.026907	train-mlogloss:0.703856
## [66]	val-mlogloss:1.025500	train-mlogloss:0.699402
## [67]	val-mlogloss:1.024090	train-mlogloss:0.695040
## [68]	val-mlogloss:1.023212	train-mlogloss:0.690821
## [69]	val-mlogloss:1.022427	train-mlogloss:0.687205
## [70]	val-mlogloss:1.021268	train-mlogloss:0.683740
## [71]	val-mlogloss:1.020453	train-mlogloss:0.679436
## [72]	val-mlogloss:1.020049	train-mlogloss:0.675814
## [73]	val-mlogloss:1.019447	train-mlogloss:0.672031
## [74]	val-mlogloss:1.019127	train-mlogloss:0.668735
## [75]	val-mlogloss:1.019105	train-mlogloss:0.664999
## [76]	val-mlogloss:1.018330	train-mlogloss:0.661002
## [77]	val-mlogloss:1.017969	train-mlogloss:0.657266
## [78]	val-mlogloss:1.016831	train-mlogloss:0.654118
## [79]	val-mlogloss:1.017005	train-mlogloss:0.650460
## [80]	val-mlogloss:1.015969	train-mlogloss:0.646964
## [81]	val-mlogloss:1.016083	train-mlogloss:0.643579
## [82]	val-mlogloss:1.016130	train-mlogloss:0.640616
## [83]	val-mlogloss:1.014962	train-mlogloss:0.637862
## [84]	val-mlogloss:1.014912	train-mlogloss:0.635358
## [85]	val-mlogloss:1.015668	train-mlogloss:0.632128
## [86]	val-mlogloss:1.015334	train-mlogloss:0.628814
## [87]	val-mlogloss:1.014151	train-mlogloss:0.625450
## [88]	val-mlogloss:1.014392	train-mlogloss:0.622806
## [89]	val-mlogloss:1.014351	train-mlogloss:0.619587
## [90]	val-mlogloss:1.014730	train-mlogloss:0.617160
## [91]	val-mlogloss:1.014991	train-mlogloss:0.614247
## [92]	val-mlogloss:1.015112	train-mlogloss:0.611346
## [93]	val-mlogloss:1.014643	train-mlogloss:0.608458
## [94]	val-mlogloss:1.015371	train-mlogloss:0.606079
## [95]	val-mlogloss:1.015929	train-mlogloss:0.602996
## [96]	val-mlogloss:1.015972	train-mlogloss:0.600237
## [97]	val-mlogloss:1.015238	train-mlogloss:0.597016
## [98]	val-mlogloss:1.015074	train-mlogloss:0.594226
## [99]	val-mlogloss:1.014812	train-mlogloss:0.590992
## [100]	val-mlogloss:1.014548	train-mlogloss:0.588286
## [101]	val-mlogloss:1.014648	train-mlogloss:0.585326
## [102]	val-mlogloss:1.014786	train-mlogloss:0.582756
## [103]	val-mlogloss:1.014340	train-mlogloss:0.580652
## [104]	val-mlogloss:1.014005	train-mlogloss:0.578498
## [105]	val-mlogloss:1.013833	train-mlogloss:0.575397
## [106]	val-mlogloss:1.013753	train-mlogloss:0.572872
## [107]	val-mlogloss:1.014162	train-mlogloss:0.569962
## [108]	val-mlogloss:1.013819	train-mlogloss:0.567636
## [109]	val-mlogloss:1.014174	train-mlogloss:0.565752
## [110]	val-mlogloss:1.013492	train-mlogloss:0.563507
## [111]	val-mlogloss:1.013502	train-mlogloss:0.561280
## [112]	val-mlogloss:1.014239	train-mlogloss:0.559229
## [113]	val-mlogloss:1.014788	train-mlogloss:0.556708
## [114]	val-mlogloss:1.014760	train-mlogloss:0.554808
## [115]	val-mlogloss:1.014873	train-mlogloss:0.552493
## [116]	val-mlogloss:1.015329	train-mlogloss:0.550759
## [117]	val-mlogloss:1.015159	train-mlogloss:0.548503
## [118]	val-mlogloss:1.015371	train-mlogloss:0.545967
## [119]	val-mlogloss:1.016084	train-mlogloss:0.543564
## [120]	val-mlogloss:1.016084	train-mlogloss:0.541570
## [121]	val-mlogloss:1.016467	train-mlogloss:0.539090
## [122]	val-mlogloss:1.017337	train-mlogloss:0.537389
## [123]	val-mlogloss:1.017493	train-mlogloss:0.535167
## [124]	val-mlogloss:1.018021	train-mlogloss:0.533276
## [125]	val-mlogloss:1.018554	train-mlogloss:0.532195
## [126]	val-mlogloss:1.019542	train-mlogloss:0.530230
## [127]	val-mlogloss:1.019874	train-mlogloss:0.527978
## [128]	val-mlogloss:1.020485	train-mlogloss:0.526104
## [129]	val-mlogloss:1.021216	train-mlogloss:0.524623
## [130]	val-mlogloss:1.021609	train-mlogloss:0.522807
## [131]	val-mlogloss:1.021855	train-mlogloss:0.520978
## [132]	val-mlogloss:1.021801	train-mlogloss:0.519111
## [133]	val-mlogloss:1.022010	train-mlogloss:0.517586
## [134]	val-mlogloss:1.022735	train-mlogloss:0.515925
## [135]	val-mlogloss:1.022904	train-mlogloss:0.514064
## [136]	val-mlogloss:1.023072	train-mlogloss:0.512430
## [137]	val-mlogloss:1.023158	train-mlogloss:0.510579
## [138]	val-mlogloss:1.023666	train-mlogloss:0.508805
## [139]	val-mlogloss:1.023804	train-mlogloss:0.507285
## [140]	val-mlogloss:1.024949	train-mlogloss:0.505673
## [141]	val-mlogloss:1.025144	train-mlogloss:0.504010
## [142]	val-mlogloss:1.025611	train-mlogloss:0.502415
## [143]	val-mlogloss:1.026423	train-mlogloss:0.500580
## [144]	val-mlogloss:1.026997	train-mlogloss:0.499119
## [145]	val-mlogloss:1.027250	train-mlogloss:0.496865
## [146]	val-mlogloss:1.028180	train-mlogloss:0.495279
## [147]	val-mlogloss:1.028278	train-mlogloss:0.493426
## [148]	val-mlogloss:1.028553	train-mlogloss:0.491444
## [149]	val-mlogloss:1.028579	train-mlogloss:0.490144
## [150]	val-mlogloss:1.029164	train-mlogloss:0.488235
## [151]	val-mlogloss:1.030640	train-mlogloss:0.486696
## [152]	val-mlogloss:1.030533	train-mlogloss:0.485393
## [153]	val-mlogloss:1.031575	train-mlogloss:0.484088
## [154]	val-mlogloss:1.032483	train-mlogloss:0.482766
## [155]	val-mlogloss:1.033470	train-mlogloss:0.481424
## [156]	val-mlogloss:1.034227	train-mlogloss:0.479877
## [157]	val-mlogloss:1.034673	train-mlogloss:0.478428
## [158]	val-mlogloss:1.035489	train-mlogloss:0.476811
## [159]	val-mlogloss:1.035815	train-mlogloss:0.475326
## [160]	val-mlogloss:1.036737	train-mlogloss:0.473603
## Stopping. Best iteration: 111
{% endhighlight %}
 
Once the model is done, I like to look at the importance of the features included:
 
![plot of chunk unnamed-chunk-2](/figures/unnamed-chunk-2-1.png)
 
We can see that the various solicitation counts and age seem to have the biggest impact so we can start there.
 

 
I should note that at present this model is not performing too well but it is just a quick one for now.  Here is the logloss score which is a measure of error.  The closer to 0 the better.
 

{% highlight text %}
## [1] 23.16573
{% endhighlight %}
 
We are a long way off.
 
Here is a quick break down for each group.  What the model does is assign a probability for each id as to which giving method they used so each row should sum to 1.  I labeled the first column as intercept which may not be quite right.  It might be more like the residual or the probability that it belongs to no class.  In any case, that probability is always very small and can be ignored.
 
We can see that the model struggles to identify those that give in person.
 

{% highlight text %}
##            int     person       mail       phone      online   id target
## 1  0.003894710 0.11145866 0.78291196 0.012381732 0.089352980  162      1
## 4  0.001766358 0.05437046 0.90918684 0.006218268 0.028458007  519      1
## 10 0.003768742 0.06368329 0.31300664 0.013795818 0.605745494 1280      1
## 11 0.003268404 0.16128048 0.72526485 0.060543004 0.049643245 1402      1
## 13 0.003768742 0.06368329 0.31300664 0.013795818 0.605745494 2420      1
## 18 0.003077914 0.38370138 0.53153563 0.005768122 0.075916946 4899      1
## 19 0.001138055 0.02211350 0.97121710 0.002240609 0.003290790 4928      1
## 25 0.001526082 0.03727388 0.95324224 0.003565117 0.004392726 5866      1
## 26 0.002925514 0.78065723 0.08614931 0.027948357 0.102319583 5868      1
## 27 0.003892025 0.05962785 0.42144647 0.012931160 0.502102494 6409      1
{% endhighlight %}
 
It does fairly well with those that give by sending a check through the mail.
 

{% highlight text %}
##            int     person      mail       phone      online   id target
## 2  0.002487211 0.09657648 0.8848721 0.006085883 0.009978349  345      2
## 3  0.001138055 0.02211350 0.9712171 0.002240609 0.003290790  503      2
## 5  0.001766358 0.05437046 0.9091868 0.006218268 0.028458007  519      2
## 6  0.001138055 0.02211350 0.9712171 0.002240609 0.003290790  679      2
## 7  0.002765174 0.19667074 0.7871242 0.005444095 0.007995760  753      2
## 8  0.001138055 0.02211350 0.9712171 0.002240609 0.003290790  757      2
## 9  0.002765174 0.19667074 0.7871242 0.005444095 0.007995760  771      2
## 12 0.004976281 0.39584348 0.5061289 0.020338623 0.072712757 1606      2
## 14 0.005154495 0.46643564 0.1989040 0.038126353 0.291379601 2434      2
## 15 0.003854415 0.15715955 0.7807682 0.006974746 0.051243130 2736      2
{% endhighlight %}
 
It also is decent at guessing donors who give over the phone.
 

{% highlight text %}
##             int     person       mail      phone     online    id target
## 73  0.004506220 0.15115131 0.41069493 0.30802345 0.12562412 29079      3
## 180 0.005614924 0.17375767 0.19609810 0.49497694 0.12955232 84977      3
## 183 0.005274627 0.21108665 0.08738465 0.51206005 0.18419404 84997      3
## 184 0.004322461 0.13194817 0.11993220 0.58163881 0.16215838 85001      3
## 192 0.004385666 0.38481158 0.03770124 0.50182945 0.07127207 85952      3
## 193 0.005727138 0.20266503 0.25251114 0.20584677 0.33324993 86052      3
## 194 0.005069392 0.43627498 0.11189300 0.38965049 0.05711211 86077      3
## 198 0.004667855 0.34375128 0.16319472 0.02286492 0.46552125 86345      3
## 206 0.003257909 0.13403651 0.08382569 0.66423970 0.11464017 86731      3
## 207 0.001866081 0.05640193 0.01631556 0.87584889 0.04956752 86800      3
{% endhighlight %}
 
However, it does not do as well predicting who gives online.
 

{% highlight text %}
##             int     person       mail       phone     online    id target
## 29  0.003635308 0.09235315 0.44117379 0.022745626 0.44009209  6636      4
## 47  0.001138055 0.02211350 0.97121710 0.002240609 0.00329079 10046      4
## 70  0.002654173 0.11769284 0.06580912 0.766535223 0.04730868 27512      4
## 75  0.003768742 0.06368329 0.31300664 0.013795818 0.60574549 30896      4
## 80  0.003892025 0.05962785 0.42144647 0.012931160 0.50210249 34686      4
## 81  0.003892025 0.05962785 0.42144647 0.012931160 0.50210249 35702      4
## 106 0.002754199 0.72612566 0.16267577 0.025286134 0.08315827 59474      4
## 115 0.004557631 0.38230813 0.36322647 0.033862792 0.21604498 66903      4
## 124 0.006003804 0.55004025 0.26239112 0.126836970 0.05472788 72788      4
## 128 0.003513141 0.15629244 0.36962089 0.019229865 0.45134369 72858      4
{% endhighlight %}
 
With all of that out of the way, to start the conversation and add a few ideas on what factors contribute to which method of giving, here are a few graphs.
 
First here is the break down of the number of donors in each category.  It's not even but it is not overly dominated by any one category either which is good.
 
 
![plot of chunk unnamed-chunk-9](/figures/unnamed-chunk-9-1.png)
 
Next we can look at age which I think is the most revealing.  I was very surprised that so many young people chose to give over the phone.  The popular narrative that I hear is that young people do not answer their phones but this appears to tell a different story.
I also feel that every age bracket has a method so this seems useful.
 
To make each category clear:
 
* Lockbox is check in the mail
* Online Giving is online giving
* Staff is giving in person to a staff member
* TOP Gift is giving over the phone through our Telephone Outreach Program
 
![plot of chunk unnamed-chunk-10](/figures/unnamed-chunk-10-1.png)
 
Looking at that variable on whether a constituent is married, not married and male or not married and female, it's a little interesting that men seem to give more online and women give more over the phone but it is not a very significant difference.  The fact that married couples give more through the mail I believe is using the age data since older people are more likely to be married and we already saw that older people prefer to give in this way.
 
Again, these codes are:
 
* 1 means married
* 2 means not married and constituent is male
* 3 means not married and constituent is female
* 4 means company
* 5 means foundation
* 6 means missing information or other
 
![plot of chunk unnamed-chunk-11](/figures/unnamed-chunk-11-1.png)
 
A quick note on these plots.  The y-axis is density so it is basically answering the question: of those that fall into a particular category what is the distribution in giving method choice; what is the percentage that choose to give via a particular method by category type.
 
The chart on ask count was also quite insightful.  It appears that people give based on how you ask them.  If you call someone numerous times they will give that way and if you email numerous times then online will be their choice and so on.  
 
This actually reminds me of a small nonprofit in Detroit.  I don't remember the name but they won an award at a Blackbaud conference in 2012 and their trick for increasing gifts was to just ask more often which I think may fly in the face of some of the conventional thought that seems to suggest that this type of behavior would be detrimental.  In any case, this is obviously just the trends in our data and it doesn't mean it would work everywhere but it seems like a possible factor to consider.
 
Those with two calls gave over the phone more often than those that just received one.
 
![plot of chunk unnamed-chunk-12](/figures/unnamed-chunk-12-1.png)
 
Mail is interesting as it almost seems to be a reminder to give online at first but after four mailings we finally wear down the holdouts and they relent and send us money perhaps just to stop the mailings.
 
![plot of chunk unnamed-chunk-13](/figures/unnamed-chunk-13-1.png)
 
Email also shows this trend where the more we ask via email for a gift, the more we see constituents choosing to give online.
 
![plot of chunk unnamed-chunk-14](/figures/unnamed-chunk-14-1.png)
 
The next plot looks at the constituent type which is a little busy and for me there are not many insights to glean.
 
I will provide a legend for a few of the types:
 
* C is company
* F is foundation
* rs is retired staff
* S is staff
* I is any individual who is not staff
 
Most companies give directly to a staff member which makes sense.  Retired staff send in checks which again references the trends seen in the age data earlier.  Current staff give online since that is probably the easiest way for them to give.  
 
![plot of chunk unnamed-chunk-15](/figures/unnamed-chunk-15-1.png)
  
The last plot is on geographic distance which does not offer very much additional insight.
 
![plot of chunk unnamed-chunk-16](/figures/unnamed-chunk-16-1.png)
 
This is just a post and some code to get the conversation started.  It will be interesting to see if anyone has anything additional to add.  For me these are the takeaways at this stage in researching this question:
 
* Age seems to matter and I think experimenting with targeting the way we ask by age may be beneficial
* Ask frequency seems to help.  These days it is so easy to ignore anything that we do not want to deal with immediately that these types of reminders may actually be helpful and keep your organization top of mind as well as increase your chance of connecting with someone when they have the time to give right at the moment.
