---
title: "flex-wrap: balance, or How to Stop Your Last Row Looking Broken"
publishDate: "2026-07-21T10:00:00.000Z"
slug: "css-flex-wrap-balance-last-row-problem"
description: "Chrome 150 shipped flex-wrap: balance, an eight-year-old proposal that finally kills the orphaned last row. The algorithm behind it is from 1981. Heres how it works, a lab to play with, and why its still Chromium-only."
tags: ["css", "frontend", "web development", "web standards", "flexbox", "tutorial"]
featured: true
draft: false
readTime: 13
---

## The row that ruins everything

Youve written this layout a hundred times. Cards, tags, logos, filter chips. `display: flex`, `flex-wrap: wrap`, a `gap`, and `flex: 1` or `flex: auto` on the children so they fill the width. Looks great. Then someone adds a seventh item.

Three on the first line. Three on the second. And one poor item alone on the third, stretched across the entire container because `flex: auto` told it to eat every pixel of leftover space. A tag pill the size of a billboard. A logo card four times wider than its siblings. Nothing broke. It just looks like something broke, which as far as anyone reviewing your PR is concerned is the same thing.

Thats the last-row problem, and its probably the single most common reason people give up on wrapping flexboxes and reach for something else. [Ian Kilpatrick](https://github.com/bfgeek), the Chrome engineer who wrote the explainer for the fix, put it plainly: without balancing, "it is trivial to create flex-lines which appear unbalanced (lots of whitespace in a particular line). This often makes developers avoid wrapping flexboxes, instead allowing content to overflow."

Chrome 150 landed on 30 June 2026 with a one-word fix:

```css
.cards {
  display: flex;
  flex-wrap: balance;
  gap: 12px;
}
```

Thats it. Thats the feature. What the browser does behind that keyword, and the eight years it took to get there, is where this gets interesting.

## What we did instead, and why all of it was bad

Quick tour of the workarounds first, because knowing what youre replacing tells you what youre actually getting.

The classic is the ghost element. Stick an invisible filler child at the end with `flex: auto` and no height, so the last real row has something to share space with:

```css
.cards::after {
  content: "";
  flex: auto;
}
```

Works, sort of. It stops the last item from stretching. It doesnt balance anything, though. You still end up with one item alone on the last line, just left-aligned now instead of stretched. Youve also stuck a pseudo-element in the box tree that some screen readers will happily announce if you get sloppy with `content`.

Then theres quantity queries: `:nth-last-child(n+7)` selectors that switch the layout based on child count. Fine when the count is known and small. Useless the moment the list is dynamic, and they cant see container width at all, which is the actual problem. How many items fit per line depends on available space, and no CSS selector has ever been able to measure that.

`column-count` does balance properly. Thats why people reached for it. But it fills top-to-bottom within each column, so the reading order goes all the way down column one before jumping to column two. For a card grid thats just wrong, and its wrong in a way that hurts keyboard and screen reader users rather than merely looking odd.

Grid with `auto-fill` is the one I still recommend most often. `repeat(auto-fill, minmax(200px, 1fr))` gives you tidy equal columns, no stretched orphan, and it has worked everywhere for years. Keep using it. But grid forces every item into the same track width, so if your items have genuinely different intrinsic widths (tags, chips, buttons with labels of wildly varying length) grid flattens them all to the same size. Thats a different design, not a fix.

And then JavaScript. Measure everything, compute the breaks, apply widths. Perfect results, at the cost of a resize observer, layout thrash on every reflow, and a flash of unbalanced content before the script runs. The same deal we made with Masonry.js for about a decade.

All of these give something up. `flex-wrap: balance` doesnt, because the layout engine already knows every items size at the exact moment its deciding where to break.

## The syntax

The formal grammar in [CSS Flexible Box Layout Module Level 2](https://drafts.csswg.org/css-flexbox-2/) is:

```
flex-wrap = nowrap | [wrap | wrap-reverse] || balance
```

That `||` is doing real work. `balance` isnt a fourth sibling of `nowrap`, `wrap` and `wrap-reverse`; its a modifier that combines with them. So all of these are valid:

```css
flex-wrap: balance;              /* balancing, wrapping implied */
flex-wrap: wrap balance;         /* the same thing, spelled out */
flex-wrap: wrap-reverse balance; /* balanced, lines stacked in reverse */
```

`nowrap balance` isnt, which makes sense. Nothing to balance if you never wrap.

The specs own description, tucked into the module interactions section, is that `flex-wrap: balance` "allows for flex items to be balanced across lines, so each line is as similar in length as possible." Note *length*. Not item count. Ill come back to this later because it catches people out, but the short version is that balance is not trying to put four items on each row. Its trying to make each rows total content length come out roughly equal. For uniform items those happen to be the same thing. For mixed widths they arent, and length is the one that looks right to a human eye.

Spec editors, for the record: [Tab Atkins Jr.](https://www.xanthir.com/blog/) at Google, [Elika J. Etemad / fantasai](https://fantasai.inkedblade.net/) at Apple, and [Rossen Atanassov](https://github.com/atanassov) at Microsoft.

## The algorithm is from 1981

This is my favorite part. The browser isnt doing anything new and clever to balance your cards. Its running a paper from 1981.

The [Knuth-Plass line-breaking algorithm](https://onlinelibrary.wiley.com/doi/abs/10.1002/spe.4380111102), from [Donald Knuth](https://www-cs-faculty.stanford.edu/~knuth/) and Michael Plass, is how TeX decides where to break a paragraph into lines. Its insight is that greedy breaking (cram as much as fits, move on, repeat) is locally optimal and globally awful. It produces precisely the failure mode were talking about: beautiful early lines, a wrecked final one. Knuth-Plass assigns a "badness" cost to every candidate set of breaks across the whole paragraph and picks the arrangement with the lowest total. It looks ahead instead of being greedy.

A wrapping flexbox is that same problem wearing different clothes. Flex items are words, flex lines are text lines, and each items hypothetical main size is its width. So Kilpatricks proposal is: run Knuth-Plass over the flex items hypothetical main sizes. Same math, different content model.

Two details from the explainer tell you what this costs you. First, it targets O(N log N), deliberately staying in the same complexity class as things flexbox already does, like sorting items by `order`. Not free, but not a quadratic trap either. Second, theres a fast path for uniform items. When everything is the same size, which is the common case for a card grid, the expensive search gets skipped entirely because the optimal answer is just arithmetic. You only pay for the real algorithm when your content actually varies.

Column-direction flexboxes need an extra step. In a row flexbox the container has a known width, so you know what youre breaking against. In a column flexbox there usually isnt a height constraint at all, so theres nothing to break against. The fix is bisection: search for the line-break size that yields the minimum number of lines, then run the balancing algorithm with that constraint pinned. Same trick browsers already use for `column-count` height balancing, applied one layer up.

## One difference from text-wrap: balance

Everyone reaches for the `text-wrap: balance` comparison, including the Chrome release notes, and its a fair one. But theres a difference that matters.

Chromium caps `text-wrap: balance` at six lines. Past that it quietly gives up and wraps normally. Thats purely a performance guard: balancing text means iterating over candidate break points, and the cost climbs with the amount of text. Which is why `text-wrap: balance` is a headline tool and not a paragraph tool, and why slapping it on `*` is wasted work on everything that blows the cap.

There is no equivalent documented cap for `flex-wrap: balance`. That falls out of the fast path and the O(N log N) target rather than being an oversight. The set of flex items in a container is far smaller and far more tractable than the break opportunities in a block of prose, and the uniform-size shortcut covers most real usage anyway. So unlike its text cousin, you dont have to ration this one.

Doesnt mean spray it everywhere. On a container that never wraps its a no-op. On one that wraps into exactly two full lines, balance and wrap give you the same output. It earns its keep when the item count doesnt divide cleanly into the line capacity, which admittedly is most of the time.

## The lab: try it yourself

Heres a live flexbox to push around. Change the item count, the container width, the base size, then hit the toggle to flip between `wrap` and `balance` and watch the last line snap into place.

If your browser supports it, youll see real balancing. If it doesnt, the toggle does nothing and the banner says so, which is a decent demo of the degradation story in its own right.

<div class="bg-white/5 border border-white/10 rounded-xl p-6 my-6">
  <div id="fwb-support" class="mb-4 px-3 py-2 rounded-md text-sm text-center font-mono"></div>

  <div class="flex flex-wrap gap-5 justify-center mb-5 text-sm">
    <label class="flex flex-col items-center gap-1">
      <span class="text-gray-400">items: <span id="fwb-count-val" class="text-indigo-400 font-mono">7</span></span>
      <input type="range" id="fwb-count" min="3" max="16" step="1" value="7" class="accent-indigo-500 cursor-pointer">
    </label>
    <label class="flex flex-col items-center gap-1">
      <span class="text-gray-400">container: <span id="fwb-width-val" class="text-indigo-400 font-mono">100</span>%</span>
      <input type="range" id="fwb-width" min="40" max="100" step="5" value="100" class="accent-indigo-500 cursor-pointer">
    </label>
    <label class="flex flex-col items-center gap-1">
      <span class="text-gray-400">base size: <span id="fwb-basis-val" class="text-indigo-400 font-mono">140</span>px</span>
      <input type="range" id="fwb-basis" min="80" max="220" step="10" value="140" class="accent-indigo-500 cursor-pointer">
    </label>
    <label class="flex flex-col items-center gap-2 justify-center">
      <span class="text-gray-400">item widths</span>
      <button id="fwb-vary" class="px-3 py-1.5 bg-white/10 border border-white/20 rounded-md text-gray-300 cursor-pointer text-xs hover:bg-white/20 transition-colors">uniform</button>
    </label>
    <label class="flex flex-col items-center gap-2 justify-center">
      <span class="text-gray-400">wrapping</span>
      <button id="fwb-mode" class="px-3 py-1.5 bg-indigo-500/20 border border-indigo-400/40 rounded-md text-indigo-200 cursor-pointer text-xs hover:bg-indigo-500/30 transition-colors">flex-wrap: wrap</button>
    </label>
  </div>

  <div class="w-full flex justify-center">
    <div id="fwb-demo" class="border border-dashed border-white/20 rounded-lg p-2"></div>
  </div>

  <code id="fwb-css" class="block bg-black/30 px-4 py-3 rounded-lg text-xs text-sky-300 mt-5 whitespace-pre"></code>
</div>

<script>
(function() {
  const demo = document.getElementById('fwb-demo');
  const cssOut = document.getElementById('fwb-css');
  const support = document.getElementById('fwb-support');
  const countInput = document.getElementById('fwb-count');
  const widthInput = document.getElementById('fwb-width');
  const basisInput = document.getElementById('fwb-basis');
  const varyBtn = document.getElementById('fwb-vary');
  const modeBtn = document.getElementById('fwb-mode');

  let balanced = false;
  let varied = false;

  const supported = CSS.supports('flex-wrap', 'balance');
  if (supported) {
    support.textContent = 'flex-wrap: balance supported — the toggle below is doing real work';
    support.className += ' bg-emerald-500/15 text-emerald-300 border border-emerald-500/30';
  } else {
    support.textContent = 'flex-wrap: balance not supported here — toggle falls back to plain wrap (Chrome/Edge 150+)';
    support.className += ' bg-amber-500/15 text-amber-300 border border-amber-500/30';
  }

  const colors = ['#6366f1','#8b5cf6','#ec4899','#f59e0b','#10b981','#06b6d4','#3b82f6','#ef4444','#84cc16','#a855f7','#14b8a6','#f97316','#eab308','#22c55e','#0ea5e9','#d946ef'];
  const jitter = [1, 1.6, 0.8, 1.2, 2, 0.9, 1.4, 1, 1.8, 0.85, 1.3, 1.1, 1.7, 0.95, 1.5, 1.05];

  demo.style.display = 'flex';
  demo.style.gap = '10px';

  function apply() {
    const count = +countInput.value;
    const width = +widthInput.value;
    const basis = +basisInput.value;

    document.getElementById('fwb-count-val').textContent = count;
    document.getElementById('fwb-width-val').textContent = width;
    document.getElementById('fwb-basis-val').textContent = basis;

    demo.style.width = width + '%';
    demo.style.flexWrap = (balanced && supported) ? 'balance' : 'wrap';

    demo.innerHTML = '';
    for (let i = 0; i < count; i++) {
      const item = document.createElement('div');
      const b = varied ? Math.round(basis * jitter[i]) : basis;
      item.style.flex = '1 1 ' + b + 'px';
      item.style.background = colors[i];
      item.style.height = '56px';
      item.style.borderRadius = '8px';
      item.style.color = 'white';
      item.style.display = 'flex';
      item.style.alignItems = 'center';
      item.style.justifyContent = 'center';
      item.style.fontFamily = 'monospace';
      item.style.fontWeight = 'bold';
      item.style.fontSize = '14px';
      item.textContent = i + 1;
      demo.appendChild(item);
    }

    cssOut.textContent =
      '.cards {\n' +
      '  display: flex;\n' +
      '  flex-wrap: ' + (balanced ? 'balance' : 'wrap') + ';\n' +
      '  gap: 10px;\n' +
      '}\n\n' +
      '.cards > * {\n' +
      '  flex: 1 1 ' + basis + 'px;\n' +
      '}';
  }

  countInput.addEventListener('input', apply);
  widthInput.addEventListener('input', apply);
  basisInput.addEventListener('input', apply);

  varyBtn.addEventListener('click', function() {
    varied = !varied;
    this.textContent = varied ? 'varied' : 'uniform';
    apply();
  });

  modeBtn.addEventListener('click', function() {
    balanced = !balanced;
    this.textContent = 'flex-wrap: ' + (balanced ? 'balance' : 'wrap');
    apply();
  });

  apply();
})();
</script>

The setting to look at is 7 items, full width, uniform sizing. With `wrap` you get a clean first line, a clean second line, and one absurd item alone at the bottom. Flip to `balance` and the engine pulls items down until the lines even out. Now switch widths to "varied" and watch it behave differently: its equalising line *length*, so youll see rows with different item counts. Thats correct, not a bug.

## Eight years, and it wasnt the syntax

This proposal is older than a lot of the CSS you use every day. The timeline is honestly the most interesting thing about the whole feature.

Tab Atkins opened [csswg-drafts issue #3070](https://github.com/w3c/csswg-drafts/issues/3070) on **30 August 2018**, titled "[css-flexbox-2] Add flex-wrap: balance;". The framing was borrowed openly: developers want for flex lines what `text-wrap: balance` gives them for text lines. Tagged for Level 2. Then it sat there.

**22 December 2024**, [Johannes Odland](https://github.com/johannesodland) publishes ["Web Wish 22: Flex-Wrap Balance"](https://odland.dev/2024/12/22/web-wish-22-flex-wrap-balance.html), part of an advent-calendar run of platform requests. His one-line description of the pain is still the cleanest Ive read: with `flex: auto`, "the items on that last row can end up awkwardly large." He also raises something the spec still hasnt answered, which is whether authors should get to say which end of the cross axis absorbs the leftover space.

**21 May 2025**, Kilpatrick shows up on the issue with an implementation plan and publishes the [flex-wrap-balance explainer](https://github.com/bfgeek/flex-wrap-balance): Knuth-Plass, the O(N log N) target, the uniform-size fast path, the bisection for column flexboxes. It also lists what got rejected along the way. A separate `flex-line-style` property. Direct break-after-item controls. And the perennial "just use multi-column".

Chrome 149 puts it behind a developer trial on desktop and Android. **27 May 2026**, the [Intent to Ship](https://groups.google.com/a/chromium.org/g/blink-dev/c/Z8Rb8Q4W2jM/m/1swkEDvLBgAJ) goes out on blink-dev targeting Chrome 150 across desktop, Android and WebView, with full [Web Platform Tests coverage](https://wpt.fyi/results/css/css-flexbox) confirmed. [Chrome 150](https://developer.chrome.com/release-notes/150) hits stable on **30 June 2026**, shipping it next to `text-fit`, animatable `zoom`, `background-clip: border-area` and rounded `polygon()`. [Edge 150](https://learn.microsoft.com/en-us/microsoft-edge/web-platform/release-notes/150) follows two days later on **2 July**, as Chromium downstreams do.

Eight years. And the thing that unblocked it wasnt a syntax fight, which is what usually holds these up. The syntax was basically settled back in 2018. What took the time was somebody working out an implementation fast enough to be worth shipping.

## The honest support situation

Small warning before the good news. Ive seen a handful of posts this month calling `flex-wrap: balance` broadly supported and citing Chrome 114, Firefox 121, Safari 17.5. Those numbers belong to `text-wrap: balance`. Somebody copied them onto the wrong feature and the rest of the internet dutifully reposted it. Dont plan around them.

Heres the real position, straight from the Intent to Ship filed on 27 May 2026:

- **Mozilla (Gecko):** no signal
- **WebKit:** no signal
- **Interoperability risk:** stated as significant — "other browsers do not implement"

So: Chromium-only, with neither Mozilla nor Apple saying anything either way. The argument Chrome makes in the same document is the one that actually justifies shipping anyway, and its that the thing degrades gracefully. A browser that doesnt understand `balance` throws the declaration out as invalid, falls back to whatever `flex-wrap` was before it, and you get an ordinary wrapping flexbox. Nothing breaks. Nobody sees an error. The layout is just a bit less tidy. Thats the exact adoption path `text-wrap: balance` took and it worked out fine.

So you can ship this today. No build step, no polyfill:

```css
.cards {
  display: flex;
  flex-wrap: wrap;   /* everyone gets this */
  gap: 12px;
}

@supports (flex-wrap: balance) {
  .cards {
    flex-wrap: balance;  /* Chromium 150+ gets the tidy last row */
  }
}
```

The `@supports` wrapper isnt strictly necessary, mind you. Write `flex-wrap: wrap balance` as one declaration and browsers that cant parse `balance` will bin the whole thing, which is why splitting it in two, or gating with `@supports`, is the safer spelling. Costs you two lines.

## What balance wont do for you

Three things to keep straight, since these are the questions Id expect in review.

It wont give you equal item counts per row. It equalises line length, and those two only coincide when your items are the same size. If what you actually need is "exactly four per row", thats grid, not balance.

It wont take direction from your `flex-grow` values, at least not where you might expect. Adam Argyles write-up puts it well: "uneven wrapped rows still need the browser to own the balance." Where the lines break is the engines call, made before growth gets applied. `flex-grow` still does its normal job inside each line, it just doesnt get a vote on the breaks.

And it wont touch DOM order. This one matters, and its where the feature is meaningfully less dangerous than `grid-lanes` masonry. Balancing moves line breaks, nothing else. Item 5 is still after item 4 and before item 6, in the DOM, in the accessibility tree, in tab order. No reading-order footgun. Nothing to audit.

## So, ship it?

Ship it. `flex-wrap: balance` is a tiny feature and I like it more than its size warrants. It kills a real, daily, deeply boring annoyance without a new layout mode to learn, without a new mental model, and without the tradeoffs every workaround made us accept. One keyword.

Theres also something pleasing about where the fix came from. Your card grid gets sorted out by a typesetting algorithm Knuth published in 1981 for laying out TeX paragraphs, aimed at a different kind of box. Good ideas keep.

Chromium-only for now, silence from Firefox and Safari, and the worst case is exactly what you already have today. Two lines behind `@supports` and your last row quietly stops embarrassing you in Chrome.

---

## References

**Specification and standards process**

- [CSS Flexible Box Layout Module Level 2 — W3C Editor's Draft](https://drafts.csswg.org/css-flexbox-2/)
- [csswg-drafts issue #3070: [css-flexbox-2] Add flex-wrap: balance;](https://github.com/w3c/csswg-drafts/issues/3070) — opened by Tab Atkins, 30 August 2018
- [csswg-drafts issue #9086: [css-flexbox] flex-wrap: balance;](https://github.com/w3c/csswg-drafts/issues/9086)
- [Ian Kilpatrick's comment on #3070 — public-css-archive, 21 May 2025](https://lists.w3.org/Archives/Public/public-css-archive/2025May/0615.html)
- [flex-wrap-balance explainer — Ian Kilpatrick](https://github.com/bfgeek/flex-wrap-balance)

**Implementation and shipping**

- [Intent to Ship: flex-wrap:balance — blink-dev, 27 May 2026](https://groups.google.com/a/chromium.org/g/blink-dev/c/Z8Rb8Q4W2jM/m/1swkEDvLBgAJ)
- [Chrome Platform Status: flex-wrap:balance](https://chromestatus.com/feature/4547107962486784)
- [Chrome 150 release notes — 30 June 2026](https://developer.chrome.com/release-notes/150)
- [Chrome 150 beta — Chrome for Developers](https://developer.chrome.com/blog/chrome-150-beta)
- [Microsoft Edge 150 web platform release notes — 2 July 2026](https://learn.microsoft.com/en-us/microsoft-edge/web-platform/release-notes/150)
- [Web Platform Tests: css-flexbox results](https://wpt.fyi/results/css/css-flexbox)

**Documentation and community**

- [flex-wrap — MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/CSS/flex-wrap)
- [Flex Wrap Balance — Adam Argyle, 30 May 2026](https://nerdy.dev/flex-wrap-balance)
- [Web Wish 22: Flex-Wrap Balance — Johannes Odland, 22 December 2024](https://odland.dev/2024/12/22/web-wish-22-flex-wrap-balance.html)
- [CSS text-wrap: balance — Chrome for Developers](https://developer.chrome.com/docs/css-ui/css-text-wrap-balance)
- [A Complete Guide to CSS Flexbox — CSS-Tricks](https://css-tricks.com/snippets/css/a-guide-to-flexbox/)

**People and prior art**

- [Ian Kilpatrick (bfgeek) — GitHub](https://github.com/bfgeek)
- [Tab Atkins Jr. — xanthir.com](https://www.xanthir.com/blog/)
- [Elika J. Etemad (fantasai)](https://fantasai.inkedblade.net/)
- [Rossen Atanassov — GitHub](https://github.com/atanassov)
- [Adam Argyle — nerdy.dev](https://nerdy.dev/) · [@argyleink on X](https://x.com/argyleink) · [@nerdy.dev on Bluesky](https://bsky.app/profile/nerdy.dev)
- [Una Kravets — una.im](https://una.im/)
- [Johannes Odland — GitHub](https://github.com/johannesodland) · [@johannes on Mastodon](https://front-end.social/@johannes)
- [Donald E. Knuth — Stanford](https://www-cs-faculty.stanford.edu/~knuth/)
- [Breaking Paragraphs into Lines — Knuth & Plass, Software: Practice and Experience, 1981](https://onlinelibrary.wiley.com/doi/abs/10.1002/spe.4380111102)
