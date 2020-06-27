---
layout: post
title: "2018 Campaign Finance, Part II: A Closer Look"
date: 2020-03-16 16:00:00 -0400
---
######We continue our investigation of FEC campaign finance data sets. <!--more--> Observations include:

* *treasurers associated with nearly 100 committees,*
* *individuals contributing tens of millions of dollars to (super-)PACs,*
* *individuals appearing to exceed maximum limits to principal campaign committees,*
* *committees making highest total independent expenditures,*
* *who's receiving the independent expenditures and for what,*
* *visual representation of finances for opposing candidates, including...*
* *evidence that number of in-state individual contributors may be useful in predicting election outcome.*

Do you have domain knowledge of campaign finance regulations? We do not and would love to discuss. Please email contact@volsweep.com. Cheers, thanks for reading! &#8212;Rebecca


The [first post](https://blog.volsweep.com/articles/19/2018-Campaign-Finance_I) in this series was an overview of trends and exceptions in Congressional midterm contests with respect to party affiliation, incumbency status, and relative funding status. (Recap: incumbents usually lead in fundraising and win. There appear to be some patterns in the exceptions.) This post will be a more in-depth look at the full set of data that the FEC publishes.[^1] As before, all relevant code is in [this](https://github.com/volsweep/volsweep.github.io/tree/master/projects/FEC/2018) GitHub repo.[^2] See notebook links for full outputs too long to include here. Documentation will improve in the future! Please ask any/all question until then (and after).


### Data set 1: "House/Senate current campaigns"
> (notebook [here](https://github.com/volsweep/volsweep.github.io/tree/master/projects/FEC/2018/01b%20-%202018_HouseSenateCurrentCampaigns_withwinners.ipynb))

This data set has one candidate ID per row. We used this data set to construct the plots in the first post of this series, where we showed candidate fundraising status by contest and compared opponents. We know from this set the following breakdown of the top three contest "types" for each branch of Congress (compared close to election day):

*Senate contests*
* 58.8% had a Democratic incumbent ahead in fundraising,
* 8.8% had a Republican incumbent ahead in fundraising,
* 8.8% had a Republican challenger ahead in fundraising.

*House contests*
* 30.8% had a Republican incumbent ahead in fundraising,
* 24.0% had a Democratic incumbent ahead in fundraising,
* 14.6% had a Democratic incumbent running unopposed.


### Data set 2: "Candidate-committee linkages"
> (notebook [here](https://github.com/volsweep/volsweep.github.io/tree/master/projects/FEC/2018/02a%20-%202018_CandidateCommitteeLinkages_clean.ipynb))

This data set has one row per candidate-committee pairing (note that it does not contain committees that are not linked to candidates). You can see the ones linked to at least three candidates, including candidate info, by searching "list starts here" on [this](https://github.com/volsweep/volsweep.github.io/tree/master/projects/FEC/2018/03a%20-%202018_CommitteeMaster_clean.ipynb) page. The following candidates are linked to more than ten committees each: Tammy Baldwin, Sherrod Brown, Joe Donnelly, Heidi Heitkamp, Amy Klobuchar, Claire McCaskill, Bill Nelson, Jacky Rosen, Debbie Stabenow, and Jon Tester.


### Data set 3: "Committee master"
> (notebook [here](https://github.com/volsweep/volsweep.github.io/tree/master/projects/FEC/2018/03a%20-%202018_CommitteeMaster_clean.ipynb))

This data set has one row per committee. After deduplicating several columns, we found there are some treasurers associated with large numbers of committees, and some addresses associated with large numbers of committees. (A reminder that this is the low end count because committees linked to candidates not appearing on final ballots were removed during cleaning.) Here are some examples (committee counts in parentheses; only treasurers with two or more associated committees shown):

_Example #1_<br/>
**Address:** 228 S Washington St, Alexandria, VA 22314 (156)<br/>
**Treasurers:** Lisa Lisker (74), Keith Davis (37), and David Satterfield (36)<br/>

_Example #2_<br/>
**Address:** 918 Pennsylvania Ave SE, Washington, DC 20003 (112)<br/>
**Treasurers:** Judith Zamore (97), Kristin Solander (3), Ellen Tauscher (2), Megan Mielnik (2)<br/>

_Example #3_<br/>
**Address:** 824 S Milledge Ave, Athens, GA 30605 (101)<br/>
**Treasurers:** Paul Kilgore (96), Michael Goode (2), Megan Brown (2)<br/>

_Example #4_<br/>
**Address:** PO Box 26141, Alexandria, VA 22313 (95)<br/>
**Treasurers:** Christopher Marston (85), Brenda Hankins (4), (no treasurer listed) (2)<br/>


Search the phrase, "look here," in the [notebook](https://github.com/volsweep/volsweep.github.io/tree/master/projects/FEC/2018/03a%20-%202018_CommitteeMaster_clean.ipynb) for full lists of committee names by address and treasurer.


### Data set 4: "Contributions from committees to candidates & independent expenditures"
> (all of the following notebooks: [here](https://github.com/volsweep/volsweep.github.io/tree/master/projects/FEC/2018/04ai%20-%202018_CommitteeContributions_clean_withwinner.ipynb), [here](https://github.com/volsweep/volsweep.github.io/tree/master/projects/FEC/2018/04aii%20-%202018_CommitteeContributions_clean_withwinner.ipynb), [here](https://github.com/volsweep/volsweep.github.io/tree/master/projects/FEC/2018/04bi%20-%202018_CommitteeContributions_EDA1.ipynb), [here](https://github.com/volsweep/volsweep.github.io/tree/master/projects/FEC/2018/04bii%20-%202018_CommitteeContributions_EDA1.ipynb), and [here](https://github.com/volsweep/volsweep.github.io/tree/master/projects/FEC/2018/04bii%20-%202018_CommitteeContributions_EDA2.ipynb))

This data set has one contribution/independent expenditure (IE from now on) per row {% sidenote "1" "Data sets analyzed in this post found here: [https://www.fec.gov/data/browse-data/?tab=bulk-data](https://www.fec.gov/data/browse-data/?tab=bulk-data)" %}. The different types of contributions/independent expenditures are:

* "contribution made to nonaffiliated committee,"
* "independent expenditure advocating election of candidate,"
* "independent expenditure opposing election of candidate,"
* "in-kind contribution made to registered filer,"
* "coordinated party expenditure,"
* "election recount disbursement,"
* "communication cost against candidate (only for Form 7 filer)."

Some observations from the cleaning process: Invenergy PAC made 691 contributions and/or independent expenditures with no date given (only 3 additional had a date). Embraer Aircraft Holding Inc PAC made 154 contributions and/or independent expenditures with no date given. The Democratic Senatorial Campaign Committee (DSCC) received $306,644 total in contributions with no individuals' names given. The National Republican Senatorial Committee (NRSC) received $294,519 total in contributions with no individuals' names given.

Fig. 1 shows one dot per committee (mostly political action committees, or PACs) which made IE(s). The following spent at least $10MM with respect to either Republican-affiliated candidates, Democratic-affiliated candidates, or both (total amount in parentheses):

{% marginfigure "committees scatter" "https://blog.volsweep.com/assets/img/FECpt2/committees_log.png" "Fig. 1 PAC independent expenditures, Republican vs Democrat" %}   

* Congressional Leadership Fund ($125MM),
* SMP ($108MM),
* Senate Leadership Fund ($86MM),
* DCCC ($79MM)
* NRCC ($69MM),
* House Majority PAC ($59MM),
* NRSC ($42MM),
* Majority Forward ($40MM),
* DSCC ($40MM),
* Independence USA PAC ($38MM),
* New Republican PAC ($31MM),
* Priorities USA Action ($27MM),
* America First Action, Inc. ($26MM),
* Women Vote! ($24MM),
* DefendArizona ($16MM),
* LCV Victory Fund ($14MM),
* End Citizens United ($11MM).

Fig. 1 shows that most committees make IEs totaling from around $1,000 to $1MM. The committees seem to be spending with respect to candidates affiliated with both major parties, although not always on the same order of magnitude. (Note: This plot does not distinguish between IEs advocating vs opposing.) We also see that committees represented by yellower dots made IEs with respect a large number of candidates (100 or more).

Now, let's look at where these IEs are going. Each plot below represents, per recipient (i.e., the 'to' in each plot title), IEs advocating and/or opposing candidates. Each plot symbol represents one candidate. It's not immediately apparent, but the plots are sorted in decreasing order of total IE dollars received, from upper left corner to lower right corner (we left some out, go to notebook for full list & plots). Discussion below.

{% marginfigure "waterfront" "https://blog.volsweep.com/assets/img/FECpt2/ie_WaterfrontStrategies.png" "Fig. 2 Waterfront Strategies Independent Expenditures" %}

{% marginfigure "nebo" "https://blog.volsweep.com/assets/img/FECpt2/ie_NeboMedia.png" "Fig. 3 Nebo Media Independent Expenditures" %}

{% marginfigure "del ray" "https://blog.volsweep.com/assets/img/FECpt2/ie_DelRay.png" "Fig. 4 Del Ray Media Independent Expenditures" %}

{% marginfigure "bully pulpit" "https://blog.volsweep.com/assets/img/FECpt2/ie_BullyPulpitInteractive.png" "Fig. 5 Bully Pulpit Interactive Independent Expenditures" %}

{% marginfigure "SKDK" "https://blog.volsweep.com/assets/img/FECpt2/ie_SKDKnickerbocker.png" "Fig. 6 SKDKnickerbocker Independent Expenditures" %}

{% marginfigure "Facebook" "https://blog.volsweep.com/assets/img/FECpt2/ie_Facebook.png" "Fig. 7 Facebook Independent Expenditures" %}


**Waterfront Strategies**

Waterfront Strategies (Fig. 2) has no direct online presence. It received at least $246MM in IEs. As you can see in the plot, the advocating vs opposing split is highly partisan, with about 10% of IEs advocating Democratic-affiliated candidates (many of whom were challengers) and about 90% of IEs opposing Republican-affiliated candidates. Those three Democratic-affiliated incumbents standing out on the right (blue circles) are Bill Nelson ($7.1MM), Joe Manchin III ($4.6MM), and Joe Donnelly ($2.7MM). The largest total IEs to Waterfront Strategies came from SMP ($96.9MM), House Majority PAC ($49.7MM), Majority Forward ($39.5MM), and Women Vote! ($18.8MM).


**Nebo Media**

Nebo Media (Fig. 3) doesn't have a direct online presence, either. It received at least $112.7MM in IEs, the vast majority of it from the Congressional Leadership Fund (over $112MM). Almost 95% of the total IE dollar amount Nebo Media received went toward opposing candidates. Looking at the plot, the IEs _advocated_ Republican-affiliated candidates (mostly incumbents) and _opposed_ mostly Democratic-affiliated challengers. Interestingly, some Republican-affiliated candidates were opposed; they are Young Kim, Rodney Davis, and Dana Rohrabacher, who were all opposed by the Congressional Leadership Fund.


[**Del Ray Media**](http://delraymediabuying.com/)

Del Ray Media (Fig. 4) has a very minimal [online presence](http://delraymediabuying.com/). It received almost $54.4MM in IEs, 98% of which was in opposition to candidates. We see in the plot that there are three Republican-affiliated candidates advocated, whereas almost all the opposed candidates are Democratic-affiliated challengers. The IEs came from NRCC ($49.5MM) and NRSC ($4.8MM).


[**Bully Pulpit Interactive**](https://bpimedia.com/)

Bully Pulpit Interactive (BPI) (Fig. 5) has a pretty strong [online presence](https://bpimedia.com/). It received $41.7MM in IEs, $26.7MM of which came from Priorities USA Action. Other big spenders included Independence USA PAC ($5.9MM), NextGen Climate Action Committee ($3.9MM), Human Rights Campaign Equality Votes ($1.7MM), and LCV Victory Fund ($1.4MM). About 35% of IE dollars to BPI advocated candidates, and about 65% opposed. The U.S. Senate Florida contest saw a lot of IEs flowing to BPI, mostly from Priorities USA Action, with $4.5MM advocating incumbent Bill Nelson and $5.9MM opposing Rick Scott.


[**SKDKnickerbocker**](https://www.skdknick.com/)

[SKDKnickerbocker](https://www.skdknick.com/) (Fig. 6) received about $30.5MM in IEs, the overwhelming majority of which came from Independence USA PAC ($25.4MM). Others who made large IEs to SKDKnickerbocker are the Environmental Defense Action Fund ($1.5MM), LCV Victory Fund ($1.4MM), and Everytown for Gun Safety Victory Fund ($724K). Interestingly, Everytown for Gun Safety Victory Fund made a very large number of $851 IEs to SKDKnickerbocker in opposition of candidates. Looking at the plot, the Republican-affiliated incumbent in the middle stands out; this is Randy Hultgren, who had Independence USA PAC spend about $460K opposing him and $19K advocating him. Otherwise the plot is very partisan, with most advocating IEs made with respect to Democratic-affiliated challengers and most opposing IEs made with respect to Republican-affiliated incumbents.


[**Facebook**](https://www.facebook.com/gpa)

[Facebook, Inc.](https://www.facebook.com/gpa) (Fig. 7), received about $4.4MM in IEs, mostly from MoveOn.org Political Action ($2.7MM). Just over 80% of the total IE dollars to Facebook advocated candidates and the rest opposed. As you can see from the plot, proportionately more candidates had IEs both advocating and opposing them than in other plots we've just seen (i.e., the center of the plot is crowded).

The lefthand plots two sections down were constructed using this section's data set but are presented where they are in order to allow side-by-side comparisons.


### Data set 5: "Any transaction from one committee to another"
> (notebook [here](https://github.com/volsweep/volsweep.github.io/tree/master/projects/FEC/2018/06%20-%20CommitteeToCommittee.ipynb))

We took the subset of this data set containing any transfer to a principal campaign committees. The righthand plots in the next section were constructed using this data set.


### Data set 6: "Contributions by individuals"
> (notebook [here](https://github.com/volsweep/volsweep.github.io/tree/master/projects/FEC/2018/05a%20-%202018_IndividualContributions_clean1.ipynb))

This data set has one contribution from an individual per row. We had to do a lot of cleaning in this set in particular. Any names containing "anonymous", "unitemized", and/or anything like "hat pass" we switched to simply "Anonymous." The FEC rules state:

> *"An anonymous contribution of cash is limited to $50. Any amount in excess of $50 must be promptly disposed of and may be used for any lawful purpose unrelated to any federal election, campaign or candidate." [^4][^5]*

This doesn't seem to be the case, as $246,892 total across two contributions to Composition Roofers Local Union #30 PAC and $54,458 total across two contributions to Association for Firefighters PAC have no associated donor name(s). These appear to be above the limits allowed by the FEC.

In terms of known individual contributors, some make many small contributions and others make a few extremely large ones. Here are the 20 donors with the highest contribution totals (in parentheses) and any itemized totals over $1MM displayed:

* __Michael R Bloomberg, $85.9MM total:__ *Independence USA PAC ($54.3MM), SMP ($20MM), LCV Victory Fund ($5MM), Women Vote! ($1.96MM), VoteVets ($1.5MM), Planned Parenthood Votes ($1.5MM)*
* __Thomas F Steyer, $65.7MM total:__ *NextGen Climate Action Committee ($52.4MM), Need to Impeach ($12.1MM)*
* __Sheldon G Adelson, $61.9MM total:__ *Congressional Leadership Fund ($27.5MM), Senate Leadership Fund ($25MM), America First Action, Inc. ($5MM), New Republican PAC ($2.5MM), ESAFund ($1MM)*
* __Miriam O Adelson, $61.7MM total:__ *Congressional Leadership Fund ($27.5MM), Senate Leadership Fund ($25MM), America First Action, Inc. ($5MM), New Republican PAC ($2.5MM), ESAFund ($1MM)*
* __Richard Uihlein, $34.8MM total:__ *Restoration PAC ($8.20MM), Americas PAC ($5.4MM), Solutions for Wisconsin ($4.4MM), Club for Growth Action ($3.2MM), CFG Action Wisconsin ($2.5MM), Tea Party Patriots Citizens Fund ($2MM), CFG Action Missouri ($2MM)*
* __Selwyn Donald Sussman, $24.5MM total:__ *Priorities USA Action ($6.45MM), SMP ($5.25MM), House Majority PAC ($4.75MM), Women Vote! ($2.8MM), Democratic Grassroots Victory Fund ($1.6MM), Win Justice ($1.1MM)*
* __Kenneth C Griffin, $19.4MM total:__ *New Republican PAC ($10MM), Congressional Leadership Fund ($4.5MM), DefendArizona ($2MM), Future45 ($1MM)*
* __James Harris Simons, $19.4MM total:__ *House Majority PAC ($10MM), SMP ($6.8MM)*
* __George Soros, $17.4MM, total:__ *Priorities USA Action ($5MM), Win Justice ($5MM), SMP ($3.4MM)*
* __Stephen A Schwarzman, $12.7MM total:__ *Senate Leadership Fund ($8MM), Congressional Leadership Fund ($3.8MM)*
* __Fred J Eychaner, $11.4MM total:__ *SMP ($6MM), House Majority PAC ($4MM)*
* __Jeffrey P Bezos, $10.2MM total:__ *With Honor Fund, Inc ($10.1MM)*
* __Timothy Mellon, $10.1MM total:__ *Congressional Leadership Fund ($10MM)*
* __George M Marcus, $9.7MM total:__ *House Majority PAC ($5MM), SMP ($3MM)*
* __Reid G Hoffman, $8.5MM total:__ *House Majority PAC ($3.1MM), SMP ($2MM), Forward Majority Action ($1MM)*
* __Charles R Schwab, $8.1MM total:__ *Congressional Leadership Fund ($3.25MM), Senate Leadership Fund ($2MM), Future45 ($1MM)*
* __Karla T Jurvetson, $7.8MM total:__ *Women Vote! ($5.4MM), SMP ($1.1MM)*
* __Jeffrey S Yass, $7.6MM total:__ *Club for Growth Action ($3.8MM), Protect Freedom Political Action Committee Inc ($1.8MM)*
* __Bernard Marcus, $7.2MM total:__ *Senate Leadership Fund ($4MM)*
* __Seth A Klarman, $6.9MM total:__ *House Majority PAC ($2.5MM), SMP ($1.5MM)*


With the previous FEC blog post in mind, let's go through some contests' cumulative financial plots to see what the fundraising landscape was like leading up to election day. We'll keep an eye out for things like who has more opposition money spent against them (usually in the form of attack ads), who has a higher in-state to out-of-state individual contributions ratio, etc. The statistical modeling in a future post will help us quantify the significance of these observations; right now, we're exploring the scene. As a refresher, see the Senate contest fundraising overview plot [here](https://blog.volsweep.com/assets/img/FECpt1/senate_2018.png) and the House one (without contests where incumbents ahead in fundraising won) [here](https://blog.volsweep.com/assets/img/FECpt1/house_2018_unexpecteds.png). Remember, we're only looking at contests with one incumbent running (i.e., no open seats, no unopposed incumbents, and no contests with more than one incumbent (it did happen once in 2018 due to redistricting)). The faint vertical lines that are the same in every plot are FEC filing deadlines and election day.


_**Scenario: incumbent ahead in fundraising who won**_

As discussed in the previous FEC post, the most common scenario is when an incumbent leads in fundraising and wins. We'll go over one example of this to start.

**U.S. House, Alabama District 3 (AL-03)**

{% marginfigure "AL03_committee" "https://blog.volsweep.com/assets/img/FECpt2/contributions_AL03_committee.png" "Fig. 8(a)<br />AL-03 Committee contributions [(enlarge)](https://blog.volsweep.com/assets/img/FECpt2/contributions_AL03_committee.png)" %}

{% marginfigure "AL03_individual" "https://blog.volsweep.com/assets/img/FECpt2/contributions_AL03_individual.png" "Fig. 8(b)<br />AL-03 Individual contributions [(enlarge)](https://blog.volsweep.com/assets/img/FECpt2/contributions_AL03_individual.png)" %}

{% marginfigure "AL03_committeeTocommitee" "https://blog.volsweep.com/assets/img/FECpt2/contributions_AL03_cm2cm.png" "Fig. 8(c)<br />AL-03 Intercommittee contributions [(enlarge)](https://blog.volsweep.com/assets/img/FECpt2/contributions_AL03_cm2cm.png)" %}

Fig. 8(a) shows the incumbent's principal campaign committee, Mike Rogers for Congress, raised money consistently starting in January 2017. The highest contributors &#x2014; giving $10,000 each &#x2014; included PACs for Blue Cross Blue Shield, Northrop Grumman, Raytheon, Bechtel Group, General Atomics, General Dynamics, PeanutPAC, Harris Corp, and others. Everytown For Gun Safety Action Fund made an $851 IE to SKDKnickerbocker opposing Rogers.

Challenger Mallory Hagan's fundraising started over a year later, around March 2018. The campaign had an advocating IE from Vote Me Too PAC to Facebook for $39 and committee contributions (all under the maximum of $10K) from PACs for CWA-COPE, United Transportation Union, IBEW, End Citizens United, Seeking Justice, Brotherhood of Locomotive Engineers and Trainmen, and RWDSU COPE.

Now, looking at the individual contributions plot Fig. 8(b), we see that the incumbent got a big boost shortly after the campaign started, and then again after what looks like the challenger's campaign launch. The incumbent's proportion of in-state individual contribution dollars to total individual contribution dollars is very high (i.e., dashed red line closely mirrors solid red line). We can see that the out-of-state individual contributions raised by the incumbent are almost equal to in-state individual contributions raised by the challenger.

Fig. 8(c) shows a $150K infusion from the National Republican Congressional Committee to Mike Rogers for Congress pretty close to election day.

So! That is a review of a typical contest where an incumbent ahead in fundraising defeated a challenger and not a lot of independent expenditures were made. If something isn't clear, let us know in the comments section at the bottom and we'll clarify. Now, let's look at some of the contests discussed in the previous post that had unexpected outcomes.


_**Scenario: incumbent behind in fundraising who won**_

The two instances of this scenario in the Senate contests are Texas incumbent Ted Cruz's win over challenger/leading fundraiser Beto O'Rourke and New Jersey incumbent Bob Menendez's win over challenger/leading fundraiser Bob Hugin. There are many House contests with Republican incumbents satisfying this scenario; the only one with a Democratic incumbent is Pennsylvania District 8, where incumbent Matt Cartwright beat challenger/leading fundraiser John Chrin. The ones with Republican incumbents will appear in the appendix.

**U.S. Senate, Texas (TX_senate)**

{% marginfigure "TXsenate_commitee" "https://blog.volsweep.com/assets/img/FECpt2/contributions_TXsenate_committee.png" "Fig. 9(a)<br />TX_senate committee contributions [(enlarge)](https://blog.volsweep.com/assets/img/FECpt2/contributions_TXsenate_committee.png)" %}

{% marginfigure "TXsenate_individual" "https://blog.volsweep.com/assets/img/FECpt2/contributions_TXsenate_individual.png" "Fig. 9(b)<br />TX_senate individual contributions [(enlarge)](https://blog.volsweep.com/assets/img/FECpt2/contributions_TXsenate_individual.png)" %}

{% marginfigure "TXsenate_committeeTocommittee" "https://blog.volsweep.com/assets/img/FECpt2/contributions_TXsenate_cm2cm.png" "Fig. 9(c)<br />TX_senate Intercommittee contributions [(enlarge)](https://blog.volsweep.com/assets/img/FECpt2/contributions_TXsenate_cm2cm.png)" %}

Fig. 9(a) shows that IEs opposing challenger Beto O'Rourke began earlier than and also dwarf those opposing incumbent Ted Cruz. Big spenders opposing O'Rourke were: Texans Are to Main Street Media Group ($4.7MM, also $250K to Cross Screen Media and $26K to Prime Media Partners), ESAFund to Del Cielo Media ($1.4MM, also $150K to CD, $102K to Wilson Research Strategies, and $35K to McCarthy Hennings Whalen), Club for Growth Action to Target Enterprises ($1.1MM, also $29K to Prime Media Partners). The biggest spender opposing Cruz was Texas Forever, which paid $2.3MM to Waterfront Strategies.

Neither opponent personally contributed to the respective principal campaign committee. O'Rourke's, Beto for Texas, received contributions from about 70K individuals under the combined $5,400 FEC limit. Cruz's, Ted Cruz for Senate, received contributions from close to 40K individuals under the FEC limit.

Fig. 9(c) shows committee transfer totals to Beto for Texas including $4.6MM from the Texas Democratic Party, $36.9K from 2018 Senate Impact, and $16.8K from O'Rourke Washington Democratic Victory Fund. Transfers to Ted Cruz for Senate include $3.9MM from Ted Cruz Victory Committee, $260K from Cruz himself, $200K from Republican Party of Texas, $129K from 2018 Senators Classic Committee, and $124K from Cruz for President.


**U.S. Senate, New Jersey**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 10(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_NJsenate_committee.png">
      <img alt="New Jersey Senate committee contributions" src="{{ site.url }}/assets/FECpt2/contributions_NJsenate_committee.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 10(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_NJsenate_individual.png">
      <img alt="New Jersey Senate individual contributions" src="{{ site.url }}/assets/FECpt2/contributions_NJsenate_individual.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 10(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_NJsenate_cm2cm.png">
      <img alt="New Jersey Senate transfers between committees" src="{{ site.url }}/assets/FECpt2/contributions_NJsenate_cm2cm.png" style="width: 100%">
    </a>
  </div>
</div>

Fig. 10(a) shows that incumbent Mendendez raised contributions to nonaffiliated committees consistently across his term, while challenger Hugin started around February 2018. The IEs opposing Hugin took off in July 2018 and were from SMP to Waterfront Strategies ($7.6MM, also $50K to SCN Strategies), Patients for Affordable Drugs Action to Pier 91 Media ($2.4MM, also $853K to Trilogy Interactive and smaller amounts to Patients for Affordable Drugs Now, Politico, Sea of Reeds, and Insider NJ), Leadership Alliance to Ethica Media ($1.2MM), and Committee to Build the Economy to Switchboard Communications ($25K, also $2K to MMA Consultants). IEs opposing Menendez began later and came mostly from Integrity NJ ($5.7MM to Pinpoint Media, $370K to Advictory, $268K to Mottola Consulting, $156K to US Postmaster, and $100K to Red Maverick Media); also included are $158K from Americas PAC to Statenet, and smaller amounts from Citizens for a Stronger New Jersey to Majority Strategies and Tar Heel Targeting.

Menendez's principal campaign committee, Menendez for Senate, received contributions from approximately 5,000 individuals giving less than the combined FEC maximum of $5,400. Of those who exceeded, the ones contributing a total above $10,000 include:

* Lenard Liberman, Burbank CA, Liberman Broadcasting Inc. president, $29,700
* Maria Monzon, Norwood NJ, AM Real Estate Management manager, $29,200
* Joseph Daibes, Edgewater NJ, Daibes Enterprises project manager, $17,759
* Robert Galvin, Camden NJ, Holtec International CFO, $13,500
* Lawrence B Rasky, Jamaica Plain MA, Rasky Baerlein Strategic Comm Inc. CEO, $10,800
* John D Ek, San Pedro CA, self-employed consultant, $10,800.

Hugin personally contributed $73,477 to his own principal campaign committee, Bob Hugin for Senate Inc. In addition, at least 1,500 individuals contributed less than the combined FEC maximum. Those exceeding $10,000 are [here]({{ site.url }}/assets/FECpt2/bigdonors/mccaskill_bigdonors.png).

In Fig. 10(c) we see why Hugin appears so far ahead in fundraising from the plot in the previous blog post; he transferred $44MM across 24 instances to Bob Hugin for Senate Inc. New Jersey Republican State Committee also transferred $375K, and the Founders Committee transferred $26K. Menendez for Senate received $2.2MM from the Mendendez Victory Fund, and $38K from Blue Senate 2018.

All in all, Fig.s 10(a) & 10(b) resemble plots where the blue line is the incumbent and wins. Whatever financial edge Hugin shows in Fig. 10(c) was not utilized to sufficient effect.


**U.S. House, Pennsylvania District 8**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 11(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_PA08_committee.png">
      <img alt="Pennsylvania District 08 committee contributions" src="{{ site.url }}/assets/FECpt2/contributions_PA08_committee.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 11(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_PA08_individual.png">
      <img alt="Pennsylvania District 08 individual contributions" src="{{ site.url }}/assets/FECpt2/contributions_PA08_individual.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 11(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_PA08_cm2cm.png">
      <img alt="Pennsylvania District 08 transfers between committees" src="{{ site.url }}/assets/FECpt2/contributions_PA08_cm2cm.png" style="width: 100%">
    </a>
  </div>
</div>

Fig. 11(a) shows that incumbent Matt Cartwright dominated in committee contributions to nonaffiliated committees (the solid line); challenger John Chrin started raising in earnest around June 2018. IEs advocating Cartwright mirrored this with a delay and came mostly from SEIU COPE ($62.3K), and also from For Our Future, MoveOn.org Political Action, Communications Workers of America Working Voices, NEA Advocacy Fund, and Environment America Action Fund. The IEs opposing Cartwright were made by the NRCC ($375.9K to Del Ray Media, $144.7K to FP1 Strategies, and $35.8K to The Strategy Group) and Pennsylvania Pro-Life Federation PAC (<$100 each to Erdman Advertising Marketing and Design and SSS Printing).

Fig. 11(b) shows fairly competitive individual contribution fundraising. Cartwright spent no personal money on his campaign, and over 1,200 individuals contributed less than the combined FEC minimum (seven contributed over the minimum, anywhere from $5,450 to $8,100). Chrin contributed $521.8K to his own campaign committee, John Chrin for Congress, with fewer than 300 individuals contributing. We notice the large vertical jumps in John Chrin's solid red line, which suggests perhaps the number of individuals contributing in-state is lower than the number contributing in-state to Cartwright, even though Chrin's in-state dollar amount is larger. Cartwright seems to have gotten more out-of-state attention than Chrin.

Similarly to the Senate contest in New Jersey, Chrin as challenger made a relatively large transfer to his principal campaign committee ($1.87MM across 18 instances). The Republican Federal Committee of Pennsylvania also transferred $100K. In comparison, Cartwright transferred $140K to Cartwright for Congress. Again, this large transfer does not appear to have helped Chrin.


_**Scenario: incumbent behind in fundraising who lost**_

There are many House contests with Republican incumbents that satisfy this scenario; they will be included in the appendix. Aside from those, there are the three Senate contests below.

**U.S. Senate, Florida**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 12(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_FLsenate_committee.png">
      <img alt="Florida Senate committee contributions" src="{{ site.url }}/assets/FECpt2/contributions_FLsenate_committee.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 12(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_FLsenate_individual.png">
      <img alt="Florida Senate individual contributions" src="{{ site.url }}/assets/FECpt2/contributions_FLsenate_individual.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 12(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_FLsenate_cm2cm.png">
      <img alt="Florida Senate transfers between committees" src="{{ site.url }}/assets/FECpt2/contributions_FLsenate_cm2cm.png" style="width: 100%">
    </a>
  </div>
</div>

First of all, it's important to note that Rick Scott was Governor of Florida for the duration of this contest. Unlike many challengers, he did not have to overcome the hurdle of name recognition.

Fig. 12(a) shows incumbent Bill Nelson's fundraising for his campaign committee, Bill Nelson for U.S. Senate, grew at a pretty constant rate through election day. As soon as Rick Scott's fundraising appears around April 2018, the IEs with respect to Nelson take off. Top advocating expenditures include SMP to Waterfront Strategies ($6.0MM), Priorities USA Action to Bully Pulpit Interactive ($4.1MM), and Win Justice to Hard Knocks Field ($1.2MM). Top opposing expenditures include New Republican PAC to Matson Media ($29.3MM, also $191K to SRCP Media and $70K to Strategic Direction Com) and Americans for Prosperity Action Inc. (AFP Action) to In Pursuit Of ($1.1MM, also $227K to USPS and $130K to Presstige Printing). Nelson received contributions to nonaffiliated committees at a pretty constant rate from January 2017 through election day 2018. About $40K came from an entity/ies with a null name value; beyond that, $15K each came from M-PAC, Narragansett Bay PAC, American Federation of Teachers/AFL-CIO Committee on Political Education, Fearless for the People PAC, America Works Federal PAC, Impact, Forward Together PAC, MurphPAC, End Citizens United, and Narragansett Bay PAC.

The largest IEs opposing Scott were: SMP to Waterfront Strategies ($15.2MM), Priorities USA Action to Bully Pulpit Interactive ($5.4MM), Majority Forward to Waterfront Strategies ($4.5MM), VoteVets to Waterfront Strategies ($3.8MM), LCV Victory Fund to Waterfront Strategies ($1.8MM, also $500K to Bully Pulpit Interactive), Environmental Defense Action Fund to Waterfront Strategies ($757K), Giffords PAC to Deliver Strategies ($527K), Progressive Turnout Project to Deliver Strategies ($488K), and For Our Future to Deliver Strategies ($369K, also $324K to For Our Future Action Fund).

In Fig. 12(b) we see that 1) Bill Nelson's fundraising from individuals was very smooth over the course of his term, 2) he raised about equally in-state vs out-of-state, 3) Scott raised fiercely starting around April 2018, and 4) most of his funds came from in-state (because, as we're about to see, they were mostly from himself). Scott contributed $27.1MM as an individual and $42.9MM as a candidate to his principal campaign committee. Over 11,500 individuals contributed under the FEC maximum to his principal campaign committee, compared to approximately 22,000 under the maximum who contributed to Bill Nelson's.

Fig. 12(c) shows the following largest transfers to Bill Nelson for US Senate: $394K from Florida Senate 2018, $390K from Democratic Executive Committee of Florida, $148K from Florida Senate 2018 (Unitemized) (note: not clear what this distinction means), $99K from 2018 Senate Impact, and $91K from Senate 2018 Impact. Not clear without digging further whether "2018 Senate Impact" and "Senate 2018 Impact" are actually the same committee, but it seems likely. The largest transfers to Rick Scott for Florida were: $968K from Rick Scott Victory Fund, $16K from the Founders Committee, and $10K from Murray Energy PAC.


**U.S. Senate, Indiana**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 13(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_INsenate_committee.png">
      <img alt="Indiana Senate committee contributions" src="{{ site.url }}/assets/FECpt2/contributions_INsenate_committee.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 13(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_INsenate_individual.png">
      <img alt="Indiana Senate individual contributions" src="{{ site.url }}/assets/FECpt2/contributions_INsenate_individual.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 13(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_INsenate_cm2cm.png">
      <img alt="Indiana Senate transfers between committees" src="{{ site.url }}/assets/FECpt2/contributions_INsenate_cm2cm.png" style="width: 100%">
    </a>
  </div>
</div>

Fig. 13(a) shows a similar story to the senate contest in Florida, but here the IEs opposing incumbent Joe Donnelly started later. The largest of those were: Senate Leadership Fund to Main Street Media Group ($11.2MM, also $2.8MM to Cross Screen Media and $834K to Majority Strategies), NRSC to Del Ray Media ($2.9MM, also $1.3MM to Targeted Victory), America First Action Inc. to Red Eagle Media Group ($2.1MM), and National Rifle Association of America Political Victory Fund to Starboard Strategic ($1MM). The largest IEs advocating Donnelly were: SMP to Waterfront Strategies ($2.7MM, also $215K to Bully Pulpit Interactive and $105K to Shorr Johnson Magnus), Black Progressive Action Coalition to Revolution Field Strategies ($942K, also $233K to Resonance Campaigns), and Vote for Hoosier Values to JVA Campaigns ($723K).

The largest IEs opposing challenger Mike Braun were: SMP to Waterfront Strategies ($13.7MM, also $270K to Shorr Johnson Magnus), DSCC to Great American Media ($6.8MM), Majority Forward to Waterfront Strategies ($4.5MM), Priorities USA Action to Bully Pulpit Interactive ($3.5MM). The largest IEs advocating Braun were: Senate Leadership Fund to Main Street Media Group ($1.3MM, also $662K to Connection Strategy and $321K to Cross Screen Media), and NRSC to Del Ray Media ($191K).  

In terms of individual contributions, Joe Donnelly contributed $15K to Donnelly for Indiana. Approximately 9,000 individuals contributed less than the combined FEC maximum; of those who contributed over the maximum, none contributed over $10K. Mike Braun for Indiana received contributions from approximately 4K individuals under the combined FEC maximum. While Donnelly raised more individually contributed dollars than Braun, we notice that Braun raised more in-state dollars than Donnelly. It follows that Braun's ratio of in-state to out-of-state was higher than Donnelly's.

Fig. 13(c) shows Braun way ahead of Donnelly in terms of transfers between committees. The largest transfers to Donnelly for Indiana were from Senate Impact 2018 ($492K), Donnelly Victory Fund ($342K), and Democrats for Opportunity Fund ($96K). The largest transfers to Mike Braun for Indiana were from Mike Braun ($15.1MM), Indiana Republican State Committee ($342K), Braun Victory Committee ($202K), Team Braun Committee ($130K), Keep the Senate Red 2018 ($126K), and Missouri for GOP Senate Majority ($111K).


**U.S. Senate, Nevada**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 14(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_NVsenate_committee.png">
      <img alt="Nevada Senate committee contributions" src="{{ site.url }}/assets/FECpt2/contributions_NVsenate_committee.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 14(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_NVsenate_individual.png">
      <img alt="Nevada Senate individual contributions" src="{{ site.url }}/assets/FECpt2/contributions_NVsenate_individual.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 14(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_NVsenate_cm2cm.png">
      <img alt="Nevada Senate transfers between committees" src="{{ site.url }}/assets/FECpt2/contributions_NVsenate_cm2cm.png" style="width: 100%">
    </a>
  </div>
</div>

Fig. 14(a) shows that incumbent Dean Heller's fundraising was consistent throughout his term. Interestingly, unlike in other contests we've looked at, it appears that challenger Jacky Rosen started fundraising at the beginning of the incumbent's term. IEs opposing Heller began in earnest around June 2018 and continued strongly through the election. The largest of these were: SMP to Waterfront Strategies ($11.1MM, also $3MM to Blueprint Interactive), DSCC to Great American Media ($7.8MM), Majority Forward to Waterfront Strategies ($2.7MM), American Federation of State County and Municipal Employees PEOPLE to Waterfront Strategies ($1.5MM), and Women Vote! to Waterfront Strategies ($1.5MM). The largest IEs opposing Rosen were: Senate Leadership Fund to Main Street Media Group ($13.2MM, also $1.5MM to Arena Online, $852K to Arena Communications, $217K to DMM Media, $57K to Connection Strategy, and $33K to Richard Sales Media), NRSC to Flexpoint Media ($4.7MM, also $217K to OnMessage and $2,317 to i360).

In terms of Rosen's campaign, Fig. 14(b) shows Rosen for Nevada received contributions from at least 19K individuals below the combined FEC maximum. Those contributing over the limit include, but are not limited to the ones shown [here]({{ site.url }}/assets/FECpt2/bigdonors/rosen_bigdonors.png). Heller for Senate received contributions from at least 5,600 individuals below the combined FEC maximum. Those contributing over the limit to Heller for Senate include, but are not limited to, those shown [here]({{ site.url }}/assets/FECpt2/bigdonors/heller_bigdonors.png). Both opponents received more out-of-state dollars than in-state dollars from individual contributors.

Fig. 14(c) shows that Heller's campaign had transfers from other committees relatively consistently throughout his term, and Rosen's campaign had transfers starting around October 2017 that came in at a much higher rate. The largest ones to Rosen for Nevada were: $1MM from Rosen Victory Fund (also $34.5K from "Rosen Victory Fund (Unitemized)," not clear the distinction), $256K from Arizona Nevada Victory Fund, $185K from 2018 Senate Impact, and $94K from Nevada State Democratic Party. The largest transfers to Heller for Senate were: $153K from Heller Victory Committee, $101K from 2017 Senators Classic Committee, and $63K from 2018 Senators Classic Committee.


_**Scenario: incumbent ahead in fundraising who lost**_

The two instances of this scenario in Senate contests are the Missouri Senate contest where incumbent/leading fundraiser Claire McCaskill lost to challenger Joshua Hawley and the North Dakota Senate contest where incumbent/leading fundraiser Heidi Heitkamp lost to challenger Kevin Cramer. There are six instances of this scenario in House contests (all with Republican incumbents) and we will review them in the final section in a plot-reading exercise.

**U.S. Senate, Missouri**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 15(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_MOsenate_committee.png">
      <img alt="Missouri Senate committee contributions" src="{{ site.url }}/assets/FECpt2/contributions_MOsenate_committee.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 15(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_MOsenate_individual.png">
      <img alt="Missouri Senate individual contributions" src="{{ site.url }}/assets/FECpt2/contributions_MOsenate_individual.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 15(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_MOsenate_cm2cm.png">
      <img alt="Missouri Senate transfers between committees" src="{{ site.url }}/assets/FECpt2/contributions_MOsenate_cm2cm.png" style="width: 100%">
    </a>
  </div>
</div>

The first thing we notice looking at the committee contributions in Fig. 15(a) is that there were a _lot_ of IEs made to oppose candidates. IEs opposing challenger Joshua Hawley picked up pace around February 2018, while IEs opposing incumbent Claire McCaskill came in fast and strong around July 2018 and never let up. The largest IEs opposing Hawley were: SMP to Waterfront Strategies ($15.8MM), Women Vote! to Waterfront Strategies ($4.3MM), Priorities USA Action to Bully Pulpit Interactive ($3.7MM), and Majority Forward to Waterfront Strategies ($2.8MM). The largest IEs opposing McCaskill were: Senate Leadership Fund to Main Street Media Group ($16.1MM), NRSC to National Media Research Planning & Placement ($4.0MM), Americans for Prosperity to In Pursuit Of ($3.9MM), Senate Leadership Fund to Arena Online ($3.5MM), CFG Action Missouri (Club for Growth) to Ax Media ($2.9MM), America First Action, Inc. to Red Eagle Media Group ($2.2MM), NRSC to Cavalry ($1.1MM), and Missouri Rising Action to Strategic Media Services ($1.1MM).

Looking at the individual contributions in Fig. 15(b), we see that incumbent Claire McCaskill dominated overall; however, notice that her campaign has far more out-of-state dollars than in-state, and that Hawley's total individual contributions almost equal McCaskill's in-state contributions. Since the vast majority of out-of-state contributors won't participate in an election for a Congressperson in the end, one wonders whether in-state contributions are a better proxy for support at the polls than overall contributions.

McCaskill as a candidate contributed $41K. As an individual she contributed $134K. About 28K individuals contributed under the FEC combined limit of $5,400 ($2,700 (primary) + $2,700 (general) = $5,400). More than a few others contributed over the FEC limit; see some of them [here]({{ site.url }}/assets/FECpt2/bigdonors/mccaskill_bigdonors.png). (Note that many contributed $21,600, which in the code you can see is the sum of eight one-time contributions, and that $2,700 x 8 = $21,600.)


Hawley contributed no money to his own principal campaign committee while ~6,000 individuals did amounts totaling under the combined FEC limit. Individuals contributing total amounts over the limit include:

* Larry Potterfield, Columbia MO, Midway USA CEO, $10,800
* Paul Atkins, Arlington VA, Patomak Global Partners LLC consultant, $10,800
* William H Darr, Springfield MO, ADF exec VP, $10,800
* Harry M Cornell, Carthage MO, retired, $10,400
* Sharon J Herschend, Branson MO, Herschend Family Entertainment CEO, $10,000.

The main committee transfers to McCaskill for Missouri in Fig. 15(c) were $1.7MM total from McCaskill Victory Fund, $900K total from Missouri Democratic State Committee, $176K total from McCaskill 2018 Victory, $167K total from Justice 2018, and $116K total from Senate 2018 Impact. The main committee transfers to Josh Hawley for Senate were $805K total from Hawley Victory Committee, $181K total from Hawley Win Fund, $72K total from Indiana/Missouri Victory Committee, $49K total from Protecting the Majority Committee, and $46K total from Strengthen the Senate Majority 2018.


**U.S. Senate, North Dakota**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 16(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_NDsenate_committee.png">
      <img alt="North Dakota Senate committee contributions" src="{{ site.url }}/assets/FECpt2/contributions_NDsenate_committee.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 16(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_NDsenate_individual.png">
      <img alt="North Dakota Senate individual contributions" src="{{ site.url }}/assets/FECpt2/contributions_NDsenate_individual.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 16(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_NDsenate_cm2cm.png">
      <img alt="North Dakota Senate transfers between committees" src="{{ site.url }}/assets/FECpt2/contributions_NDsenate_cm2cm.png" style="width: 100%">
    </a>
  </div>
</div>

Fig. 16(a) shows that IEs opposing each candidate picked up around early summer 2018 and dwarfed any IEs advocating the candidates. Big spenders opposing incumbent Heidi Heitkamp were the Senate Leadership Fund to Main Street Media Group ($2.3MM) and NRSC to National Media Research Planning & Placement ($2.1MM). Big spenders opposing challenger Kevin Cramer were SMP to Waterfront Strategies ($2.7MM), DSCC to Great American Media ($2.0MM), and Majority Forward to Waterfront Strategies ($1.1MM).

Neither opponent spent personal money on the campaigns. Heitkamp's principal campaign committee, Heidi for Senate, had around 19K individuals contributing below the combined $5,400 FEC limit. Individuals exceeding that limit include, but are not limited to those listed [here]({{ site.url }}/assets/FECpt2/bigdonors/heitkamp_bigdonors.png). Cramer's principal campaign committee, Cramer for Senate, had about 2.5K individuals contributing under the combined $5,400 FEC limit. Individuals contributing over the limit include, but are not limited to those listed [here]({{ site.url }}/assets/FECpt2/bigdonors/cramer_bigdonors.png).

Fig. 16(b) shows that the opponents had about the same amount of in-state individual contribution dollars, and that out-of-state individual contributions made up a very high proportion of each (particularly for Heitkamp).

Fig. 16(c) shows committee transfers totals including North Dakota Democratic NLP Party's $3.2MM across 20 instances to Heidi for Senate; also included in that curve are Minnesota Democratic-Farmer-Labor Party ($500K), Heidi Victory Fund ($381K), Missouri Democratic State Committee ($200K), Indiana Democratic Congressional Victory Committee ($200K), and others. Cramer for Senate received $397K from Cramer Victory Fund, $118K from Keep the Senate Red 2018, and others <$100K each.


**U.S. House, California District 21**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 17(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_CA21_committee.png">
      <img alt="California district 21 committee contributions" src="{{ site.url }}/assets/FECpt2/contributions_CA21_committee.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 17(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_CA21_individual.png">
      <img alt="California district 21 individual contributions" src="{{ site.url }}/assets/FECpt2/contributions_CA21_individual.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 17(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_CA21_cm2cm.png">
      <img alt="California district 21 transfers between committees" src="{{ site.url }}/assets/FECpt2/contributions_CA21_cm2cm.png" style="width: 100%">
    </a>
  </div>
</div>

Fig. 17(a) shows incumbent David Valadao had a sizeable lead in contributions to nonaffiliated committees. IEs opposing him started a tiny bit in June 2017 and then shot up starting around September 2018. The largest ones were: Protect Our Kids and Our Healthcare PAC to AKPD Message and Media ($333K, also $216K to the Strategy Group and $34K to David Binder Research), House Majority PAC to Waterfront Strategies ($267K), and Red to Blue CA to Mission Control ($117K). Not many IEs against challenger TJ Cox: NRCC to Del Ray Media ($332K) and two <$200 from National Rifle Association of America Political Victory Fund to Master Print and the Herald Group.

Cox personally contributed $8K to his campaign. Fig. 17(b) shows Cox's individual contributions really start to take off around April 2018. Both opponents had a high proportion of in-state dollars; Cox's were slightly lower but as we are about to see, dollars are not directly proportional to individuals. Approximately 2K individuals contributed to TJ Cox for Congress under the FEC maximum, with another seven individuals contributing from $5,598 to $9,400. Valadao for Congress had just over 1K individual contributors under the FEC maximum, with another thirteen over it including five at $10K and then the highest:

* William Lazzerini, Bakersfield CA, Advance Beverage Company principal, $38,000
* Robert Smittcamp, Fresno CA, Lyon Magnus president, $25,400
* Edward M Kashian, Fresno CA, Lance Kashian Enterprises partner, $25,000.

Fig. 17(c) shows no committees transferred in assistance for the incumbent. TJ Cox transferred $150K one-time as "Terrence J Cox" and another $65K across seven instances as "Terrance J Cox". California Candidates Victory Fund transferred $133K. All other transfers were under $100K. This contest looks like effective IEs opposing the incumbent were made while no committees transferred in to assist, and likely more individuals contributed to the challenger.


**U.S. House, Florida District 26**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 18(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_FL26_committee.png">
      <img alt="Florida district 26 committee contributions" src="{{ site.url }}/assets/FECpt2/contributions_FL26_committee.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 18(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_FL26_individual.png">
      <img alt="Florida district 26 individual contributions" src="{{ site.url }}/assets/FECpt2/contributions_FL26_individual.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 18(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_FL26_cm2cm.png">
      <img alt="Florida district 26 transfers between committees" src="{{ site.url }}/assets/FECpt2/contributions_FL26_cm2cm.png" style="width: 100%">
    </a>
  </div>
</div>

Fig. 18(a) shows IEs in this contest started off with some advocating incumbent Carlos Curbelo and then a whole lot more both advocating and opposing challenger Debbie Mucarsel-Powell. Advocating Mucarsel-Powell were: DCCC to Visionary Strategy and Insights ($4.1MM, also $120K to Trilogy Interactive and $57K to 4C Partners), and others under $1MM. The largest opposing Mucarsel-Powell were: Congressional Leadership Fund to Nebo Media ($2.8MM) and NRCC to Del Ray Media ($2.3MM, also $295K to National Media Digital, $79K to SRCP Media, $21K to the Factor, and $4.8K to IMGE).

Mucarsel-Powell contributed $1K as a candidate to Debbie for Congress. Her campaign had about 1,000 in-state and just over 1,500 out-of-state individuals contributing. Eleven individuals contributed over the combined FEC maximum; those contributing over $10K were:

* Nelson Helm, Louisville KY, retired, $11,300
* Darielle Linehan, Towson MD, not employed, $10,800
* Judy Blume, Key West FL, writer, $10,000.

Approximately 800 in-state and 700 out-of-state individuals contributed to Curbelo for Congress. Fourteen contributed over the FEC maximum; those contributing over $10K were:

* Armando Codina, Coral Gables FL, The Codina Group chairman, $20,800
* David L Flory, Miami Beach FL, retired, $20,000
* Edward E Haddock, Winter Park FL, Full Sail University CEO, $15,000
* Christina Leyva, Coral Gables FL, Garcia & Garcia CPA, $10,800.

These individual contribution figures are interesting, as in the previous contest we looked at, because although the challenger's in-state individual contributions dollars are lower than the incumbent's, we see that the incumbent's average contribution must have been higher because there are actually more in-state individuals contributing to the challenger. She also appears to have gotten out-of-state attention starting in April 2018 which rode through to election day.


**U.S. House, Georgia District 6**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 19(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_GA06_committee.png">
      <img alt="Georgia District 06 committee contributions" src="{{ site.url }}/assets/FECpt2/contributions_GA06_committee.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 19(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_GA06_individual.png">
      <img alt="Georgia District 06 individual contributions" src="{{ site.url }}/assets/FECpt2/contributions_GA06_individual.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 19(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_GA06_cm2cm.png">
      <img alt="Georgia District 06 transfers between committees" src="{{ site.url }}/assets/FECpt2/contributions_GA06_cm2cm.png" style="width: 100%">
    </a>
  </div>
</div>

These plots don't look like any we've seen so far! The major IEs opposing the incumbent, Karen Handel, were from DCCC to the following: Screen Strategies Media ($4.4MM), Left Hook Communications ($547K), and Sage Media Planning and Placement ($134MM). The major IEs advocating Handel were from Ending Spending Inc to Mentzer Media Services ($1.1MM, also $106K to DDC and $76K to Nasica Tactical), US Chamber of Commerce to Revolution Media Group ($650K), and NRCC both to Red Eagle Media Group ($426K) and to FP1 Strategies ($270K). The largest IEs advocating the challenger, Lucia McBath, were mostly from Everytown for Gun Safety Victory Fund ($2.1MM to Canal Partners Media, $705K to Bully Pulpit Interactive, $317K to the Pivot Group) and Everytown for Gun Safety Action Fund ($660K to Waterfront Strategies, $285K to Bully Pulpit Interactive, and $268K to Mission Control).

McBath contributed $25 as a candidate to Friends of Lucy McBath Inc. The committee had a little over 600 in-state and about 1,000 out-of-state individual contributors. Eleven contributed over the combined FEC maximum, all of whom contributed less than $10K. Handel for Congress Inc. had about 2,300 in-state and over 3,000 out-of-state individual contributors, many of whom contributed over the combined FEC maximum (40+). Those contributing over $10K were:

* Clay Newman, Richmond Hill GA, Integrated Power Solutions CEO, $16,000
* Patricia Terwilliger, Atlanta GA, photographer, $13,500
* Bart Whitaker, Mabelton GA, Whitaker Oil Company owner, $10,800
* Gary W Rollins, Atlanta GA, Rollins Inc. CEO, $10,800
* Kathleen Rollins, Atlanta GA, homemaker, $10,800
* Patria Henssler, Kennesaw GA, G W Henssler & Associates LTD CPA, $10,800
* Steve Chaudoin, Atlanta GA, retired, $10,800
* Karen Wright, Mount Vernon OH, Ariel Corporation president/CEO, $10,800
* James Van Epps, Roswell GA, Van Epps LLC, $10,800
* William Lako, Kennesaw GA, Henssler Financial adviser, $10,800
* David Hanna, Atlanta GA, Atlanticus Holdings CEO, $10,800
* G Niles Bolton, Atlanta GA, Niles Bolton Associates Inc architect, $10,600
* Roger Santi, Alpharetta GA, Santi & Associates PC CPA, $10,093
* David N Farr, St Louis MO, Emerson Electric Co executive, $10,000.

Something strange is going on here with the difference in number of in-state contributors. It does look like the challenger had momentum going into election day, but the disparity was still the biggest we've seen so far. In terms of the committee transfers to Friends of Lucia McBath Inc. in Fig. 19(c), those were mostly $40K from Battleground Victory Fund 2018 and $11K from Lucy McBath.


**U.S. House, Illinois District 6**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 20(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_IL06_committee.png">
      <img alt="Illinois District 06 committee contributions" src="{{ site.url }}/assets/FECpt2/contributions_IL06_committee.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 20(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_IL06_individual.png">
      <img alt="Illinois District 06 individual contributions" src="{{ site.url }}/assets/FECpt2/contributions_IL06_individual.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 20(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_IL06_cm2cm.png">
      <img alt="Illinois District 06 transfers between committees" src="{{ site.url }}/assets/FECpt2/contributions_IL06_cm2cm.png" style="width: 100%">
    </a>
  </div>
</div>

Here, we see the IEs opposing the challenger, Sean Casten, began earlier but ended up totaling significantly less than those opposing incumbent Peter Roskam. The largest ones opposing Casten were Congressional Leadership Fund to Nebo Media ($2.75MM) and the New Prosperity Foundation to XPS Professional Services ($435K). The largest ones opposing Roskam were DCCC to Sage Media Planning and Placement ($1.98MM) and House Majority PAC to Waterfront Strategies ($1.9MM).

Fig. 20(b) shows Casten got a bigger boost from out-of-state individual contributions starting around August 2018. Casten for Congress had about 3K in-state and over 1,500 out-of-state individual contributors, which leads us to believe that the out-of-state average was higher since the total dollars look about equal for in-state and out-of-state. In-state contributor employers include the University of Chicago, Northwestern University, Kirkland & Ellis LLP, and Argonne National Laboratory. Out-of-state contributor employers include Google, Stanford University, Harvard Univerity, and Dartmouth College. Eighteen individuals contributed over the combined FEC maximum; those contributing over $10K were:

* Mary Jo G Schuler, Oak Park IL, not employed, $13,500
* Frederick Andreae, Bentonville VA, self-employed architect, $11,300
* Anthony R Rossi, Hinsdale IL, RMK Management Corp president, $10,800.

Roskam for Congress Committee had just over 2K in-state and just over 1K out-of-state. In-state contributor employers include Northwestern Mutual, Madison Dearborn Partners, DuPage Medical Group, Wheaton Eye Clinic, and Associated Urological Specialists LLC. Out-of-state contributor employers include The Hartford, Podesta Group, Capitol Counsel LLC, Neurological Surgery PC, Venn Strategies, and Breakthru Beverage Group. Twenty-three individuals contributed over the combined FEC maximum; those contributing over $10K were:

* David W Grainger, Lake Forest IL, W W Grainger Inc. chairman, $16,200
* Erik Fyrwald, Hinsdale IL, Syngenta CEO, $16,200
* Oswaldo Palomo, Fairfield CT, Public Sphere Inc principal, $10,800
* James E Bramsen, Barrington IL, Spraying Systems Co CEO, $10,800
* Thomas M Sodeika, Oakbrook Terrace IL, Precision Payroll of America president/CEO, $10,800
* Charles W Pollard, Carol Stream IL, retired, $10,000.

Fig. 20(c) shows significantly more dollars transferred to the challenger's committee than the incumbent's. By far the largest transfer to Casten was $1.1MM from Casten himself, across eleven instances. The largest to Roskam were far smaller: $81K from Roskam Victory Committee and $45K from GOP Majority Victory Fund.


**U.S. House, Oklahoma District 5**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 21(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_OK05_committee.png">
      <img alt="Oklahoma District 05 committee contributions" src="{{ site.url }}/assets/FECpt2/contributions_OK05_committee.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 21(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_OK05_individual.png">
      <img alt="Oklahoma District 05 individual contributions" src="{{ site.url }}/assets/FECpt2/contributions_OK05_individual.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
  </div>
</div>

In Fig. 21(a) We see a last-minute injection of IE advocating the challenger, Kendra Horn, which was $452K from Independence USA PAC to SKDKnickerbocker. IEs advocating incumbent Steven Russell were mostly from With Honor Fund Inc.: $94K to Red Eagle Media Group and $18.5K to Poolhouse.

Horn blew away Russell in overall individual contribution dollars, and in particular in-state ones. Kendra Horn for Congress had approximately 1K in-state and just over 300 out-of-state individual contributors. In-state contributor employers include Phillips Murrah, Oklahoma City University, Crowe & Dunlevy, and Oklahoma Heart Hospital; out-of-state contributor employers include Keller Williams, Cultural Heritage Partners, Ploughshares Fund, and Sony Pictures Television. Fourteen individuals contributed over the combined FEC maximum in totals ranging from $5,425 to $8,100; all were in-state.

Steve Russell for Congress had around 250 in-state and 100 out-of-state individual contributors. In-state contributor employers include OGE Energy, "Indian Tribe," Devon Energy, and Love's Travel Stops. Out-of-state contributor employers include the Lanier Law Firm, "Indian Tribe," and Cornerstone Government Affairs. Two individuals contributed over the combined FEC maximum; both $8,100, one in-state and one out-of-state.

Note that Fig. 21(c) isn't shown because no transfers were recorded.


**U.S. House, Utah District 4**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 22(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_UT04_committee.png">
      <img alt="Utah District 04 committee contributions" src="{{ site.url }}/assets/FECpt2/contributions_UT04_committee.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 22(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_UT04_individual.png">
      <img alt="Utah District 04 individual contributions" src="{{ site.url }}/assets/FECpt2/contributions_UT04_individual.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 22(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_UT04_cm2cm.png">
      <img alt="Utah District 04 transfers between committees" src="{{ site.url }}/assets/FECpt2/contributions_UT04_cm2cm.png" style="width: 100%">
    </a>
  </div>
</div>

Fig. 22(a) shows something we haven't seen yet; IEs advocating the incumbent increasingly steadily across the term (and then faster closer to election day). These IEs advocating incumbent Mia Love were mostly from VIGOP (Virgin Islands Republican Party), including: $386K to Consolidated Mailing Services, $252K to Forthright Strategies, $152K to Direct Support Services, $44K to Legacy List Management, $38K to Integram, $28K to DSSI, and $14K to Magic Box Group. Another large one was from Congressional Leadership Fund to Nebo Media ($344K).

The IEs opposing the candidates were all made starting in October 2018. The largest ones opposing Love were: Patriot Majority USA to Waterfront Strategies ($668K, also $8.8K to Three Point Media), DCCC to Amplify Media ($200K, also $13K to Murphy Vogel Askew Reilly), and LCV Victory Fund to Deliver Strategies ($19K). The IEs opposing challenger Ben McAdams were: Congressional Leadership Fund both to Nebo Media ($801K) and Ascent Media ($35K), and National Rifle Association of America Political Victory Fund to Prolist ($5K).

 Fig. 22(b) shows McAdams with a solid margin over Love starting from February 2018 in total individual contribution dollars. His lead over her in in-state individual contribution dollars is gigantic. Friends of Ben McAdams had about 1,600 in-state and 600 out-of-state individual contributors. In-state contributor employers include University of Utah, Salt Lake County, Intermountain Healthcare, and Parsons Behle & Latimer. Out-of-state contributor employers include Williams & Connolly LLP, Google, Subject Matter, Miller & Chevalier, and Akin Gump. Five individuals exceeded the combined FEC maximum in amounts ranging from $5,500 to $8,100.

 Friends of Mia Love had at least 325 in-state and over 4,100 out-of-state individual contributors. In-state contributor employers include Doterra, Enerbank USA, Symphony Homes, and 4Life Research. Out-of-state contributors were mostly not employed/retired/self-employed. Seven individuals exceeded the combined FEC maximum in amounts ranging from $7,402 to $8,100.

 Fig. 22(c) shows no transfers to McAdams's committee. Friends of Mia Love received large transfers from Utah Republican Party ($123K) and Love Victory Committee ($112K).


**U.S. House, Virginia District 10**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 23(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_VA10_committee.png">
      <img alt="Virginia District 10 committee contributions" src="{{ site.url }}/assets/FECpt2/contributions_VA10_committee.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 23(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_VA10_individual.png">
      <img alt="Virginia District 10 individual contributions" src="{{ site.url }}/assets/FECpt2/contributions_VA10_individual.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 23(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/contributions_VA10_cm2cm.png">
      <img alt="Virginia District 10 transfers between committees" src="{{ site.url }}/assets/FECpt2/contributions_VA10_cm2cm.png" style="width: 100%">
    </a>
  </div>
</div>

Fig. 23(a) shows there was a battle of opposing IEs starting around August 2018. The largest IE opposing challenger Jennifer Wexton by far was NRCC to Del Ray Media ($4.4M); NRCC also spent $490K to National Media Digital, $110K to Prime Media Partners, and $35K to IMGE. The large IEs opposing incumbent Barbara Comstock were: DCCC to Amplify Media ($2.6MM, also $276K to GPS Impact and $32K to Murphy Vogel Askew Reilly), Independence USA PAC to SKDKnickerbocker ($1.2MM), and Giffords PAC to the Campaign Group ($980K).

Fig. 23(b) is another case where the challenger's total (and in-state, specifically) individual contribution dollars gained momentum toward the end of the summer and continued through election day. Wexton for Congress's in-state dollars alone are essentially equal to Comstock's total dollars. Wexton had approximately 3,200 in-state and 2,000 out-of-state individual contributors. Nine of them contributed over the combined FEC maximum in amounts ranging from $5,800 to $9,400. The out-of-state curve also shows that she was getting attention outside of Virginia.

Comstock for Congress had about 2,000 in-state and over 1,000 out-of-state individual contributors. Ten of them contributed over the combined FEC maximum; the ones contributing over $10K were:

* Ann Holtzman, Mount Jackson VA, Holtzman Oil Co. executive, $14,600
* Pamela F Bayliss, Winchester VA, homemaker, $10,800
* Tatnall Lea Hillman, Aspen CO, retired, $10,800
* Michael Dendy Young, Delaplane VA, McLean Capital partner, $10,800.

The largest transfers to Comstock for Congress shown in Fig. 23(c) are: Comstock Victory Fund ($349K), Protect the House ($93K), and Winning Women Victory 2018 ($72K). The largest of the tiny ones to Wexton are Digidems Committee ($23K) and Virginia House Victory Fund ($5K).


### Conclusion

This post was long and varied. Thanks for sticking with it! The main takeaways are:

* there is a relatively small number of individuals contributing disproportionately large amounts of money into elections;<br/>

* many, many committees use the same treasurers (whatever the significance);<br/>

* Facebook received the most independent expenditures advocating & opposing the same Democratic and Republican candidates (other recipients received more globally partisan expenditures);<br/>

* exceeding the combined primary/general FEC individual contribution limit of $5,400 seems very common;<br/>

* independent expenditures opposing candidates appear to be effective;<br/>

* total in-state individual contributors (approximate) appears to be a better predictor of election outcome than total in-state individual dollars contributed (deduplication of contributors done on first name/last name/employer).<br/>

We need to take the info from here and add it to the models we've started building to predict election outcome. There will also be a post, possibly sooner than the prediction one, on the current state of finances in 2020 contests. If you have any thoughts or insight about what's here, please let us know either in the comments below or at contact@volsweep.com. Reminder that every calculation mentioned here is shown in the code notebooks, plus many more not mentioned. Thanks again for reading. &#8212;_Team VolSweep_


### Appendix

If you remember from [this]({{ site.url }}/assets/FECpt1/house_2018_unexpecteds.png) plot in the previous FEC blog post, there is some pattern going on where Democratic challengers in House contests seem to win when the incumbent's funding is over ~$3MM, and lose when the incumbent's funding is below that. We're going to display ten instances where the incumbent was behind and lost and ten where the incumbent was behind and won. Each section is sorted in decreasing order of winner's total funds raised.

_**Scenario: incumbent behind in fundraising who lost**_

**U.S. House, New York District 19**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 24(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/NY19a.png">
      <img alt="NY_19 committee contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/NY19a.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 24(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/NY19b.png">
      <img alt="NY_19 individual contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/NY19b.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 24(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/NY19c.png">
      <img alt="NY_19 transfers between committees" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/NY19c.png" style="width: 100%">
    </a>
  </div>
</div>

**U.S. House, California District 25**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 25(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/CA25a.png">
      <img alt="CA_25 committee contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/CA25a.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 25(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/CA25b.png">
      <img alt="CA_25 individual contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/CA25b.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 25(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/CA25c.png">
      <img alt="CA_25 transfers between committees" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/CA25c.png" style="width: 100%">
    </a>
  </div>
</div>

**U.S. House, California District 10**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 26(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/CA10a.png">
      <img alt="CA_10 committee contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/CA10a.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 26(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/CA10b.png">
      <img alt="CA_10 individual contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/CA10b.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 26(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/CA10c.png">
      <img alt="CA_10 transfers between committees" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/CA10c.png" style="width: 100%">
    </a>
  </div>
</div>

**U.S. House, Michigan District 08**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 27(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/MI08a.png">
      <img alt="MI_08 committee contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/MI08a.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 27(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/MI08b.png">
      <img alt="MI_08 individual contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/MI08b.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 27(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/MI08c.png">
      <img alt="MI_08 transfers between committees" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/MI08c.png" style="width: 100%">
    </a>
  </div>
</div>

**U.S. House, Virginia District 07**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 28(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/VA07a.png">
      <img alt="VA_07 committee contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/VA07a.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 28(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/VA07b.png">
      <img alt="VA_07 individual contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/VA07b.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 28(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/VA07c.png">
      <img alt="VA_07 transfers between committees" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/VA07c.png" style="width: 100%">
    </a>
  </div>
</div>

**U.S. House, California District 45**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 29(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/CA45a.png">
      <img alt="CA_45 committee contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/CA45a.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 29(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/CA45b.png">
      <img alt="CA_45 individual contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/CA45b.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 29(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/CA45c.png">
      <img alt="CA_45 transfers between committees" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/CA45c.png" style="width: 100%">
    </a>
  </div>
</div>

**U.S. House, New Jersey District 03**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 30(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/NJ03a.png">
      <img alt="NJ_03 committee contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/NJ03a.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 30(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/NJ03b.png">
      <img alt="NJ_03 individual contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/NJ03b.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 30(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/NJ03c.png">
      <img alt="NJ_03 transfers between committees" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/NJ03c.png" style="width: 100%">
    </a>
  </div>
</div>

**U.S. House, Minnesota District 03**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 31(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/MN03a.png">
      <img alt="MN_03 committee contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/MN03a.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 31(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/MN03b.png">
      <img alt="MN_03 individual contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/MN03b.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 31(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/MN03c.png">
      <img alt="MN_03 transfers between committees" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/MN03c.png" style="width: 100%">
    </a>
  </div>
</div>

**U.S. House, New Jersey District 07**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 32(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/NJ07a.png">
      <img alt="NJ_07 committee contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/NJ07a.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 32(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/NJ07b.png">
      <img alt="NJ_07 individual contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/NJ07b.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 32(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/NJ07c.png">
      <img alt="NJ_07 transfers between committees" src="{{ site.url }}/assets/FECpt2/appendix/incumbentlost/NJ07c.png" style="width: 100%">
    </a>
  </div>
</div>


_**Scenario: incumbent behind in fundraising who won**_

**U.S. House, Kentucky District 06**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 33(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/KY06a.png">
      <img alt="KY_06 committee contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/KY06a.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 33(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/KY06b.png">
      <img alt="KY_06 individual contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/KY06b.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 33(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/KY06c.png">
      <img alt="KY_06 transfers between committees" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/KY06c.png" style="width: 100%">
    </a>
  </div>
</div>

**U.S. House, Texas District 23**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 34(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/TX23a.png">
      <img alt="TX_23 committee contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/TX23a.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 34(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/TX23b.png">
      <img alt="TX_23 individual contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/TX23b.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 34(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/TX23c.png">
      <img alt="TX_23 transfers between committees" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/TX23c.png" style="width: 100%">
    </a>
  </div>
</div>

**U.S. House, New York District 01**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 35(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/NY01a.png">
      <img alt="NY_01 committee contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/NY01a.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 35(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/NY01b.png">
      <img alt="NY_01 individual contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/NY01b.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 35(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/NY01c.png">
      <img alt="NY_01 transfers between committees" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/NY01c.png" style="width: 100%">
    </a>
  </div>
</div>

**U.S. House, Illinois District 13**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 36(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/IL13a.png">
      <img alt="IL_13 committee contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/IL13a.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 36(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/IL13b.png">
      <img alt="IL_13 individual contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/IL13b.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 36(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/IL13c.png">
      <img alt="IL_13 transfers between committees" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/IL13c.png" style="width: 100%">
    </a>
  </div>
</div>

**U.S. House, Pennsylvania District 01**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 37(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/PA01a.png">
      <img alt="PA_01 committee contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/PA01a.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 37(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/PA01b.png">
      <img alt="PA_01 individual contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/PA01b.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 37(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/PA01c.png">
      <img alt="PA_01 transfers between committees" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/PA01c.png" style="width: 100%">
    </a>
  </div>
</div>

**U.S. House, Indiana District 02**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 38(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/IN02a.png">
      <img alt="IN_02 committee contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/IN02a.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 38(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/IN02b.png">
      <img alt="IN_02 individual contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/IN02b.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 38(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/IN02c.png">
      <img alt="IN_02 transfers between committees" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/IN02c.png" style="width: 100%">
    </a>
  </div>
</div>

**U.S. House, Illinois District 12**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 39(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/ILcumbentwon/IL12a.png">
      <img alt="IL_12 committee contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/IL12a.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 39(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/ILcumbentwon/IL12b.png">
      <img alt="IL_12 individual contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/IL12b.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 39(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/ILcumbentwon/IL12c.png">
      <img alt="IL_12 transfers between committees" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/IL12c.png" style="width: 100%">
    </a>
  </div>
</div>

**U.S. House, Washington District 03**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 40(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/WA03a.png">
      <img alt="WA_03 committee contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/WA03a.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 40(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/WA03b.png">
      <img alt="WA_03 individual contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/WA03b.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 40(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/WA03c.png">
      <img alt="WA_03 transfers between committees" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/WA03c.png" style="width: 100%">
    </a>
  </div>
</div>

**U.S. House, Nebraska District 02**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 41(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/NE02a.png">
      <img alt="NE_02 committee contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/NE02a.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 41(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/NE02b.png">
      <img alt="NE_02 individual contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/NE02b.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 41(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/NE02c.png">
      <img alt="NE_02 transfers between committees" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/NE02c.png" style="width: 100%">
    </a>
  </div>
</div>

**U.S. House, Ohio District 12**

<div class="clearfix">
  <div class="img-container">
    <span>Fig. 42(a)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/OH12a.png">
      <img alt="OH_12 committee contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/OH12a.png" style="width: 100%">
    </a>
  </div>  
  <div class="img-container">
    <span>Fig. 42(b)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/OH12b.png">
      <img alt="OH_12 individual contributions" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/OH12b.png" style="width: 100%">
    </a>
  </div>
  <div class="img-container">
    <span>Fig. 42(c)</span><br/>
    <a href="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/OH12c.png">
      <img alt="OH_12 transfers between committees" src="{{ site.url }}/assets/FECpt2/appendix/incumbentwon/OH12c.png" style="width: 100%">
    </a>
  </div>
</div>

_**Footnotes**_

[^2]: Cleaning notes to consider: 1) all figures are rounded, please check the code notebooks for more precise values, 2) only data pertaining to candidates appearing on final ballots remain, 3) any candidate not affiliated with one of the two major parties has been categorized as, "Third party," 4) some entries in the state abbreviation column do not match those of U.S. states or territories, but we left them for now as they only constitute ~0.2% of total observations (the reason why the state abbreviation per observation is important is that we want to distinguish between in-state contributions/IEs and out-of-state ones), 5) employers and occupations have not been deduped yet.
[^3]: Read more about independent expenditures: [https://ballotpedia.org/Independent_expenditure](https://ballotpedia.org/Independent_expenditure)
[^4]: [https://www.fec.gov/help-candidates-and-committees/candidate-taking-receipts/contribution-limits/](https://www.fec.gov/help-candidates-and-committees/candidate-taking-receipts/contribution-limits/)
[^5]: PDF of FEC limits (2017-18): [https://transition.fec.gov/info/contriblimitschart1718.pdf](https://transition.fec.gov/info/contriblimitschart1718.pdf)
