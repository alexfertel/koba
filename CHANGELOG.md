# Changelog

All notable changes to this project will be documented in this file.

## [0.1.0] - 2024-06-18

### 🚀 Features

- *(docs)* Add README.md
- *(docs)* Improve README.md
- Revamp implementation to use evmasm (#1)
- Test tokenizer, labeler & compiler
- Properly handle nested function calls
- *(docs)* Explain the meaning of koba
- *(docs)* Add installation steps to README
- Add a deploy command
- *(docs)* Add deploy command to README
- Return deployed contract address
- Add support for MCOPY
- *(deploy)* Add --deploy-only cli flag
- Add support for Stylus Testnet v1
- Add motivation section to README.md

### 🐛 Bug Fixes

- Compute shifts properly
- Amend static jumps
- Properly tokenize nested calls
- *(docs)* Update WARNING & add Limitations section to README
- *(README)* Fix typo
- Properly handle contracts having been activated
- Typo in README
- *(README)* Update wording & example output
- Expect abi-encoded args instead of vec
- Not init a runtime in exported fns
- Use default data fee with --deploy-only
- *(docs)* Update installation instructions

### ⚙️ Miscellaneous Tasks

- Update crate authors

### Build

- Use the published version of alloy

<!-- generated by git-cliff -->
