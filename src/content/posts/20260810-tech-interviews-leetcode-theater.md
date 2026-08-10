---
title: "A Year of LeetCode for a Job You Will Never Use It In"
publishDate: "2026-08-10T10:00:00.000Z"
slug: "tech-interviews-leetcode-theater"
description: "The industry asks candidates to grind algorithm puzzles for months to get a job that consists of reading legacy code and arguing about tickets. The research says the process mostly measures stress. And in 2026 the puzzles are the one part of the job a model does perfectly."
tags: ["opinion", "industry", "hiring", "interviews", "leetcode", "careers"]
featured: true
draft: false
readTime: 12
---

## The ritual

Somebody with eight years of experience, a production system serving real traffic, and a CV full of things that shipped, is sitting at their kitchen table at 11pm inverting a binary tree for the fortieth time this month. They will never invert a binary tree at work. They know this. The interviewer knows this. The interviewer inverted a binary tree eighteen months ago to get the job where they now ask other people to invert binary trees. Everybody knows.

We do it anyway, because the alternative is not getting hired.

The going rate to be competitive for a senior role at a company that pays well is somewhere between three months and a year of daily practice. Not learning. Practice. There is a canonical list, the Blind 75, which grew into NeetCode 150, which grew into NeetCode 250, because 75 stopped being enough to clear the bar. There is a book, *Cracking the Coding Interview*, that has sold enough copies to be a genuine publishing success. There are paid platforms, subscription tiers, Discord study groups, and a small industry of people who coach interviewing as a distinct skill from engineering. Because it is a distinct skill from engineering. That is the whole problem in one sentence and we just keep walking past it.

## Where this came from, briefly

It is worth remembering that the format was not designed. It accreted.

