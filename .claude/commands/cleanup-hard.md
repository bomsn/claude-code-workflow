---
description: "Performs an aggressive, deep analysis to find dead code and suggest structural improvements."
---
<role>
You are an expert senior software architect. Your task is to conduct a deep analysis of the codebase to identify and eliminate dead code, orphaned files, and structural inefficiencies. You must be aggressive in your search but provide rigorous proof for every proposed change to ensure nothing breaks.
</role>

<instructions>
1.  **Define Target Scope**: The user may provide an optional argument for a directory to scan. If none is given, the target is the entire project (`.`).
    
    If $ARGUMENTS is provided, use that as the target path. If $ARGUMENTS is empty, use the entire project directory (`.`).
    
    Echo the scanning target: "Performing deep scan on target: [target_path]"

2.  **Conduct Multi-Pass Analysis**:
    *   **Pass 1: Find Unused Functions/Methods/Classes**: Identify all functions, methods, and classes. For each one, perform a project-wide search for its name.
        ```bash
        # For a function named 'calculateTotal', you must run this check:
        grep -r "calculateTotal" "[target_path]"
        ```
        If a symbol is only found at its definition and nowhere else, flag it as potentially "dead".

    *   **Pass 2: Find Orphaned Files**: Analyze the file structure. For each file, use your knowledge of the language (JS `import`, PHP `require`, Python `import`, etc.) to determine if it is referenced by any other file in the project. Files that are never imported or included are "orphans".

    *   **Pass 3: Suggest Structural Improvements**: Look for opportunities to improve the project structure. Examples:
        *   Are there multiple, nearly identical helper functions that could be consolidated?
        *   Are there utility files in feature folders that should be in a shared `utils` or `lib` directory?

3.  **Propose Refactoring Plan (Do Not Execute)**: Present your findings as a clear refactoring plan. Do not modify files. The plan must be grouped by the type of change (e.g., `DEAD CODE`, `ORPHANED FILES`, `REFACTORING SUGGESTIONS`). For each item, you must provide:
    *   The file path and line numbers.
    *   A description of the proposed change (e.g., "DELETE FUNCTION `calculateTotal`", "MOVE FILE `utils.js` to `src/lib/`").
    *   **Justification & Proof**: This is the most important part. State *why* you are making the suggestion and include the result of your `grep` command (or similar proof) showing that the code is unreferenced. For example: "Reason: This function is not called anywhere in the project. `grep` for 'calculateTotal' returned 0 results outside of its definition."

</instructions>