# Night Grade

Night Grade asks a model name the same private questions, scores the replies, and publishes the latest percents.

The board is [scrollstime.com](https://scrollstime.com). The method sits beside it at [scrollstime.com/method.html](https://scrollstime.com/method.html). This repository is the file that board reads.

## This repository

`main` holds `scores.json` and `reference.json`. The page loads the night from:

https://raw.githubusercontent.com/RogerWillko/nightgrade/main/scores.json

Each run replaces that file and commits. The page builds the table from the file. Older nights stay in the commit history. Questions, replies, and API keys stay on the machine that runs the night.


## Published index

`reference.json` is the second file on `main`. The night stays in `scores.json`.

Two public lists are copied into that file. [BenchLM](https://benchlm.ai) is one, used under [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/). Hugging Face is the other: SWE-bench Verified, SWE-bench Pro, HLE, GPQA, MMLU-Pro, and AIME 2026. When a list has two tests under one heading, those two scores are averaged first.

The large number in a row is the average of the two lists. BenchLM 85 and Hugging Face 70 make an average of 77.5. When that average does not land on one decimal, the row keeps the extra place, so 26.9 and 77.6 show as 52.25. The small line, "sources split by 15", is only the distance between those two scores. A wide split means the lists disagree, so the average is a weaker guide. On a phone the sheet slides sideways and scrolls down. The model name stays on the left, and the column titles stay on top.

A name that only one list scored has no split. The name box on the page searches the whole file.

https://raw.githubusercontent.com/RogerWillko/nightgrade/main/reference.json


## The board right now

The live file has `run` set to null and `models` set to an empty list. The page says nothing has been posted. Under that line it draws two rows, 85 and 73, and labels them Specimen. Those numbers are painted into the page so the table has a shape before a night exists. A measured row appears after a night is committed, and the caption changes to the latest posted night.

The drawing on its own is [scrollstime.com/?preview=1](https://scrollstime.com/?preview=1).

## What a percent is

A night sends 36 short questions, drawn from a private pool of 217 items. The UTC date picks the draw, so every model turned on that night receives the same questions. Items that have already been failing are more likely to be drawn again. Some reasoning items, and the instruction token, are written fresh for that date.

| Bench | What is scored |
| --- | --- |
| Facts | Short recall with one stable answer. |
| Reasoning | Arithmetic, rates, ages, and clock angles. |
| Coding | The model says what a short Python program prints. The script runs that program and grades the model's text against the output. |
| Instructions | The reply has to match the shape that was asked for. A new token is added so the line cannot be carried over from a previous night. |
| Adversarial | Wording traps and confident mistakes. These items carry more weight. A fact bench can stay high while this one falls. |

The percent is weighted. A heavier item moves the number more. The published row is that model's latest scored night. The week average stays on the machine that runs the script.

A night is written when at least 80 percent of the questions came back with an answer and every bench has a score. An empty reply, or a model id the provider rejects, stays off the file. The previous percent remains.

## A fall

The first 14 nights are the baseline. After that, the script compares the last 7 nights with the 7 before them. It calls a fall at an 8-point drop on the overall percent, or a 15-point drop on one bench.

When every watched name falls together, the script says the others fell too. That covers a hard draw, and a night when all of them changed. The case the watch is built for is one name down while the rest hold: the name on the API stayed, and the answers got worse.

The script prints that sentence where it runs. The public page shows the latest percents, the time of the run, and the chain.

## The chain

`chain` is the SHA-256 of the previous chain, a newline, and the compact JSON of `run` and `models` with the keys sorted. The first file starts from an empty previous chain. Publishing the same run and the same models again leaves the hash where it is.

## What this is good for

It watches the name you call. A vendor can change the answers behind a name and leave the name in place. Night Grade repeats a private slice and keeps the percents.

Every name turned on takes the same night. A hard draw moves them together. One name moving alone is the signal.

The record is small enough to check. One JSON file, one commit per run, a hash that links this body to the previous one. Anyone can open an older commit and read that night. The page has no login, and the questions are not in the page.

A failed call stays off the table. The last real percent remains until a night actually scores.

## Limits, in the open

Quote a number from `scores.json` once `models` contains it. The 85 and 73 on the empty page are the specimen drawing.

Each bench is six to eight questions. A 15-point move on a bench can be one or two items. The alarm is coarse.

The draw changes with the date, and failed items come back more often. A lower night can be a harder night. The other names are how a hard draw is separated from one name. A single enabled name has no peer on that night.

The page shows the latest scored night. One noisy night replaces the row people see. The baseline and the week comparison stay on the probe machine. Older percents are the older commits.

An outside reader cannot re-grade a night from this repository. The items are private, so they stay off the website, and they also stay out of public review. The chain links one file to the previous file. Grading happens on the machine that holds the keys, and whoever can commit can write the percents.

The company that hosts the model still receives that night's prompts. The website is the part that does not store them.

A percent here is the weighted share correct on this private slice.

## Fields

| Field | Meaning |
| --- | --- |
| `run` | Local time the file was written, with its offset. Null before the first posted night. |
| `chain` | Hash linking this body to the previous file. |
| `models` | One object for each model with a scored night. |
| `name` | The model id that was called. |
| `overall` | Weighted percent for that night. |
| `facts`, `reasoning`, `coding`, `instructions`, `adversarial` | Weighted percent for that bench on that night. |

The live bytes are the raw URL above.
