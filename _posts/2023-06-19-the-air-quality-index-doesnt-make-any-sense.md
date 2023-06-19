---
layout: post
title: The Air Quality Index doesn’t make any sense
description: A metric designed by committee 
image: /assets/2023_aqi/aqi_annotated_bounded.png
---

Have you ever felt a little hazy on what the Air Quality Index (AQI) actually means? I have, which is why I decided to deep dive on it. What I found was a bizarre Frankensteinian metric emerging from a web of federal judges, EPA employees, interest groups, and random members of the public. 

The first thing to know about AQI is that it is different for every country. I’ll focus on the US version.

The second thing to know about AQI is that it aggregates levels from different pollutants in an unusual way. There are separate AQIs for PM<sub>2.5</sub> (small particles), PM<sub>10</sub> (coarse particles), Ozone, CO, SO<sub>2</sub>, and NO<sub>2</sub>. The overall AQI is just the maximum of the pollutant-specific AQIs. So when you hear that overall AQI is 140, you won’t know _which_ of the pollutants was the cause. I’ll focus on the PM<sub>2.5</sub> version here.

The third thing to know about AQI is that the relationship between AQI and PM<sub>2.5</sub> is a fascinating case study in how decisions are made at federal agencies. This is the focus on this blog post.

Based on the [AQI calculation rules](https://www.airnow.gov/sites/default/files/2020-05/aqi-technical-assistance-document-sept2018.pdf), the relationship between PM<sub>2.5</sub> and AQI looks like this.

<div class="wrapper">
  <img src='/assets/2023_aqi/aqi.png' class="inner" style="position:relative border: #222 2px solid; max-width:80%;" >
</div>

It’s a piecewise linear function composed of seven breakpoints, likely a holdover from when people used hand-held calculators and did not have access to software. That’s fair enough. But what’s really odd is that the curve is _wiggly_! 

There is no physiological reason that the relationship between PM<sub>2.5</sub> and air quality should decrease, then increase, then decrease, then increase, and then decrease again. 

To investigate, I turned to the latest (2012) revision of the [National Ambient Air Quality Standards (NAAQS) for Particulate Matter](https://www.govinfo.gov/content/pkg/FR-2013-01-15/pdf/2012-30946.pdf), which is used primarily to set air quality standards that have some legal force, but secondarily to inform the AQI. The justification for each breakpoint is laid out in over 200 pages. This is what I learned:

### The first breakpoint: 12 μg/m3
The first breakpoint is defined as the **annual** NAAQS standard, which is the PM<sub>2.5</sub> level each U.S. state is [required](https://ww2.arb.ca.gov/resources/national-ambient-air-quality-standards) to attain, when averaging over a year. Up until 2012, the standard was 15 μg/m3, based somewhat arbitrarily on the empirical average of PM<sub>2.5</sub> values in the US. But after a lawsuit from the farm lobby, a federal judge [remanded](https://casetext.com/case/american-farm-v-epa) the standard. After multiple public hearings, testimony from 168 individuals, comments from [230,000](https://www.regulations.gov/docket/EPA-HQ-OAR-2007-0492) members of the public, and a review of the scientific literature, the EPA decided in 2012 to reduce the value to 12 μg/m3, moving it in the opposite direction of what the farm lobby wanted.

### The second breakpoint: 35 μg/m3
The second breakpoint is defined as the **24-hour** NAAQS standard. As far as I can tell, there is no principled reason that this breakpoint should be based on the 24-hour standard while the first breakpoint was based on the annual standard, except that the 24-hour standard is stricter. 

### The third breakpoint: 55 μg/m3
This breakpoint was based on an arbitrary scalar extension of the 24-hour standard ([p. 3181](https://www.govinfo.gov/content/pkg/FR-2013-01-15/pdf/2012-30946.pdf)). Since no interest groups or members of the public objected to it, the EPA codified it.

### The last breakpoint: 500 μg/m3
This breakpoint was initially picked somewhat arbitrarily. Since no interest groups or members of the public objected to it, the EPA codified it.

### The other breakpoints
The EPA decided that the breakpoints between 55 and 500 μg/m3 should produce a linear relationship to AQI. Unfortunately, this clashed with their preference for round number multiples of 50, which is why there are elbows at 150 and 350 μg/m3. In a world with software, I don’t understand why there is any benefit to round numbers. But even weirder, if there were any benefits to round numbers, they are partially nullified by the EPA’s inexplicable decision to append a decimal point of 0.4 to each cutoff, so that the actual breakpoints are 55.4, 150.4, 250.4, etc.

Here’s the graph again with annotations.

<div class="wrapper">
  <img src='/assets/2023_aqi/aqi_annotated.png' class="inner" style="position:relative border: #222 2px solid; max-width:80%;" >
</div>

The authors of the EPA report are thoughtful public servants who are keenly aware that they must manage multiple stakeholders. They know the NAAQS standards face litigation and must be backed by pages of justification. And they realize that not every outcome will make perfect scientific sense.

And in some ways, the wiggliness of the AQI curve doesn’t matter that much. It’s basically a linear function with some mild deviations, and I’m not sure how much the public would actually benefit from smoothing it out. 

But still, I can’t shake the feeling that our AQI curve represents a failure of government process. There is no principled reason why annual and 24-hour NAAQS standards – representing two different durations – should be points on a curve that represents point-in-time air quality. The only justification I could find was that it is the “historical approach” ([p. 3180](https://www.govinfo.gov/content/pkg/FR-2013-01-15/pdf/2012-30946.pdf)). Nor is there any reason for the clashing mix of linearity and rounding used at higher concentrations. 

Better ways are possible. The Canadians use a [modern exponential curve](https://en.wikipedia.org/wiki/Air_Quality_Health_Index_(Canada)) for their AQI metric. So if Canada is going to [share their air with us](https://www.nytimes.com/live/2023/06/07/us/canada-wildfires-air-quality-smoke), maybe we could also share their AQI standards. Or perhaps we should abandon the notion of AQI altogether and just report the raw components (PM<sub>2.5</sub>, Ozone, etc) individually, just like we do with temperature and humidity. 

<div class="wrapper">
  <img src='/assets/2023_aqi/us_canada.png' class="inner" style="position:relative border: #222 2px solid; max-width:100%;" >
</div>