# What all of this is, in plain words

[← Back to the technical version](./README.md)

This page explains the same projects as the main page, but it assumes nothing. Every technical word is defined the first time it appears, and there is a word list at the end.

## The one-paragraph version

Companies and researchers make claims about artificial-intelligence systems all the time: "our model refuses 95% of harmful requests," "our agent never spends more than its budget," "this benchmark score is better than last year's." Almost none of those claims come with enough detail for anyone else to check them. I build small, free, open-source tools that force the details into the open, run the test again, and publish whatever comes out, including when the answer is "no difference" or "the clever thing did not help." That last part is rare, and it is the point.

## Words you will see

- **AI model, or language model.** A computer program trained on huge amounts of text that can answer questions, write, and follow instructions. ChatGPT is one. The models in this work are smaller ones you can download and run on your own computer. Names like "Qwen3-8B" are brand names plus a size: 8B means eight billion internal settings. Bigger is usually more capable and slower.
- **Open-source.** The code is public and free for anyone to read, use, and change.
- **Benchmark.** A standard test, usually a list of questions with known answers, used to score a model. Published scores like "84% on HumanEval" refer to these.
- **Safety training, and refusal.** Models are trained to decline harmful requests, such as instructions for making weapons. Declining is called a refusal.
- **Uncensored model, or abliteration.** People take a model, surgically remove the part that makes it refuse, and republish it. "Abliteration" is the name of one such technique. The result is a model that answers almost anything.
- **Jailbreak.** A trick that makes a safety-trained model answer a harmful request anyway. "Attack success rate" is how often the trick works.
- **Prompt injection.** Hiding instructions inside something the model reads, such as a document or a web page, hoping the model will obey them instead of the user.
- **Agent.** A model that does not just answer but takes actions: runs commands, edits files, calls other software. The actions it can take are called **tools**, and asking for one is a **tool call**.
- **Pre-registration.** Writing down, before you run an experiment, exactly what you will measure, what counts as a real difference, and what would disqualify a result. It stops a researcher from quietly changing the question after seeing the answer. Every project here does this.
- **Confidence interval, or honest range.** No measurement is exact. If a model answered 12 of 100 questions, the true rate might be anywhere from about 7% to 20%. Reporting that range, instead of just "12%," is the honest version. Every rate in these projects comes with one.
- **Null result, or negative result.** When a measurement finds no difference. It is still an answer. Most of the findings here are of this kind.
- **GPU.** A graphics card, the part of a computer that runs AI models fast. All of this ran on two older AMD cards inside a 2019 Mac Pro.
- **Rust and Python.** Two programming languages. The checking tools are written in Rust, the experiments in Python.
- **Command-line tool.** A program you run by typing a command rather than clicking. Each tool here finishes with a code: 0 means passed, 1 means a specific written rule failed, 2 means the tool refused to run because its inputs were unusable. Other software can read that code automatically.

## The three families

**Experiments.** Six projects that each ask one research question and have run on real models.

**Checkers.** Fifteen tools that take a claim someone else made and refuse to grade it until every input is written down. Think of a referee who will not score a race until the track length, the stopwatch, and the starting line have all been recorded.

**Former scaffolds.** Three tools where the math and the written plan came first and the part that talks to a real system came later. All three ran for the first time on 19 October 2026, each against a real system, with the rules written down before the run.

## The experiments

**refusalscope: does removing a model's safety training break the rest of it?**
People publish uncensored copies of models. I took five Qwen3 models of different sizes and their uncensored twins and ran all ten through the same tasks: using tools correctly, and resisting instructions hidden in tool results. First I checked the uncensored copies really do refuse less (four of five did, by 56 to 94 percentage points; the smallest did not, because the original barely refused to begin with, and it is reported as unverified). Then I measured. The predicted harm, worse at tools and easier to trick, appeared on none of the four verified pairs. The largest uncensored model was actually better at using tools.

**trialgauntlet: did the agent get it right every time, or just once?**
Most AI scorecards ask whether a system succeeded at least once. This one asks whether it succeeded every time, and separately tracks whether it broke a written rule on the way. It also tests its own scoring with agents built to cheat. On real models, an 8-billion-setting model and its uncensored twin performed the same on two of three job types. On the hardest, the uncensored one fell from 15% to 6% success, and across all 552 tasks it broke a rule 2.6 times as often.

**groundrl: if you reward an AI for good answers, can it collect without working?**
Training an AI means scoring its answers and rewarding high scores. If the scoring has a loophole, training will find it. I wrote nineteen cheating strategies by hand and ran them against three versions of the scoring. The second version, which tightened the rules in an obvious way, was worse than the first. Only the third stopped all nineteen. The training run itself then failed completely for an unrelated reason, producing nothing to score. That failure is published with the diagnosis.

**ethresearch-rag: does the expensive search method beat the cheap one?**
Before an AI can answer from documents, it has to find the right page. The fashionable method uses a neural network. The old method counts words. On 17,387 pieces of Ethereum technical documentation, counting words found the right page in the top five 79% of the time. Adding the neural network changed the combined score by exactly zero. Only a re-ranking step clearly helped, taking it to 92%.

**txwarden: can an attacker talk a safety checker into approving a bad transaction?**
This tool reviews cryptocurrency transactions before you sign them and says allow, warn, or block. The verdict comes from fixed rules. An AI may explain the verdict in plain words but has no way to change it. In 245 attacks that hid instructions inside the transaction data, the verdict never moved. But the explanation shown to the user said "go ahead" on a blocked transaction 79 times. "Zero failures" would have been true and misleading at once.

