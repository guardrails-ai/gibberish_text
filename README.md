## Overview

| Developed by | Guardrails AI |
| --- | --- |
| Date of development | Feb 15, 2024 |
| Validator type | Format |
| Blog | - |
| License | Apache 2 |
| Input/Output | Output |

## Description

This validator checks if an LLM-generated text is gibberish/non-sensical. It can validate either sentence by sentence or the entire text as a whole.

## Requirements
- Dependencies: `nltk`, `transformers`, `torch`

## Installation

```bash
guardrails hub install hub://guardrails/gibberish_text
```

## Usage Examples

### Validating string output via Python

In this example, we use the `gibberish_text` validator on any LLM generated text.

```python
# Import Guard and Validator
from guardrails.hub import GibberishText
from guardrails import Guard

# Initialize Validator
val = GibberishText(threshold=0.75, validation_method="sentence", on_fail="fix")

# Setup Guard
guard = Guard.from_string(
    validators=[val, ...],
)

# Pass LLM output through guard
guard.parse("Zoom is a great video conferencing tool. It's very easy to use.")  # Pass
guard.parse("The quick brown fox jumps over the lazy dog. Fox fox fox fox fox. Floppyland gsdkd%$klsdml")  # Fail

```

## API Reference

**`__init__(self, threshold=0.5, validation_method='sentence', on_fail="noop")`**
<ul>

Initializes a new instance of the Validator class.

**Parameters:**

- **`threshold`** *(float):* The confidence threshold (model inference) for text "cleanliness". 
    Defaults to 0.5.
- **`validation_method`** *(str):* Whether to validate at the sentence level or over the full text. Must be one of `sentence` or `full`. 
    Defaults to `sentence`
- **`on_fail`** *(str, Callable):* The policy to enact when a validator fails. If `str`, must be one of `reask`, `fix`, `filter`, `refrain`, `noop`, `exception` or `fix_reask`. Otherwise, must be a function that is called when the validator fails.

</ul>

<br/>

**`__call__(self, value, metadata={}) → ValidationResult`**

<ul>

Validates the given `value` using the rules defined in this validator, relying on the `metadata` provided to customize the validation process. This method is automatically invoked by `guard.parse(...)`, ensuring the validation logic is applied to the input data.

Note:

1. This method should not be called directly by the user. Instead, invoke `guard.parse(...)` where this method will be called internally for each associated Validator.
2. When invoking `guard.parse(...)`, ensure to pass the appropriate `metadata` dictionary that includes keys and values required by this validator. If `guard` is associated with multiple validators, combine all necessary metadata into a single dictionary.

**Parameters:**

- **`value`** *(Any):* The input value to validate.
- **`metadata`** *(dict):* A dictionary containing metadata required for validation. No additional metadata keys are needed for this validator.

</ul>
