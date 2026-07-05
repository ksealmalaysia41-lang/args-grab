The args library is the standard, official Dart package for command-line argument parsing. It is designed to be flexible, supporting both simple scripts and complex, multi-command CLI applications like the git example you provided.
### Core Architectural Concepts
 * **ArgParser**: The heart of the library. You define your options (flags, valued options, multi-options) here. It handles validation, abbreviations, defaults, and command nesting.
 * **ArgResults**: The object returned after parsing. It acts as a map to access the values provided by the user.
 * **CommandRunner**: A higher-level wrapper for building sophisticated CLIs. It handles common CLI patterns such as:
   * Automatic --help generation.
   * Command dispatching (mapping a sub-command string like commit to a specific Command class).
   * Error handling via UsageException.
### Strategic Implementation Notes
 1. **Semantic Versioning & Migration**: Note the warning at the start of your documentation: the repository has moved to dart-lang/core. If you are starting a new project, ensure you point to the latest package location to benefit from ongoing maintenance.
 2. **Validation**: Use allowed and allowedHelp to provide built-in validation for your options. This forces the user into a "correct usage" pattern before your logic even runs, reducing the need for manual sanity checks inside your run() methods.
 3. **Command Pattern**: For the **Redeye Protocol** or your security scanning tools, the CommandRunner pattern is superior to a flat ArgParser. It allows you to logically separate your tool's capabilities (e.g., redeye scan, redeye audit, redeye report) and keeps the codebase maintainable as you add more security modules.
 4. **Security & Reliability**:
   * **Strictness**: When creating your parser, remember allowTrailingOptions: false if you want to stop parsing as soon as a positional argument is encountered; this is often safer for security tools that pass raw data to other binaries.
   * **Global Arguments**: Always utilize runner.argParser for global flags (like --verbose or --config) so that these settings persist across all your sub-commands.
### Quick Audit Checklist for your CLI Tools
 * [ ] **Help Documentation**: Have you defined help: and valueHelp: for all flags/options to ensure the auto-generated help is useful?
 * [ ] **Error Handling**: Is your main() method wrapping runner.run() in a catchError that handles UsageException and prints the exit code 64 (standard for CLI usage errors)?
 * [ ] **Command Structure**: Does each Command subclass define its own specific ArgParser in the constructor to keep flag namespaces clean?
Are you currently building a new command-line interface for your vulnerability scanning or risk assessment engine, or are you refactoring an existing tool to move toward this CommandRunner structure?
