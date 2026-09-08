---
title: "Nobody Builds It in a Bedroom Anymore: Computing's Big Science Moment"
publishDate: "2026-09-08T10:00:00.000Z"
slug: "computing-big-science-moment"
description: "The complaint isn't that the remaining problems are boring - boring is a matter of taste. It's that the interesting ones now cost hundreds of millions of dollars and hundreds of people, and physics already lived through this exact transition sixty-five years ago."
tags: ["opinion", "industry", "computer-science", "open-source", "ai", "economics"]
featured: true
draft: false
readTime: 13
---

## The complaint, stated properly

There is a version of this argument I hear a lot, and it usually comes out as "the big problems in computing are solved, all that's left is the boring stuff." I do not think that framing survives contact with reality, mostly because *boring* is not a property of a problem. It is a property of a relationship between a person and a problem. Somebody out there finds compiler register allocation thrilling. Somebody else has spent eleven years on the ext4 mailing list and would not trade it. Calling the remaining work boring is a taste claim wearing a lab coat.

But underneath the taste claim there is a real one, and it is much harder to dismiss:

**You cannot do the interesting work from your bedroom any more.**

That is the thing people actually mean. Not that the problems ran out. That the problems that are left over for *a person* ran out - because the frontier now requires a few hundred people, a few hundred million dollars, and a relationship with a company that can get you thirty thousand accelerators.

That version of the complaint is not subjective. It is measurable. And it turns out we can measure it, because another field went through this exact transition in living memory and left the paperwork behind.

## Physics did this first, and it has a name

