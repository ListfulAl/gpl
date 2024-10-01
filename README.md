# Gato Prompt Language (GPL)

A system for generating focused instructions and short-form outputs.

A comprehensive guide and implementation of Gato Prompt Language (GPL), a structured system for generating instructions and short-form outputs in JSON format.

GPL uses TAGS to provide shorthand instructions. Using TAGS, you can define variables that will be used in the output, and communicate the functional purpose of the word bit of information while minimizing text content. There are only 3 rules to GPL:

1. TAG:Anything before : is the shorthand instruction. Text after defines use.
2. Anything between ==GPL== means those are the rules for this case.
3. All responses must be in valid json

Use tags to communicate your intent behind providing the text so that the system can better understand your prompt.

# Goals of GPL

- Provide a structured system for generating instructions and short-form outputs in JSON format.
- Allow for easy integration of context and data from our system into the prompt.
- Improve speed
- Get json output. As of now it works 95% of the time. I'd like it to be 99% (Open to contributions!)
- System outputs can be funned to be used by other systems or to append to the prompt.

# Usage

Insert GPL text at the begining of your system prompt. Follow the rules of GPL to provide information for context.

User prompt is passed in as INPUT:

# Example 1

You use SYSTEM_PROMPT to define the rules of your prompt.

User Prompt:

```gpl
<GPL> INPUT: What is GPL?
```

Anthropic Output:
claude-3-5-sonnet-20240620
max_tokens: 1000

t=0

```json
{
  "OUTPUT_HUMAN": "Based on the information provided, GPL stands for Gato Prompt Language. It's a system for generating instructions and short-form outputs. GPL has three main rules:\n\n1. TAG: Anything before a colon is a shorthand instruction, and the text after defines its use.\n2. Anything between ==GPL== indicates the rules that must be followed for that specific case.\n3. All responses must be in valid JSON format.\n\nGPL also uses specific input and output formats, as well as conditional logic and variable representations. Is this understanding correct?",
  "OUTPUT_SYSTEM": {
    "GPL_definition": "Gato Prompt Language for generating instructions and short-form outputs",
    "GPL_rules": [
      "TAG: Shorthand instruction before colon, definition after",
      "Rules between ==GPL== must be followed",
      "All responses in valid JSON format"
    ],
    "additional_features": [
      "INPUT and OUTPUT specifications",
      "Conditional logic using {}",
      "Variable representation using <>",
      "Data representation using 'data:'"
    ]
  },
  "EXPORTED_SHARED_DATA": {
    "GPL_name": "Gato Prompt Language",
    "rule_count": 3,
    "response_format": "JSON"
  }
}
```

t=1

```json
{
  "OUTPUT_HUMAN": "Based on the provided information, GPL stands for Gato Prompt Language. It is a language used for generating instructions and short-form outputs. GPL follows three main rules:\n\n1. TAG: Anything before a colon (:) is considered a shorthand instruction. The text after the colon defines its use.\n2. Any content between ==GPL== represents the rules that must be followed for that specific case.\n3. All responses must be in valid JSON format.\n\nGPL also includes various conventions for input, output, and information processing. Is this understanding correct?",
  "OUTPUT_SYSTEM": {
    "GPL_DEFINITION": "Gato Prompt Language for generating instructions and short-form outputs",
    "GPL_RULES": [
      "TAG:Shorthand instruction before colon, definition after",
      "==GPL== denotes rules to follow",
      "All responses in valid JSON format"
    ],
    "ADDITIONAL_CONVENTIONS": [
      "INPUT: User-provided data for assistance",
      "OUTPUT: Data returned to the system",
      "INFO: Background information (not included in final output)",
      "{}: Conditional logic",
      "<>: In-place shorthand for variable names",
      "...: Omission of fields for brevity",
      "data: Preceding text is assistive data"
    ]
  },
  "EXPORTED_SHARED_DATA": {
    "GPL_NAME": "Gato Prompt Language",
    "RULE_COUNT": 3,
    "RESPONSE_FORMAT": "JSON"
  }
}
```

# Example 2

You use SYSTEM_PROMPT to define the rules of your prompt.
At the end of your prompt you will add additional context like so:

SYSTEM_PROMPT:

```gpl
<GPL>
You are a programmer who is writing python code to emulate physical systems on earth.

INFO: Permitted Libraries
from decimal import Decimal
from dataclasses import dataclass
from typing import Optional
from math import \*

NLP INPUT: Input from a person who is looking for a python function to model a physical system.
OUTPUT: Your final product is a python function with example data and calling the function with the example data.
- OUTPUT_HUMAN: a description of the function and a USER_FRIENDLY_NAME for the function.
- OUTPUT_SYSTEM: executable python code using only libraries mentioned in INFO:. Include all imports at the top of the file. In addition to the generated function you will be providing a user friendly name to be stored in variable called
USER_FRIENDLY_NAME

```

User Prompt:

```gpl
Calculate the time it takes for a glass of hot water to cool down from 100 degrees to 80 degrees, in an ambient temperature of 70 degrees.
```

```json
{
    "OUTPUT_HUMAN": "This function calculates the time it takes for a glass of hot water to cool down from an initial temperature to a final temperature in a given ambient temperature. The function is based on Newton's Law of Cooling. The USER_FRIENDLY_NAME for this function is 'calculate_cooling_time'.",

    "OUTPUT_SYSTEM": """
from math import log
from typing import Optional

USER_FRIENDLY_NAME = "calculate_cooling_time"

def calculate_cooling_time(
    initial_temp: float,
    final_temp: float,
    ambient_temp: float,
    cooling_constant: float = 0.1,
    time_unit: str = 'minutes'
) -> Optional[float]:
    \"\"\"
    Calculate the time it takes for an object to cool from an initial temperature to a final temperature.

    Args:
        initial_temp (float): The initial temperature of the object.
        final_temp (float): The final temperature of the object.
        ambient_temp (float): The ambient temperature of the environment.
        cooling_constant (float): The cooling constant (default is 0.1, which is an approximation).
        time_unit (str): The unit of time for the result (default is 'minutes').

    Returns:
        Optional[float]: The time taken to cool, or None if the calculation is not possible.
    \"\"\"
    if final_temp <= ambient_temp or initial_temp <= ambient_temp:
        return None

    time = (1 / cooling_constant) * log((initial_temp - ambient_temp) / (final_temp - ambient_temp))

    if time_unit == 'seconds':
        return time * 60
    elif time_unit == 'hours':
        return time / 60
    else:
        return time

# Example usage
initial_temperature = 100  # degrees
final_temperature = 80     # degrees
ambient_temperature = 70   # degrees

cooling_time = calculate_cooling_time(initial_temperature, final_temperature, ambient_temperature)

print(f"Time taken for the water to cool from {initial_temperature}°C to {final_temperature}°C: {cooling_time:.2f} minutes")
""",

    "EXPORTED_SHARED_DATA": "<EXPORTED_SHARED_DATA>"
}

```

More examples can be found in the compare directory.

# Contributing

I am new at this, so I am sure there is a lot of room for improvement. Please feel free to submit a PR.

# Motivation

By controlling the verbosity of AI responses intended for other systems, we can reduce computational load, lower costs, and ultimately decrease carbon emissions that impact our planet.

# License

MIT
