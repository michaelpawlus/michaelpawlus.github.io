---
layout: post
title: Giving Vehicle Analysis
author: Michael Pawlus
published: true
status: publish
draft: false
---
 
There was an interesting question on PRSPCT-L a few weeks ago:
 
> Do you know of or have experience with demographic modeling and data appends that help > predict a donor’s preferred giving method (i.e., event, direct mail, online, etc.)? 
 
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
    + type2 = 6 means missing inforation or other
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
 
My approach to problems like this is to get a decent sized set of data points like this together based on intuition basically and then create a really quick model.  I will skip the modelling code here for now but it will be in my fundraising analytics folder.
 
 

{% highlight text %}
## [1] 2148
{% endhighlight %}



{% highlight text %}
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 18 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 48 extra nodes, 0 pruned nodes ,max_depth=6
## [0]	val-mlogloss:1.573203	train-mlogloss:1.565432
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## [1]	val-mlogloss:1.540131	train-mlogloss:1.524711
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 20 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## [2]	val-mlogloss:1.512376	train-mlogloss:1.488557
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## [3]	val-mlogloss:1.481203	train-mlogloss:1.450160
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## [4]	val-mlogloss:1.451190	train-mlogloss:1.413982
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 56 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## [5]	val-mlogloss:1.427974	train-mlogloss:1.383705
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 44 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## [6]	val-mlogloss:1.403725	train-mlogloss:1.348457
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 46 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## [7]	val-mlogloss:1.381196	train-mlogloss:1.316518
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 48 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## [8]	val-mlogloss:1.363959	train-mlogloss:1.291620
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 52 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## [9]	val-mlogloss:1.350740	train-mlogloss:1.272122
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 56 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## [10]	val-mlogloss:1.332200	train-mlogloss:1.243804
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 14 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 46 extra nodes, 0 pruned nodes ,max_depth=6
## [11]	val-mlogloss:1.314429	train-mlogloss:1.219759
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 44 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## [12]	val-mlogloss:1.296415	train-mlogloss:1.194625
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 18 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## [13]	val-mlogloss:1.280542	train-mlogloss:1.173762
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## [14]	val-mlogloss:1.265408	train-mlogloss:1.152522
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 66 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 52 extra nodes, 0 pruned nodes ,max_depth=6
## [15]	val-mlogloss:1.251762	train-mlogloss:1.131859
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## [16]	val-mlogloss:1.238660	train-mlogloss:1.112600
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 54 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## [17]	val-mlogloss:1.227792	train-mlogloss:1.097045
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## [18]	val-mlogloss:1.214443	train-mlogloss:1.079119
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 20 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 20 extra nodes, 0 pruned nodes ,max_depth=6
## [19]	val-mlogloss:1.201978	train-mlogloss:1.061745
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 48 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## [20]	val-mlogloss:1.193260	train-mlogloss:1.047438
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 52 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 44 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## [21]	val-mlogloss:1.184046	train-mlogloss:1.032102
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 44 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## [22]	val-mlogloss:1.172815	train-mlogloss:1.015575
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## [23]	val-mlogloss:1.164206	train-mlogloss:1.002681
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 46 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## [24]	val-mlogloss:1.156916	train-mlogloss:0.990220
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 14 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## [25]	val-mlogloss:1.150111	train-mlogloss:0.978759
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 46 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## [26]	val-mlogloss:1.141749	train-mlogloss:0.963865
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 46 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## [27]	val-mlogloss:1.134323	train-mlogloss:0.952619
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 16 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## [28]	val-mlogloss:1.126164	train-mlogloss:0.939973
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 50 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## [29]	val-mlogloss:1.121275	train-mlogloss:0.929384
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 54 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## [30]	val-mlogloss:1.114381	train-mlogloss:0.916699
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 50 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## [31]	val-mlogloss:1.108221	train-mlogloss:0.904206
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 50 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 48 extra nodes, 0 pruned nodes ,max_depth=6
## [32]	val-mlogloss:1.104377	train-mlogloss:0.895642
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 48 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## [33]	val-mlogloss:1.099845	train-mlogloss:0.885268
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## [34]	val-mlogloss:1.093925	train-mlogloss:0.874094
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 50 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## [35]	val-mlogloss:1.089966	train-mlogloss:0.865218
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 52 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 44 extra nodes, 0 pruned nodes ,max_depth=6
## [36]	val-mlogloss:1.085617	train-mlogloss:0.855213
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## [37]	val-mlogloss:1.079991	train-mlogloss:0.845563
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 56 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## [38]	val-mlogloss:1.076938	train-mlogloss:0.837284
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 56 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## [39]	val-mlogloss:1.073052	train-mlogloss:0.829881
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 56 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## [40]	val-mlogloss:1.068762	train-mlogloss:0.819878
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 48 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 18 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## [41]	val-mlogloss:1.064631	train-mlogloss:0.812057
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## [42]	val-mlogloss:1.062329	train-mlogloss:0.805235
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 44 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 44 extra nodes, 0 pruned nodes ,max_depth=6
## [43]	val-mlogloss:1.058584	train-mlogloss:0.796397
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 46 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## [44]	val-mlogloss:1.054710	train-mlogloss:0.787677
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 52 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## [45]	val-mlogloss:1.052750	train-mlogloss:0.781322
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## [46]	val-mlogloss:1.049306	train-mlogloss:0.773480
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 16 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## [47]	val-mlogloss:1.046119	train-mlogloss:0.767209
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 54 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## [48]	val-mlogloss:1.043659	train-mlogloss:0.761670
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## [49]	val-mlogloss:1.041983	train-mlogloss:0.755324
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## [50]	val-mlogloss:1.039943	train-mlogloss:0.749947
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## [51]	val-mlogloss:1.037922	train-mlogloss:0.744174
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 20 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 54 extra nodes, 0 pruned nodes ,max_depth=6
## [52]	val-mlogloss:1.035310	train-mlogloss:0.737093
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 46 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## [53]	val-mlogloss:1.032923	train-mlogloss:0.730200
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 20 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## [54]	val-mlogloss:1.030331	train-mlogloss:0.723759
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 44 extra nodes, 0 pruned nodes ,max_depth=6
## [55]	val-mlogloss:1.028020	train-mlogloss:0.717074
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 48 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## [56]	val-mlogloss:1.025815	train-mlogloss:0.711107
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 48 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## [57]	val-mlogloss:1.023647	train-mlogloss:0.705290
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## [58]	val-mlogloss:1.021937	train-mlogloss:0.699157
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 50 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## [59]	val-mlogloss:1.020262	train-mlogloss:0.693391
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## [60]	val-mlogloss:1.018299	train-mlogloss:0.687218
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## [61]	val-mlogloss:1.015800	train-mlogloss:0.682088
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## [62]	val-mlogloss:1.014051	train-mlogloss:0.676344
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## [63]	val-mlogloss:1.012515	train-mlogloss:0.671776
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 18 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 52 extra nodes, 0 pruned nodes ,max_depth=6
## [64]	val-mlogloss:1.011644	train-mlogloss:0.667084
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 56 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 54 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 18 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## [65]	val-mlogloss:1.011568	train-mlogloss:0.662046
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 52 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## [66]	val-mlogloss:1.010706	train-mlogloss:0.657718
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## [67]	val-mlogloss:1.009151	train-mlogloss:0.653292
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 20 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## [68]	val-mlogloss:1.008786	train-mlogloss:0.649644
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## [69]	val-mlogloss:1.008092	train-mlogloss:0.645698
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 16 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 50 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## [70]	val-mlogloss:1.007060	train-mlogloss:0.640865
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## [71]	val-mlogloss:1.006994	train-mlogloss:0.636372
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 46 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## [72]	val-mlogloss:1.007140	train-mlogloss:0.632764
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## [73]	val-mlogloss:1.005908	train-mlogloss:0.629421
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## [74]	val-mlogloss:1.004676	train-mlogloss:0.625410
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 54 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 44 extra nodes, 0 pruned nodes ,max_depth=6
## [75]	val-mlogloss:1.004331	train-mlogloss:0.620831
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 48 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 44 extra nodes, 0 pruned nodes ,max_depth=6
## [76]	val-mlogloss:1.004264	train-mlogloss:0.616860
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 54 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## [77]	val-mlogloss:1.004418	train-mlogloss:0.613007
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 52 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 48 extra nodes, 0 pruned nodes ,max_depth=6
## [78]	val-mlogloss:1.004445	train-mlogloss:0.608202
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 56 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## [79]	val-mlogloss:1.004061	train-mlogloss:0.604551
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## [80]	val-mlogloss:1.003769	train-mlogloss:0.601413
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 16 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## [81]	val-mlogloss:1.003867	train-mlogloss:0.598077
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## [82]	val-mlogloss:1.003706	train-mlogloss:0.594231
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 44 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## [83]	val-mlogloss:1.003594	train-mlogloss:0.591109
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 48 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## [84]	val-mlogloss:1.003466	train-mlogloss:0.587887
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 54 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## [85]	val-mlogloss:1.003714	train-mlogloss:0.585171
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 48 extra nodes, 0 pruned nodes ,max_depth=6
## [86]	val-mlogloss:1.003658	train-mlogloss:0.581873
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## [87]	val-mlogloss:1.003634	train-mlogloss:0.578812
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 58 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 44 extra nodes, 0 pruned nodes ,max_depth=6
## [88]	val-mlogloss:1.003368	train-mlogloss:0.575277
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## [89]	val-mlogloss:1.002396	train-mlogloss:0.572484
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 46 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## [90]	val-mlogloss:1.002701	train-mlogloss:0.569845
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 20 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 16 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## [91]	val-mlogloss:1.002431	train-mlogloss:0.567776
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 54 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## [92]	val-mlogloss:1.001969	train-mlogloss:0.565637
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## [93]	val-mlogloss:1.002082	train-mlogloss:0.562954
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 44 extra nodes, 0 pruned nodes ,max_depth=6
## [94]	val-mlogloss:1.001732	train-mlogloss:0.560262
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 52 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 20 extra nodes, 0 pruned nodes ,max_depth=6
## [95]	val-mlogloss:1.002522	train-mlogloss:0.557055
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 46 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## [96]	val-mlogloss:1.002644	train-mlogloss:0.554409
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 44 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 52 extra nodes, 0 pruned nodes ,max_depth=6
## [97]	val-mlogloss:1.002522	train-mlogloss:0.552242
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 52 extra nodes, 0 pruned nodes ,max_depth=6
## [98]	val-mlogloss:1.002553	train-mlogloss:0.549445
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 20 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## [99]	val-mlogloss:1.003066	train-mlogloss:0.547004
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 50 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=5
## tree prunning end, 1 roots, 20 extra nodes, 0 pruned nodes ,max_depth=6
## [100]	val-mlogloss:1.003302	train-mlogloss:0.544641
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## [101]	val-mlogloss:1.003628	train-mlogloss:0.541882
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 52 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## [102]	val-mlogloss:1.004227	train-mlogloss:0.539655
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## [103]	val-mlogloss:1.004366	train-mlogloss:0.537591
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 46 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 16 extra nodes, 0 pruned nodes ,max_depth=6
## [104]	val-mlogloss:1.004758	train-mlogloss:0.535562
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 46 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## [105]	val-mlogloss:1.004941	train-mlogloss:0.533303
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## [106]	val-mlogloss:1.004930	train-mlogloss:0.531334
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## [107]	val-mlogloss:1.004433	train-mlogloss:0.529054
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 16 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 56 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 20 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## [108]	val-mlogloss:1.005091	train-mlogloss:0.527139
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 50 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## [109]	val-mlogloss:1.005073	train-mlogloss:0.525149
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 18 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## [110]	val-mlogloss:1.006056	train-mlogloss:0.523134
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 46 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 14 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## [111]	val-mlogloss:1.006485	train-mlogloss:0.520845
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 52 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## [112]	val-mlogloss:1.006650	train-mlogloss:0.518378
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 20 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## [113]	val-mlogloss:1.006793	train-mlogloss:0.516569
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 48 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 18 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## [114]	val-mlogloss:1.007294	train-mlogloss:0.514648
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## [115]	val-mlogloss:1.007997	train-mlogloss:0.513256
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 18 extra nodes, 0 pruned nodes ,max_depth=6
## [116]	val-mlogloss:1.008217	train-mlogloss:0.511207
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## [117]	val-mlogloss:1.008567	train-mlogloss:0.509393
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 20 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## [118]	val-mlogloss:1.009230	train-mlogloss:0.507475
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 54 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## [119]	val-mlogloss:1.009649	train-mlogloss:0.505327
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## [120]	val-mlogloss:1.010192	train-mlogloss:0.503393
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 52 extra nodes, 0 pruned nodes ,max_depth=6
## [121]	val-mlogloss:1.011044	train-mlogloss:0.501072
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 58 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## [122]	val-mlogloss:1.011369	train-mlogloss:0.498824
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## [123]	val-mlogloss:1.011305	train-mlogloss:0.497077
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## [124]	val-mlogloss:1.011213	train-mlogloss:0.495378
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## [125]	val-mlogloss:1.011396	train-mlogloss:0.493962
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 62 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 52 extra nodes, 0 pruned nodes ,max_depth=6
## [126]	val-mlogloss:1.012593	train-mlogloss:0.491165
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 20 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## [127]	val-mlogloss:1.012689	train-mlogloss:0.489758
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 18 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 46 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## [128]	val-mlogloss:1.013223	train-mlogloss:0.487591
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## [129]	val-mlogloss:1.014220	train-mlogloss:0.485558
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 48 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## [130]	val-mlogloss:1.014598	train-mlogloss:0.483741
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## [131]	val-mlogloss:1.014327	train-mlogloss:0.481985
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 48 extra nodes, 0 pruned nodes ,max_depth=6
## [132]	val-mlogloss:1.014946	train-mlogloss:0.480168
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 54 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## [133]	val-mlogloss:1.015925	train-mlogloss:0.478584
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 50 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 14 extra nodes, 0 pruned nodes ,max_depth=6
## [134]	val-mlogloss:1.016277	train-mlogloss:0.476949
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 20 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## [135]	val-mlogloss:1.016478	train-mlogloss:0.475337
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 46 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 52 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 24 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## [136]	val-mlogloss:1.016996	train-mlogloss:0.473712
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 20 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## [137]	val-mlogloss:1.017337	train-mlogloss:0.472372
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=5
## tree prunning end, 1 roots, 42 extra nodes, 0 pruned nodes ,max_depth=6
## [138]	val-mlogloss:1.017732	train-mlogloss:0.470478
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 46 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 48 extra nodes, 0 pruned nodes ,max_depth=6
## [139]	val-mlogloss:1.018123	train-mlogloss:0.468766
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 38 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 28 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## [140]	val-mlogloss:1.018608	train-mlogloss:0.467432
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 30 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 46 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## [141]	val-mlogloss:1.019266	train-mlogloss:0.466323
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 18 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 36 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 22 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 48 extra nodes, 0 pruned nodes ,max_depth=6
## [142]	val-mlogloss:1.020258	train-mlogloss:0.464656
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 34 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 40 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## [143]	val-mlogloss:1.020611	train-mlogloss:0.463226
## tree prunning end, 1 roots, 0 extra nodes, 0 pruned nodes ,max_depth=0
## tree prunning end, 1 roots, 32 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 26 extra nodes, 0 pruned nodes ,max_depth=6
## tree prunning end, 1 roots, 46 extra nodes, 0 pruned nodes ,max_depth=6
## [144]	val-mlogloss:1.021738	train-mlogloss:0.461683
## Stopping. Best iteration: 95
{% endhighlight %}
 
