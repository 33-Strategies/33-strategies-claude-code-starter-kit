Please take a methodical, hypothesis-driven approach to debugging this issue:

1) **Review & Understand**: Use @task-context to guide a deep read all relevant docs, developer guides, READMEs, config files, and reference materials to build a complete mental model of the intended behavior. Pay attention to expected data shapes, dependencies, environment requirements, and any known edge cases.

2) **Map the Expected Flow**: Create a detailed, step-by-step articulation of what SHOULD happen—trace the full path from initial input through all functions, API calls, data transformations, and state changes to the expected final outcome. Document the expected inputs and outputs at each stage.

3) **Instrument for Visibility**: Add debug logging at each critical point along the path defined in step 2. Capture function entry/exit, inputs, outputs, intermediate state, and any conditional branches taken. Make logs clearly labeled and easy to follow chronologically.

4) **Identify & Fix**: Run the process, analyze the logs to pinpoint exactly where and why it fails (wrong data, missing values, unexpected state, unhandled errors, etc.), then implement the fix.

5) **Verify**: Use @agent-quick-check-expert to confirm everything is working as expected end-to-end before checking back in with me.

6) **Clean Up**: After I confirm that everything is working correctly, remove all temporary debug logging, test files, and any other artifacts created during this debugging process.
