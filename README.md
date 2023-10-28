# refaci

A toolbox for changing imports in enormous codebases after a large refactoring.

---

<p align="center">
<a href="https://github.com/ovsyanka83/refaci/actions?query=workflow%3ATests+event%3Apush+branch%3Amain" target="_blank">
    <img src="https://github.com/Ovsyanka83/refaci/actions/workflows/test.yaml/badge.svg?branch=main&event=push" alt="Test">
</a>
<a href="https://codecov.io/gh/ovsyanka83/refaci" target="_blank">
    <img src="https://img.shields.io/codecov/c/github/ovsyanka83/refaci?color=%2334D058" alt="Coverage">
</a>
<a href="https://pypi.org/project/refaci/" target="_blank">
    <img alt="PyPI" src="https://img.shields.io/pypi/v/refaci?color=%2334D058&label=pypi%20package" alt="Package version">
</a>
<a href="https://pypi.org/project/refaci/" target="_blank">
    <img src="https://img.shields.io/pypi/pyversions/refaci?color=%2334D058" alt="Supported Python versions">
</a>
</p>

## Installation

```bash
pip install refaci
```

## Guide

```python

import json
from pathlib import Path
​
from refaci.refactor import ContentsRegex, FilePathRegex, refactor

{
    "moved": {
        "old_package.worker": {
            "DEAD_MESSAGE_TTL": { # Symbol that we changed
                "path": "new_package.worker",
                "alias": "DEAD_MESSAGE_TTL"
            },
            "RETRY_TIME": {
                "path": "new_package.worker",
                "alias": "NEW_RETRY_TIME_NAME"
            }
        },
    },
    "new": { # These imports will be added to all modules automatically
        "b64_decode_url_params": "new_package.pagination",
        "PaginationToken": "new_package.pagination"
    }
}​
​
​
replacements = {
    FilePathRegex(r""): (
        ("internal_config=", "internal_settings="),
        ("= get_logger", "= getLogger"),
    ),
    FilePathRegex(r"api/app.py"): [
        ("LogicError", "BusinessLogicError"),
    ],
    ContentsRegex(r"class .+\(.+Controller\)"): (
        ("super().get(", "self.client.get("),
    ),
    FilePathRegex(r"tests/"): [
        ["Response(", "make_response("],
    ],
}
​
​
refactor(Path(input("Path to your service:")), import_replacements["moved"], import_replacements["new"], replacements)
```
