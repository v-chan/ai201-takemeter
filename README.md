# TakeMeter

> ### 👋 Start here
>
> **New to this repo? Read [RUNNING.md](RUNNING.md) first** — setup, the
> notebook, the baseline, and what to do when something breaks.
>
> Once `python test.py` passes:
>
> ```bash
> head -5 data/practice_labels.csv     # the shape your labels.csv needs
> ```
>
> Then open `takemeter.ipynb` **in this folder** — in VS Code, or with
> `jupyter notebook` if you prefer. Pick the kernel: the `.venv` inside this
> project. Run section 1, which reports the hardware you'll be training on.
> Everything else waits until you have data.
>
> Nothing to upload, nothing to connect, no accounts and no keys. The notebook
> runs on your machine and writes next to your code.
>
> **The rest of this file is your submission.** Fill it in as you go.

---

<!-- ─────────────────────────────────────────────────────────────────────────
     HOW TO USE THIS FILE

     Unit 5 asks for the first five sections. Unit 6 adds the five below them.

     Everything is pasted as TEXT. No screenshots, no images.

     ⚠️ The confusion matrix especially. The notebook prints one as a markdown
     table, ready to copy. A screenshot of a matrix earns nothing. Paste the
     table.
     ───────────────────────────────────────────────────────────────────────── -->

<!-- ═══════════════════════ UNIT 5 — THE BUILD ═══════════════════════ -->

## What This Does

<!-- Your community, and what your classifier sorts posts into. Three or four
     sentences. -->
This app classifies comments from Y Combinator’s Hacker News into 3 different labels: `unsupported_claim`, `supported_claim`, and `non_argument`.  The first two labels are based on comments that make claims and whether or not it provides evidence to back up their claim whereas the non_argument label are for comments such as questions or reactions. The training dataset was obtained from the comments on this article https://news.ycombinator.com/item?id=49820134.

---

## Label Taxonomy

<!-- Each label: a one-sentence definition and two real examples from your
     reading. Then your decision rule for the hardest boundary.

     The decision rule is worth a point on its own and it's the thing most
     people leave out. Every taxonomy has a hardest boundary. Name yours. -->

### `supported_claim`

**Definition:** A comment that makes a claim and supports it with a specific real example, a source, or a stated reason that’s not a speculation.

**Example 1:**
> Stephen Wolfram had a great description of this effect in the early days (GPT 3.5 era): Machine learning trains the network to do... anything that you reward it for. If you keep training, it keeps getting better. Next word prediction can always keep getting better. At first, simply "learning" spelling is what makes the predictions better because tokens are word chunks, not always whole words. Then, the models "run out of steam" and can't get any better by learning more spelling rules, but the gradient descent forces them to get better... so they do... by learning the rules of grammar. At this point the AIs can output correctly spelled and grammatically coherent sentences, but the sentences ramble on about nonsense topics. So what happens next as the models run out of grammar rules is that they're forced to learn the rules "above grammar": logic, world knowledge, coherent story telling, etc. At some point they learn to output pages and pages of fluid, coherent text, but... if they're not smart, if they don't think, and if they don't know what they're talking about, then they're still "suboptimal" and their forced gradient descent will make them close those gaps. Eventually, the only way they can improve at "next token prediction" is by building up to human-like intelligence, including an inner monologue, theory of mind, and everything. We can even read their "thoughts": https://transformer-circuits.pub/2026/workspace/index.html


**Example 2:**
> In addition to the other comment mentioning CAR-T therapies, we also have therapies based on tumor-infiltrating lymphocytes (TILs) https://www.cancer.gov/news-events/cancer-currents-blog/2024... https://jitc.bmj.com/content/8/2/e000848 (careful: Figure 1 can be very graphical, but it shows the huge positive impact of this therapy) We also have therapies based on monoclonal recombinant antibodies conjugated with chemotherapeutics. Simply put, we can produce antibodies that are specific for markers present in the surface of cancer cells, and we can attach drugs that can kill those cells. The antibody part is what makes this type of therapy very effective (you target only cancer cells, and not healthy cells) and also very expensive. 


### `unsupported_claim`

**Definition:** A comment that makes a claim but is based on speculation or does not provide a source or example.

**Example 1:**
> The singularity, as defined by Hinton (and others) as RSI (Recursive Self Improvement) may actually be beginning already, as OpenAI has announced an AI acting as a "research intern" (!).


