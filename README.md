## Overview

| Developed by | Guardrails AI |
| --- | --- |
| Date of development | Feb 15, 2024 |
| Validator type | Format |
| Blog |  |
| License | Apache 2 |
| Input/Output | Output |

## Description

This validator checks if an LLM-generated text is gibberish/non-sensical. It validates either sentence-by-sentence or the entire text.

## Installation

```bash
$ guardrails hub install hub://guardrails/gibberish_text
```

## Usage Examples

### Validating string output via Python

In this example, we use the `gibberish_text` validator on any LLM generated text.

```python
# Import Guard and Validator
from guardrails.hub import GibberishText
from guardrails import Guard

# Initialize Validator
val = GibberishText()

# Setup Guard
guard = Guard.from_string(
    validators=[val, ...],
)

# Pass LLM output through guard
guard.parse("Zoom is a great video conferencing tool. It's very easy to use.")  # Pass
guard.parse("The quick brown fox jumps over the lazy dog. Fox fox fox fox fox. Floppyland gsdkd%$klsdml")  # Fail

```
### Validating JSON output via Python

In this example, we use the `gibberish_text` validator on a pet description string.

```python
# Import Guard and Validator
from pydantic import BaseModel
from guardrails.hub import GibberishText
from guardrails import Guard

val = GibberishText()

# Create Pydantic BaseModel
class PetInfo(BaseModel):
    pet_description: str = Field(validators=[val])

# Create a Guard to check for valid Pydantic output
guard = Guard.from_pydantic(output_class=PetInfo)

# Run LLM output generating JSON through guard
guard.parse("""
{
    "pet_description": "Caesar is a great dog",
}
""")

guard.parse("""
{
    "pet_description": "Cdfhgg great coffee"
}
""")
```


## API Reference

`__init__`

- `on_fail`: The policy to enact when a validator fails.
