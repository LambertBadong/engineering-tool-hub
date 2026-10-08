# Building it with AI: what went wrong, and the rules I work by now

[← Back to overview](../README.md)

## Where I started

I am a mechanical designer. When I began this project I knew basic programming and nothing more. Over the following months I had to learn Python, the SolidWorks API, C#, automated testing and interface design.

The skill that mattered most was none of those. It was learning to build reliable software with an AI coding assistant (Claude Code): how to ask for the right thing, how to put guardrails around the result, how to catch a confident wrong answer, and how to think about the cases nobody mentions.

My honest estimate is that without AI this project would have taken years, or a dedicated senior software or automation engineer. With it, I shipped 48 releases in under six months while doing my design job full time.

## What I learned first

**Getting code written is the easy part.** An AI assistant will produce a working-looking tool in an afternoon.

**Knowing whether it is safe is the hard part.** These tools rename, move and overwrite the files a factory builds from. A tool that is right 99 times and silently wrong once is worse than no tool, because people stop checking.

And AI-written code tends to fail in one specific way: it looks finished, it reports success, and it is wrong. Nothing crashes. Nothing turns red. The three stories below are all that same shape.

## Story 1: the refusal that said "finished"

**What happened.** The hub's SolidWorks panel has a button that copies and renumbers a set of parts. It must never run twice on the same job, because that would stamp the same part numbers in twice. So there is a guard: press the button while a run is in progress, and the second press is refused.

The refusal worked. But the screen showed **"Pack and Go finished"**.

**Why.** The part of the tool that does the work is written in C#. The part that draws the screen is written in JavaScript. Each had its own list of status words.

```
C# sends:               "busy"     "started"   "running"
JavaScript expected:    "start"    "begin"     "running"
```

Only one word matched. The screen code looked like this:

```javascript
var starting = phase === "start" || phase === "begin" || phase === "running";

if (starting)      show(label + " running…");
else if (d.ok)     show(label + " finished");      // everything unrecognised lands here
else               show(label + " failed: " + d.error);
```

`"busy"`, the refusal, matched nothing in the first line. It was not an error either, so it fell through to the middle branch and was announced as a completed run.

Both word lists were reasonable guesses. That is exactly why it survived: reading either one alone, nothing looks wrong.

**What I changed.**

- A set of values that crosses a boundary between two programs gets **one** definition, not two copies.
- An unrecognised value must be treated as *unknown*, never quietly handed to whichever branch comes last.
- Better still, the side that knows what happened sends the finished sentence, and the screen displays it without interpreting it.

## Story 2: the safety guard that checked a folder against itself

**What happened.** The hub has a test mode. When it is on, every write is supposed to stay inside a sandbox folder, so a new feature can be tried without touching a real job. One command creates a folder structure inside a job.

With test mode on, and the tool itself reporting `sandbox: true`, it created those folders inside a **live** job.

**Why.** The guard was eight lines:

```python
if sandbox_on():
    sandbox_root = active_roots()[0]          # wrong
    if not inside(sandbox_root, job_folder):
        refuse()
```

`active_roots()` returned a list that had been worked out once, when the program started. If the sandbox folder was added to the settings while the program was running, test mode switched on but that list still held only the live folder. So the guard asked "is this live job inside the live folder?", got "yes", and let it through.

The guard was correct in every situation except the one it existed for.

**What I changed.**

```python
if sandbox_on():
    sandbox_root = read_sandbox_setting()     # the setting itself, read fresh every time
    if sandbox_root is None:
        refuse("Test mode is on but no test folder is set.")
    if not inside(sandbox_root, job_folder):
        refuse()
```

- A safety check reads the **original source of truth**, never a convenient copy of it that something else worked out earlier.
- When the check cannot be sure, it **refuses**. It does not fall back to a guess.

## Story 3: the confident wrong answer

**What happened.** During one long session the AI told me that the BOM tool saved the SolidWorks assemblies it opened. It said so in several messages.

It was not true. The evidence that disproved it had been sitting on disk the whole time; nobody had looked, because nobody doubts a statement made that confidently.

A wrong belief costs more than a bug. A bug gets hunted. A wrong belief gets built on.

**What I changed.** I set a standing rule for every working session: when a mistake is found and fixed, it is written down **before moving on**, as a general rule and not as a story.

- Not "the delete removed eleven functions". That exact thing never happens again.
- Instead: "an edit that cuts between two text markers can silently take everything in between; list what exists before and after, and refuse to save unless exactly the intended items are gone." That happens constantly.

Each note records how the problem *looked* before it was understood, because the symptom is what you meet first next time. Those notes are loaded into every new session, so the assistant starts each day already knowing the mistakes it made on this project. A two-hour hunt becomes a five-minute check.

## The rules I work by now

| Rule | What it prevents |
|---|---|
| **A check must be able to fail.** Before trusting a passing test, confirm it fails when the thing is broken | Tests that pass because the item they inspect is not there at all |
| **Verify the number, not the wording.** When a figure is displayed, count it independently | A screen that said "50 parts" for a folder of 25, because hidden lock files were being counted |
| **Read the source of truth in a safety guard**, and refuse when unsure | Story 2 |
| **One definition for anything shared between two programs** | Story 1 |
| **If a failure must be explained to a person, return the reason. Do not just log it** | Errors swallowed quietly, so a code fault looks like bad data |
| **Test in the sandbox only.** Live job folders are off limits until I say otherwise, explicitly, per feature | An experiment writing into a real job |
| **Archive first, never overwrite** | Any mistake becoming unrecoverable |
| **Test from someone else's machine** | The first coworker install failing because the folder name had a space in it, something two weeks on my own machine could not reveal |
| **Write every mistake down the same session, as a general rule** | Relearning the same lesson |

## What AI did, and what it did not

**It did:** write most of the code, explain unfamiliar parts of the SolidWorks API, draft tests, and work through problems faster than I could have alone.

**It did not:** know what a designer actually needs on a busy day, know which mistake would cost a day of rework on the shop floor, or notice on its own when its checks were measuring nothing. Deciding what to build, defining what "safe" means for each tool, and refusing to accept "it passed" without proof were my job.

That division is the main thing I would tell another engineer starting out with these tools. The assistant makes you fast. It does not make you right. Being right is still engineering.

---

[← A full 50-part job](full-job-walkthrough.md) · [Back to overview](../README.md)