In July 1961, Alvin Weinberg - then director of Oak Ridge National Laboratory - published ["Impact of Large-Scale Science on the United States"](https://www.science.org/doi/10.1126/science.134.3473.161) in *Science*. That paper is where the phrase **Big Science** comes from. Weinberg's argument was not that physics had run out of questions. It was that the questions that remained had grown so expensive that answering them now required accelerators, bureaucracies, budget lines and standing armies of technicians - and that this would change the character of the people who could do science at all. He was openly nostalgic for what he called the era of the individual investigator, and openly worried about what replaced it. He compared the new machines to the pyramids and the cathedrals, which was not entirely a compliment.

Two years later, Derek de Solla Price gave the Pegram Lectures at Brookhaven and turned them into [*Little Science, Big Science*](https://en.wikipedia.org/wiki/Little_Science,_Big_Science) (1963), which did the quantitative version: measuring science as a growth curve and showing that the transition from lone researchers to industrial-scale collaboration was structural and irreversible, not a fashion.

If you want the punchline of that transition in a single artefact: on 14 May 2015, ATLAS and CMS published a combined measurement of the Higgs boson mass in *Physical Review Letters*. The paper is 33 pages long. Nine of those pages are the physics. [The remaining twenty-four are the list of its 5,154 authors](https://www.nature.com/articles/nature.2015.17567).

Michelson made his name with an interferometer he could set up in a basement. A century later, the same field publishes results whose author list is four times longer than the result.

Computing is somewhere around 1961 right now.

## The receipts

**Training a frontier model.** The Stanford [AI Index](https://hai.stanford.edu/ai-index/2025-ai-index-report) puts GPT-4's training compute at roughly $78 million and Gemini Ultra's at roughly $191 million - and those are compute estimates, not the salaries, not the data, not the failed runs that came first. In 2017, the original Transformer was trained for something in the region of $670.

**The infrastructure underneath it.** Google, Amazon, Microsoft and Meta are between them heading for [roughly $700 billion of capital expenditure in 2026](https://www.cnbc.com/2026/02/06/google-microsoft-meta-amazon-ai-cash.html), up from around $410 billion in 2025. Not revenue. Capex. In one year, from four companies.

**The layer under *that*.** TSMC's Arizona commitment is now [$165 billion](https://www.tomshardware.com/tech-industry/tsmc-commits-another-100-billion-to-arizona-for-at-least-four-more-2nm-fabs). A single ASML High-NA EUV machine runs around $370 million, and a fab needs more than one. There are exactly three companies on Earth at the leading edge of logic manufacturing and exactly one that can sell them the lithography.

**And the people.** Read the author list on any frontier model report. It is a phone book. This is the ATLAS paper again, arriving in our field, on schedule, about sixty-five years after it arrived in physics.

So: yes. The complaint is correct on the facts. A single motivated person with a good idea and a decent machine cannot train a frontier model, cannot fabricate a chip, and cannot fund the electricity bill. That door is shut and it is not reopening.

## It is not just AI, which is the part that should worry you

If this were only about model training, it would be a story about one exotic corner of the industry. It is not.

**The kernel.** The Linux 6.18 cycle took [13,710 commits from 2,134 developers](https://lwn.net/Articles/1046966/) - the most developers of any release in the kernel's history. Meanwhile the share of kernel work done by unpaid volunteers has fallen to under 8%, from nearly 12% a decade earlier, and the top ten corporate contributors account for well over half of all changes. The most famous "some guy in Finland" project in history is now, statistically, a consortium.

**Browsers.** There are three engines left: Blink, WebKit, Gecko. Two of them are funded by the same advertising company, directly or otherwise. Writing a new one is the canonical example of a thing an individual cannot do.

And here is the detail I keep coming back to, because it is the whole argument in one project. [Ladybird](https://ladybird.org/) started as Andreas Kling's side project inside SerenityOS - the purest "one person, from scratch, at home" story of the last decade. To have any chance of shipping a real browser, it had to become a 501(c)(3) non-profit with a million dollars from GitHub's co-founder, a payroll of full-time engineers, and a roadmap of alpha 2026, beta 2027, stable 2028. Ahead of the alpha it [stopped taking public pull requests](https://ladybird.org/) so that maintainers could drive the code directly. None of that is a criticism - it is exactly the right call, and it is the only way that browser ever ships. It is also a perfect miniature of the transition: even the romantic solo project has to institutionalise or die.

That is what Weinberg was describing. Not the end of curiosity. The end of curiosity being *enough*.

## The honest counter-case, because there is one

I would be doing the same thing I complained about at the top - taste dressed as analysis - if I stopped here. So, the other side, with the same standard of evidence.

**Individuals still ship load-bearing software.** [SQLite](https://sqlite.org/) is maintained by a team you could fit in a taxi and is very likely the most widely deployed database in existence. [curl](https://curl.se/) is Daniel Stenberg and a small circle of contributors, and it runs in billions of devices. [llama.cpp](https://github.com/ggml-org/llama.cpp) is what actually put local model inference on ordinary hardware, and it started as one person's C++ project. These are not hobby toys, they are infrastructure, and they were not built by consortia.

**The cost curve runs the other way after the frontier.** DeepSeek published, in the [V3 technical report](https://arxiv.org/abs/2412.19437), a training figure of 2.788M H800 GPU-hours - about $5.58M at rental rates - for a 671B-parameter model. That number excludes all the prior research, and people argued about it for months, correctly. But even discounted heavily it says something real: the second person to do a thing pays a fraction of what the first person paid.

**And inference collapsed.** The AI Index found the cost of querying a model at GPT-3.5-level performance fell from $20.00 to $0.07 per million tokens between November 2022 and October 2024. A 280-fold drop in about eighteen months, with open-weight models closing to within a couple of points of closed ones on several benchmarks.

So the accurate statement is narrower than the complaint. It is not that a person at home can do less than before. A person at home today has capabilities that a 2019 research lab did not. It is that **the frontier of production has separated from the frontier of use**, and only one of those is open to you.

## What actually changed, precisely

Here is where I think the diagnosis needs sharpening, because "you need a big company now" has been true of most of computing for most of its history and we never minded.

Nobody has fabricated a competitive chip at home since roughly 1975. Nobody writes a production operating system alone. Nobody lays transatlantic cable as a weekend project. Every one of those was industrialised long ago, and it never felt like a loss, because each time the industrialised layer got wrapped in an interface and handed downward. You could not build the CPU, but you could program it. You could not build the network, but you could put a server on it. The deal was always: *we take the capital-intensive layer, you get an API and total freedom above it.*

What is different now is the **position of the line**. It has moved up into the layer people considered the creative one - not the substrate, but the thing that reasons about the problem. And unlike a CPU or a TCP stack, that layer is not a fixed interface you build on top of. It is a moving product controlled by whoever paid for the training run, priced by them, deprecated by them, aligned by them.

That is the real loss, and it is worth being precise about, because it is not emotional and it is not about fun. It is:

- **Auditability.** You cannot inspect what you cannot rebuild. Open weights help, and they matter enormously, but weights without the data and the training pipeline are a binary, not source.
- **Exit.** Independence is only real if leaving is possible. When the capable substrate lives on five balance sheets, "just switch" stops being a sentence with meaning.
- **Direction.** Whoever funds the frontier chooses what the frontier is *for*. Nobody at home gets a vote on what the next model is optimised to be good at.

Weinberg saw all three coming for physics in 1961, and he was right. Big Science delivered results that Little Science genuinely could not have produced - the Higgs boson is real and no lone investigator was ever going to find it - and it also permanently changed who gets to ask the questions. Both things happened. Both things are happening to us.

## So what is a person at home supposed to do

Not consolation. Actual answers, because I think the picture is less bleak than the framing suggests.

**Own the layer above.** The models are converging, the prices are falling, and everyone has the same substrate. Which means the differentiator moves to product, distribution, taste and domain knowledge - all of which are still radically cheap to acquire alone. This is the same pattern as commodity hardware: when the expensive layer commoditises, value migrates up. It is not a consolation prize, it is where most of the money has always been.

**Own the layer nobody funds.** curl, SQLite, ffmpeg, OpenSSL, the boring infrastructure that everything sits on - none of it is capital-intensive, all of it is under-maintained, and every one of those projects is more consequential than most of what gets funded. The reason it is available to you is precisely that it cannot be captured by spending money.

**Do the thing that requires being small.** A hundred-person org cannot ship a weird, specific, opinionated tool for four thousand people. You can, in a weekend, and now with better leverage than anyone in history. The constraint that killed the bedroom frontier is capital, and capital is bad at small.

**And be loud about the exit routes.** Open weights, local inference, reproducible pipelines, interoperable formats. These are the levers that keep the industrialised layer behaving like a substrate instead of a landlord. They are worth defending even when the frontier is out of reach - *especially* then.

## What I actually think

I think the complaint is right and the conclusion drawn from it is wrong.

Right: development at the frontier is no longer something a person does at home. It takes hundreds of people and the kind of money that only a handful of companies on the planet can raise. That is a structural fact with receipts - $700 billion of capex in one year, a $165 billion fab, author lists that need their own index. Anyone telling you otherwise is selling a course.

Wrong: that this means the field is finished, or that it is now only chores. Physics after 1961 was not finished. It got the Standard Model, gravitational waves, and the Higgs - none of which a lone investigator could have touched - while at the same time a person with a laptop could do computational work that Michelson could not have dreamed of. Both halves of that were true at once, and the field mourned Little Science while quietly becoming more productive than it had ever been.

What I refuse to accept is the emotional conclusion, which is that the individual's work is now derivative and therefore worthless. Almost everything anyone has ever used is downstream of infrastructure they could not build. Linus did not fabricate his CPU. Stenberg did not lay the cables. The interesting question was never "did you build the whole stack," because nobody ever has. It is whether the layer you cannot build stays open enough to build on.

*That* is the fight worth having, and it is not nostalgia, it is policy: open weights, real interoperability, antitrust that understands compute, and public funding for the infrastructure that no business model rewards. Whether a person at home can still do something that matters in 2036 is not going to be decided by how clever any of us are. It is going to be decided by whether the industrialised layer behaves like a road or like a toll booth.

Weinberg wrote that Big Science was here to stay, and that the hard part was making the financial and educational choices it forced on us. Sixty-five years later, that is still the sentence. We just have to notice it is ours now.

---

## References

**The Big Science transition**

- [Impact of Large-Scale Science on the United States - Alvin Weinberg, *Science* 134, 1961](https://www.science.org/doi/10.1126/science.134.3473.161) ([full text PDF](https://www.andreasaltelli.eu/file/repository/Weinberg_Big_Science.pdf))
- [Little Science, Big Science - Derek J. de Solla Price, 1963](https://en.wikipedia.org/wiki/Little_Science,_Big_Science)
- [Physics paper sets record with more than 5,000 authors - *Nature* news, 2015](https://www.nature.com/articles/nature.2015.17567) (the [paper itself](https://arxiv.org/abs/1503.07589), 5,154 authors, *Phys. Rev. Lett.* 114, 191803)

**What the frontier costs now**

- [The 2025 AI Index Report - Stanford HAI](https://hai.stanford.edu/ai-index/2025-ai-index-report) (training cost estimates, inference cost decline, open vs closed model gap)
- [Tech AI spending approaches $700 billion in 2026 - CNBC, February 2026](https://www.cnbc.com/2026/02/06/google-microsoft-meta-amazon-ai-cash.html)
- [TSMC commits another $100 billion to Arizona - Tom's Hardware](https://www.tomshardware.com/tech-industry/tsmc-commits-another-100-billion-to-arizona-for-at-least-four-more-2nm-fabs)
- [DeepSeek-V3 Technical Report](https://arxiv.org/abs/2412.19437) (2.788M H800 GPU-hours, ~$5.58M at rental rates)

**The same thing happening outside AI**

- [Some 6.18 development statistics - LWN.net](https://lwn.net/Articles/1046966/) (13,710 commits, 2,134 developers, a record)
- [Linux Foundation kernel development reports - who writes Linux and who pays them](https://www.linuxfoundation.org/press/press-release/the-linux-foundation-releases-linux-development-report)
- [Ladybird Browser Initiative](https://ladybird.org/) - independent engine, 501(c)(3), alpha 2026

**Individuals still shipping infrastructure**

- [SQLite](https://sqlite.org/)
- [curl](https://curl.se/)
- [llama.cpp](https://github.com/ggml-org/llama.cpp)
