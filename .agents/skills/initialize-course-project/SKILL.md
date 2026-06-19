name: initialize-course-project
description: initialize the course project prior to adding content
---

Configure the course project by following these steps:

0. **Verify initialization hasn't been run**
   - Check if `course-init.sh` exists
   - If the script doesn't exist, stop and inform the user that the course has already been initialized
   - If it exists, proceed with the following steps

1. **Configure course type and lab environment**
   - Invoke `bash course-init.sh` without arguments and parse its USAGE message
   - Ask user which values to use for the arguments
   - Ask user the course title
   - Invoke `bash course-init.sh` using the values provided by the user for arguments
   - Edit the files indicated by the output of the script to set the course title
   - Also edit `modules/ROOT/pages/index.adoc` to include the course title

2. **Remove unnecessary files**
   - Remove all files from `modules/LABENV/pages` except for `index.adoc`
   - Remove the `modules.bfx` directory
   - Remove `course-init.sh`

3. **Install Node dependencies**
   - Run `npm` to install dependencies
     ```
     npm install
     ```

4. **Build the course book**
   - Run `npm run build` to verify initialization is correct
   - If there are no errors, proceed to the next step
   - If it fails, offer to discard all local changes and restore the project from its git remote and stop

5. **Offer to start watch mode**
   - Ask the user if they want to start watch mode
   - If yes, start both `npm run watch:adoc` and `npm run serve` as background tasks
   - Inform the user that watch mode is active and the course can be previewed in a browser


