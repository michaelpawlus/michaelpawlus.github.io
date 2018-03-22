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
 

 
Once the model is done, I like to look at the importance of the features included:
 
![plot of chunk unnamed-chunk-2](/figures/unnamed-chunk-2-1.png)
 
We can see that the various solicitation counts and age seem to have the biggest impact so we can start there.
 

 
I should note that at present this model is not performing too well but it is just a quick one for now.  Here is the logloss score which is a measure of error.  The closer to 0 the better.
 

{% highlight text %}
## Warning in model.matrix.default(~0 + ., data.frame(as.character(y_true))):
## term names will be truncated
{% endhighlight %}



{% highlight text %}
## Warning in model.matrix.default(~0 + ., data.frame(as.character(y_true))):
## term names will be truncated
{% endhighlight %}



{% highlight text %}
## Warning in model.matrix.default(~0 + ., data.frame(as.character(y_true))):
## term names will be truncated
{% endhighlight %}



{% highlight text %}
## Warning in model.matrix.default(~0 + ., data.frame(as.character(y_true))):
## term names will be truncated
{% endhighlight %}



{% highlight text %}
## Warning in y_true * log(y_pred): longer object length is not a multiple of
## shorter object length
{% endhighlight %}



{% highlight text %}
## Error in MultiLogLoss(pred$target, pred[, 2:5]): dims [product 16] do not match the length of object [2148]
{% endhighlight %}
 
We are a long way off.
 
Here is a quick break down for each group.  What the model does is assign a probability for each id as to which giving method they used so each row should sum to 1.  I labeled the first column as intercept which may not be quite right.  It might be more like the residual or the probability that it belongs to no class.  In any case, that probability is always very small and can be ignored.
 
We can see that the model struggles to identify those that give in person.
 

{% highlight text %}
##             int       person      mail        phone       online   id
## 1  4.658754e-05 4.532368e-03 0.9953577 5.274246e-06 5.805526e-05  162
## 4  1.283136e-06 1.673028e-06 0.9999956 1.192595e-07 1.340992e-06  519
## 10 3.346076e-03 1.561175e-01 0.1476913 2.989647e-01 3.938804e-01 1280
## 11 7.138878e-04 4.262211e-01 0.5302647 1.018624e-05 4.279006e-02 1402
## 13 3.346076e-03 1.561175e-01 0.1476913 2.989647e-01 3.938804e-01 2420
## 18 3.243815e-04 3.671909e-01 0.5161801 2.868456e-04 1.160178e-01 4899
## 19 1.027581e-04 8.821204e-03 0.9910619 1.185995e-05 2.235854e-06 4928
## 25 1.214771e-05 9.357628e-04 0.9989618 8.941843e-05 8.102461e-07 5866
## 26 3.542171e-03 4.254337e-01 0.3106384 7.940135e-02 1.809843e-01 5868
## 27 1.624461e-03 1.757128e-01 0.3567415 2.011217e-01 2.647996e-01 6409
##    target
## 1       1
## 4       1
## 10      1
## 11      1
## 13      1
## 18      1
## 19      1
## 25      1
## 26      1
## 27      1
{% endhighlight %}
 
It does fairly well with those that give by sending a check through the mail.
 

{% highlight text %}
##             int       person      mail        phone       online   id
## 2  3.031990e-07 1.408535e-07 0.9999974 2.231828e-06 2.643368e-09  345
## 3  1.027581e-04 8.821204e-03 0.9910619 1.185995e-05 2.235854e-06  503
## 5  1.283136e-06 1.673028e-06 0.9999956 1.192595e-07 1.340992e-06  519
## 6  1.027581e-04 8.821204e-03 0.9910619 1.185995e-05 2.235854e-06  679
## 7  3.017682e-04 3.855880e-01 0.6140689 3.482892e-05 6.401686e-06  753
## 8  1.027581e-04 8.821204e-03 0.9910619 1.185995e-05 2.235854e-06  757
## 9  3.017682e-04 3.855880e-01 0.6140689 3.482892e-05 6.401686e-06  771
## 12 1.068792e-03 8.234528e-02 0.9153901 9.959318e-05 1.096226e-03 1606
## 14 3.346076e-03 1.561175e-01 0.1476913 2.989647e-01 3.938804e-01 2434
## 15 2.293826e-07 3.666242e-06 0.9999959 5.846894e-08 4.401463e-08 2736
##    target
## 2       2
## 3       2
## 5       2
## 6       2
## 7       2
## 8       2
## 9       2
## 12      2
## 14      2
## 15      2
{% endhighlight %}
 
It also is decent at guessing donors who give over the phone.
 

{% highlight text %}
##              int     person        mail        phone       online    id
## 73  1.624461e-03 0.17571278 0.356741488 2.011217e-01 2.647996e-01 29079
## 180 5.562930e-02 0.08200957 0.704636931 1.432091e-01 1.451516e-02 84977
## 183 3.542171e-03 0.42543367 0.310638428 7.940135e-02 1.809843e-01 84997
## 184 1.708388e-03 0.09312628 0.434117019 2.111959e-03 4.689364e-01 85001
## 192 3.756493e-06 0.99990821 0.000076864 7.021056e-07 1.045678e-05 85952
## 193 1.708388e-03 0.09312628 0.434117019 2.111959e-03 4.689364e-01 86052
## 194 3.243815e-04 0.36719093 0.516180098 2.868456e-04 1.160178e-01 86077
## 198 1.624461e-03 0.17571278 0.356741488 2.011217e-01 2.647996e-01 86345
## 206 3.542171e-03 0.42543367 0.310638428 7.940135e-02 1.809843e-01 86731
## 207 3.346076e-03 0.15611745 0.147691339 2.989647e-01 3.938804e-01 86800
##     target
## 73       3
## 180      3
## 183      3
## 184      3
## 192      3
## 193      3
## 194      3
## 198      3
## 206      3
## 207      3
{% endhighlight %}
 
However, it does not do as well predicting who gives online.
 

{% highlight text %}
##              int      person       mail        phone       online    id
## 29  0.0017083881 0.093126282 0.43411702 2.111959e-03 4.689364e-01  6636
## 47  0.0001027581 0.008821204 0.99106193 1.185995e-05 2.235854e-06 10046
## 70  0.0035421711 0.425433666 0.31063843 7.940135e-02 1.809843e-01 27512
## 75  0.0033460758 0.156117454 0.14769134 2.989647e-01 3.938804e-01 30896
## 80  0.0016244611 0.175712779 0.35674149 2.011217e-01 2.647996e-01 34686
## 81  0.0016244611 0.175712779 0.35674149 2.011217e-01 2.647996e-01 35702
## 106 0.0035421711 0.425433666 0.31063843 7.940135e-02 1.809843e-01 59474
## 115 0.0035421711 0.425433666 0.31063843 7.940135e-02 1.809843e-01 66903
## 124 0.0009403804 0.954315722 0.04346919 1.114532e-03 1.602176e-04 72788
## 128 0.0015277402 0.174877986 0.35842142 9.545509e-02 3.697177e-01 72858
##     target
## 29       4
## 47       4
## 70       4
## 75       4
## 80       4
## 81       4
## 106      4
## 115      4
## 124      4
## 128      4
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
