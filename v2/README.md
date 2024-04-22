# Gameboy (Sharp LR35902)

The Gameboy's CPU is a custom chip called the Sharp LR35902. The chip is very similar to the much more popular Intel 8080 and the Zilog Z80.

1,000 tests are provided per opcode, which the exception of `0xCB` which has 10,000 tests. All tests assume a full 64KB of uniquely-mapped RAM is mapped to the processor; this deviates from the actual memory layout of a Gameboy.

`STOP`, `HALT`, `EI` and `DI` are currently omitted until I can devise a meaningful way to test their behaviour. 

Sample test:
```json
[
    {
        "name": "d1 da ea",
        "initial": {
            "a": 173,
            "b": 29,
            "c": 30,
            "d": 182,
            "e": 58,
            "f": 192,
            "h": 244,
            "l": 78,
            "pc": 31781,
            "sp": 37355,
            "ram": [
                [
                    31781,
                    209
                ],
                [
                    31782,
                    218
                ],
                [
                    31783,
                    234
                ]
            ]
        },
        "final": {
            "a": 173,
            "b": 29,
            "c": 30,
            "d": 202,
            "e": 248,
            "f": 192,
            "h": 244,
            "l": 78,
            "pc": 31782,
            "sp": 37357,
            "ram": [
                [
                    31781,
                    209
                ],
                [
                    31782,
                    218
                ],
                [
                    31783,
                    234
                ],
                [
                    37355,
                    248
                ],
                [
                    37356,
                    202
                ]
            ]
        },
        "cycles": [
            [
                37355,
                248,
                "read"
            ],
            [
                37356,
                202,
                "read"
            ],
            [
                31781,
                209,
                "read"
            ]
        ]
    }
]
```

| Field     | Description                                                                                                                                                               |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`    | Provided for human consumption and has no formal meaning.                                                                                                                 |
| `initial` | The initial state of the processor; `ram` contains a list of values to store in memory prior to execution, each one in the form `[address, value]`.                       |
| `final`   | The state of the processor and relevant memory contents after execution.                                                                                                  |
| `cycles`  | A cycle-by-cycle breakdown of bus activity in the form `[address, value, type]` where type is either `"read"` or `"write"`. Note: Cycles in this case refers to M-cycles. |