Once the model is done, I like to look at the importance of the features included:
 

{% highlight r %}
xgb.plot.importance(importance_matrix[1:10,])
{% endhighlight %}

![plot of chunk unnamed-chunk-2](/figures/unnamed-chunk-2-1.png)
 
We can see that age does matter as we might expect and also the number of solicitations that we send.
 

{% highlight r %}
mtest <- sparse.model.matrix(method ~ ., data = test[,names(test)[c(2:ncol(test))], with=FALSE])
 
preds <- predict(clf, mtest)
preds = matrix(preds,5,length(preds)/5)
preds = t(preds)
 
pred <- as.data.frame(preds)
pred$id <- test$core_id
pred$target <- test$method
 
names(pred) <- c("int","person","mail","phone","online","id","target")
{% endhighlight %}
 
I should note that at present this model is not performing too well but it is just a quick one for now.  Here is the logloss score which is a measure of error.  The closer to 0 the better.
 

{% highlight text %}
## [1] 23.39736
{% endhighlight %}
 
We are a long way off.
 
Here is a quick break down for each group.
 
The model struggles to identify those that give in person.
 

{% highlight text %}
##            int     person      mail       phone      online   id target
## 1  0.003372298 0.04928143 0.9002628 0.005961165 0.041122388  162      1
## 16 0.005636623 0.06625742 0.5194558 0.031139564 0.377510637 2420      1
## 18 0.003659413 0.37198728 0.5700680 0.011297124 0.042988207 4899      1
## 19 0.002613053 0.04045082 0.9453130 0.004237259 0.007385951 4928      1
## 23 0.002217089 0.02403448 0.9629787 0.003963221 0.006806481 5866      1
## 24 0.005444750 0.24939714 0.2292069 0.026203996 0.489747226 5868      1
## 26 0.005664767 0.07304688 0.4803542 0.027295396 0.413638741 6409      1
## 31 0.005790996 0.39766484 0.3585390 0.206661701 0.031343468 7056      1
## 32 0.004393117 0.10193853 0.1666347 0.561594903 0.165438712 7750      1
## 38 0.003151925 0.04860531 0.9326518 0.005094481 0.010496452 9806      1
{% endhighlight %}
 
