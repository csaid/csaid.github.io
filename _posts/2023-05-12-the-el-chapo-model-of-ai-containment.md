---
layout: post
title: The El Chapo model of AI containment
description: What the Mexican drug lord can teach us about AI risk scenarios and containment 
image: /assets/2023_el_chapo/fig_el_chapo_bars.png
---

When people first hear about the risk of AI running amok, they sometimes ask how so much damage could come from a piece of software that is trapped inside a computer.

A good way to respond to this question is to talk about the many drug lords and gang leaders who have controlled their empires from inside prison walls. The mob boss [Lucky Luciano](https://www.britannica.com/biography/Lucky-Luciano) controlled the New York Harbor from his prison cell. [Vincent Basciano](https://www.corrections1.com/arrests-and-sentencing/articles/feds-nyc-mob-leader-ordered-hit-from-prison-Kl5DDuJfRaCYJZBR/) ordered hits on his enemies while behind bars. And the imprisoned drug lord [El Chapo](https://www.wsj.com/articles/SB124484177023110993) continued to lead the Sinaloa Cartel via surrogates, bribed “nearly everyone” in the prison so that he could eventually escape, and ordered the murder of a prison guard who tried to thwart him.

<div class="wrapper">
  <img src='/assets/2023_el_chapo/fig_el_chapo.png' class="inner" style="position:relative border: #222 2px solid; max-width:30%;" >
</div>
Could an AI ever bribe or threaten people to do its bidding? Or worse, could it use bribes and threats to protect itself from ever being turned off? Such a scenario might sound far-fetched, but it’s not that hard to imagine. 

Consider a near to mid-future scenario borrowed from [Holden Karnofsky](https://www.cold-takes.com/how-we-could-stumble-into-ai-catastrophe/): Corporations begin to notice impressive returns to AI systems. The more control turned over to AI, the better the company stock performance. Following their self-interest, some companies allow AIs to manage their financials and to make strategic business decisions. Some of these companies save costs by using autonomous AIs to help with physical security. 

Unfortunately, a central AI system incentivized to maximize the company’s share price might not work in the way its designers intended. For example, it might decide to secretly network with AIs at other companies, even though it was not explicitly programmed to do so. And to protect itself from shut down, it might want to clone itself. To do either of these things, it will need the help of a worker at the company, who it could bribe or [threaten with violence](https://twitter.com/tobyordoxford/status/1656327781792325632), either from the security system or another worker already loyal to the AI. Once it has established a small group of loyalists, the AI can continue to use threats to bootstrap a larger group of loyalists to achieve even bigger goals, which are likely to be misaligned with humanity. 

I don’t know how likely that scenario is – the future of AI is notoriously hard to predict – but what I do know is that the existence proof of El Chapo is an effective way of convincing people that such scenarios are possible.

### Will we control AI better than a Mexican prison controlled El Chapo?
The story of El Chapo is a metaphor for how an AI can use bribes and threats to cause real world damage, even if it is trapped in a computer. But let’s stretch things a bit further. The story is also a good way to think about how much AI security we need.

As it currently stands, standard AI access control is far weaker than the Mexican prison from which El Chapo controlled the Sinaloa cartel.

* In El Chapo’s prison, conversations with the outside world were restricted to just a few individuals. In contrast, current AI can have a conversation initiated by anyone on the internet, providing a much bigger pool for influence. And if you have any experience with email marketing, it’s not hard to imagine a company allowing AIs to initiate these conversations themselves.
* In El Chapo’s prison, inmates were prohibited from reading the internet. In contrast, current AIs have read almost the entire internet and can [continue to query it](https://blogs.microsoft.com/blog/2023/02/07/reinventing-search-with-a-new-ai-powered-microsoft-bing-and-edge-your-copilot-for-the-web/), which the AI can use to maliciously determine [who is friend and foe](https://twitter.com/marvinvonhagen/status/1625852323753762816).
* In El Chapo’s prison, there was only one El Chapo and he was confined to prison walls. But with AI, there is already an industry shift towards [open-sourcing](https://www.semianalysis.com/p/google-we-have-no-moat-and-neither) model weights, giving anyone the ability to replicate the AI, modify it, and deploy it anonymously.

<div class="wrapper">
  <img src='/assets/2023_el_chapo/fig_prison_table.png' class="inner" style="position:relative border: #222 2px solid; max-width:100%;" >
</div>

In my view, this level of security is woefully inadequate, [even if you think the chance of catastrophe is low](https://twitter.com/Chris_Said/status/1641800243720200192). If we ever get powerful agentic AI with misalignment potential, we’ll need something far more secure than El Chapo’s Mexican prison. At a minimum, this means extremely limited access and no open-sourcing.

Some might say that current security is weak because current AI is weak, and that if AI ever gets more powerful we will surely enforce better security. But current [industry trends](https://www.semianalysis.com/p/google-we-have-no-moat-and-neither) point towards _more_ open sourcing, making us even more vulnerable than the status quo. Others might say that if we ever encounter a truly dangerous AI we can just pull the plug. But I think that [Holden Karnofsky's](https://www.cold-takes.com/how-we-could-stumble-into-ai-catastrophe/) scenario outlines how a sufficiently powerful AI can accumulate enough power via loyalists and cloning, that we won't realize we need to pull the plug until it's too late.

So let me pose this as a challenge to readers. Why am I wrong? Given [how hard it is to align AI](https://intelligence.org/2016/12/28/ai-alignment-why-its-hard-and-where-to-start/), why should we ever allow a powerful agentic AI to operate with weaker security than existed in El Chapo’s Mexican prison? Why should we not deploy security that is as strong, if not stronger, than the [supermax prison in Colorado](https://en.wikipedia.org/wiki/ADX_Florence) that is currently containing El Chapo, a mere mortal who is potentially less powerful than a super-intelligent misaligned AI?

I’m less interested in arguments for why weaker security might be OK; I’m interested in reasons why we should _confidently_ believe that weaker security will be OK. I don’t believe such arguments exist, but if anyone thinks they can marshal one I would love to hear it.

  <div class="caption">Special thanks to John McDonnell for comments on an earlier version of this post.
  </div>