**Example 2:**
> It's really good at pattern recognition. So I'm not sure how it knows to be 'surprised' that alone is pretty fascinating.


### `non_argument`

**Definition:** A comment that doesn't make a claim. It can be a reaction, a clarifying question, or lists a source with no commentary.

**Example 1:**
> tl;dr but i <3 it. upvoted this shit.

**Example 2:**
> Did Claude find this on it's own, or did somebody using Claude find it? 



### The hardest boundary

**Which two labels:** supported claim and unsupported claim

**The decision rule I used every time:**
<!-- e.g. "If the post names a specific checkable fact, it's `analysis`, even
     if the tone is heated." -->
If the post provides reasoning or sources that are unreleated to the claim, then it is an unsupported claim.


---

## The Dataset

<!-- Where you collected from, how you labelled, your counts, and three hard
     cases. -->

**Where the posts came from:** I chose comments from Y Combinator’s Hacker News in response to an article discussing Claude discovering a novel enzyme system. https://news.ycombinator.com/item?id=49820134.

**How I labelled them:** <!-- Cold first? Pre-labelled with AI and corrected?
Say so plainly — the disclosure is required, not penalised. -->
I chose 20 posts and labeled them cold first. I then pre-labeled with AI based on my definitions and corrected them as needed.

**Counts per label:**

| Label             | Count | Share |
|-------------------|-------|-------|
| supported_claim   | 59    | 29.5% |
| unsupported_claim | 86    | 43.0% |
| non_argument      | 55    | 27.5% |
| **Total**         | 200   | 100%  |

**Three hard cases**

<!-- Any post that made you pause: what it was, which two labels it could have
     been, and what you chose. These are worth more than the easy 190. -->

**1.**
> *The post:* 
> It isn't a convenient gotcha. It's about what the people pushing the given thing are intending. 
> 
> Person demoing something they made is usually trying to hide the fact they had claude built it and sell it like they didn't. This sort of person often lacks the technical skills to vet that what claude actually produced is actually working as they expect. Hence the snark.
> 
> On the other hand, with anthropic's case, they are trying to say "claude did this, how smart it is" while trying to downplay the fact that they needed it to be steered by domain experts to produce anything worthwhile.
> 
> *Could have been:* `unsupported_claim`
> 
> I chose `supported_claim`, because since they provide specific examples and reasoning to support their claims.

**2.**
> *The post:* 
> No, it’s not odd; IMO. It’s by design.
> I see all of this leading to a setup for: We did cure Cancer, everyone else (Healthcare, Gov., Rx) etc... has just not caught up or even worse; “you just don’t have access top that model/version”.
> I have seen several times on HN recently how people don’t see the impact of AI/more code etc... and I believe this is because its following the K-shape of the current economy.
> At the top where most of us aren’t but CAN see via stock market news etc...; they are making more money by adding efficiencies etc...
> At the bottom; efficiencies are being applied at a scale that they could not before such that social and Gov. programs are more manageable and optimized at scale.

> 
> *Could have been:* `supported_claim`
> 
> I chose `unsupported_claim`, because they provide reasoning based on speculation even though they provide reasoning to support their claim.

**3.**
> *The post:* 
>  “At best an AI might use all those resources to build out further compute and expand its own capabilities. But at that point nobody should be worrying about competing in the labour market, they should be worried about AI making our planet unliveable for carbon based life.”
> It’s the same thing. There will be a period where the AI will be creating consumer products for humans because humans still have some purchasing power, or some resources to trade, then this transitions to the stage where the AI is making the planet unlivable.
> The AI and robots which do not seek resources (for whatever purpose) will be outcompeted in the resource market by AI and robots which DO seek resources. Yes. Compute, land, energy will all be things which AI’s seek. They might also have stranger preferences which emerge like the equivalent of luxury sports cars are for humans. Maybe they’ll be competing to make the largest tungsten cube possible to dunk on their competitors, who knows? That stuff is harder to predict.
> 
> *Could have been:* `suppported_claim`
> 
> I chose `unsupported_claim`  because it relies on speculation about the future to support it’s claim.
---

## The Training Run

<!-- Your starting model, your settings, and anything you changed and why. -->

**Base model:** distilbert-base-uncased

**Settings:** <!-- epochs, learning rate, batch size, seed -->
```
EPOCHS = 3
LEARNING_RATE = 2e-5
BATCH_SIZE = 16
SEED = 42
```



**Anything I changed from the defaults, and why:**
```
MAX_LENGTH = 512
```