It does fairly well with those that give over the phone.
 

{% highlight text %}
##            int     person      mail       phone      online  id target
## 3  0.001821973 0.02800478 0.9609123 0.002952908 0.006308064 345      2
## 4  0.007963935 0.17352940 0.4297309 0.023120333 0.365655482 353      2
## 5  0.002613053 0.04045082 0.9453130 0.004237259 0.007385951 355      2
## 6  0.001646569 0.02225475 0.9688551 0.002856219 0.004387403 378      2
## 7  0.002613053 0.04045082 0.9453130 0.004237259 0.007385951 503      2
## 9  0.005285592 0.07657687 0.7925199 0.010747825 0.114869766 519      2
## 10 0.001923618 0.03054638 0.9587505 0.003438619 0.005340878 520      2
## 11 0.002613053 0.04045082 0.9453130 0.004237259 0.007385951 679      2
## 12 0.002613053 0.04045082 0.9453130 0.004237259 0.007385951 689      2
## 13 0.006701720 0.13141139 0.4651566 0.017720899 0.379009485 778      2
{% endhighlight %}
 
It also is decent at guessing donors who give over the phone.
 

{% highlight text %}
##             int     person       mail     phone     online    id target
## 2   0.006447881 0.27951929 0.21420161 0.3432616 0.15656960   339      3
## 17  0.006447881 0.27951929 0.21420161 0.3432616 0.15656960  3061      3
## 81  0.006031199 0.55505157 0.23232101 0.1552101 0.05138609 41044      3
## 127 0.004716711 0.16161546 0.12784827 0.5885134 0.11730611 75114      3
## 151 0.003545512 0.05394986 0.05254861 0.8255025 0.06445345 83825      3
## 161 0.003273208 0.06271372 0.03206094 0.8624527 0.03949941 84552      3
## 167 0.003933806 0.03814729 0.03396326 0.8022864 0.12166924 84774      3
## 168 0.008249116 0.15024789 0.57274908 0.1861677 0.08258623 84977      3
## 170 0.004717048 0.04138076 0.08577289 0.7200073 0.14812200 84997      3
## 171 0.004387685 0.06947684 0.07751994 0.7959223 0.05269326 85001      3
{% endhighlight %}
 
