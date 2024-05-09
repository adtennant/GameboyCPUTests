# Game Boy CPU (SM83) Tests

Test data for developers of Game Boy emulators. These files are designed for unit testing the CPU of the Game Boy (SM83) in isolation and don't expect any other hardware components to be implemented.

* `alu_tests` **(Removed since [c7b4e6a](https://github.com/adtennant/sm83-test-data/commit/c7b4e6a5ee935d0c02dbc655a52b560ac42de392))** - Contains full test data for the 8-bit ALU operations of the Game Boy CPU.
* `v1` **(Deprecated since [c7b4e6a](https://github.com/adtennant/sm83-test-data/commit/c7b4e6a5ee935d0c02dbc655a52b560ac42de392))** - Contains randomly generated test data for all Game Boy CPU instructions.
* `v2` - Updated randomly generated test data for all Game Boy CPU instructions in the same format as https://github.com/TomHarte/ProcessorTests.

The main differences between `v2` and `v1` are:
* The test format now correctly matches https://github.com/TomHarte/ProcessorTests.
* Register and RAM values are recorded as decimal numbers instead of hexidecimal to make them easier to parse.
* The tests now assume you're doing a decode-execute-prefetch loop instead of a fetch-decode-execute loop.
* Improved coverage of `CB` opcodes.
* Removed `STOP`, `HALT`, `EI`, `DI` as the tests are not meaningful.

Please see the README in each individual folder for more details.
