---
layout:     post
title:      "Brave vs Google Chrome (with ad blockers)"
date:       2019-03-11 07:39:00 +0000
permalink:  'brave-vs-chrome-with-ad-blockers'
---

[Brave Browser](https://brave.com/) has been around for a few years, it became more usable when the company released a new version that was built on Chromium. A few people recommended that I switch to Brave. I tried but never stuck with it. However, with growing controversy about Google's data collection practices and Chrome's dominance, I decided to test Brave and figure out whether it's worth switching. If Brave's browsing experience is good, I plan to switch perminantly.

### Brave's mission
On the surface Brave is just another browser, like Firefox or Safari. But Brave is built on Chromium so it feels familiar to Chrome users. But below the surface Brave is trying to be more than just a web browser.

It comes with a built in adblocker. Personally, I used an adblocker with Chrome, so that doesn't affect me. However, people who use ad blockers are still in the minority. According to The Guardian only [22% of U.K internet users use adblockers, as of 2016](https://www.theguardian.com/media/2016/mar/01/more-than-nine-million-brits-now-use-adblockers), and I've seen similar numbers for the rest of the world. If Brave was widely adopted it would kill the internet's ad revenue model. Meaning, the primary method by which content creators get paid would no longer be advertising. A new model would be required.

Brave's answer to this new model is Basic Attention Tokens (BAT). These are blockchain based tokens stored in Brave's built in wallet. Users can buy BAT tokens and configure Brave to distribute them to their favourite sites. Brave will distribute the money based on time spent on each site. For example, If you agree to distribute £100 of BAT tokens per month and spend 5% of your browsing time this month watching [MKBHD tech videos](https://www.youtube.com/user/marquesbrownlee), he will get £5 in BAT.

### Why would anyone switch from Chrome to Brave?
Chrome has a large market share, [somewhere between 60-70%](https://en.wikipedia.org/wiki/Usage_share_of_web_browsers). [Many](http://www.zeldman.com/2018/12/07/browser-diversity-starts-with-us/) [people](https://redalemeden.com/blog/2019/we-need-chrome-no-more) [believe](https://www.chriskrycho.com/2017/chrome-is-not-the-standard.html) this is having an adverse affect on the web because it gives Google huge power over web standards. As a private company, they’re incentivised to use their power for personal benefit over what is good for the web.

Google has come under fire lately for privacy concerns. For example, Chrome shipped a change that when you sign into any Google service, [Chrome automatically signs you in to Chrome](https://blog.cryptographyengineering.com/2018/09/23/why-im-leaving-chrome/). Then there's the concern about sharing your browsing data with a company that also has your Google search data, Andriod mobile data, YouTube browsing history, reads all your Gmail messages, can see all your trips on Google Maps and these are just the well known Google services.

I've already discussed the ad revenue model. Well, Google thrives on this model. It reportedly earned [86% of it's revenue from ads last quarter](https://www.theverge.com/2018/10/25/18024654/alphabet-google-q3-2018-earnings-ad-business-booming). There is growing discontentment with this model. Creators revenue is falling and consumers are increasingly using adblockers to avoid annoying ads and protect their privacy.

Google recently announced that they would [start blocking ads](https://www.wired.com/story/google-chrome-ad-blocker-change-web/). However, the details reveal that they're only blocking sites which have clearly disruptive and obnoxious ads, Wired estimated about 100,000 sites in total. What Google are really trying to do is block just enough ads to stop the uptake of third party adblockers.

### Speed and Performance
Brave is reportedly faster and consumes less memory than Chrome so I wanted to put that to the test.

**Load speed**

I tested the load speed of Brave and Chrome across 3 different sites; YouTube, Amazon & The Guardian. Load times were 21-27% faster on Brave than Chrome. I assume this is because Brave's adblocker is stricter and is blocking more network calls.

On The Guardian's site, for example, Brave reports that it blocked 17 items where ABP only blocked 8.

![abp-guardian-website](/assets/abp-guardian-website.png){:.image-50}

![brave-guardian-website](/assets/brave-guardian-website.png){:.image-50}

Looking at the network traffic Brave made 178 requests and Chrome, with ABP, made 238 requests.

![abp-guardian-website-network-traffic](/assets/abp-guardian-website-network-traffic.png){:.image-50}

![brave-guardian-website-network-traffic](/assets/brave-guardian-website-network-traffic.png){:.image-50}

<br>

With a fast internet connection, 20-30% speed improvement is not particularly noticeable. But for users with slower connections it could be a big difference.

**Memory consumption**

I tested the memory consumption of Brave, Chrome and Chromium. I tested them with 0 tabs, 1 tab, 2 tabs, 6 tabs, 10 tabs and 20 tabs. The tests were conducted 3 times each. Here are my findings:

![brave-chrome-chromium-memory-consumption](/assets/brave-chrome-chromium-memory-consumption.png)

Chrome and Brave have the same resting (0 tabs) memory usage. As the number of tabs increase, Brave outperforms. Above 6 tabs Brave uses 20-30% less memory. I haven't found any evidence to explain why Brave has better memory usage, given that both browsers are based off Chromium. I can only guess that Chrome has more bloatware.

I also tested Chromium and it had the lowest memory footprint between 0-6 tabs. Surprisingly, Brave outperformed Chromium beyond 6 tabs. Given that Brave is build on Chromium I'm not sure how Brave was faster. I'd like to investigate that further.

Back to Chrome vs Brave. The test shows that Brave is more memory efficient. This will benefit users who open a large number of tabs or users on low powered machines.

### Things that don't work well in Brave

Unfortunately, there are some things that don't work well in Brave. This is important to me, if I’m going to switch I don't want my browser experience to be compromised. Here are some examples that I found from using Brave for the last month:

**2FA Google authentication app**

I use the Google app on my iPhone to authenticate with my Google account. When I login I enter my password and get prompted to open the Google app on my phone. I just open the app, press OK and I'm automatically signed in.

This doesn't work in Brave, which might be okay but there's no error message and Brave still tries to use this method of authentication. The whole experience is broken. The first time I used it, I re-tried a few times before I realised it was a bug.

![brave-google-auth-app](/assets/brave-google-auth-app.png)

**Broken form CSS on tomkadwill.com**

My own site is broken in Brave. At the bottom of the homepage I have a Mailchimp subscription form. Brave blocks Mailchimp's CDN so the form doesn't render properly because no CSS is loaded.

![tomkadwill-com-broken-form-with-shields-up](/assets/tomkadwill-com-broken-form-with-shields-up.png){:.image-50}

![tomkadwill-com-working-form-with-shields-down](/assets/tomkadwill-com-working-form-with-shields-down.png){:.image-50}

The form looks fine with ABP.

**gs.statcounter.com is completely broken**

Whilst doing research for this blog post I came across gs.statcounter.com which has some interesting browser usage statistics. At first, I thought there was something wrong with the site because no assets were being loaded. Then I switched off Brave's shield and the page reloaded perfectly.

The site loads fine in Chrome with ABP.

![gs-statscounter-brave-shields-down](/assets/gs-statscounter-brave-shields-down.png){:.image-50}

![gs-statscounter-brave-shields-up](/assets/gs-statscounter-brave-shields-up.png){:.image-50}

**Spotify webplayer**

Spotify webplayer doesn't work out of the box. There is a small icon in the bowser bar indicating an error. Upon clicking the icon Brave explains that Google Wavevine is not installed. Although Brave discourages it, you can install the software manually.

This may be simple for some but many users will find this confusing and assume that Spotify doesn't work in Brave.

### Will I switch to Brave?

What initially piqued my interest in Brave was Chrome’s growing market dominance and stories about Google’s questionable data/privacy decisions.

Through researching this post I've found that Brave has many benefits over Chrome. For a start it's faster and has a lower memory footprint. More importantly, I like Brave's mission. I'm interested to see if their idea for monetizing content works, as an alternative to the current ad revenue model.

However, the things that don't work in Brave really are annoying. I've outlined some of the issues in this post but there are many more. When using Brave (with the native adblocker) I hit annoying issues regularly. Although there clearly is a desire for an alternative to Chrome. Brave is not a practical solution at this time.

### References and links
1. [https://brave.com](https://brave.com)
2. [https://www.theguardian.com/media/2016/mar/01/more-than-nine-million-brits-now-use-adblockers](https://www.theguardian.com/media/2016/mar/01/more-than-nine-million-brits-now-use-adblockers)
3. [https://en.wikipedia.org/wiki/Usage_share_of_web_browsers](https://en.wikipedia.org/wiki/Usage_share_of_web_browsers)
4. [http://www.zeldman.com/2018/12/07/browser-diversity-starts-with-us](http://www.zeldman.com/2018/12/07/browser-diversity-starts-with-us)
5. [https://redalemeden.com/blog/2019/we-need-chrome-no-more](https://redalemeden.com/blog/2019/we-need-chrome-no-more)
6. [https://www.theverge.com/2018/10/25/18024654/alphabet-google-q3-2018-earnings-ad-business-booming](https://www.theverge.com/2018/10/25/18024654/alphabet-google-q3-2018-earnings-ad-business-booming)
7. [https://www.chriskrycho.com/2017/chrome-is-not-the-standard.html](https://www.chriskrycho.com/2017/chrome-is-not-the-standard.html)
8. [https://blog.cryptographyengineering.com/2018/09/23/why-im-leaving-chrome/](https://blog.cryptographyengineering.com/2018/09/23/why-im-leaving-chrome/)
9. [https://www.wired.com/story/google-chrome-ad-blocker-change-web/](https://www.wired.com/story/google-chrome-ad-blocker-change-web/)
