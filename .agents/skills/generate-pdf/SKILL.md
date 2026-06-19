---
name: generate-pdf
description: Generates, or exports, the entire course contents as a single, unstyled PDF file.
---

Follow these steps to generate and preview the course PDF:

1. **Check and install asciidoctor-pdf gem**
   - First check if it's already installed:
     ```
     gem list asciidoctor-pdf
     ```
   - If not installed, install it:
     ```
     gem install asciidoctor-pdf
     ```
   - If the gem installation fails, inform the user they may need to install Ruby or check permissions.

2. **Check and install Antora PDF extension**
   - Check if the package is already in package.json dependencies or node_modules:
     ```
     npm list @antora/pdf-extension
     ```
   - If not installed, install it:
     ```
     npm i @antora/pdf-extension
     ```
   - If npm installation fails, inform the user and check if package.json exists.

3. **Run the Antora build with PDF extension**
   - Execute the build command:
     ```
     npx antora --extension @antora/pdf-extension antora-playbook.yml
     ```
   - If this fails, check the error output and report it to the user.
   - Common issues: missing antora-playbook.yml, configuration errors, or missing content.

4. **Locate the generated PDF file**
   - Find the generated PDF using:
     ```
     ls build/assembler-pdf/*/*/_exports/index.pdf
     ```
   - Verify the file exists before attempting to open it.
   - If no PDF is found, inform the user that the build may have failed or the output path may be different.

5. **Open the PDF for preview**
   - Use the appropriate command based on the platform:
     - **Linux**: `xdg-open <pdf-path>` or `evince <pdf-path>`
     - **macOS**: `open <pdf-path>`
     - **Windows**: `start <pdf-path>`
   - Replace `<pdf-path>` with the actual path found in step 4.
   - Inform the user where the PDF is located so they can open it manually if the command fails.

6. **Display the PDF file path**
   - Display the path found in step 4 for the user, so they can re-open the file, email it, or perform other tasks after previewing.

**Important notes:**
- Always verify each step succeeded before proceeding to the next step.
- If any command fails, provide the error message to the user and suggest troubleshooting steps.
- The PDF will be located at a path like `build/assembler-pdf/<component-name>/<version>/_exports/index.pdf`.
