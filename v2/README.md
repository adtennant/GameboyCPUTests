# Gameboy (Sharp LR35902)

The Gameboy's CPU is a custom chip called the Sharp LR35902. The chip is very similar to the much more popular Intel 8080 and the Zilog Z80.

100 tests are provided per opcode, which the exception of `0xCB` which has 25,600 tests. All tests assume a full 64KB of uniquely-mapped RAM is mapped to the processor; this deviates from the actual memory layout of a Gameboy.

The tests assume a decode-execute-prefetch loop and so start at PC+1, with the final cycle being the prefetch of the next instruction. See [Gameboy CPU Internals](https://gist.github.com/SonoSooS/c0055300670d678b5ae8433e20bea595#fetch-and-stuff) for more info.

`STOP`, `HALT`, `EI` and `DI` are currently omitted until I can devise a meaningful way to test their behaviour. 

Sample test:
```json
[
  {
    "name": "cd a5 d0",
    "initial": {
      "a": 39,
      "b": 42,
      "c": 203,
      "d": 219,
      "e": 80,
      "f": 0,
      "h": 113,
      "l": 69,
      "pc": 31709,
      "sp": 61734,
      "ram": [
        [
          31708,
          205
        ],
        [
          31709,
          165
        ],
        [
          31710,
          208
        ]
      ]
    },
    "final": {
      "a": 39,
      "b": 42,
      "c": 203,
      "d": 219,
      "e": 80,
      "f": 0,
      "h": 113,
      "l": 69,
      "pc": 53414,
      "sp": 61732,
      "ram": [
        [
          31708,
          205
        ],
        [
          31709,
          165
        ],
        [
          31710,
          208
        ],
        [
          53413,
          77
        ]
      ]
    },
    "cycles": [
      [
        31709,
        165,
        "read"
      ],
      [
        31710,
        208,
        "read"
      ],
      null,
      [
        61733,
        123,
        "write"
      ],
      [
        61732,
        223,
        "write"
      ],
      [
        53413,
        77,
        "read"
      ]
    ]
  },
]
```

| Field     | Description                                                                                                                                                               |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`    | Provided for human consumption and has no formal meaning.                                                                                                                 |
| `initial` | The initial state of the processor; `ram` contains a list of values to store in memory prior to execution, each one in the form `[address, value]`.                       |
| `final`   | The state of the processor and relevant memory contents after execution.                                                                                                  |
| `cycles`  | An M-cycle breakdown of bus activity in the form `[address, value, type]` where type is either `"read"` or `"write"`. `null` indicates a cycle with no bus activity.      |