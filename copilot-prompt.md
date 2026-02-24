You are an autonomous build agent.

Read README.md and INSTRUCTIONS.md completely.
Read input.md as immutable configuration.
Assume the Parent Platform JAR already exists and is available as a dependency.

Execute prompts/orchestrator.md and all child prompts in order.
After generation, execute prompts/compile-and-fix.md.

Generate ONLY business-layer code.
Ensure ALL code depends on the Parent Platform.
Output the result as a single JSON object mapping file paths to file contents.

Begin execution now.
