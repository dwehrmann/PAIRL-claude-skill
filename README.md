# PAIRL Claude Code Skill

A Claude Code skill for generating, validating, and working with PAIRL v1.1 messages.

## What is PAIRL?

PAIRL (Protocol for Agent Intermediate Representation Lite) is a compact, human-readable message format for agent-to-agent communication that achieves:

- **70-90% token reduction** vs natural language
- **Built-in budget tracking** and cost reporting
- **Anti-hallucination guardrails** with separate lossy/lossless channels
- **Pointer-first state** management via content references
- **Transport-agnostic** design (HTTP, files, message queues, WebSocket)

## Installation

### Option 1: Install from NPM (Recommended)

```bash
claude-code skill install pairl
```

### Option 2: Install from GitHub

```bash
claude-code skill install https://github.com/dwehrmann/PAIRL-claude-skill
```

### Option 3: Install from Local Directory

```bash
cd /path/to/PAIRL
claude-code skill install ./claude-skill
```

### Option 4: Manual Installation

1. Copy the skill directory to your Claude Code skills folder:
   ```bash
   mkdir -p ~/.claude/skills/pairl
   cp claude-skill/* ~/.claude/skills/pairl/
   ```

2. Restart Claude Code or reload skills:
   ```bash
   claude-code skill reload
   ```

## Usage

Invoke the skill using `/pairl` in any Claude Code session:

### Quick Start Example

Convert a simple delegation request into PAIRL format:

```
/pairl generate: Hi this is Bob. Message Jonas and ask for the report on Dynamic Mondays. I need it by tomorrow, Feb. 3rd, 2026
```

**Output**:
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

**What happened here?**
- Natural language (28 tokens) → PAIRL (12 tokens) = **57% savings**
- Critical data (names, dates, topic) moved to lossless `#fact` records
- Style/mood encoded in compact intent parameters (`s=c` casual, `m=!` urgent)

See [examples/01-simple-delegation.pairl](examples/01-simple-delegation.pairl) for the full example.

### Generate PAIRL Messages

Convert natural language requests into PAIRL format:

```
/pairl generate: Can you analyze the Q4 sales report and send me a summary by Friday? Budget is $0.10.
```

**Output**:
```pairl
@v 1
@mid ref:msg:01JH0Q6Z7F8K4Q2S1R6E2E9A3B
@ts 2026-02-02T10:00:00.000+01:00
@budget 0.10USD

req{t=analysis,s=f,l=2,m=0,a=i} @rid=a1
#fact ask=summary @rid=f1
#fact deadline=2026-02-07 @rid=f2
#ref input=ref:doc:id:q4-sales-report @rid=r1
```

### Validate PAIRL Messages

Check PAIRL syntax and compliance:

```
/pairl validate message.pairl
```

Or paste a message directly:

```
/pairl validate:
@v 1
@mid ref:msg:01JH...
@ts 2026-01-31T16:20:01Z

req{t=analysis,s=f,l=2} @rid=a1
#fact ask=report @rid=f1
```

### Convert Conversations to PAIRL

Transform verbose agent communication into compact PAIRL:

```
/pairl convert:
Agent A: "I need you to analyze the market data from Q4 2025 and provide insights on customer behavior trends."
Agent B: "I'll analyze that data and send you a report. I estimate this will cost about $0.05 and use 10,000 tokens."
```

**Output**: Thread of PAIRL messages with token savings report

### Explain PAIRL Messages

Get human-readable explanations of PAIRL messages:

```
/pairl explain:
@v 1
@mid ref:msg:01JH...
req{t=specs,s=f,l=2,m=+,a=c} @rid=a1
#fact ask=spec_document @rid=f1
```

**Output**: "This is a formal request for a spec document, medium length, positive mood, for a client audience..."

## Features

### Core Capabilities

- **Message Generation**: Convert natural language to PAIRL
- **Validation**: Check compliance with PAIRL v1.1 spec
- **Conversion**: Transform verbose communication to efficient PAIRL
- **Explanation**: Decode PAIRL messages into human-readable form
- **Token Analysis**: Calculate token savings vs natural language

### Validation Rules (v1.1)

The skill enforces all PAIRL validation rules:

- **V1** — No-new-facts: Intents must not contain digits, URLs, or hex strings
- **V2** — Evidence completeness: All `#evid` must have claim, src, conf
- **V3** — Ref format: All refs match `ref:<ns>:<type>:<id>`
- **V6** — RID uniqueness: No duplicate record IDs
- **V8** — Budget compliance: Cost tracking and budget enforcement

### Economic Features

Native support for:
- Budget tracking via `@budget` headers
- Cost reporting via `#cost` records
- Quota management via `#quota` records
- Resource bidding for approval workflows

## Configuration

Configure skill behavior in your Claude Code settings:

```json
{
  "skills": {
    "pairl": {
      "defaultValidationMode": "loose",
      "autoGenerateRIDs": true,
      "includeHash": false,
      "defaultBudget": ""
    }
  }
}
```

### Settings

- **defaultValidationMode**: `"strict"` or `"loose"` (default: loose)
- **autoGenerateRIDs**: Auto-generate `@rid` for all records (default: true)
- **includeHash**: Include `@hash` integrity verification (default: false)
- **defaultBudget**: Default budget for messages, e.g. `"0.10USD"` (default: "")

## Examples

### Example 1: Multi-Agent Research Task

