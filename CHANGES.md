# Version History

## v1.3

Added min/max parameters to `RandomNumberGenerator.byte()` and fixed some bugs.

## v1.2

Convert logic to implement RNGs as value-type structs without forcing them to
be wrapped in pointers.

### Fixes

- Seed bytes beyond a certain point were ignored, but now the whole seed is used.

## v1.1

Bug fixes and updates to reflect new Tomo syntax and internals.

## v1.0

Initial version
