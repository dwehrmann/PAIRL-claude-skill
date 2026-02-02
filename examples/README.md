# PAIRL Examples

This directory contains example PAIRL messages demonstrating real-world usage with the Claude Code skill.

---

## Example 1: Simple Delegation

**File**: [01-simple-delegation.pairl](01-simple-delegation.pairl)

A straightforward task delegation from Bob to Jonas requesting the "Dynamic Mondays" report by a specific deadline.

### Natural Language Input

```
/pairl generate: Hi this is Bob. Message Jonas and ask for the report on Dynamic Mondays. I need it by tomorrow, Feb. 3rd, 2026
```

### Generated PAIRL Message

```pairl
@v 1
@mid ref:msg:01JH1A2B3C4D5E6F7G8H9J0K1M
@ts 2026-02-02T10:00:00.000+01:00

req{t=report,s=c,l=1,m=!,a=i} @rid=a1
#fact from=Bob @rid=f1
#fact to=Jonas @rid=f2
#fact topic=Dynamic_Mondays @rid=f3
#fact deadline=2026-02-03 @rid=f4
```

### Human-Readable Translation

> **From**: Bob
> **To**: Jonas
> **Sent**: February 2, 2026 at 10:00 AM CET
> **Priority**: Urgent
>
> Hi Jonas,
>
> I need the report on **Dynamic Mondays** by tomorrow (February 3rd, 2026). It's urgent!
>
> Thanks,
> Bob

### Token Efficiency

- **Natural language**: 28 tokens
- **PAIRL (body only)**: 12 tokens
- **Savings**: ~57% reduction

### Key Concepts Demonstrated

1. **Two-Channel Design**
   - **Lossy**: Style (`s=c` casual), mood (`m=!` urgent), length (`l=1` short)
   - **Lossless**: Names (Bob, Jonas), topic (Dynamic_Mondays), deadline (2026-02-03)

2. **Anti-Hallucination**
   - Critical data (names, dates, report title) in `#fact` records
   - Style/tone can vary in rendering, but facts remain exact

3. **Compact Intent**
   - `req{t=report,s=c,l=1,m=!,a=i}` encodes:
     - Request type: report
     - Casual style
     - Short response expected
     - Urgent priority
     - Internal audience

---

## Using These Examples

### With Claude Code Skill

```bash
# Explain an example
/pairl explain examples/01-simple-delegation.pairl

# Validate an example
/pairl validate examples/01-simple-delegation.pairl

# Generate a similar message
/pairl generate: Message Sarah and ask for the Q4 budget review by Friday
```

### Testing Your Own Messages

Use these examples as templates:

1. Copy the structure
2. Replace `@mid` with a new ULID
3. Update `@ts` timestamp
4. Modify `#fact` values for your use case
5. Adjust intent parameters to match your tone/style

---

## More Examples

For comprehensive examples including:
- Multi-agent conversations
- Evidence-based reporting
- Budget tracking and cost reporting
- Complex threading with dependencies

See the main [PAIRL repository examples](https://github.com/dwehrmann/PAIRL/tree/main/examples).

---

## Validation

All examples pass PAIRL v1.1 validation:

```bash
# Using the PAIRL validator
python /path/to/PAIRL/tools/validator.py examples/*.pairl
```

Expected output:
```
✓ 01-simple-delegation.pairl: Valid
```

---

For the complete PAIRL v1.1 specification, see [SPEC.md](https://github.com/dwehrmann/PAIRL/blob/main/SPEC.md).
