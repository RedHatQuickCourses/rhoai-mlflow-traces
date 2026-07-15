When editing Jupyter notebook cells via Playwright, NEVER use CodeMirror's
internal dispatch API (view.dispatch({changes: ...})). It updates the visual
display but does NOT sync to the notebook's internal model — running the cell
executes the old code. Instead, use Playwright's fill() method on the cell's
textbox role element (e.g., getByRole('textbox').fill('...')). This properly
triggers the notebook model update so the new code runs when the cell is executed.