However, it again does not do well with online.
 

{% highlight text %}
##            int     person       mail       phone      online    id target
## 8  0.004850092 0.08467796 0.73958880 0.011224560 0.159658581   513      4
## 22 0.005589177 0.08343919 0.66263264 0.011564235 0.236774772  5850      4
## 27 0.007457472 0.26031843 0.30433097 0.172368959 0.255524188  6636      4
## 30 0.001923618 0.03054638 0.95875055 0.003438619 0.005340878  6925      4
## 56 0.004713824 0.08342808 0.79466563 0.009873253 0.107319258 22339      4
## 66 0.004661369 0.14799589 0.09183192 0.687670052 0.067840777 27512      4
## 74 0.005664767 0.07304688 0.48035419 0.027295396 0.413638741 34686      4
## 75 0.005664767 0.07304688 0.48035419 0.027295396 0.413638741 35702      4
## 78 0.005664767 0.07304688 0.48035419 0.027295396 0.413638741 36357      4
## 89 0.005531315 0.26654366 0.56780285 0.025520090 0.134602100 50783      4
{% endhighlight %}
 
With all of that out of the way, to start the conversation and add a few ideas on what factors contribute to which method of giving, here are a few graphs.
 
First here is the break down of the number of donors in each category.  It's not even but it is not overly donimated by any one categoy either which is good.
 
 