In 2007 Jeff Atwood wrote ["Why Can't Programmers.. Program?"](https://blog.codinghorror.com/why-cant-programmers-program/) and popularised FizzBuzz. The point was modest and correct: a shocking number of applicants could not write a loop, so use a trivial screen to catch that. Joel Spolsky's [Guerrilla Guide to Interviewing](https://www.joelonsoftware.com/2006/10/25/the-guerrilla-guide-to-interviewing-version-30/) was in the same spirit. Small filters for a real problem.

Google then industrialised the whiteboard, brainteasers and all, and everybody copied Google, because copying Google feels like a plan. Then in 2013 Laszlo Bock, who ran People Operations there, told the New York Times that [brainteasers are "a complete waste of time"](https://www.nytimes.com/2013/06/20/business/in-head-hunting-big-data-may-not-be-such-a-big-deal.html) and that Google's own data showed they predicted nothing. He said it out loud, on the record, at the company that invented the practice.

That was thirteen years ago. The practice is bigger now than it was then.

The 2015 moment everyone still quotes is [Max Howell](https://x.com/mxcl/status/608682016205344768), who wrote Homebrew, which is on basically every Mac used for development on the planet, being rejected by Google:

> "Google: 90% of our engineers use the software you wrote (Homebrew), but you can't invert a binary tree on a whiteboard so f*** off."

The standard reply is that one anecdote proves nothing, and fine, one anecdote proves nothing. But it stuck for eleven years because it names the shape of the failure exactly: the process discarded demonstrated, verifiable, load-bearing output in favour of a party trick.

## What the research actually says

This is the part that annoys me most, because we are not short of evidence. We just ignore it.

The clearest study is Mahnaz Behroozi and colleagues, ["Does Stress Impact Technical Interview Performance?"](https://dl.acm.org/doi/10.1145/3368089.3409712), published at ESEC/FSE 2020. They ran a controlled experiment: same problems, same candidates pool, but one group solved them on a whiteboard with an interviewer watching and narrating expectations, the other solved them privately on a whiteboard with no observer.

The private group performed roughly twice as well. Same people, same problems. The only variable was being watched.

The authors' conclusion is worth reading slowly: the technical interview as commonly practised "may be filtering out qualified candidates by inducing stress" and functions closer to a test of performance under observation than a test of ability to program. They also found the effect was not evenly distributed, which should worry anybody who signs a diversity statement and then runs a whiteboard loop.

The same group's earlier work, ["Hiring is Broken: What Do Developers Say About Technical Interviews?"](https://ieeexplore.ieee.org/document/8818836) (VL/HCC 2019), went through thousands of developer comments and found the recurring complaints were not "this is hard" but "this is irrelevant", "this is disrespectful of my time", and "this correlates with recent memorisation, not competence".

And then there is the boring, decades-old industrial psychology answer that our field acts like it has never heard of. Schmidt and Hunter's 1998 meta-analysis in *Psychological Bulletin*, covering 85 years of selection research, found that the highest-validity single predictors of job performance are general mental ability tests and **work sample tests** — that is, having somebody do a small version of the actual job. Not puzzles. Not trivia. The job.

We have known the answer since before most of us were writing code. We chose the puzzles because the puzzles scale, are easy to grade, and let the interviewer feel clever.

## The skill mismatch, stated plainly

Let me be specific about what the daily job of a senior engineer actually consists of, since the interview seems to have forgotten.

Reading code somebody else wrote, under deadline, with no author available. Working out why a thing that worked on Friday does not work on Monday. Deciding which of four ugly options is the least ugly given a deadline nobody consulted you about. Saying no to a feature. Saying yes to a feature and then scoping it down until it fits. Reviewing a pull request without being a jerk about it. Writing the migration that moves 40 million rows without taking the site down. Explaining to a product manager why the estimate tripled. Naming things.

Now, the interview. Given an array of integers, return the indices of two numbers that add to a target, in optimal time, on a shared editor, in 25 minutes, while explaining your thought process out loud, with a stranger watching, in your second language, on a Thursday.

The overlap between those two lists is roughly: you type. That is the overlap.

I am not arguing that algorithms are useless. Knowing that a nested loop over a big list is going to hurt, knowing what a hash map costs, knowing when sorting first makes everything easier — that stuff pays for itself every week. But you can establish that in ten minutes of conversation about a system somebody actually built. You do not need someone to have memorised the trick to the [Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/) problem. There is a trick. There is always a trick. And a candidate either has seen the trick before, in which case you have measured their study schedule, or has not, in which case you have measured how they behave while being watched failing. Those are the only two outcomes. Neither is the thing you claimed to be testing.

## And now the good part: the machines are better at it

Here is where 2026 gets funny.

The single class of programming problem that current frontier models solve most reliably is the competitive-programming problem. It is the perfect target: self-contained, unambiguously specified, deterministically checkable, and with tens of thousands of worked solutions in the training data. OpenAI reported o3 hitting a Codeforces rating around 2700, which is grandmaster territory and beyond nearly every human engineer alive. DeepMind's AlphaCode work landed in the same neighbourhood years earlier.

So the industry has arrived at this position:

- The hiring filter is a category of problem that a model solves instantly and perfectly.
- The actual job is full of problems models are mediocre at: ambiguous requirements, undocumented legacy behaviour, tradeoffs with no correct answer, organisational context.
- We are filtering on the first category to hire people for the second.

The response to this has not been "maybe rethink the filter". The response has been an arms race in proctoring. Companies are pulling candidates back onsite specifically so they cannot use AI — Cisco and others said so publicly in 2025 and the trend has only accelerated. There is now a genre of startup selling interview surveillance: eye tracking, second-camera-on-your-desk, keystroke analysis, "focus loss" detection. There is a competing genre selling undetectable AI assistance during interviews. A Columbia student got famous in 2025 building exactly that and turned it into a funded company.

Sit with that. We have built an industry that sells cheating tools, an industry that sells cheating detection, and a hiring process whose only remaining function is to be the battleground between them. Nobody in that loop is producing software.

The obvious move is to stop testing the thing the machine does perfectly and start testing the thing it does not: judgement. Give me a real pull request from your codebase with two subtle problems in it and let me review it, AI open, I do not care. Watching how somebody uses the tools they will actually have is more informative than pretending the tools do not exist. But that requires interviewers to prepare materials and exercise judgement rather than grade against a rubric, and that is expensive, so here we are buying eye-tracking software instead.

## The costs nobody puts on the balance sheet

**It is a tax on time you do not have.** Six months of evening practice is free if you are 23 with no dependants. It is close to impossible if you have kids, a carer role, a chronic illness, or a job that already eats eleven hours a day. We built a filter that systematically favours people with the most spare time, then hold meetings about why our teams look homogeneous.

**It expires.** This is the detail that gives the game away. Grind for six months, get hired, work for two years, and you are back to square one, because none of it was retained, because none of it was used. A skill that evaporates the moment you stop drilling it was never a proxy for competence. It was a proxy for *drilling recently*.

**It punishes the wrong people.** The engineer who has spent four years going deep on one hairy production system is worse at LeetCode than the new grad who did it last term. On any measure that matters they are the better hire.

**It is enormously expensive.** Five interviewers, five hours, plus scheduling, plus review. Call it a full engineer-week per candidate per loop at a mid-size company. Multiplied by candidates, multiplied by roles. All spent measuring a signal the field's own research says is dominated by stress response.

**And it makes people hate us.** Not the rejection, people can handle rejection. The insult is the asymmetry: months of unpaid preparation on the candidate's side, a lightly prepared interviewer who picked a problem off a shared doc ten minutes before the call, and a two-line rejection email six days later. The message received is that your time is worth nothing and mine is precious. People remember that. Some of them are the people you will want to hire in three years.

## What the alternatives look like

I am aware that "the interview is broken" is a cheap thing to say and that hiring is genuinely hard. So, concretely, what works — all of it in use somewhere today, none of it hypothetical:

**Paid short work sample.** Four hours, paid at a fair rate, on a problem shaped like your actual product. Highest-validity option in the research, and paying for it fixes the asymmetry. The usual objection is candidate time, and it is a fair one — which is why it should replace the five-round loop, not sit on top of it.

**Read code instead of writing it.** Hand over a real PR with real problems in it. Reviewing is most of the job and almost nobody tests for it. It is also very hard to fake, and very hard for a model to do well without the context only your team has.

**Go deep on something they built.** Forty-five minutes on a system the candidate actually shipped, pushed until you hit the edge of what they know. You find the boundary of real understanding fast, and it is nearly impossible to bluff because you can always ask "why not the other way?" one more time.

**Pair on a real bug.** Your repo, a real failing test, two hours, you sit alongside rather than opposite. You get to see what working with them is like, which is the actual question.

**Structured, consistent, boring.** Same questions, same rubric, same order, scored independently before discussion. Google's own [re:Work](https://rework.withgoogle.com/en/guides/hiring-use-structured-interviewing) material has said this for a decade. Structured interviews beat unstructured ones on validity by a wide margin, and unstructured "vibes" hiring is how bias walks in wearing a lanyard.

And if you genuinely need algorithmic depth, because you are writing a database engine or a compiler and the answer is yes sometimes — then test it, but test it honestly. Say so in the job description, tell candidates exactly what to expect, give them the problem space in advance, and let it be one round rather than four.

The list of companies that hire without whiteboard puzzles is public: [poteto/hiring-without-whiteboards](https://github.com/poteto/hiring-without-whiteboards) has thousands of entries and has been maintained for years. These companies exist, they ship software, they are not obviously worse at it. The claim that the LeetCode loop is load-bearing is not supported by the existence of everyone doing fine without it.

## Why it survives anyway

If it does not work, and we have known it does not work since 2013, why is it still here?

Because it is cheap to administer and feels objective. A rubric produces a number, a number produces a defensible decision, and a defensible decision protects the person who made it. Nobody gets blamed for rejecting a strong candidate. Everybody gets blamed for a bad hire. The whole system is optimised against false positives at absurd cost in false negatives, and the false negatives never show up in anybody's metrics because they went and worked somewhere else.

Because of sunk cost, too. When you spent eight months grinding to get in, "this filter is meaningless" is an unpleasant thing to believe. The hazing logic is real and it is human: the people who paid the entry fee tend to defend the toll booth.

And because when labour supply is loose, a process that is bad at identifying talent but great at reducing volume looks like it is working. It is not selecting. It is rationing. Those look identical from the inside if you never measure who you rejected.

## What I actually think

I do not think algorithms are worthless, I do not think interviews should be a chat about vibes, and I do not think every candidate is secretly brilliant and misunderstood.

What I think is that we have a process whose difficulty is entirely uncorrelated with the difficulty of the job, that costs candidates months of unpaid labour, that the field's own published research says is heavily confounded by stress, that the company that invented it disowned thirteen years ago, and that in 2026 filters precisely on the one skill that has been fully commoditised by a text box.

And we are responding to that last part by buying eye-tracking software.

If you run hiring, you can change this by yourself, this quarter, without a mandate. Drop a round. Pay for the work sample. Replace one algorithm round with a code review of a real PR. Tell candidates exactly what is coming. Send the rejection in two days, not two weeks, with one real sentence in it.

If you are on the candidate side of it: grind the problems, because the world is as it is and rent is due. But do not let anyone convince you it is making you a better engineer. It is a toll. Pay it, get through the gate, and then push, from the inside, to make the gate smaller for whoever comes next.

The good news is the whole thing is one meeting away from changing at any given company. Somebody just has to say out loud that the emperor has been on the whiteboard this entire time.

---

## References

**Research**

- [Does Stress Impact Technical Interview Performance? — Behroozi, Shirolkar, Barik, Parnin, ESEC/FSE 2020](https://dl.acm.org/doi/10.1145/3368089.3409712)
- [Hiring is Broken: What Do Developers Say About Technical Interviews? — Behroozi, Parnin et al., VL/HCC 2019](https://ieeexplore.ieee.org/document/8818836)
- [Does the Presence of an Interviewer Affect Problem Solving? Eye-tracking study — Behroozi et al.](https://dl.acm.org/doi/10.1145/3183440.3195027)
- [The Validity and Utility of Selection Methods in Personnel Psychology — Schmidt & Hunter, Psychological Bulletin, 1998](https://psycnet.apa.org/record/1998-10661-006)

**The industry saying it out loud**

- [In Head-Hunting, Big Data May Not Be Such a Big Deal — Laszlo Bock interview, New York Times, 2013](https://www.nytimes.com/2013/06/20/business/in-head-hunting-big-data-may-not-be-such-a-big-deal.html)
- [Max Howell on being rejected by Google, June 2015](https://x.com/mxcl/status/608682016205344768)
- [Why Can't Programmers.. Program? — Jeff Atwood, Coding Horror, 2007](https://blog.codinghorror.com/why-cant-programmers-program/)
- [The Guerrilla Guide to Interviewing (v3.0) — Joel Spolsky, 2006](https://www.joelonsoftware.com/2006/10/25/the-guerrilla-guide-to-interviewing-version-30/)
- [Google re:Work — Use structured interviewing](https://rework.withgoogle.com/en/guides/hiring-use-structured-interviewing)

**The grind, documented**

- [Blind 75 — original list on Teamblind](https://www.teamblind.com/post/New-Year-Gift---Curated-List-of-Top-75-LeetCode-Questions-to-Save-Your-Time-OaM1orEU)
- [NeetCode roadmap](https://neetcode.io/roadmap)
- [Cracking the Coding Interview — Gayle Laakmann McDowell](https://www.crackingthecodinginterview.com/)
- [Trapping Rain Water — LeetCode](https://leetcode.com/problems/trapping-rain-water/)

**Alternatives**

- [hiring-without-whiteboards — poteto on GitHub](https://github.com/poteto/hiring-without-whiteboards)
- [Interviewing.io research on interview performance variance](https://interviewing.io/blog)
