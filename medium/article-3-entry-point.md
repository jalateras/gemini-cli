**Article 3: The Gemini CLI's First Steps - Bootstrapping and Execution Modes**

*   **Introduction:** Briefly recap the previous articles (architecture and build process) and introduce the topic of how the CLI starts and operates.
*   **The `bin` Entry Point:**
    *   Explain that the `package.json` in `packages/cli` defines `dist/index.js` as the executable for the `gemini` command.
    *   Clarify that `dist/index.js` is the bundled output of `packages/cli/index.ts`.
*   **`packages/cli/index.ts` - The Initial Loader:**
    *   Describe `index.ts` as a thin wrapper that imports and calls the `main` function from `src/gemini.tsx`.
    *   Mention its role in catching and logging unexpected critical errors.
*   **`packages/cli/src/gemini.tsx` - The Core Bootstrapper:**
    *   **Initial Setup:** Explain that this file performs crucial initializations:
        *   Loading CLI configuration (`loadCliConfig`).
        *   Loading user settings (`loadSettings`).
        *   Handling startup warnings.
        *   Setting up authentication (including auto-detection of `GEMINI_API_KEY` or `CLOUD_SHELL`).
        *   Configuring memory usage (`getNodeMemoryArgs`, `relaunchWithAdditionalArgs`).
    *   **Sandbox Integration:** Detail how `gemini.tsx` checks for sandbox configuration and initiates the sandbox environment (`start_sandbox`) if enabled and not already running within one. Emphasize the authentication validation before entering the sandbox.
    *   **Interactive vs. Non-Interactive Mode Determination:**
        *   Explain the core logic for deciding the execution mode:
            *   If `process.stdin.isTTY` is true (interactive terminal) AND there's no command-line input (`input?.length === 0`), the interactive UI is rendered using `ink` (`render(<AppWrapper />)`).
            *   If `process.stdin.isTTY` is false (input piped from another command) OR there's command-line input, the CLI enters non-interactive mode.
            *   Mention the `readStdin()` function for handling piped input.
    *   **Error Handling:** Briefly touch upon the global unhandled promise rejection handler.
*   **`packages/cli/src/nonInteractiveCli.ts` - Headless Execution:**
    *   Describe this file's responsibility for handling non-interactive CLI operations.
    *   Explain that it directly interacts with the Gemini API (`geminiClient.getChat()`).
    *   Detail the loop for sending messages and processing responses, including:
        *   Extracting text responses.
        *   Handling `functionCalls` from the Gemini API.
        *   Executing tool calls (`executeToolCall`) via the `ToolRegistry`.
        *   Logging errors and exiting gracefully or with an error code.
    *   Emphasize that in non-interactive mode, "thoughts" (internal model reasoning) are not returned to `STDOUT`.
*   **Summary of Flow:** Provide a concise summary of the boot-up process, from the `gemini` command to either the interactive UI or the non-interactive execution loop.