{% highlight text %}
## Error in fread("giving_vehicle.csv"): File 'giving_vehicle.csv' does not exist. Include one or more spaces to consider the input a system command.
{% endhighlight %}

![plot of chunk unnamed-chunk-9](/figures/unnamed-chunk-9-1.png)
 
Next we can look at age which I think is the most revealing.  I was very surprised that so many young people chose to give over the phone.  The popular narrative that I hear is that young people do not answer their phones but this appears to tell a different story.
I also feel that every age bracket has a method so this seems useful.
 

{% highlight text %}
## Warning: Removed 2202 rows containing non-finite values (stat_density).
{% endhighlight %}

![plot of chunk unnamed-chunk-10](/figures/unnamed-chunk-10-1.png)
 
First looking at that variable on whether a constiuent is married, not married and male or not married and female.  It's a little interesting that men seem to give more online and women give more over the phone but it is not a very significant difference.
 
![plot of chunk unnamed-chunk-11](/figures/unnamed-chunk-11-1.png)
 
The chart on ask count was also quite insightful.  It appears that people give based on how you ask them.  If you call someone numerous times they will give that way and if you email numerous times then online will be their choice and so on.  This actually reminds me of a small nonprofit in Detroit.  I don't remember the name but they won an award at a Blackbaud conference and their trick for increasing gifts was to just ask more often which flies in the face of some of the conventional thought that seems to suggest that this type of behavior would be detrimental.  In any case, this is obviously just the trends in our data and it doesn't mean it would work everywhere but it seems like a possible factor to consider.
 
![plot of chunk unnamed-chunk-12](/figures/unnamed-chunk-12-1.png)![plot of chunk unnamed-chunk-12](/figures/unnamed-chunk-12-2.png)![plot of chunk unnamed-chunk-12](/figures/unnamed-chunk-12-3.png)
 
Next, looking at the constiuent type which is a little busy and for me there are not many insights to glean.
 
![plot of chunk unnamed-chunk-13](/figures/unnamed-chunk-13-1.png)
  
Lastly, geographic distance also does not add too much more information.
 
![plot of chunk unnamed-chunk-14](/figures/unnamed-chunk-14-1.png)
 
