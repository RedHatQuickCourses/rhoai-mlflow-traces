---
name: generate-course-structure
description: generate the course structure from a course design document.
---

Generate a structured Antora course from the course design document by following these steps:

## Prerequisites Check

0. **Verify directory structure**
   - Check that `modules/ROOT/` exists
   - Check that `templates/` directory exists
   - If missing critical directories, inform the user and stop

1. **Verify course design document exists**
   - Check that `prompts/course-design.md` exists
   - If missing, inform the user and stop
   - If watch mode is active, stop its background tasks.

2. **Parse the course design table**
   - Locate the "COURSE DESIGN" heading
   - Extract the markdown table that follows
   - Table structure:
     * Learning objective rows: Span both columns, text format "LEARNING OBJECTIVE #N: Description"
     * Section rows: Two columns - first contains description, second contains session type
   - Parse algorithm:
     * If first column starts with "LEARNING OBJECTIVE #", this is a learning objective header
     * Otherwise, it's a section row belonging to the most recent learning objective
   - If table format is unexpected, show a sample and ask the user for clarification

3. **Create course index page**
   - File: `modules/ROOT/pages/index.adoc`
   - Read `templates/ROOT.adoc`
   - Map content from course-design.md to template:
     * Course title: Extract from H1 heading (first heading after image)
     * Course goal: Content under "# COURSE GOAL" heading
     * Audience: Content under "# TARGET AUDIENCE" heading
     * Prerequisites: Content under "# PREREQUISITES" heading
     * Learning objectives: All learning objective titles from the table (without the "LEARNING OBJECTIVE #N:" prefix)
   - Replace template placeholders with extracted content

4. **Check for existing modules**
   - List current `modules/ch*` directories
   - Ask user if they want to keep existing modules or start fresh
   - Store user's choice for cleanup steps (steps 11 and 13)

## Chapter Creation

For each learning objective in the table:

5. **Create chapter directory**
   - Extract the learning objective number (e.g., "LEARNING OBJECTIVE #2" → 2)
   - Extract the main keyword from the learning objective title using keyword extraction algorithm (see below)
   - Determine zero-padding:
     * If total learning objectives >= 10: use 2-digit padding (ch01, ch02, ..., ch10)
     * If total learning objectives < 10: use single digit (ch1, ch2, ch3)
   - Create directory: `modules/ch{number}-{keyword}/`
   - Create subdirectories: `pages/` and `images/`

6. **Create chapter index page**
   - File: `modules/ch{number}-{keyword}/pages/index.adoc`
   - Read `templates/index.adoc`
   - Title: Generate a short, objective, engaging title from the learning objective (without the "LEARNING OBJECTIVE #N:" prefix)
   - Fill in the learning objective text in the Goal:: definition list

## Section Creation

For each section under a learning objective:

7. **Determine section properties**
   - Section number (sequential within the chapter, starting from 1)
   - Session type: Extract from the second column (Presentation, Quiz, Lab, Lecture, etc.)
   - Suffix mapping:
     - Lecture → `.adoc` (no suffix)
     - Presentation → `.adoc` (no suffix)
     - Quiz → `-quiz.adoc`
     - Lab → `-lab.adoc`
     - Default → `.adoc` (no suffix, inform user of unknown type)
   - Keyword extraction algorithm:
     * Take the first paragraph or clause from the section description
     * Remove any leading bullet points or list markers
     * Identify 1-3 key nouns or noun phrases (e.g., "device fleets", "gitops", "quadlets")
     * Convert to lowercase kebab-case (e.g., "device-fleets", "gitops", "quadlets")
     * If multiple concepts, choose the most specific or primary one
     * Ensure uniqueness within the chapter by appending number if needed (e.g., "config-1", "config-2")
     * Use different keywords for sections in the same chapter
     * It's OK to use the same keyword for sections from different chapters

8. **Create section file**
   - File: `modules/ch{number}-{keyword}/pages/s{section-number}-{section-keyword}{suffix}`
   - Title: Generate from the section description, ignoring bullets
   - Section objective: Extract from first paragraph of description
   - Append section description as an AsciiDoc comment block at the end:
     ```
     ////
     {full section description from course design table}
     ////
     ```
   - Generate "What's next" content:
     * If not the last section in chapter:
       - Read the next section's description from the table
       - Generate 1-2 sentences connecting current section to next section
       - Focus on learning progression (e.g., "Now that you understand X, the next section explores Y...")
     * If last section in chapter but not last chapter:
       - Read the next chapter's learning objective
       - Generate 1-2 sentences about transitioning to the new topic
       - Emphasize how current chapter enables next chapter
     * If final section of course:
       - Generate brief conclusion (1-2 sentences)
       - Acknowledge completion and reinforce main course goal

