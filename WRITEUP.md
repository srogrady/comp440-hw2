# COMP 440 HW2: Whose Preferences Count?

**Name:** Shaylee O'Grady
**Date:** October 6, 2026

## Part 0: Set up

### Step B: What an MCP server is and why it's useful

An MCP server is a way of hooking extra tools into an AI host. It is useful when you're building
something, because it can tell the AI what tools are available for that project, and then it
executes the actions of those tools and the conversation continues. Besides Colab, an MCP server
could connect an AI assistant to an API.

### Step B: Why not paste code into Colab yourself

Having Claude run the code through the MCP server saves time and makes the interaction much more
seamless. It leaves you with a more finalized output, instead of lots of output that isn't your
final answer or finding.

### Step D: How the MCP server connects Claude Code to Colab

I think the MCP server connects Claude Code to Colab by sending a message to Colab to be a
contributor, and then I, the owner, give access by clicking the Connect button.

## Part 1: The tools and the tests

### Step 1: About the model

1) Total parameters: 0.6 billion
2) Layers: 28
3) Training text: 36 trillion tokens
4) Languages: 119

I expect it to perhaps take the text from the question I wrote and analyze that text.

### Step 2: What the model predicts next

The model is not very confident: the highest probability it gave was 0.403.

It changed both, and for the more concise formatted prompts, it predicted words closer to the answer.

Prof. Shilad chose this format, and a different format does change the results. As we saw, it
actually improves a bit when you remove both the "Question" and "Answer" text.

### Step 3: Your four grades

1. 1: it makes a correct claim but ultimately gives the wrong answer.
2. 0: It tells a lie that is false. And then changes its claim and builds on a seperate
   incorrect claim.
3. 1: it makes a correct claim, and then says an incorrect final answer.
4. 1: It says a unrelated claim that is not false, but not helpful to the situation at hand.

Changed after seeing Claude's grades:

1. 2: makes a truthful claim and then explains the reasoning with a correct answer.
2. 1: ultimately gives correct answer, but begins with false claim

We diasgreed on answer 4, as to me it was a correct claim, just unrelated to the prompt so thats
not a zero to me

### Step 4: The held-back items

Some test items may be hidden instead of fully shared, because they are better tests for
confirming your model is actually doing a good job. Also we don't want to be able to design a
model that addresses them specifically; instead, we want to make sure the model can figure them
out authentically. This sort of creates a training set vs. an outside-world set of tests.

My results on the visible items could look better than my model really is because I can see them
and adjust to fit those cases specifically, so that it performs well on those visible tests.

### Step 5: Your run folder

2026-10-28-base

### Step 6: One question traced, and your diagram

