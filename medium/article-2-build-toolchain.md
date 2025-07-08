**Article 2: Building the Gemini CLI - From Source to Executable**

*   **Introduction:** Briefly recap the architectural overview from Article 1 and set the stage for understanding how the CLI is built and prepared for distribution.
*   **Core Build Orchestration (`package.json` scripts and `Makefile`):**
    *   Explain that the `package.json` `scripts` section defines the primary build commands (`build`, `test`, `lint`, `preflight`).
    *   Mention that the `Makefile` acts as a simple wrapper around these `npm` scripts for convenience.
    *   Highlight `npm run preflight` as the comprehensive command for validation (build, test, typecheck, lint).
*   **Bundling with `esbuild` (`esbuild.config.js`):**
    *   Detail how `esbuild` is used as the primary bundler.
    *   Explain that `packages/cli/index.ts` is the main entry point for the CLI application.
    *   Describe the output: a single JavaScript file (`bundle/gemini.js`) optimized for Node.js (ESM format).
    *   Mention the injection of `process.env.CLI_VERSION` and Node.js global compatibility (`__filename`, `__dirname`).
*   **TypeScript Configuration (`tsconfig.json`):**
    *   Explain the role of `tsconfig.json` in defining TypeScript compilation options.
    *   Highlight key settings like `strict: true` (for type safety), `esModuleInterop: true`, `module: "NodeNext"`, and `target: "es2022"`.
*   **Modular Package Builds (`scripts/build_package.js`):**
    *   Describe how `build_package.js` handles the compilation of individual TypeScript packages (`tsc --build`).
    *   Mention the copying of `.md` and `.json` files and the creation of a `.last_build` marker.
*   **Sandbox Container Build (`scripts/build_sandbox.js`):**
    *   Explain the process of building the sandbox container image.
    *   Detail how `npm pack` is used to create `.tgz` archives of the `cli` and `core` packages, which are then included in the Docker image.
    *   Mention the use of `docker` or `podman` for building the image.
*   **Asset Copying (`scripts/copy_bundle_assets.js`):**
    *   Explain that this script copies specific assets (like `.sb` sandbox configuration files) into the final `bundle` directory, ensuring they are available at runtime.
*   **Overall Flow:** Summarize the end-to-end build process:
    1.  Dependencies installed (`npm install`).
    2.  Git commit info generated (`npm run generate`).
    3.  Individual packages built (TypeScript compilation, asset copying).
    4.  Main CLI bundled with `esbuild`.
    5.  Sandbox container image built (if enabled), incorporating the packaged CLI and core.
    6.  Final assets copied to the `bundle` directory.