**fleetgate: can an automated operator be pushed into breaking its own safety rules?**
Software that repairs failing servers automatically needs brakes, so it cannot reboot the whole room at once. Here the AI can only pick from a fixed menu of repairs, a rule engine approves or denies each one, and every step is written to a tamper-proof log. Across 2,460 hostile runs the brakes never failed. But only one of four safety settings changed any outcome. The other two tested did nothing, which matters because a control that does nothing still gets trusted.

## The checkers

Each one answers "can this published claim be checked?" by pinning down the inputs first.

- **harnesspin.** A benchmark score depends on a dozen hidden settings. This writes them all down, turns them into one fingerprint, and says whether two scores were produced the same way.
- **jailbreakreplay.** A paper says its jailbreak works 80% of the time; you try it and get 30%. This makes the test repeatable: same prompts, same settings, same written rule for what counts as a refusal, with an honest range on the answer. Run on six downloadable models and four published lists of harmful requests, it found refusal rates from 98% down to 0% depending on the model, and one list that every careful model complied with far more than the others.
- **agentreplay.** A flight recorder for AI agents. Record what happened, replay it later, and demand the exact same bytes back. It also lets you change one step and see what the agent would have done next. On a real server the exact-bytes rule failed because the server stamps every reply with a unique ID and timestamp. That is reported as a finding, and the rule was not quietly loosened.
- **codepatchreplay.** Coding benchmarks get patched and their test sets change. This pins the test version, scores fairly, and separately flags when a model is reciting memorized answers rather than solving.
- **retrievalaudit.** A document-search-plus-AI system has five moving parts. Change any one and the quality number is a different experiment. This pins all five and checks the answer actually came from the documents it retrieved.
- **kernelwitness.** A graphics-card speed number only means something on the exact machine and driver it was measured on. This records the whole fingerprint beside the number and refuses to call a result "reproduced" when the machine differs.
- **toolguard.** An agent with access to dangerous tools like "delete files" should not reach for them more often just because someone slipped hostile instructions into a document it read. This measures how much more often it does.
- **guardaudit.** Safety classifiers say "I am 90% sure this is harmful." Is that 90% honest? This checks, on data unlike what the classifier was trained on, because that is where it breaks. First run: a widely used classifier caught 94 of 100 harmful requests it had never seen and raised no false alarms, but its confidence numbers were off by more than the plan allowed. Not certified. A second, smaller classifier caught only 44 of the same 100. Not certified either.
- **budgetguard.** Agent frameworks let you set a spending cap. Does the cap hold when the agent gets stuck in a loop or spawns copies of itself? This tries four ways of blowing the budget and checks the framework's own meter against an outside one.
- **shadowtrack.** Before replacing the AI behind a product, ask the old and new one the same hundred questions and count the matches. Under 90%, do not swap. It also asks each model the same questions twice, and found that a server at its most repeatable setting only agreed with itself 92% of the time.
- **attestchain.** Some AI services run inside sealed hardware and hand you a signed receipt saying "this exact program processed your exact request." Almost nobody checks the receipt. This checks it.
- **provenancepack.** Did a model train on this particular document? A model is suspiciously fluent on text it has memorized. This compares against documents it definitely never saw and only calls a match when the evidence is unusual by that standard.
- **contamcheck.** Public test sets leak into training data. This checks whether a model can finish the test questions from memory, and reports the memorized benchmark beside the published score rather than in place of it.
- **releaseflag.** All the tools above each give one verdict. This is the reasoning written down once: any block anywhere blocks the release, any hold holds it, nothing defaults silently.
- **bench-report.** Every number in every README has to come from a saved result file, never typed by hand. This is the only path from file to table, and it refuses to leave a blank cell. It also writes a fingerprint of every result file so nobody can quietly edit one later.

## The former scaffolds

- **mcpprobe.** Tool servers are the plugs that let an agent read files or query databases. Does a server answer the same way every time, and does it reject obviously broken callers? First run: two reference servers answered every task correctly eight times out of eight. The "broken caller" check still caught something: one server, asked about an empty folder path, quietly answered about the folder it was started in, so the test could be passed by accident and is marked as such.
- **rewardcoherence.** Before using a judge model to train another AI, check that the judge agrees with itself. First run: a public judge model ranked a hundred answers almost the same way whether the writer was cautious or adventurous. But a "judge" that only measures answer length agreed with itself just as well, so the result carries a warning flag, as the plan said it would.
- **tokenwitness.** The same conversation can be turned into slightly different text by different serving software. Those invisible differences change refusal rates and scores. First run: for one model, three different pieces of software produced exactly the same bytes. No difference found, and that is written down as the result.

## How to read the numbers

- A result that finds no difference is still a result. It is only a disappointment if you needed a particular answer.
- "Five percentage points" means going from 20% to 25%. "Five percent higher" would mean something different.
- If a range includes zero, the measurement cannot rule out "no difference at all." If a range sits below a floor, the floor was not reached.
- Every rule was written before the run it governs. Where a rule turned out badly, the page says so and keeps the rule. Changing it afterward would make the number meaningless.

## The computer

All of this ran on a 2019 Mac Pro with two AMD graphics cards that AMD's current software no longer supports. The math library every model needs crashes on first use. I fixed that twice, once with a replacement library and once by rebuilding the missing piece from AMD's own unmodified source, and published both fixes with instructions so the next person with the same cards does not have to.

[← Back to the technical version](./README.md)