I chose to increase the max length because the posts can be quite lengthy and found the inital results had an f1 of 0 for `supported_claim` and `non_argument`. I found that after increasing beyond 128, the f1 of `supported_claim` was non-zero, but the f1 of non_argument remained 0.

**Split sizes:** <!-- train / val / test, and per-label counts in the test
split. If a label had fewer than about 8 in test, say so — it explains a lot
of next unit's variance. -->
```
train 139  ·  val 31  ·  test 30

                   train  val  test
label                              
supported_claim       41    9     9
unsupported_claim     60   13    13
non_argument          38    9     8

```


---

## How I Used AI

<!-- Two specific moments — what you asked, what came back, what you changed.

     ⚠️ Plus disclosure of any pre-labelling. If you had a model pre-label a
     batch and then read and corrected every one, say that. It's an allowed
     workflow and disclosing it costs you nothing. Not disclosing it is the
     problem. -->

**Moment 1**

- *What I asked for:* I asked Claude to fetch comments and put it into a CSV file using the Hacker News API.
- *What came back:* A python script that allowed me to fetch the comments and put it into a CSV file.
- *What I changed:* The script was fine as is so I didn’t not change it itself. I only pasted selected comments from the resulting csv to create my `labels.csv` dataset.

**Moment 2**

- *What I asked for:* I asked for Claude to check the definitions for my labels for unsupported claim and supported claim.
- *What came back:* It suggested I define something more specific for reasoning.
- *What I changed:* I modifed my defintions to account for speculation.

**Pre-labelling disclosure:** I asked Claude to pre-label the remaining comments based on the definitions I wrote for the labels. I then reviewed each of the labels and changed them if they did not meet the definitions. 

Stretch Goal:
1. **50 extra examples for the thinnest label**

Adding more examples for non_agument increased the f1 for it from 0 to 0.811 and improved the overall accuracy. 
**before**:
```
accuracy  0.467
macro F1  0.272

  supported_claim  F1 0.167   (n=9)
  unsupported_claim F1 0.650   (n=13)
  non_argument     F1 0.000   (n=8)
```
**after**:
```
accuracy  0.684
macro F1  0.606

  supported_claim  F1 0.364   (n=9)
  unsupported_claim F1 0.643   (n=13)
  non_argument     F1 0.811   (n=16)

```

**New counts per label:**

| Label             | Count | Share |
|-------------------|-------|-------|
| supported_claim   | 59    | 23.6% |
| unsupported_claim | 86    | 34.4% |
| non_argument      | 105   | 52.5% |
| **Total**         | 250   | 100%  |
** New Split sizes:** 
```
train 174  ·  val 38  ·  test 38

                   train  val  test
label                              
supported_claim       41    9     9
unsupported_claim     60   13    13
non_argument          73   16    16

```


<!-- ═══════════════════════ UNIT 6 — THE TEST ═══════════════════════

     Don't fill these in during unit 5.
     ═══════════════════════════════════════════════════════════════════ -->

---

## Baseline vs. Trained

<!-- Both models on the same posts. `python baseline.py --trained results.json`
     prints this table for you. -->

| Measure | Baseline | Trained | Difference |
|---|---|---|---|
| Overall accuracy |  |  |  |
| Macro F1 |  |  |  |
| F1 — `label_one` |  |  |  |
| F1 — `label_two` |  |  |  |

**What I predicted before I looked:**
<!-- Milestone 1 asks you to write this BEFORE seeing the trained numbers. A
     prediction made afterwards isn't one. -->

**What the gap actually means:**
<!-- If the baseline matched your trained model, your fine-tuning added
     nothing — and that is a real finding, not a failure. Say it plainly. -->



---

## Run Log — Before

<!-- Five criteria across three seeds. The notebook's section 6 prints the
     spread table; the Target and Verdict columns are yours. -->

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

### Confusion matrix

<!-- ⚠️ TYPED AS A MARKDOWN TABLE. The notebook prints one ready to paste.
     An image of a matrix earns nothing. -->

| true \ predicted |  |  |  |
|---|---|---|---|
| **** |  |  |  |
| **** |  |  |  |
| **** |  |  |  |

**My biggest off-diagonal number, and what it means:**
<!-- Not "the model made mistakes" — WHICH boundary it didn't learn, and which
     direction. "7 real analysis posts were called hot_take and only 3 went the
     other way" is a direction, not just an error rate. -->



---

## Verdicts and Diagnoses

