# IEEE CIS Domain-Specific Question Answering Challenge with LLMs - Starter Kit

Example submissions for the [IEEE-CIS Technical Challenge on Domain-Specific
Question Answering with LLMs](https://cis.taskforce.ieee.org/gpais/tc-domain-qa-llms).
See the challenge's Codabench page for the full submission contract, rules,
and timeline.

Each folder below is a complete, working submission. Run its `build.sh` to
produce a ready-to-upload zip (see "Building a submission" below), or use
the folder as a starting point for your own.

## Examples

- **[`01_zero_shot/`](01_zero_shot)** — answers using only the base model's
  own knowledge, no retrieval. The reference floor every other approach
  should beat.
- **[`02_rag/`](02_rag)** — retrieves the most relevant fragments from the
  whole corpus (blind search, same as you'd have to do: nothing tells this
  code which document a question came from) and adds them to the prompt
  before asking the model. Tested end to end: ~36 minutes for the full
  corpus plus 50 questions, well inside the 1-hour limit.

## Building a submission

Submission zips and downloaded model weights aren't kept in this repo (some
are well over GitHub's 100 MB per-file limit, and binaries don't belong in a
source repo anyway — see `.gitignore`). Each example's `build.sh` builds its
zip locally:

```
cd 01_zero_shot && ./build.sh   # -> ../01_zero_shot_submission.zip
cd 02_rag && ./build.sh         # -> ../02_rag_submission.zip
```

`02_rag/build.sh` also downloads the two small models it needs (Docling's
layout model and the embedding model) and bundles them into the zip — see
the comments at the top of that script for why, and what to install first
if you don't already have `docling`/`sentence-transformers` locally.

## Environment

[`requirements-participant.txt`](requirements-participant.txt) lists every
package pre-installed in the evaluation environment, pinned to the exact
version running there. There is no internet access at submission time, so
anything not on that list can't be used.

## Sample questions

[`sample/sample_qa.json`](sample/sample_qa.json) has 25 example question/answer
pairs across both domains (AI Papers, DONES) — illustrative of the style,
length, and reasoning expected, not part of either Leaderboard's actual
question set. [`sample/sites.txt`](sample/sites.txt) lists the source domains
the whole corpus (real documents and distractors alike) was drawn from.

## Testing before you submit

The evaluation environment has no internet access, so `AutoModelForCausalLM.from_pretrained(model_id)`
must resolve the base model from a local cache — you can't test that part
outside the challenge's own servers. Everything else in `main.py`, though,
is plain Python: syntax errors, typos, or `answer()` not returning a string
are all things you can and should catch on your own machine first, since a
mistake like that only shows up after your submission has already spent its
turn in the queue.

## License

[MIT](LICENSE)
