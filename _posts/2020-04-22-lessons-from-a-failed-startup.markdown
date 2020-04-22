---
layout:     post
title:      "Lessons from a Failed Startup"
date:       2020-04-22 06:52:00 +0000
permalink:  'lessons-from-a-failed-startup'
---

### The story of a failed startup

At 9am I was making porridge in the kitchen of our WeWork office. The CEO and COO of our company arrived through the elevator and said hello. After a few minutes of small talk they asked if I could grab Lawrence, the iOS tech lead, and join them in the adjacent meeting room. As we sat down I could sense that this was not a regular meeting. “We’re going to have to make some redundancies” they said. I don’t remember their exact words but they were going to fire 5 people. The team was only 20 people so 5 was a big chunk of the company. It was at that point I first suspect the startup I worked for was going to fail.

TODO: continue from here 2nd draft

I joined the company in 2017. At that time it has 6 permanent members of staff, some agency developers and some consultants. The company was building an app to manage hospital shift workers. There were other systems that did it but we had an opportunity to build a modern, easy to use web app that could be rolled out to hospitals nationwide. It seemed like a good idea. When I joined we were working with 1 hospital and planning to roll out to another. I was hired as a software engineer to help build the backend system, admin site and API.

At first there were a lot of positive signs, the company was hiring and growth is usually a good sign for tech startups. The deal with our second hospital seemed to be progressing nicely and we were adding features that seemed logical. Although I hadn’t met any of our users, the feature requests that the tech team were getting seemed to make sense. The tech team grew from 3 in-house developers to 8. We hired a designer, QA and a product person.

As time went on the deal with our second hospital seemed to drag on. The CEO and company chairman also started talking about deals in America which seemed strange to me because we hadn’t even closed a two hospitals in the UK. More importantly we didn’t have a single paying customer in the UK. The first hospital has signed up on a pilot basis. I was always a bit concerned that we weren’t making money but way of running a startup seems so normal in the tech world that I just ignored it.

Then, I got the chance to go into our second partner hospital to help debug some tech issues. Whilst I was there I took the opportunity to talk to some of our users. The first few hospital employees that I spoke to gave nice feedback. One said “This app is great, can you just tweak the colours so it matches our hospital?”. The fourth person I spoke to was a man in his 30s, who seemed reasonably tech savvy. He said “I wouldn’t use this app, it doesn’t make my life easier, I’d just log into the existing hospital system”. I shared the feedback with the operational team and suggested we quiz the guy more to understand this. But I got the impression the feedback was not what the operations team wanted to hear.

Back to my 9am meeting. This was the moment I realised we were running out of money. The Chairman did not want to keep funding the company out of his own pocket at we were making a last attempt to raise money. The regencies were a way to elongate our runway and keep us going a bit longer. It was at this point that things became desperate. Feature requests started from the operations side of the business began to get more obscure, it began to feel like they were making feature demands up to cover the fact that they couldn’t sell the produce. For example, we had chat functionality in the app that was provided by a third party, which supported 250 users. The operational team said that we needed to be able to support 300 users because an entire group of doctors wanted to talk to one another. If you look at most successful tech company they have a core idea that becomes very popular; 140 character tweets or booking a taxi with a few clicks. It was clear that we didn’t have one. Instead of doubling down on the apps essences we were adding more and more crap.

Even though I knew the company was failing, I pushed it to the back of my mind. I doubted the CEO and COO’s ability but we had a great CTO and a strong tech team. Maybe this is what people mean when they say startups are hard. Perhaps I just need to knuckle down, work hard and we’ll get through this. As a tech team we tried to recover from the redundancies. We refocused our backlog and tried to focus on things that would allow us to signup more users and appeal to investors. This went on for a few months and I almost began to forget about the redundancies. However, one afternoon there was another shock. The COO walked in to our square, glass, WeWork room and shut the door. She asked how we were all doing - probably a bad sign. She proceeded to explain how our CTO had decided to leave the company. Our CTO was in the room too. I could tell he found the awkwardness in the room difficult. I caught up with him after the meeting and I could tell that there we’re things he wanted to tell me but was unable to. After the meeting I had a chat with my friend, who I had helped hire into the company. At the same time, we both blurted out “I’m leaving”.

From there, it didn’t take long to find a job. I’m very fortunate to work in a job like software development where demand is so high. I left Ryalto within a month. But it was still disappointing and a bit sad. I had spent almost two years working for a company which had amounted to nothing. Later on that year, after I had left, there were more rounds of redundancy. Eventually they closed the company.

### Lessons from a failed startup

I hope you enjoyed the story. It’s all completely true. Told from my perspective. Before moving on, I think it’s worth trying to extract some lessons.

#### 1. Focus on core app mechanism

Most new tech products have one cost app mechanism. This can also be thought of as a ‘soundbite’ about what the app does. Twitter had 140 character tweets. Uber allowed you to hire a limousine with a few clicks. Dropbox let you sync files across all your devices.

When Ryalto started it had a core idea: Allow contract hospital workers to find and register for shifts, with a few clicks. However, as the company struggled to sell it’s product to hospitals management began agreeing to feature requests from sales meetings. If a hospital wanted our app to handle training documents, management would agree to it. If a hospital wanted our app to handle NHS promotions/discounts management would agree. Management even suggested that we add weather to our app so workers could check their weather, via our app, before leaving for shift. The problem is, each of these ideas or companies in themselves. A startup cannot handle all of them. It must choose one and do it well.

Lesson 1: A startup should focus on a single idea and execute is really well. If it’s not working well, the company should pivot. Diversification doesn’t work, it only diluted the product.

#### 2. Tech should work closely with operations team and users

The initial startup idea may not be what users want. Once a startup releases something into the market they often need to iterate and evolve the product based on user feedback. To facilitate that you ideally want the developers speaking to users. If that’s not possible you want those people who are talking to users to be working as closely with developers as possible.

At Ryalto the operations team and tech team did not work closely together. Features were often built wrong or slowly due to the message being diluted as it was passed through so many people. 

Lesson 2: Developers should talk to users or work very closely with people who do

#### 3. Trust is important, don’t work for a company that isn’t transparent

Working at a startup requires a lot of trust. More so than working at a big company, I would say. Bigger companies have public press releases, share option schemes, defined job titles etc. At a startup, the company nobody knows what’s going on. To get through this uncertainty it helps to trust your team. More importantly it helps to trust the founders/leaders of the company.

At Ryalto, over time, I no-longer trusted the founder/main-investor along with the CEO and COO. I began to suspect they were hiding information. Being in that redundancy meeting at 9am was a good example. I was completely blindsided. I had not received any indication that the company was running out of funding or that we might have to tighten our budget. The news was bad but what was really bad was that I had lost my trust in the company.

#### 4. As an engineer, don’t ignore the way the business makes money (or plans to). When you join a company, ask those questions and make sure they make sense to you

When looking for a new job, as a developer, it’s easy to focus on the tech stack. That’s what you’ve spent your career learning and that’s what you’re comfortable talking about. But what’s equally important is to ask questions about the company’s business model and future plans to make money. If you don’t think the companies business plan is viable do not ignore it. Ask more questions until it does make sense or walk away.

I had many doubts about the way Ryalto were executing on their business model and I chose to ignore those feeling. I should have dug deeper to learn more. Ultimately I would have found that the companies strategy for making money did not make sense.