<!-- MET or MISSED against LAST UNIT's target. The target has to hold across
     all three seeds, not turn up sometimes. -->

| # | Criterion | Target | Verdict | How I decided |
|---|---|---|---|---|
| 1 |  |  |  |  |
| 2 |  |  |  |  |
| 3 |  |  |  |  |
| 4 |  |  |  |  |
| 5 |  |  |  |  |

**Diagnoses**

<!-- For each miss: the cause, and how you know. The four common causes are:
     too few examples for a label, a boundary you applied inconsistently, a
     genuinely hard label pair, and a task the model can't reach from this
     much data.

     ⚠️ Use your agreement report as evidence. It is the only instrument you
     have that can tell a LABELLING problem from a MODEL problem, and this
     section is graded on whether you used it that way. -->



---

## Agreement Report

<!-- Your rate against the staff set, and every disagreement adjudicated.

     Remember you labelled these 30 under the STAFF taxonomy in
     data/staff_taxonomy.md, not your own — so every argument below is made
     from those definitions and those decision rules. -->

**Agreement rate:** ___ / 30 = ___%

<!-- Nobody grades this number. A 60% who argues every disagreement from the
     stated rules beats a 95% who wrote "staff was right" nine times. Several
     of the 30 were chosen because they're genuinely ambiguous — you should be
     winning some of these. -->

**Disagreements**

<!-- Three lines each: the post, both labels, and who you think is right and
     why — grounded in the staff definitions you were both applying.

     Then sort each into one of three piles:
       (a) the rule covered it and I applied it loosely → a consistency problem
       (b) the rule genuinely doesn't say               → a gap in the definitions
       (c) the rule is ambiguous here and my reading is defensible → argue it.
           This is a legitimate win.

     Pile (a) is the one that matters most for your diagnosis: if you applied a
     written rule two different ways on 30 posts, that is direct evidence about
     what you did across your own 200. -->

**1.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**2.**
> *The post:*
>
> *Staff said / I said:*
>
> *My call, and why:*
>
> *Which pile:*

**What the pattern in my disagreements tells me:**



---

## The Improvement

**What I changed:**

**Which diagnosis pointed at it:**

### Run Log — After

| Criterion | Target | Seed 42 | Seed 7 | Seed 2024 | Verdict |
|---|---|---|---|---|---|
| 1.  |  |  |  |  |  |
| 2.  |  |  |  |  |  |
| 3.  |  |  |  |  |  |
| 4.  |  |  |  |  |  |
| 5.  |  |  |  |  |  |

**Did it help, and how do I know:**

<!-- If it didn't, say so. Relabelling that didn't help is a genuinely
     interesting result and earns full credit. -->



---

## What's Still Broken

<!-- For each criterion still missed: what you'd do, and why you stopped. -->



**The gap between what I meant my labels to capture and what the model
learned:**
<!-- Two sentences. Your confusion matrix is the evidence. -->



<!-- ═════════════════════════════════════════════════════════════════════

     SUBMISSION CHECKLIST — unit 5

       [ ] criteria.md has five numbered criteria, each naming a NUMBER
       [ ] Each has a reason underneath tied to your data or taxonomy
       [ ] labels.csv: at least 150 rows, text/label/note, ONE file not split
       [ ] No label above 70%
       [ ] All five unit 5 sections have real content
       [ ] Label Taxonomy includes the decision rule for your hardest boundary
       [ ] The Dataset includes three hard cases
       [ ] results.json and test_split.csv committed (the notebook does this)
       [ ] At least four new commits
       [ ] Repository URL submitted — WRITE IT DOWN

     SUBMISSION CHECKLIST — unit 6

       [ ] Baseline vs. Trained table, with your prediction written beforehand
       [ ] Run Log — Before, five criteria across three seeds
       [ ] Confusion matrix TYPED AS A MARKDOWN TABLE
       [ ] A verdict on every criterion
       [ ] A diagnosis for every miss, using the agreement report as evidence
       [ ] Agreement Report with every disagreement adjudicated
       [ ] One improvement, with Run Log — After
       [ ] What's Still Broken
       [ ] results_three_seeds_before.json, results_three_seeds_after.json,
           baseline_results.json and
           agreement_results.json committed
       [ ] At least four new commits
       [ ] The SAME repository URL as last unit

     Do not delete and recreate this repository.
     ═════════════════════════════════════════════════════════════════════ -->

---

📖 **How to run this project: [RUNNING.md](RUNNING.md)**
