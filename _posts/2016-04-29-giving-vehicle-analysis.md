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
 
My approach to problems like this is to get a decent sized set of data points together based on intuition like the ones described and then create a really quick model.  I will skip the modelling code here for now but it will be in my [fundraising analytics](https://github.com/michaelpawlus/fundraising_analytics) folder.
 
 

{% highlight text %}
## [0]	val-mlogloss:1.582715	train-mlogloss:1.573866
## [1]	val-mlogloss:1.553963	train-mlogloss:1.538293
## [2]	val-mlogloss:1.531163	train-mlogloss:1.507515
## [3]	val-mlogloss:1.508348	train-mlogloss:1.478492
## [4]	val-mlogloss:1.488688	train-mlogloss:1.452502
## [5]	val-mlogloss:1.469154	train-mlogloss:1.424387
## [6]	val-mlogloss:1.444639	train-mlogloss:1.393060
## [7]	val-mlogloss:1.425079	train-mlogloss:1.363681
## [8]	val-mlogloss:1.401279	train-mlogloss:1.333201
## [9]	val-mlogloss:1.380213	train-mlogloss:1.304643
## [10]	val-mlogloss:1.358288	train-mlogloss:1.275912
## [11]	val-mlogloss:1.338099	train-mlogloss:1.250834
## [12]	val-mlogloss:1.319267	train-mlogloss:1.227067
## [13]	val-mlogloss:1.307815	train-mlogloss:1.208305
## [14]	val-mlogloss:1.294118	train-mlogloss:1.188139
## [15]	val-mlogloss:1.281574	train-mlogloss:1.171527
## [16]	val-mlogloss:1.265494	train-mlogloss:1.150507
## [17]	val-mlogloss:1.251441	train-mlogloss:1.131359
## [18]	val-mlogloss:1.237740	train-mlogloss:1.112782
## [19]	val-mlogloss:1.225983	train-mlogloss:1.096067
## [20]	val-mlogloss:1.216479	train-mlogloss:1.078800
## [21]	val-mlogloss:1.203582	train-mlogloss:1.059727
## [22]	val-mlogloss:1.192012	train-mlogloss:1.042441
## [23]	val-mlogloss:1.182644	train-mlogloss:1.027063
## [24]	val-mlogloss:1.173443	train-mlogloss:1.011006
## [25]	val-mlogloss:1.165647	train-mlogloss:0.998287
## [26]	val-mlogloss:1.159199	train-mlogloss:0.986045
## [27]	val-mlogloss:1.152454	train-mlogloss:0.972666
## [28]	val-mlogloss:1.146981	train-mlogloss:0.961647
## [29]	val-mlogloss:1.141437	train-mlogloss:0.951718
## [30]	val-mlogloss:1.135511	train-mlogloss:0.938669
## [31]	val-mlogloss:1.130664	train-mlogloss:0.927784
## [32]	val-mlogloss:1.122512	train-mlogloss:0.916208
## [33]	val-mlogloss:1.115641	train-mlogloss:0.904204
## [34]	val-mlogloss:1.108702	train-mlogloss:0.893425
## [35]	val-mlogloss:1.102542	train-mlogloss:0.883271
## [36]	val-mlogloss:1.096559	train-mlogloss:0.872655
## [37]	val-mlogloss:1.091135	train-mlogloss:0.862228
## [38]	val-mlogloss:1.086243	train-mlogloss:0.852190
## [39]	val-mlogloss:1.080592	train-mlogloss:0.842428
## [40]	val-mlogloss:1.076474	train-mlogloss:0.833512
## [41]	val-mlogloss:1.072122	train-mlogloss:0.825842
## [42]	val-mlogloss:1.066944	train-mlogloss:0.815583
## [43]	val-mlogloss:1.063194	train-mlogloss:0.806556
## [44]	val-mlogloss:1.060498	train-mlogloss:0.798484
## [45]	val-mlogloss:1.056354	train-mlogloss:0.789853
## [46]	val-mlogloss:1.052711	train-mlogloss:0.782954
## [47]	val-mlogloss:1.049281	train-mlogloss:0.775809
## [48]	val-mlogloss:1.047596	train-mlogloss:0.769226
## [49]	val-mlogloss:1.045115	train-mlogloss:0.763462
## [50]	val-mlogloss:1.041053	train-mlogloss:0.756046
## [51]	val-mlogloss:1.038616	train-mlogloss:0.749806
## [52]	val-mlogloss:1.036706	train-mlogloss:0.744565
## [53]	val-mlogloss:1.033949	train-mlogloss:0.737677
## [54]	val-mlogloss:1.031315	train-mlogloss:0.731192
## [55]	val-mlogloss:1.028992	train-mlogloss:0.725536
## [56]	val-mlogloss:1.027947	train-mlogloss:0.719509
## [57]	val-mlogloss:1.025696	train-mlogloss:0.713127
## [58]	val-mlogloss:1.024304	train-mlogloss:0.707649
## [59]	val-mlogloss:1.022731	train-mlogloss:0.702363
## [60]	val-mlogloss:1.021422	train-mlogloss:0.698019
## [61]	val-mlogloss:1.020482	train-mlogloss:0.694032
## [62]	val-mlogloss:1.018532	train-mlogloss:0.688700
## [63]	val-mlogloss:1.017330	train-mlogloss:0.683582
## [64]	val-mlogloss:1.015654	train-mlogloss:0.677959
## [65]	val-mlogloss:1.014942	train-mlogloss:0.673141
## [66]	val-mlogloss:1.013514	train-mlogloss:0.668230
## [67]	val-mlogloss:1.012823	train-mlogloss:0.662584
## [68]	val-mlogloss:1.012013	train-mlogloss:0.658104
## [69]	val-mlogloss:1.010821	train-mlogloss:0.653811
## [70]	val-mlogloss:1.009808	train-mlogloss:0.649237
## [71]	val-mlogloss:1.008552	train-mlogloss:0.644443
## [72]	val-mlogloss:1.008116	train-mlogloss:0.640887
## [73]	val-mlogloss:1.007359	train-mlogloss:0.635871
## [74]	val-mlogloss:1.006767	train-mlogloss:0.631848
## [75]	val-mlogloss:1.007058	train-mlogloss:0.627530
## [76]	val-mlogloss:1.006605	train-mlogloss:0.623328
## [77]	val-mlogloss:1.005875	train-mlogloss:0.619960
## [78]	val-mlogloss:1.005172	train-mlogloss:0.616315
## [79]	val-mlogloss:1.004530	train-mlogloss:0.611856
## [80]	val-mlogloss:1.004082	train-mlogloss:0.607820
## [81]	val-mlogloss:1.003519	train-mlogloss:0.603727
## [82]	val-mlogloss:1.003457	train-mlogloss:0.599815
## [83]	val-mlogloss:1.002861	train-mlogloss:0.596066
## [84]	val-mlogloss:1.002094	train-mlogloss:0.592555
## [85]	val-mlogloss:1.002167	train-mlogloss:0.588560
## [86]	val-mlogloss:1.001933	train-mlogloss:0.585276
## [87]	val-mlogloss:1.001820	train-mlogloss:0.581598
## [88]	val-mlogloss:1.002201	train-mlogloss:0.578506
## [89]	val-mlogloss:1.001997	train-mlogloss:0.575658
## [90]	val-mlogloss:1.001907	train-mlogloss:0.572952
## [91]	val-mlogloss:1.001312	train-mlogloss:0.569287
## [92]	val-mlogloss:1.000474	train-mlogloss:0.565991
## [93]	val-mlogloss:1.000041	train-mlogloss:0.562894
## [94]	val-mlogloss:1.000085	train-mlogloss:0.560082
## [95]	val-mlogloss:0.999583	train-mlogloss:0.557834
## [96]	val-mlogloss:0.999759	train-mlogloss:0.554371
## [97]	val-mlogloss:0.999530	train-mlogloss:0.551711
## [98]	val-mlogloss:0.999499	train-mlogloss:0.548754
## [99]	val-mlogloss:0.999463	train-mlogloss:0.545782
## [100]	val-mlogloss:0.999917	train-mlogloss:0.543003
## [101]	val-mlogloss:0.999920	train-mlogloss:0.541009
## [102]	val-mlogloss:1.000815	train-mlogloss:0.538137
## [103]	val-mlogloss:1.001245	train-mlogloss:0.535543
## [104]	val-mlogloss:1.001011	train-mlogloss:0.533015
## [105]	val-mlogloss:1.001262	train-mlogloss:0.530525
## [106]	val-mlogloss:1.001334	train-mlogloss:0.527835
## [107]	val-mlogloss:1.002141	train-mlogloss:0.524959
## [108]	val-mlogloss:1.002593	train-mlogloss:0.522457
## [109]	val-mlogloss:1.003325	train-mlogloss:0.520581
## [110]	val-mlogloss:1.003778	train-mlogloss:0.518056
## [111]	val-mlogloss:1.004476	train-mlogloss:0.515699
## [112]	val-mlogloss:1.005165	train-mlogloss:0.513398
## [113]	val-mlogloss:1.005466	train-mlogloss:0.511497
## [114]	val-mlogloss:1.006080	train-mlogloss:0.509410
## [115]	val-mlogloss:1.006884	train-mlogloss:0.506851
## [116]	val-mlogloss:1.007395	train-mlogloss:0.504609
## [117]	val-mlogloss:1.008347	train-mlogloss:0.502299
## [118]	val-mlogloss:1.008790	train-mlogloss:0.500757
## [119]	val-mlogloss:1.009479	train-mlogloss:0.498467
## [120]	val-mlogloss:1.009869	train-mlogloss:0.496313
## [121]	val-mlogloss:1.010233	train-mlogloss:0.494218
## [122]	val-mlogloss:1.010248	train-mlogloss:0.491609
## [123]	val-mlogloss:1.011111	train-mlogloss:0.490133
## [124]	val-mlogloss:1.011427	train-mlogloss:0.488062
## [125]	val-mlogloss:1.012138	train-mlogloss:0.486207
## [126]	val-mlogloss:1.012402	train-mlogloss:0.484039
## [127]	val-mlogloss:1.012948	train-mlogloss:0.482031
## [128]	val-mlogloss:1.013844	train-mlogloss:0.479843
## [129]	val-mlogloss:1.014783	train-mlogloss:0.478119
## [130]	val-mlogloss:1.016260	train-mlogloss:0.476388
## [131]	val-mlogloss:1.015909	train-mlogloss:0.474518
## [132]	val-mlogloss:1.016196	train-mlogloss:0.472979
## [133]	val-mlogloss:1.017033	train-mlogloss:0.471587
## [134]	val-mlogloss:1.017782	train-mlogloss:0.469493
## [135]	val-mlogloss:1.018922	train-mlogloss:0.467110
## [136]	val-mlogloss:1.019096	train-mlogloss:0.465051
## [137]	val-mlogloss:1.019786	train-mlogloss:0.463397
## [138]	val-mlogloss:1.020255	train-mlogloss:0.461616
## [139]	val-mlogloss:1.020720	train-mlogloss:0.459674
## [140]	val-mlogloss:1.021757	train-mlogloss:0.457790
## [141]	val-mlogloss:1.022003	train-mlogloss:0.455669
## [142]	val-mlogloss:1.022917	train-mlogloss:0.453019
## [143]	val-mlogloss:1.024103	train-mlogloss:0.450660
## [144]	val-mlogloss:1.024705	train-mlogloss:0.448940
## [145]	val-mlogloss:1.025474	train-mlogloss:0.447166
## [146]	val-mlogloss:1.025886	train-mlogloss:0.445536
## [147]	val-mlogloss:1.026363	train-mlogloss:0.443784
## [148]	val-mlogloss:1.027045	train-mlogloss:0.442210
## [149]	val-mlogloss:1.027593	train-mlogloss:0.440786
## Stopping. Best iteration: 100
{% endhighlight %}
 
Once the model is done, I like to look at the importance of the features included:
 
![plot of chunk unnamed-chunk-2](/figures/unnamed-chunk-2-1.png)
 
We can see that the various solicitation counts and age seem to have the biggest impact so we can start there.
 

 
I should note that at present this model is not performing too well but it is just a quick one for now.  Here is the logloss score which is a measure of error.  The closer to 0 the better.
 

{% highlight text %}
## [1] 23.53986
{% endhighlight %}
 
We are a long way off.
 
Here is a quick break down for each group.  What the model does is assign a probability for each id as to which giving method they used so each row should sum to 1.  I labled the first column as intercept which may not be quite right.  It might be more like the residual or the probability that it belongs to no class.  In any case, that probability is always very small and can be ignored.
 
We can see that the model struggles to identify those that give in person.
 

{% highlight text %}
##            int     person      mail       phone     online   id target
## 7  0.003359201 0.06729294 0.8778047 0.007526707 0.04401642  519      1
## 16 0.005569647 0.06275334 0.2402193 0.020977683 0.67048007 1280      1
## 22 0.005569647 0.06275334 0.2402193 0.020977683 0.67048007 2420      1
## 25 0.007740805 0.43735009 0.4561079 0.031226980 0.06757420 4899      1
## 26 0.002704712 0.11759853 0.8575099 0.004678586 0.01750828 4928      1
## 34 0.004448269 0.05196352 0.1850839 0.016727636 0.74177670 6409      1
## 38 0.005955158 0.24931282 0.4316050 0.276899695 0.03622730 7056      1
## 39 0.006295884 0.38608423 0.5118341 0.018325686 0.07746006 7278      1
## 46 0.004131482 0.50714368 0.4569422 0.012873973 0.01890871 8559      1
## 50 0.007615761 0.06572634 0.4316943 0.023847446 0.47111613 9389      1
{% endhighlight %}
 
It does fairly well with those that give by sending a check through the mail.
 

{% highlight text %}
##            int     person      mail       phone     online  id target
## 1  0.002704712 0.11759853 0.8575099 0.004678586 0.01750828 355      2
## 2  0.004380052 0.39190200 0.5703541 0.006143480 0.02722035 356      2
## 3  0.002684170 0.04337558 0.9115510 0.004304499 0.03808480 386      2
## 4  0.006816243 0.05887737 0.7236111 0.017922351 0.19277288 491      2
## 5  0.002704712 0.11759853 0.8575099 0.004678586 0.01750828 503      2
## 8  0.002704712 0.11759853 0.8575099 0.004678586 0.01750828 566      2
## 9  0.002704712 0.11759853 0.8575099 0.004678586 0.01750828 679      2
## 10 0.002704712 0.11759853 0.8575099 0.004678586 0.01750828 689      2
## 11 0.003804006 0.52841663 0.4409716 0.006580134 0.02022763 753      2
## 12 0.005705449 0.03896137 0.6722894 0.014751926 0.26829192 778      2
{% endhighlight %}
 
It also is decent at guessing donors who give over the phone.
 

{% highlight text %}
##             int     person       mail      phone     online    id target
## 96  0.006867969 0.39268070 0.45476663 0.09771536 0.04796930 41044      3
## 165 0.008710269 0.08021285 0.17544633 0.51470315 0.22092737 83825      3
## 176 0.008887049 0.39015675 0.18958509 0.27069449 0.14067668 84774      3
## 179 0.008767311 0.17562665 0.19748324 0.41097203 0.20715080 84997      3
## 188 0.005981983 0.09171446 0.11481086 0.65270221 0.13479042 85782      3
## 190 0.004826783 0.53774071 0.05696745 0.38079470 0.01967035 86077      3
## 194 0.006130100 0.31922567 0.20285183 0.24367577 0.22811663 86345      3
## 198 0.005744839 0.03059022 0.07675869 0.75888449 0.12802182 86604      3
## 203 0.009253982 0.10594522 0.22583945 0.53049254 0.12846878 86800      3
## 204 0.005973524 0.18114929 0.09927659 0.65132791 0.06227269 86830      3
{% endhighlight %}
 
However, it does not do as well predicting who gives online.
 

{% highlight text %}
##             int     person      mail       phone     online    id target
## 6   0.003425406 0.03227650 0.8122783 0.007251971 0.14476775   513      4
## 35  0.004963682 0.14474791 0.2614189 0.071880348 0.51698917  6636      4
## 43  0.004448269 0.05196352 0.1850839 0.016727636 0.74177670  8043      4
## 54  0.002704712 0.11759853 0.8575099 0.004678586 0.01750828 10046      4
## 58  0.005569647 0.06275334 0.2402193 0.020977683 0.67048007 12325      4
## 67  0.004086843 0.09862112 0.8144931 0.011132821 0.07166620 22339      4
## 89  0.004448269 0.05196352 0.1850839 0.016727636 0.74177670 34686      4
## 91  0.004448269 0.05196352 0.1850839 0.016727636 0.74177670 35702      4
## 93  0.004448269 0.05196352 0.1850839 0.016727636 0.74177670 36357      4
## 111 0.006371358 0.29865688 0.3690878 0.018995168 0.30688882 58827      4
{% endhighlight %}
 
With all of that out of the way, to start the conversation and add a few ideas on what factors contribute to which method of giving, here are a few graphs.
 
First here is the break down of the number of donors in each category.  It's not even but it is not overly donimated by any one categoy either which is good.
 
 
![plot of chunk unnamed-chunk-9](/figures/unnamed-chunk-9-1.png)
 
Next we can look at age which I think is the most revealing.  I was very surprised that so many young people chose to give over the phone.  The popular narrative that I hear is that young people do not answer their phones but this appears to tell a different story.
I also feel that every age bracket has a method so this seems useful.
 
![plot of chunk unnamed-chunk-10](/figures/unnamed-chunk-10-1.png)
 
First looking at that variable on whether a constiuent is married, not married and male or not married and female.  It's a little interesting that men seem to give more online and women give more over the phone but it is not a very significant difference.
 
![plot of chunk unnamed-chunk-11](/figures/unnamed-chunk-11-1.png)
 
The chart on ask count was also quite insightful.  It appears that people give based on how you ask them.  If you call someone numerous times they will give that way and if you email numerous times then online will be their choice and so on.  This actually reminds me of a small nonprofit in Detroit.  I don't remember the name but they won an award at a Blackbaud conference and their trick for increasing gifts was to just ask more often which flies in the face of some of the conventional thought that seems to suggest that this type of behavior would be detrimental.  In any case, this is obviously just the trends in our data and it doesn't mean it would work everywhere but it seems like a possible factor to consider.
 
![plot of chunk unnamed-chunk-12](/figures/unnamed-chunk-12-1.png)![plot of chunk unnamed-chunk-12](/figures/unnamed-chunk-12-2.png)![plot of chunk unnamed-chunk-12](/figures/unnamed-chunk-12-3.png)
 
Next, looking at the constiuent type which is a little busy and for me there are not many insights to glean.
 
![plot of chunk unnamed-chunk-13](/figures/unnamed-chunk-13-1.png)
  
Lastly, geographic distance also does not add too much more information.
 
![plot of chunk unnamed-chunk-14](/figures/unnamed-chunk-14-1.png)
 
