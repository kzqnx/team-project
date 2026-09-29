# Project Development Practicum — Architecture Brief

Read `README.md` before editing this file. Replace each prompt in square
brackets, including the brackets. Keep every heading and uppercase field name.
An answer may continue on the following line.

## Fixed Project Context

- The workflow receives one brain-MRI image.
- It may return zero or more labeled boxes.
- Every model output goes to a designated human reviewer.
- It does not make a diagnosis or authorize an automatic medical decision.
- Lab 02 uses no dataset images and does not train or select a model.

These are supplied facts. Do not rewrite them.

ARCHITECTURE-GOAL: One brain-MRI image enters the workflow, the detector returns
zero or more labeled boxes, and every result reaches the designated human
reviewer before anything else can happen.

## Workflow Parts

### INPUT

INTERFACE: The requested brain-MRI image enters here, and this part passes that
image, or a missing-input state when the image cannot be found, to VALIDATION.
FAILURE: When the requested image cannot be found, this part passes a
missing-input state to VALIDATION and the path stops there; nothing is prepared
and nothing is detected.

### VALIDATION

INTERFACE: VALIDATION receives the image, or the missing-input state, from INPUT
and passes only a supported brain-MRI image forward to PREPROCESSING.
FAILURE: A missing or unsupported image is rejected at this boundary and the
path stops before PREPROCESSING or DETECTOR runs; the rejection is recorded for
the reviewer.

### PREPROCESSING

INTERFACE: The accepted image arrives from VALIDATION, and prepared information
for that single image leaves this part for DETECTOR.
FAILURE: If preparation cannot complete correctly, this part stops the path
before DETECTOR and records a preparation-failure state for the reviewer.

### DETECTOR

INTERFACE: Prepared information for one image arrives from PREPROCESSING, and
zero or more labeled boxes leave this part for RESULT.
FAILURE: If detection cannot complete, this part returns no boxes, marks the
case as detection-failed, and passes that state to RESULT.

### RESULT

INTERFACE: Zero or more labeled boxes, or a failure state, arrive from DETECTOR,
and one complete reviewable result leaves this part for HUMAN-REVIEW.
FAILURE: If a complete reviewable result cannot be formed, this part marks the
case incomplete and still passes it to HUMAN-REVIEW instead of dropping it.

### HUMAN-REVIEW

INTERFACE: The designated human reviewer receives every result together with
its labeled boxes and any recorded failure, and records the review outcome.
FAILURE: If the reviewer is unavailable or the review stays incomplete, the
result remains pending review and nothing is released as a final outcome.

## Case Paths

Use the uppercase workflow-part names and `->` to show each path.

VALID_IMAGE: INPUT -> VALIDATION -> PREPROCESSING -> DETECTOR -> RESULT -> HUMAN-REVIEW. An accepted image passes through all six parts and reaches the reviewer.
MISSING_FILE: INPUT -> VALIDATION -> rejected before PREPROCESSING or DETECTOR. The missing-file case stops at the validation boundary.
UNSUPPORTED_IMAGE: INPUT -> VALIDATION -> rejected before PREPROCESSING or DETECTOR. An unsupported image stops at the validation boundary.
UNCERTAIN_RESULT: INPUT -> VALIDATION -> PREPROCESSING -> DETECTOR -> RESULT -> HUMAN-REVIEW. An uncertain result reaches the reviewer and allows no automatic action.
CONFIDENT_RESULT: INPUT -> VALIDATION -> PREPROCESSING -> DETECTOR -> RESULT -> HUMAN-REVIEW. A confident result still reaches the reviewer and never allows an automatic action.

## Boundary Decisions

VALIDATION-FIRST: Validation must accept an image before PREPROCESSING or DETECTOR runs, so an unreadable or unsupported file cannot reach detection and cannot produce boxes.
REVIEW-BOUNDARY: Both uncertain and confident results require human review, because this workflow is non-clinical and no result may lead to an automatic action.

## Team Check

TEAM-CHECK: KwangZanquan checked the DETECTOR interface and the MISSING_FILE case path; WuWenxuan checked VALIDATION and the UNSUPPORTED_IMAGE case path; Agatha checked HUMAN-REVIEW and the REVIEW-BOUNDARY decision.
