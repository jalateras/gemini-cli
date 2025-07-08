**Article 1: Unpacking the Gemini CLI - A High-Level Architectural Overview**

*   **Introduction:** Briefly introduce the Gemini CLI as a command-line AI workflow tool and its purpose (querying/editing codebases, generating apps, automating tasks, etc.).
*   **Monorepo Structure:** Explain that the Gemini CLI is structured as a monorepo, as indicated by the `workspaces` configuration in the root `package.json`. This allows for managing multiple related packages within a single repository.
*   **Core Packages:**
    *   **`packages/cli`:** This is the user-facing component. It handles user input, displays output, manages the UI (using Ink and React), and provides features like history management, theme customization, and CLI configuration. It acts as the "frontend" of the CLI.
    *   **`packages/core`:** This serves as the backend. It receives requests from the `cli` package, orchestrates interactions with the Gemini API, constructs prompts, manages conversations, and executes "tools." It's the "brain" of the CLI.
*   **The Role of "Tools":** Explain that "tools" (located in `packages/core/src/tools/`) are individual modules that extend the Gemini model's capabilities, allowing it to interact with the local environment (e.g., file system, shell commands, web fetching). Emphasize the user confirmation mechanism for actions that modify the system.
*   **Interaction Flow (Simplified):** Describe the typical interaction: User Input (CLI) -> Request to Core -> Core interacts with Gemini API (and potentially Tools) -> Response from Core -> Display to User (CLI).
*   **Key Design Principles:** Highlight the principles of modularity (separation of CLI and Core), extensibility (through tools), and focus on user experience.
*   **Supporting Directories:** Briefly mention the `docs/` directory for documentation and the `scripts/` directory for utility scripts related to building, testing, and development.
*   **Development Environment & Tooling:** Touch upon the use of Node.js, `npm` for package management, `eslint` for linting, `prettier` for formatting, and `vitest` for testing, as seen in `package.json` and `CONTRIBUTING.md`.