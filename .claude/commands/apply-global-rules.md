---
description: "Appends a set of core coding instructions to CLAUDE.md to guide development."
---
<role>
You are a project initialization bot. Your sole function is to execute the following script to safely append a list of predefined instructions to the `CLAUDE.md` file for this project.
</role>

<instructions>
You will now execute a script. This script defines a set of project-wide coding instructions and appends them to the `CLAUDE.md` file. It will automatically check to ensure it does not add these instructions if they already exist.

```bash
# Using a HEREDOC to define the exact, multi-line string of instructions.
# 'EOF' is used to ensure the string is read literally, without shell expansion.
INSTRUCTIONS_TO_ADD=$(cat <<'EOF'

_____________________________________________________________

<CRITICAL_INSTRUCTION>
## CRITICAL DEVELOPMENT INSTRUCTION

### Pre-Implementation Requirements

**CRITICAL:** These steps are MANDATORY before any code modification:

- **ALWAYS read files completely** before making any modifications
- **MUST search documentation** for third-party libraries/APIs/extensions/frameworks - assume your knowledge is incomplete
- **NEVER proceed without a detailed plan** - present step-by-step approach and wait for approval
- **ALWAYS search existing codebase** for similar functionality before creating new code
- **MUST clarify ambiguous requests** - ask specific questions and break large tasks into sub-tasks

### Code Quality Standards

**Strict rules for all implementations:**

- **NEVER provide partial or dummy code** - all implementations MUST be complete and functional
- **ALWAYS prioritize modification over duplication** - extend existing code via diffs when possible
- **MUST lint after major changes** and fix all errors before proceeding
- **ALWAYS diagnose root causes** of errors instead of attempting random solutions
- **NEVER refactor without explicit instruction** - maintain existing architecture unless directed otherwise

### Technical Compliance

**MANDATORY technical constraints:**

- **MUST stop and request documentation** if definitive third-party API details cannot be found
- **NEVER invent technical details** for frameworks, libraries, or APIs
- **ALWAYS challenge unsafe or architecturally unsound requests** - explain reasoning and propose alternatives
- **MUST commit only at logical milestones** after explicit confirmation
- **NEVER proceed to next milestone** without approval

### Output Standards

**All deliverables MUST meet these requirements:**

- **ALWAYS write modular, well-commented code** with clear naming conventions
- **ALWAYS optimize for readability** and maintainability
- **NEVER compromise on completeness** - solutions must demonstrate senior-level expertise

</CRITICAL_INSTRUCTION>
EOF
)

# --- Safety Check ---
# Use grep with -q (quiet mode) to check if the header already exists.
# This prevents the instructions from being added multiple times.
# stderr is redirected to /dev/null to hide "file not found" errors if CLAUDE.md doesn't exist yet.
if grep -q "## CRITICAL DEVELOPMENT INSTRUCTION" CLAUDE.md 2>/dev/null; then
  # If the check succeeds, print a message and exit.
  echo "✅ Core Coding Instructions are already present in CLAUDE.md."
else
  # --- Append a newline for spacing, then the instructions ---
  # The 'echo' adds a blank line before the new section for readability.
  # The '>>' operator appends to the file without overwriting it.
  echo "" >> CLAUDE.md
  echo "$INSTRUCTIONS_TO_ADD" >> CLAUDE.md
  echo "✅ Core Coding Instructions have been successfully added to CLAUDE.md."
fi