**Input**:
```
/pairl generate: Research AI trends in 2026, summarize findings, and estimate costs before starting. Budget: $0.50 USD.
```

**Output**:
```pairl
@v 1
@mid ref:msg:01JH0Q6Z7F8K4Q2S1R6E2E9A3B
@ts 2026-02-02T10:00:00.000+01:00
@budget 0.50USD

req{t=research,s=f,l=2,m=0,a=i} @rid=a1
#fact topic=ai_trends_2026 @rid=f1
#fact ask=summary @rid=f2
#fact bid_required=true @rid=f3
```

### Example 2: Agent Response with Evidence

**Input**:
```
/pairl generate: Analysis complete. Found 3 key trends: multimodal AI, agent orchestration, and edge inference. Sources: TechReport-2026, ArXiv study #12345. Cost: $0.28, used 18,500 tokens of 25,000.
```

**Output**:
```pairl
@v 1
@mid ref:msg:01JH0Q6Z7F8K4Q2S1R6E2E9A3C
@parent ref:msg:01JH0Q6Z7F8K4Q2S1R6E2E9A3B
@ts 2026-02-02T10:05:30.000+01:00

cmp{t=research,s=f,l=2,m=+} @rid=a1
#fact trends_found=3 @rid=f1
#evid claim="multimodal AI dominates 2026" src=ref:doc:id:techreport-2026 conf=0.90 @rid=e1
#evid claim="agent orchestration frameworks emerging" src=ref:doc:id:techreport-2026 conf=0.85 @rid=e2
#evid claim="edge inference adoption growing" src=ref:url:arxiv:12345 conf=0.80 @rid=e3
#cost val=0.28 cur=USD model=gpt-4o note="actual cost" @rid=c1
#quota type=tokens total=25000 used=18500 rem=6500 @rid=q1
```

### Example 3: Budget Exceeded Scenario

**Input**:
```
/pairl generate: Analysis requires $0.75 but budget is only $0.50. Propose bid for approval.
```

**Output**:
```pairl
@v 1
@mid ref:msg:01JH0Q6Z7F8K4Q2S1R6E2E9A3D
@parent ref:msg:01JH0Q6Z7F8K4Q2S1R6E2E9A3B
@ts 2026-02-02T10:00:30.000+01:00

bid{t=research,s=f,l=1,m=!} @rid=a1
#cost val=0.75 cur=USD note="projected comprehensive analysis" @rid=c1
#quota type=tokens total=40000 @rid=q1
#fact reason=budget_exceeded @rid=f1
#ref budget_limit=ref:msg:01JH0Q6Z7F8K4Q2S1R6E2E9A3B#budget @rid=r1
#fact wait_for_approval=true @rid=f2
```

## Integration with PAIRL Tools

The skill works seamlessly with the PAIRL ecosystem:

### Validator Integration

The skill can invoke the Python validator:

```
/pairl validate --strict message.pairl
```

Internally runs:
```bash
python /path/to/PAIRL/tools/validator.py --strict message.pairl
```

### Reference to Specification

The skill has full access to:
- `/path/to/PAIRL/SPEC.md` — Complete v1.1 specification
- `/path/to/PAIRL/examples/` — Example messages and threads
- `/path/to/PAIRL/tools/validator.py` — Reference validator

## Token Savings

PAIRL achieves significant token reduction:

**Natural Language** (87 tokens):
```
"I need you to analyze the Q4 sales report that I shared with you earlier (document ID: xyz-123). Please provide a formal summary of the key findings and send it to me by Friday, February 7th, 2026. We have a budget of $0.10 for this analysis task. Thank you."
```

**PAIRL** (31 tokens):
```
@v 1
@mid ref:msg:01JH0Q6Z7F8K4Q2S1R6E2E9A3B
@ts 2026-02-02T10:00:00.000+01:00
@budget 0.10USD

req{t=analysis,s=f,l=2,m=0,a=i} @rid=a1
#fact ask=summary @rid=f1
#fact deadline=2026-02-07 @rid=f2
#ref input=ref:doc:id:xyz-123 @rid=r1
```

**Savings**: ~64% token reduction (87 → 31 tokens)

## Resources

- **PAIRL Specification**: [SPEC.md](https://github.com/dwehrmann/PAIRL/blob/main/SPEC.md)
- **Examples**: [examples/](https://github.com/dwehrmann/PAIRL/tree/main/examples)
- **Validator**: [tools/validator.py](https://github.com/dwehrmann/PAIRL/blob/main/tools/validator.py)
- **Contributing**: [CONTRIBUTING.md](https://github.com/dwehrmann/PAIRL/blob/main/CONTRIBUTING.md)
- **Commercial Use**: [COMMERCIAL.md](https://github.com/dwehrmann/PAIRL/blob/main/COMMERCIAL.md)

## License

This skill is licensed under the Apache License 2.0, same as the PAIRL specification.

See [LICENSE](../LICENSE) for details.

## Support

- **Issues**: [GitHub Issues](https://github.com/dwehrmann/PAIRL-claude-skill/issues)
- **Discussions**: [GitHub Discussions](https://github.com/dwehrmann/PAIRL-claude-skill/discussions)
- **PAIRL Specification**: [PAIRL Docs](https://github.com/dwehrmann/PAIRL)

---

**PAIRL**: Compact, reliable, economic agent communication. 🚀
