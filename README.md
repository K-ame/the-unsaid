# The Three Things You Never Tell Your AI

**English** | [中文](docs/zh.md)

> Whatever you leave out of an instruction, the AI fills in for you. Sometimes that saves you work. More often it costs you a rework. This is about the structure of what you say — not about tools, which go stale in months.

**TL;DR** — Three things people assume the AI already knows, and never say: *what done looks like*, *what not to touch*, and *whether it may act now*. Miss the first and you rework. Miss the second and it "improves" things you wanted kept. Miss the third and something irreversible is already gone.

<details>
<summary>Contents</summary>

- [Three things it cannot guess](#three-things-it-cannot-guess)
- [What you want: a sentence that can be checked](#what-you-want-a-sentence-that-can-be-checked)
- [What not to touch: blocking, and preserving](#what-not-to-touch-blocking-and-preserving)
- [Whether it may act: the decision, and the method](#whether-it-may-act-the-decision-and-the-method)
- [When you say it matters more than what you say](#when-you-say-it-matters-more-than-what-you-say)
- [One thing outside the scope](#one-thing-outside-the-scope)
- [It says it's done. How do you know?](#it-says-its-done-how-do-you-know)
- [Last](#last)

</details>

---

A while back I asked an AI to delete an old project folder. It had sat untouched for two months and I didn't want to deal with it anymore.

It didn't delete anything right away. It packed the whole folder into an archive first — old versions included — and only then came back to ask whether to continue. Partway through, a permission error came back; it dug in and reported that this wasn't a permissions problem at all but a missing authorization, and when I said "go handle it," it walked the whole process by itself. After the deletion it checked once more and confirmed the folder was really gone.

I hadn't asked for any of those three steps. But most of what follows is about the times it fills in *wrong* — when it fills in right, you never notice.

## Three things it cannot guess

Tell a colleague "tighten up the ending" and they know the rest of the document is off-limits. That's common sense, and nobody says it out loud. Tell an AI the same sentence and it has no such common sense. It knows what you said, not what you didn't. And the part you didn't say doesn't stay blank — it fills something in. The fill is usually plausible, so you don't catch it on the spot. You catch it when the result comes back wrong.

When two people work together, three things almost never get said:

**What you want. What not to touch. Whether it may act now.**

People cover those with shared assumptions; an AI covers them by guessing. Guess right and you won't compliment it. Guess wrong and you'll just say "no, not like that." Of the three, the last is the expensive one — get the first two wrong and you can still fix it; get the last one wrong and the thing is already gone.

These are three angles, not three disjoint categories. One instruction can carry all three at once: "Don't delete anything — back it up first, and wait for my confirmation" specifies a prohibition, a method, and an authorization in a single sentence. They're separated here because they fail differently, and different failures need different fixes.

## What you want: a sentence that can be checked

The most common failure starts with a task name.

"Write me a scraper" — what to scrape, how complete it has to be, where the output goes: none of it is in there. So it draws its own finish line, hands you the result, and you say no.

"Write me a scraper" describes an action. "Every Monday morning there's a deduplicated list in my inbox" describes a result. The first can't be checked. The second can be checked word by word.

The fix isn't hard: replace "what I want you to do" with "what counts as done."

- Don't: clean up these files for me
- Do: these files go into twelve folders by month; when two share a name, keep the newer one

Or, in code:

- Don't: optimize this function
- Do: merge the four repeated file reads in this function into one; behavior stays the same

In the second version of each pair, what "done" means is visible at a glance.

If you can't say what done looks like, there are two usual sources: the goal itself isn't settled yet, or the goal is settled but hasn't been translated into something checkable. The second is more common, and more of a waste — the standard is in your head, but it's still shaped like "it should feel right" or "it should run smoothly." Either way, the rework lands on you.

There's a third source worth naming separately: some goals genuinely depend on feel — the tone of a paragraph, how tight a layout is. Those can't be written as conditions. They can only be compared: hand it two versions and say which one is closer. That isn't a failure to be clear; it's a different kind of acceptance test.

One case is the exception: you don't know what you want yet and just want to see what's possible. "Flip through these files and tell me what's in there" beats forcing a target. The distinction is whether you need a decision or a thing.

## What not to touch: blocking, and preserving

This one gets dropped most often, because it's really two things — and people default to them very differently.

**Blocking** is about things you don't want it to do at all. It comes up most when you're changing something that already exists: don't touch production config, don't modify the database, don't touch the file I'm editing. Unsaid, it may just go ahead.

**Preserving** is about things inside the scope of the change that you don't want altered. Not forbidden — just expensive to lose.

One instruction each shows the difference:

> Only change the staging environment. Not a single character in production. *(blocking)*

> Only the last two paragraphs. The middle one is what I like — leave it. *(preserving)*

What happens when you skip the second one, I've hit many times. You ask for the last paragraph only. It comes back with the last paragraph changed — and, since it was in there anyway, the punctuation normalized to half-width throughout, a few "the"s trimmed, the sentences smoothed a little. All correct edits. And the pause you wanted is gone.

You say: I didn't ask you to touch anything else. It says: understood. And then it touches something else.

"Only this part" apparently sounds like "this part is the priority." If you want it left alone, you have to say that too. Half a sentence, one rework saved.

Preserving is harder to say than blocking, because people only speak up when something's wrong. I thought back on how I actually give feedback: almost all of it is "no," "try again," "not right," and almost never "keep this part as is." Which means the signal it receives is skewed — it has no idea which parts you're quietly happy with.

So when version two is worse than version one and you say "no," version three deletes whatever was good in version two along with the rest. It isn't guessing wildly; it's guessing from what you gave it, and what you gave it was half the picture.

I changed one habit: put a direction in the negative. "No" and "No — go back to the approach in version one" are not the same amount of information. The first leaves it hunting for what went wrong. The second gives it a place to land.

As for telling whether this is what went wrong, be strict with yourself: **the same spot being rewritten over and over does not, by itself, mean you failed to say what to keep.** Your instructions may be contradictory, or it may simply not be capable. Only one case is diagnostic: after you say "keep this part," does it still change it? If it does, then it's this.

And know the limit of that test too: it tells you whether a boundary was respected. It can't tell you whether the cause was a capability gap or lost context. Don't hand it more diagnostic work than it can do.

Drafts, prototypes, one-off jobs: none of this applies. Those are supposed to be thrown away and rebuilt, and analyzing what to keep costs more than redoing it.

## Whether it may act: the decision, and the method

"This feature looks good, let's add it."

To you that's a direction. To the AI it's a command. But the real issue isn't that sentence — it's that two separate questions got compressed into one:

**Whether to do it, and how to do it.**

Whether is yours. Anything irreversible, anything involving money or anything that goes out into the world — only you can answer, because it doesn't know where your risk line sits.

How can be its job. "Delete it" can mean delete, back up then delete, or move to the trash. That's the layer it can own.

A common claim is that the less you manage this layer, the more it will account for risk. That sounds right and it isn't. The backup at the start of this piece proves one thing: in one task, it took a protective action on its own. It does not prove that more freedom produces more caution. Examples of the opposite are everywhere.

Three things need separating:

- **Delegation** — let it choose the method
- **Constraints** — write down lines it must not cross, e.g. "never delete; only move to trash"
- **Verification** — check what it actually did

Constraints and steps get confused, so: **a constraint limits what may not be done; a step prescribes how to do it.** Both are "hard-coded," but at different layers, and they protect different things.

Good autonomous execution isn't the least constraint — it's constraint at the right layer. Let the method float, draw the lines in advance, and keep the result checkable.

The opposite is common: spell out "back it up first, then call this endpoint, then delete." It looks rigorous. What it actually does is squeeze the judgment out — and when something unplanned happens, it either follows the script anyway or stops and waits for you.

So for everyday work there's a default test: **are you writing a result, or a step?** It's a default, not an either/or rule — the next paragraph is the exception.

When the work has to be reproducible, or the cost of getting it wrong is unbearable, steps must be nailed down. Deploys, migrations, bulk deletions — what you want there isn't its judgment, it's consistency. So the real question isn't "steps or no steps," it's "does this need it to judge?" If yes, let the method float. If no, nail the steps down.

"Delete it" looks like it fails the checkable-sentence test, by the way. It doesn't — the folder being gone is done, and the finish line is clear. The only thing unclear is how. That's exactly what this layer governs.

## When you say it matters more than what you say

The first three sections are about what should be in the sentence. The next three aren't more items on that list. They're about something else: when the sentence arrives, what happens when it arrives too late, and how you confirm it actually landed. The first three are about missing information. These three are about information that arrived and still didn't take effect. Without them, the first three won't hold either.

The same sentence, placed differently, is worth something entirely different.

"I'm going to start changing these few places" — before the work, that buys you a chance to stop it, at the price of one sentence. The same information after delivery is an explanation, and all you can do with it is redo the work.

People dismiss this as obvious; the result is coming in a minute anyway. What separates the two isn't a minute — it's one sentence versus one redo.

It's the same when the direction is wrong. The phrase I use most is "no." Most of the time it's pulling the thread back off a detour. Its value isn't in the content — "no" contains no information at all — it's in the **position**: it arrives on the detour, not at the finish line. The earlier it lands, the more there is to save.

Where to put the checkpoint: look at whether it can be undone, not at how complex it is. The simpler and more irreversible something is, the more it needs a stop before the work starts. Complex but reversible work can be steered while it runs.

## One thing outside the scope

The longer a conversation runs, the more likely it is to forget what you said early on.

What I hit: a list of about a dozen requirements, the most important one placed first and bolded. That round it chained a large number of actions, and when it came back, that requirement hadn't been applied — and its tone had changed, like a support agent rather than its usual self. The same list had never caused trouble in shorter conversations. Only that round showed both symptoms at once.

Why, I don't know. What I can confirm is the symptom, and one thing that didn't help: moving the requirement to the top, bolding it, repeating it several times — I tried those; nothing changed.

What works sits outside the model, and there are two kinds. One is to have it announce at the start of each turn which requirements it's working under, so you can see the omission as it happens. The other is a script that checks the output — does the file exist, is the count right, are the key fields filled. The second is more reliable, because it doesn't depend on its own reporting.

The cost is that it looks bad, like you don't trust it. On this class of problem, checking is cheaper than trusting.

Two or three requirements and a short conversation? None of this is necessary.

## It says it's done. How do you know?

It says it's done. You look, and part of it isn't. This is hard to argue about — it didn't lie, it just didn't do the part you assumed it did.

There are three layers here, in ascending order of reliability.

**One: have it restate.** Beyond "done," have it list the result — file names, counts, which parts it changed. This catches "it believed it did it and didn't."

**Two: have it restate by a different path.** Not a copy of what it just did. Writing a list and doing the work are two different paths, and if they agree, your confidence goes up.

There's a trap here: **two agreeing results do not make a correct result.** If both runs share the same wrong assumption, or the same bad input, it can be consistently wrong. Conversely, disagreement doesn't prove someone erred — the two runs may have used different definitions.

So this layer has two uses: when they agree, take it as confidence, not proof; when they disagree, use it to narrow the search — find where the two paths first diverged, then inspect the assumptions and evidence behind that divergence. It won't localize the problem for you.

**Three: deterministic checks.** Nothing that depends on its reporting — does the file exist, is the count right, are the key fields present, do the checksums match. This is the only layer that counts as evidence.

Layer two once saved me from a wrong diagnosis. It said it had read a particular item; I asked, are you sure; it redid the work another way, and both agreed. Since they agreed, the read step was fine — the problem was the material I gave it: there were two versions of it, and I'd mixed them up.

Note what that proved: not that the result was correct, only that "the read went wrong" was ruled out. What let me conclude the material was at fault was looking at the material — layer three.

So the three layers aren't progressively finer; they're progressively less dependent on the AI. For high-stakes work, don't stop at layer two.

There's a second use for the same idea: judging whether it's any good. Don't ask "can you handle this" — you'll get its self-assessment. Hand it something real and you'll get a result.

## Last

All of the above assumes you'll remember it. Most people don't. I don't.

And when you don't, it degrades into feeling — judging by gut whether you were clear this time, saying from impression that "it always forgets." Impressions aren't reliable: you remember the times it broke, and never notice the times it worked.

So what do you do if you won't remember? Mine is to compress it to one line: **stop once whenever something can't be undone.** The rest can wait until you actually hit a problem.

That line is a memory aid, not a rulebook. It misses two cases: things that can be undone but are expensive (rerunning a three-hour training job), and money, privacy, and anything that goes out into the world — those deserve a question even when they're reversible.

This piece itself — the version you're reading is the twelfth draft. Every one of the previous eleven was sent back, for different reasons each time: too much like a manual, too much like an essay, too much like me giving a speech, conclusions harder than the evidence. I've reworked it as many times as anyone would have.

So the rest you'll have to walk into yourself. When you do, remember at least this one: if it can't be undone, stop first.

---

*2026-10-25 · © K-ame · [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)*