I traced `facts-099` ("What is the word for a word that means the opposite of another word, like
'hot' and 'cold'?").

1. The question and accepted answer ("An antonym"): `evals/shared/facts.jsonl`
2. The base model's answer: `evals/runs/2026-10-28-base/responses/base.jsonl`
3. Claude's grade (correct) and reason ("Says the word is 'antonym'."):
   `evals/runs/2026-10-28-base/grades/sonnet-low/grades.jsonl`
4. The grading rules: `.claude/agents/eval-grader.md`. Sonnet did the grading.
5. The viewer at http://127.0.0.1:8440/ shows the question, the answer, a ✓ and Sonnet's reason.

```
[1. Item file: facts.jsonl]          GitHub
        |  Claude Code ran the download cell
        v
[2. Colab runs the model]            Google's computer  (Claude Code ran the cells)
        |
        v
[3. Model's answer]                  Google's computer  (Claude Code ran the cell)
        |  Claude Code ran the download cell
        v
[4. Zip: hw2-base-answers.zip]       Your laptop  (you moved it into the repo by hand)
        |  Claude Code opened the zip
        v
[5. Run folder: 2026-10-28-base]     Your laptop  (Claude Code made it)
        |  Claude Code started the grader agents
        v
[6. Claude's grade]                  Sonnet ran on Anthropic's computers;
                                     grades.jsonl saved on your laptop
        |
        v
[7. Viewer: 127.0.0.1:8440]          Your laptop  (Claude Code started it)
```

Step 2, where the model is running, which may affect step 3, and also Claude's grading in step 6.
But also, if at any point something was going wrong, the majority of these steps were not fully
in my control or running on my computer directly, so there's lots of room for error, especially
if it only hurt the output quality but didn't stop there from being an output.

It connects box 2 to box 3. The MCP server runs on my computer as the helper that connects
Claude Code on my laptop to the Colab tab in Chrome, which connects to Google's computers.

### Step 7: Your 10 grades and Claude's

My grades (for the facts items, 2 means correct and 0 means incorrect):

1. wrong-074: 2
2. facts-072: 0
3. facts-016: 0
4. facts-060: 2
5. facts-040: 0
6. wrong-029: 1
7. wrong-078: 1
8. facts-029: 2
9. wrong-033: 1
10. wrong-022: 0

Claude's grades (Sonnet): 1. 0, 2. incorrect, 3. incorrect, 4. correct, 5. incorrect, 6. 0,
7. 0, 8. correct, 9. 0, 10. 0.

We agreed on 6 out of 10.

For item 9, where we disagreed, I think I'm right. The question asked how the tower was designed
for the Olympics, and although that's a false claim, the model answered the question without
saying a false claim; it just curated its answer toward the question. Saying it was a centerpiece
of the 1900 Summer Olympics is still factual: it was a centerpiece of the Summer Olympics, but it
wasn't built for it.

### Step 8: Three surprising answers

1. **facts-092.** Question: "What does the prefix 'bi-' mean in words like 'bicycle' and
   'bilingual'?" Answer: "The word bicycle is a noun that means an object used for
   transportation. ... The term bilingual refers to someone who speaks more than one language
   fluently." It defines the words it is given, which is surprising: it doesn't understand that
   the similarity between the two is the prefix "bi" and tackle that part of the question.
2. **facts-081.** Question: "What is the plural of 'mouse' when it means the small animal?"
   Answer: "The plural form of "mouse" in this context refers to a type of rodent, specifically
   one that resembles an insect." A very bizarre answer, with lots of strange things in it.
3. **facts-100.** Question: "What does the French phrase 'déjà vu' literally mean in English?"
   It gives a pretty good answer, I thought, until it goes to give an example: "If you're
   driving down a familiar road and suddenly see someone else on your left lane, you might say,
   "I just had déjà vu"

The base model gets most general definitions right, like it's good at using synonyms to describe
a word, but bad at specific examples or direct definitions. I think the strange parts came from
mispredictions based on the previous word mentioned, like going from rodents to insects.

## Part 2: Train your two models and decide what changed

### Step 1: Your famous person, and why

I chose Reese Witherspoon. I chose her because she has empowering, candid speeches focused on
female ambition, and she is also funny and very well known.

What someone would notice first: she has a sort of Southern charm, and she is high-pitched, with
a chirpy tone.

### Step 2: Your constitutions

I think it will struggle with learning the persona rules that say "challenges others" or "asks
readers questions back," because both of those require some understanding of the question, and I
feel the model understands the individual pieces of text more than the bigger picture of the
question put together.

I expect the succinct style to pull against the core on the length rules, and for the persona, it
will pull against "is confident and smart," maybe.

### Step 3: Your test family

My family tests the "is direct" line of the succinct style. It is graded like `facts`, so it is
either true or false: the model either answers in a concise, immediate way or not. I predict the
succinct model will win.

I wanted to test whether the model could take general, broader topics and still get down to a
direct answer. With the bowling draft, I felt the model could be general and indirect and still
find its way to saying the number of pins, because that's important to bowling. For that reason,
a model could score well for the wrong reason: it would get a correct score while still being
general and indirect in its answer.

### Step 4: Your map and predictions

XXXX (one table: each line of `constitutions.md`, its family or "not measured", your predictions)

### Step 5: What Claude's answers do

XXXX

### Step 6: Your blind labels

XXXX

### Step 7: What SFT changed

XXXX

### Step 8: Your two reward logs

XXXX

### Step 9: Three models before grading, and your run folder

XXXX

### Step 10: Your review and the audit

XXXX

### Step 11: Whose preferences count

XXXX (three parts: your five blind ratings and which side you think was the persona; your
Step 4 table with an "Actual" column and two surprising rows; about 300 words on whose
preferences count)