9. **Apply templates (if available)**
   - Check if `templates/` directory exists
   - Template-to-section-type mapping:
     * Lecture/Presentation → `templates/presentation.adoc`
     * Lab → `templates/lab.adoc`
     * Quiz → `templates/quiz.adoc`
   - If template exists:
     * Read the template
     * Replace placeholders:
       - Title (first = heading)
       - Section objective (from Objective:: definition list)
       - Time estimate (keep default or estimate based on content type)
       - "What's next" content (in the What's Next section)
     * Keep template structure intact
     * Append section description as AsciiDoc comment at the end
   - If no template exists:
     * Create minimal structure with title and objective
     * Add section description from table as AsciiDoc comment block at the end
     * Inform user which templates were missing

## Navigation Updates

10. **Create/update chapter navigation**
   - File: `modules/ch{number}-{keyword}/nav.adoc`
   - Format:
     ```
     * xref:index.adoc[]
     ** xref:s1-{keyword}{suffix}[]
     ** xref:s2-{keyword}{suffix}[]
     ...
     ```
   - Use actual generated filenames
   - Verify all section files exist before adding to nav.adoc

11. **Update root navigation**
    - File: `antora.yml`
    - Add entries for new chapters in order
    - Preserve existing ROOT, LABENV, and appendix references
    - Format: `* xref:ch{number}-{keyword}:index.adoc[]`
    - Place new chapter entries after LABENV and before appendix entries
    - Remove entries from chapters that are not in the course design, for example `chapter1`
    - DO NOT change `modules/ROOT/nav.adoc` or `modules/LABENV/nav.adoc`
    - if user confirmed starting fresh in step 4, remove entries from chapters that are not in the course design, for example `chapter1`

## Validation

12. **Verify structure**
    - Validate each created file:
      * Check that all .adoc files have valid AsciiDoc syntax (at minimum, one = heading)
      * Verify nav.adoc xref targets point to existing files
      * Confirm all template placeholders were replaced
    - Check navigation consistency:
      * Every chapter index.adoc should be referenced in `antora.yml`
      * Every section should be referenced in its chapter's nav.adoc
    - Report summary to user:
      * Number of chapters created
      * Number of sections per chapter (broken down by type: Lab, Quiz, Presentation, etc.)
      * List of any missing templates that were needed
      * Any issues encountered during generation
      * Any edge cases that required special handling
      * Total files created

13. **Cleanup**
    - Only perform cleanup if user confirmed starting fresh in step 4
    - Remove example directories if they exist and new chapters were created, for example:
      * `modules/chapter1/`
      * `modules/chapter2/`
      * `modules/chapter3/`
      * `modules/appendix/`
    - Remove example files from all modules if they exist and new sections were created, for example:
      * `section1*.adoc`
      * `section2*.adoc`
      * `section3*.adoc`

14. **Offer to start watch mode**
   - Ask the user if they want to start watch mode
   - If yes, start both `npm run watch:adoc` and `npm run serve` as background tasks
   - Inform the user that watch mode is active and the course can be previewed in a browser

## Edge Cases

Handle the following edge cases gracefully:

- **Single learning objective**: Create ch1-{keyword} normally 
- **Learning objective with no sections**: Create chapter with index.adoc only, add note to user
- **Unknown session type**: Use default `.adoc` suffix, inform user of unknown type and log it in the summary
- **Duplicate keywords in same chapter**: Append sequential numbers (-1, -2, etc.) or derive more specific keywords
- **Empty section descriptions**: Use "section-{number}" as keyword, warn user about empty description
- **Malformed table**: Show sample rows to user, ask for clarification or manual fix before proceeding
- **Missing template directory**: Create basic structure without templates, inform user
- **Failed file creation**: Report the error immediately, ask user if they want to continue or stop to avoid partial state

## Important Notes

- **Naming conventions**:
  - Chapters: `ch{N}-{keyword}` where:
    * N is the learning objective number
    * N is zero-padded to 2 digits if there are 10+ chapters (e.g., ch01, ch02, ..., ch10)
    * N is single digit if fewer than 10 chapters (e.g., ch1, ch2, ch3)
  - Sections: `s{N}-{keyword}{suffix}` where:
    * N is the section number within the chapter (always single digit or two digits as needed)
    * suffix is based on session type (quiz, lab, or no suffix)
  - Keywords: lowercase, hyphen-separated, derived from main concept

- **Content quality**:
  - Titles should be concise (<60 characters)
  - Titles and objectives should not paraphrase each other
  - Use active, engaging language
  - "What's next" content should create logical learning flow

- **Error handling**:
  - If parsing fails at any step, show the user the problematic content and ask for guidance
  - If a template is missing, create a basic structure and inform the user
  - If file creation fails, report the error and ask whether to continue or stop to avoid partial state
  - Always validate before writing to ensure consistency

- **Idempotency**:
  - Running this skill multiple times should produce the same result
  - Always ask before overwriting existing content
  - Preserve custom modifications where possible
