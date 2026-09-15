# Import

![Import tab, before starting a project](../assets/screenshots/compart-import.png)

Starts a new COMPART project using a hull already imported in the **HULL** tab as the
outer shell. This is the only sub-tab available before a project exists.

Click **New from HULL** (disabled with a hint if no hull has been imported yet). If HULL
has more than one named geometry, a picker of secondary buttons appears so you can choose
which one to use; with just one, it proceeds immediately.

This:

- Imports the chosen hull as a mesh named **"Hull"**.
- Snapshots the hull's current station/waterline/buttock lines and its AP/FP positions
  into the COMPART project (visible read-only under the **Lines** tab of the Mesh List).
- Initializes the compartment grid used by [Modeling](modeling.md)'s script `xsplit`/
  `ysplit`/`zsplit`/`grid`/`cell` commands.

If a COMPART project already exists, **New from HULL** first asks you to confirm — it
discards everything in the current project.

![After importing a hull](../assets/screenshots/compart-modeling-box-created.png)
