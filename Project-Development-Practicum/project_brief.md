# Project Development Practicum — Project Brief

Read `README.md` before editing this file. Replace only the prompts in square
brackets, including the brackets. Keep every uppercase field name and colon.
An answer may continue on the following line.
Use the same member labels in later labs. A member label is a first name or
short initials.

## Team

TEAM-NAME: team3
MEMBER-LABELS: KwangZanquan, WuWenxuan, Agatha
TEAM-WORK: KwangZanquan runs the lab commands and writes each terminal result in the
shared team log; WuWenxuan checks our project-boundary decisions against the
fixed course facts; Agatha checks the risk, the response, and the testable
behaviour; we rotate who controls the keyboard after every checkpoint, so every
member runs at least one command; a blocker, meaning anything that stops
progress, is written in the team log and reported to the whole team before we
continue.

## Fixed Course Facts

- The project is a brain-MRI object-detection workflow.
- The input is one RGB brain-MRI image.
- The provided dataset annotates regions as `glioma`, `meningioma`, or `pituitary`. A `notumor` image is a valid negative example with no boxes.
- The result is zero or more labeled boxes for human review.
- This is non-clinical coursework and not a diagnosis.
- The full dataset is introduced in Lab 04. Do not download it for Lab 01.

These are supplied facts, not questions. Do not rewrite them.

## Project Boundary

SYSTEM-DOES: One RGB brain-MRI image enters the workflow; the workflow locates
regions annotated as glioma, meningioma, or pituitary and returns zero or more
labeled boxes for human review.
SYSTEM-DOES-NOT: The workflow does not diagnose a tumor and does not make an
automatic medical or clinical decision for any image.
REVIEWER: A designated human reviewer receives and reviews every model output
before the output is used for anything else.

## Risk And Testable Behaviour

RISK: On a notumor image, a normal bright region is marked as a tumor box, so the
workflow reports a finding on an image that should have no boxes at all.
RESPONSE: Every box goes to the human reviewer, who can reject it; the workflow
never acts on a box by itself, and the team logs each false positive so that this
image type is checked again when the workflow is built in a later lab.
TESTABLE-BEHAVIOUR: Given a notumor brain-MRI image, when the workflow is started,
then it returns no labeled boxes and the human reviewer sees an empty result for
that image.

## Initial Contributions

INITIAL-CONTRIBUTIONS: KwangZanquan ran the start command and the check command
and recorded both terminal outputs; WuWenxuan checked the project boundary
against the fixed course facts; Agatha drafted the risk, the response, and the
testable behaviour.

## Additional Risks And Responses

The brief records one risk, as Lab 01 asks. These are the other problems we
expect during the project, with the response the team agreed on.

- Label balance. The dataset may give us far more images of one annotated
  category than of the others, and a workflow built later could then work well
  only for the common category. Response: we record how many images each
  category has before any later step, and we report the split to the whole team
  instead of hiding it.
- One score hides the real mistakes. A single accuracy number can look good
  while the workflow still misses regions or draws boxes on healthy images.
  Response: we count misses and false boxes separately for each category, and we
  keep the notumor images as negative examples rather than as a fourth
  detection category.
- Tooling stops the work. A wrong Python version, an edited lab file, or a brief
  changed after checking makes the evidence invalid. Response: everyone uses
  Python 3.12 through `py -3.12`, nobody edits `data/` or `lab.py`, and a failed
  check is cleared with `py -3.12 lab.py reset` before it is run again.
- Scope drift. The boundary grows into architecture, training, or deployment
  decisions that belong to a later lab. Response: each such idea is written in
  the team log as "later lab", and this file keeps only the Lab 01 boundary.
