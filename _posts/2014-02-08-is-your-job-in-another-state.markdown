---
author: csaid81
comments: true
date: 2014-02-08 22:49:08+00:00
layout: post
slug: is-your-job-in-another-state
title: Is your job in another state?
---

National unemployment is high, but business is booming in some states. Vermont needs teachers. Nevada needs bartenders. North Dakota needs truck drivers and just about everything else.

Despite these opportunities, Americans [aren’t moving much](http://www.washingtonmonthly.com/magazine/november_december_2013/features/stay_put_young_man047332.php?page=all) and unemployment remains high.  One reason for this is that moving can be expensive and disruptive, especially for those with families and roots in their communities. But another reason may just be lack of awareness about the opportunities in other states. That’s why I have made a new website: [www.ismyjobinanotherstate.com](http://www.ismyjobinanotherstate.com). Enter your job skills, and the website will provide an interactive map showing where you are most in demand.

[{% include image.html url="/assets/securityofficer.png" %}](http://www.ismyjobinanotherstate.com)
States are ranked by their [ratio of job postings to unemployment](https://www.fas.org/sgp/crs/misc/R42943.pdf). This is a pretty good metric, but it isn’t perfect. To understand why, imagine two states with the same posting/unemployed ratio for a particular job. If you are trained for the job, you might have better luck applying in a state where the unemployed population is either untrained or unwilling to take that type of job, even though the two states have the same ratio. There also may be differences across states in the use of Indeed.com. Still, I think my results have reasonably good face validity, and the results for many jobs are close to what you would except. If you average across jobs, you get something pretty close to an independently created measure called the [“Opportunity Index”](http://www.washingtonmonthly.com/magazine/november_december_2013/features/the_2013_opportunity_index047357.php).

Job posting data was collected using the [Indeed.com api](https://ads.indeed.com/jobroll/xmlfeed). Unemployment numbers came from the [Bureau of Labor Statistics](http://www.bls.gov/news.release/laus.t03.htm). For more information about how this works, see my the [GitHub repository](https://github.com/csaid/IsMyJobInAnotherState).
