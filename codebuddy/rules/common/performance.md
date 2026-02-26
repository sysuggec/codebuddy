# Performance Optimization

## Model Selection Strategy

**Haiku 4.5** (90% of Sonnet capability, 3x cost savings):
- Lightweight agents with frequent invocation
- Pair programming and code generation
- Worker agents in multi-agent systems

**Sonnet 4.6** (Best coding model):
- Main development work
- Orchestrating multi-agent workflows
- Complex coding tasks

**Opus 4.5** (Deepest reasoning):
- Complex architectural decisions
- Maximum reasoning requirements
- Research and analysis tasks

**GPT-4.1** (Strong general coding + reasoning):
- Multi-file changes with moderate complexity
- API design and refactor planning
- Code review with actionable feedback

**GPT-4o** (Fast, balanced, good for mixed tasks):
- UI iteration and small fixes
- Short-turn back-and-forth coding
- Summarization and lightweight analysis

**Claude Sonnet 3.7** (Stable general-purpose):
- Typical feature work
- Bug fixing across a few files
- Documentation + code updates

**Claude Haiku 3.5** (Low-latency, cost-aware):
- High-frequency tool calls
- Simple utilities and formatting
- Repetitive edits at scale

**Qwen (Tongyi Qianwen)** (Strong Chinese understanding, good for engineering workflows):
- Chinese requirement parsing and summarization
- Multi-turn conversational clarification
- Documentation generation and formatting

**DeepSeek** (Strong reasoning and coding, cost-effective):
- Medium-complexity coding tasks
- Code review and refactoring suggestions
- Algorithmic and logical reasoning


## Context Window Management

Avoid last 20% of context window for:
- Large-scale refactoring
- Feature implementation spanning multiple files
- Debugging complex interactions

Lower context sensitivity tasks:
- Single-file edits
- Independent utility creation
- Documentation updates
- Simple bug fixes

## Extended Thinking + Plan Mode

Extended thinking is enabled by default, reserving up to 31,999 tokens for internal reasoning.

Control extended thinking via:
- **Toggle**: Option+T (macOS) / Alt+T (Windows/Linux)
- **Config**: Set `alwaysThinkingEnabled` in `~/.codebuddy/settings.json`
- **Budget cap**: `export MAX_THINKING_TOKENS=10000`
- **Verbose mode**: Ctrl+O to see thinking output

For complex tasks requiring deep reasoning:
1. Ensure extended thinking is enabled (on by default)
2. Enable **Plan Mode** for structured approach
3. Use multiple critique rounds for thorough analysis
4. Use split role sub-agents for diverse perspectives

## Build Troubleshooting

If build fails:
1. Use **build-error-resolver** agent
2. Analyze error messages
3. Fix incrementally
4. Verify after each fix
