[![Build Status](https://travis-ci.com/jacksmith15/statham-schema.svg?token=JrMQr8Ynsmu5tphpTQ2p&branch=master)](https://travis-ci.com/jacksmith15/statham-schema)
# Statham Schema
A tool for generating Python classes/models from [JSONSchema](https://json-schema.org/) documents.

This project aims to simplify the experience of integrating with external sources, by providing:
1. **External validation**: Ensure that incoming data matches what you expect _early_.
2. **Internal validation**: Ensure your application's use of external data sources is _consistent_ with the schema. Declared models and static type checking (see [mypy](http://mypy-lang.org/)) can do this job for you.
3. **Visibility**: Update the model layer against schema changes automatically, and let static tools identify any issues.

The tool will generate [attrs](http://www.attrs.org/en/stable/index.html) dataclasses which can then be used and extended as a model layer for the external data source.

# Requirements
This package is currently tested for Python 3.6.

# Installation
This project may be installed using [pip](https://pip.pypa.io/en/stable/):
```
pip install statham-schema
```

# Generating python classes
From the directory of the cloned repository, run
```
statham --input /path/to/schema.json
```

This will write generated python classes to stdout. Optionally specify an `--output` path to write to file.

## Command-line arguments
```
Required arguments:
  --input INPUT    Specify the path to the JSON Schema to be generated.

                   If the target schema is not at the root of a document, specify the
                   JSON Pointer in the same format as a JSONSchema `$ref`, e.g.
                   `--input path/to/document.json#/definitions/schema`


Optional arguments:
  --output OUTPUT  Output directory or file in which to write the output.

                   If the provided path is a directory, the command will derive the name
                   from the input argument. If not passed, the command will write to
                   stdout.

  -h, --help       Display this help message and exit.
```

See this [example output](https://github.com/jacksmith15/statham-schema/blob/master/tests/models/simple.py).

# Using custom format keywords
JSONSchema allows use of custom string formats as specified [here](https://json-schema.org/draft/2019-09/json-schema-validation.html#rfc.section.7.2.3). Custom validation logic for string format may be added like so:
```python
from statham.validators import format_checker

@format_checker.register("my_format")
def _check_my_format(value: str) -> bool:
    ...
```

# Supported JSONSchema features
- [x] Basic types (primitves, array, object)
- [x] Composite primitive types
- [x] Type validation on generated classes
- [x] Validation of `required`
- [x] Local references
- [x] Type-specific validation (pattern, format, minimum, maximum etc)
- [x] Custom string format validation
- [x] Remote references
- [ ] Tuple validation of arrays
- [ ] Built-in string format validation #6
- [ ] Generic keywords: `enum` #8, `const` #9
- [ ] Array keywords: `uniqueItems` #10, `contains`
- [ ] Composition keywords (`allOf` #12, `anyOf` #13, `oneOf` #11, `not` #14)
- [ ] `minProperties`, `maxProperties` #15
- [ ] Property dependencies
- [ ] Schema dependencies
- [ ] `propertyNames`, `patternProperties`, `additionalProperties` (This is  tricky with `attrs`)
- [ ] `additionalItems` keyword
- [ ] `if`, `then`, `else` keywords

# Development
1. Clone the repository: `git clone git@github.com:jacksmith15/statham-schema.git && cd statham-schema`
2. Install the requirements: `pip install -r requirements.txt -r requirements-test.txt`
3. Run `pre-commit install`
4. Run the tests: `bash run_test.sh -c -a`

This project uses the following QA tools:
- [PyTest](https://docs.pytest.org/en/latest/) - for running unit tests.
- [PyLint](https://www.pylint.org/) - for enforcing code style.
- [MyPy](http://mypy-lang.org/) - for static type checking.
- [Travis CI](https://travis-ci.org/) - for continuous integration.

# License
This project is distributed under the MIT license.

![statham](https://giant.gfycat.com/GrotesqueNauticalCaracal.gif)
