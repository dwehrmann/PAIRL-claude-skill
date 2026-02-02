# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0] - 2026-02-02

### Added
- Initial release of PAIRL Claude Code skill
- Support for PAIRL v1.1 specification
- Message generation from natural language
- PAIRL message validation (rules V1-V8)
- Conversation to PAIRL conversion
- PAIRL to human-readable explanation
- Economic features support (budget tracking, cost reporting, quota management)
- Comprehensive skill instructions in skill.md
- Installation documentation in README.md
- NPM package configuration

### Features
- Two-channel architecture (lossy intents + lossless facts)
- Pointer-first state management via references
- Anti-hallucination guardrails
- Token efficiency (70-90% reduction)
- Budget compliance validation
- Support for all standard PAIRL intents
- Record ID (RID) generation
- Message threading support (@root, @parent, @deps)

### Documentation
- Complete README with installation and usage instructions
- Example use cases and transformations
- Configuration options
- Publishing guide for NPM and GitHub
