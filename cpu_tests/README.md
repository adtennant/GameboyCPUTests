# gameboy-test-data

Full test data for CPU operations on the Gameboy. The test data is in the same format as https://github.com/TomHarte/ProcessorTests.

Each file contains 10,000 tests for each instruction, which the exception of `CB.json` which contains 100,000 tests.

Sample test:
```json
{
    "name": "d1 96 a1",
    "initial": {
        "a": "0x86",
        "b": "0x35",
        "c": "0x50",
        "d": "0xa4",
        "e": "0x82",
        "f": "0xc0",
        "h": "0x54",
        "l": "0x7d",
        "pc": "0xd396",
        "sp": "0xa10a",
        "ram": [
            [
                "0xa10a",
                "0x42"
            ],
            [
                "0xa10b",
                "0x9c"
            ],
            [
                "0xd396",
                "0xd1"
            ]
        ]
    },
    "final": {
        "a": "0x86",
        "b": "0x35",
        "c": "0x50",
        "d": "0x9c",
        "e": "0x42",
        "f": "0xc0",
        "h": "0x54",
        "l": "0x7d",
        "pc": "0xd397",
        "sp": "0xa10c",
        "ram": [
            [
                "0xa10a",
                "0x42"
            ],
            [
                "0xa10b",
                "0x9c"
            ],
            [
                "0xd396",
                "0xd1"
            ]
        ]
    },
    "cycles": [
        [
            "0xd396",
            "0xd1",
            "read"
        ],
        [
            "0xa10a",
            "0x42",
            "read"
        ],
        [
            "0xa10b",
            "0x9c",
            "read"
        ]
    ]
},
```

| Field     | Description                                                                                                                                                               |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`    | Provided for human consumption and has no formal meaning.                                                                                                                 |
| `initial` | The initial state of the processor; `ram` contains a list of values to store in memory prior to execution, each one in the form `[address, value]`.                       |
| `final`   | The state of the processor and relevant memory contents after execution.                                                                                                  |
| `cycles`  | A cycle-by-cycle breakdown of bus activity in the form `[address, value, type]` where type is either `"read"` or `"write"`. Note: Cycles in this case refers to M-cycles. |