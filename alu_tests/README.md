# gameboy-test-data

Full test data for 8-bit ALU operations on the Gameboy.

Sample test:
```json
{
    "x": "0x86",
    "y": "0x2b",
    "flags": "0x90",
    "result": {
        "value": "0xb2",
        "flags": "0x20"
    }
}
```

| Field          | Description                                                                                              |
| -------------- | -------------------------------------------------------------------------------------------------------- |
| `x`            | The left-hand side of the input to the ALU. e.g. for ADD A,E this would be the value of A.               |
| `y`            | The right-hand side of the input to the ALU. e.g. for ADD A,E this would be the value of E.              |
| `flags`        | The value of F prior to the operation.                                                                   |
| `result.value` | The result of the operation.  e.g. for ADD A,E this would be the value written to A after the operation. |
| `result.flags` | The value of F after the operation.                                                                      |

Each JSON file contains all the combinations of `x`, `y` and `flags` for the given instruction. For instructions that don't have a second argument, e.g. `CPL`, `DAA`) only the combinations of `x` and `flags` are included. All possible inputs of `flags` are included in all files to ensure they are not modified.

These files are intended to be used with generic ALU functions modelled as `Fn(u8, u8, u8) -> (u8, u8)`.

e.g.
```rust
fn res(x: u8, y: u8, flags: u8) -> (u8, u8) {
    (x & !(1 << y), flags)
}
```

These ALU generic functions could then be used in your CPU implementation as follows:
```rust
match cb {
    0x40 => {
        let (r, flags) = alu::res(cpu.b, 0, cpu.flags);
        cpu.b = r;
        cpu.flags = flags;
    }
    0x41 => {
        let (r, flags) = alu::res(cpu.c, 0, cpu.flags);
        cpu.c = r;
        cpu.flags = flags;
    }
    // ...
    0x50 => {
        let (r, flags) = alu::res(cpu.b, 1, cpu.flags);
        cpu.b = r;
        cpu.flags = flags;
    }
    // ...
}
```