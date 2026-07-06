Rules for consistency of written prose and asciidoc tagging in lab instructions.

== The following rules apply operation of web interfaces and web forms:

- Refer to combo boxes, check boxes, and text boxes as fields, ex: `The menu:Project[]` field.

- If you can click it, tag with the `menu:` macro. This includes links, combo boxes, rows in a table, and buttons.

- Tag names of form fields as bold, if they are NOT clickable, ex: `the *Name* field`.
If the field is clickable, such as a combo box or check box, tag with the `menu:` macro, ex: `the menu:Project[] field`

- Tag names of panes, pages, and categories/headings inside a page as bold, ex: `The *Configure* pane`.

- Use "select" for menu items, navigation items, check boxes, and tabs. Use "click" for buttons, links, menus, and icons.

- Use "In the pane" instead of "On the pane".

- When you refer to names of Kubernetes resources in sentences, tag them as code, ex: "the `my-first-model` project. If these names appear as names of pages, also tag them as code instead of as bold. ex: "The *Experiments* for the `my-first-model` project.

- Do not tag Kubernetes resource types as code. Use them as names of common concepts in prose.

- When names of resources appear as clickable, such as in lists and tables, tag them with `menu:` ex: `In the menu:Project[] field, select the menu:my-first-model[] project`.

- Format tooltipis using quotes, ex: the menu:fast forward[] icon, having as tooltip "Restart the kernel and run all cells"

- Format icon names using `menu:`.

- Names of resources, when used in their respective details or edit pages, are NOT clickable, and are formatted as code, ex: "The `default-dsc` resource details page".

- The RHOAI dashboard uses sentence case for menu and navigation items, for example "Gen AI studio". Use the same capitalization when referring to them as names of features in prose.

- The OpenShift web console uses either title case or camel case for menu and navitgation items, ex: "Installed Operators" and "StatefulSets"

== The following rules apply to prose, anywhere

- "MLflow" must be capitalized with uppercase ML and lowercase flow, except in links and code blocks.

- Use active voice.

- Use short sentences, and avoid long paragraphs (containing more than five sentences).

- Format paragraphs using a single sentence per line.

- Use sentence case for headings.

- Do not use contractions such as don't and can't.

- Expand acronyms on first usage, except if first usage is in a heading or code block. For example: "Red Hat OpenShift AI (RHOAI)"

- Use "GenAI" instead of "genAI" or "gen AI" for "generative AI"

- Images must provide a caption, using the attribute, ex: `::image:file.svg]title="this iamge caption"]`

- Links to external web sites use "^" to open an external browser tab, ex: `http://www.example.com[example web site^]`
