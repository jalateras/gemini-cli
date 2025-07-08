**Article 4: The Brains of the Operation - Gemini CLI's Core Logic and Tools**

*   **Introduction:** Recap the CLI's entry point and execution modes, then introduce the `packages/core` as the "brain" of the Gemini CLI, responsible for interacting with the Gemini API and executing tools.
*   **`packages/core/src/index.ts` - The Core's Public API:**
    *   Explain that this file serves as the main export for the `@google/gemini-cli-core` package.
    *   Highlight the key categories of exports:
        *   `config`: Core configuration.
        *   `core`: Client, content generation, chat, logging, prompts, token limits, turns, requests.
        *   `code_assist`: Code assistance functionalities (OAuth2, server, types).
        *   `utils`: Utility functions (paths, schema validation, errors, folder structure, memory discovery, git ignore parsing, editor).
        *   `services`: File discovery, Git service.
        *   `tools`: Base tool definitions and specific tool implementations.
        *   `telemetry`: Telemetry functions.
*   **The Tool System - Extending Gemini's Capabilities:**
    *   **Core Concepts (from `docs/core/tools-api.md`):**
        *   **`Tool` Interface/`BaseTool`:** Define the contract for all tools (name, displayName, description, parameterSchema, validateToolParams, getDescription, shouldConfirmExecute, execute).
        *   **`ToolResult`:** Explain the structure of a tool's output (`llmContent` for the model, `returnDisplay` for the user).
        *   **`ToolRegistry` (`tool-registry.ts`):** Describe its role in registering built-in and discovered tools, providing schemas to the Gemini model, and retrieving tools for execution.
    *   **Built-in Tools (`packages/core/src/tools/`):**
        *   Categorize and briefly describe the purpose of key built-in tools:
            *   **File System Tools:** `LSTool` (list directory), `ReadFileTool` (read file), `WriteFileTool` (write file), `GrepTool` (search patterns), `GlobTool` (find files), `EditTool` (modify files), `ReadManyFilesTool` (read multiple files).
            *   **Execution Tools:** `ShellTool` (execute shell commands).
            *   **Web Tools:** `WebFetchTool` (fetch URL content), `WebSearchTool` (perform web search).
            *   **Memory Tools:** `MemoryTool` (interact with AI's memory).
        *   Emphasize that each tool extends `BaseTool` and implements its specific logic.
*   **Tool Execution Flow:**
    *   Walk through the process: Model requests tool -> Core retrieves tool -> Parameter validation -> User confirmation (if needed) -> Tool execution -> Result processed -> Response sent back to model and displayed to user.
*   **Extending with Custom Tools:**
    *   Briefly mention the mechanisms for extending the tool system:
        *   **Command-based Discovery:** Using `toolDiscoveryCommand` to dynamically register tools via JSON output.
        *   **MCP-based Discovery:** Connecting to Model Context Protocol (MCP) servers to discover and use their exposed tools.
*   **Summary:** Conclude by highlighting how the robust tool system empowers the Gemini CLI to interact with the local environment and external services, making it a powerful and versatile AI assistant.