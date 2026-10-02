# Command Line

SySHIP can be driven from the command line with the `syship` command. Use it to automate repeated work (for example, running the same loading and stability check on many projects), to control SySHIP from your own programs such as Python scripts, or to let an AI agent operate SySHIP.

Every command works on a running SySHIP:

- **With the window open**, each command shows up in the window as it runs: input fields change, the 3D view redraws, and long calculations show their progress. You can watch, take over at any time, or undo.
- **Without a window** (headless), SySHIP runs in the background and only calculates. This is the fastest way to process many cases.

## Where to find it

| System | Path |
|---|---|
| Windows | `<install folder>\resources\cli\syship.exe`, by default `%LOCALAPPDATA%\Programs\SySHIP\resources\cli\syship.exe` |
| macOS | `/Applications/SySHIP.app/Contents/Resources/cli/syship` |

On Windows the installer adds this folder to your user `PATH` (and removes it when you uninstall), so
`syship` works in any terminal opened after installing. On macOS choose **Help → Install 'syship'
Command in PATH** once; it links the command into `/usr/local/bin` (it asks for your password).
**Help → Uninstall 'syship' Command** removes the link.

## Getting started

```sh
syship status                 # which SySHIP is connected
syship list                   # every command
syship describe compart.loading.set   # parameters of one command
```

If the SySHIP window isn't open, start a background instance first, and stop it when you are done:

```sh
syship serve --headless
syship app.shutdown
```

Adding `--start` to any command starts a background instance automatically when none is running.

## Writing commands

- Commands are named `<stage>.<target>.<action>`, for example `compart.stability.gz`.
- Parameters are given as `--name value`. Lists can repeat the flag (`--meshes A --meshes B`) or use commas for numbers (`--translate 0,0,25`).
- Values use the same units as the window: lengths in m, weights in t, angles in degrees, density in t/m³.
- Compartments are named as in the Mesh List. A name can use `*` and `?` as wildcards, for example `--meshes "W.B. TK*"`.
- Commands that overwrite a file or discard work (opening another project, deleting meshes) ask for `--yes`.
- Most changes can be undone with `syship compart.history.undo`, or with **Undo** in the window.

## Example: preparing a hull

These are the same steps as the HULL Import and Variation tabs:

```sh
syship hull.import --path kvlcc.stl
syship hull.transform --scale 1,1,1
syship hull.stations.create --ap 0 --fp 320
syship hull.hydrostatics --draft 20.8
syship hull.lines.create --type waterline --positions "2:28:2"
syship hull.lines.create --type buttock --positions "4, 8, 12, 16, 20"
syship hull.variation.length --targetLbp 330 --reference mid
syship hull.variation.cp --targetCb 0.82 --targetLcb 10
syship hull.project.save --path hull_v2.zip
syship compart.project.new --yes
```

Positions use the same syntax as the window: a list such as `1, 2, 2.5`, or `start:end:spacing` such as `2:28:2`. After `hull.stations.create` the hull is moved so that A.P. is at x = 0, and x positions in later commands are measured from A.P. `compart.project.new` starts a COMPART project from the finished hull, like **New from HULL**.

## Example: dividing compartments

The Create and Operation panels have matching commands, so a compartment model can be built one step at a time without a modeling script.

```sh
syship compart.mesh.plane --axis x --position 51 --name er_bhd
syship compart.mesh.skinsurf --name inner_bhd --direction x --input inner_bhd.json
syship compart.mesh.box --name box1 --min 45.9,6.37,9.624 --max 51,35,35
syship compart.mesh.split --targets Hull --cutters er_bhd
syship compart.mesh.update --meshes "Hull (1)" --name Stern
syship compart.mesh.union --meshes Hull --meshes Cap --name Hull
syship compart.mesh.delete --meshes er_bhd --yes
syship compart.layer.create --name "Cargo Tank"
syship compart.layer.move --meshes "*C.O. TK*" --layer "Cargo Tank"
syship compart.layer.list
```

`compart.mesh.split` lists every piece with its center and bounds in m, so you can tell which piece is which before you rename it. Pieces are named `<name> (1)`, `<name> (2)`, ... and the cutters are kept. `compart.mesh.subtract` removes the cutters, and `compart.mesh.union` and `compart.mesh.intersect` replace their inputs with the result. A skinsurf takes its profiles as `{"profiles": [{"position": 51, "points": [[6.37, 35], [6.37, 3]]}, ...]}`, with the points in m on the plane of each profile.

## Example: loading and stability check

```sh
syship compart.project.load --path ship.zip --yes
syship compart.category.assign --meshes "*C.O. TK*" --category "Crude oil"
syship compart.loading.set --meshes "*C.O. TK*" --fillPercent 98
syship compart.loading.set --meshes "W.B. TK*" --weight 12000
syship compart.lightship.set --weight 40000 --lcg 150 --tcg 0 --vcg 17
syship compart.stability.gz --heelStepDeg 1
syship compart.stability.evaluate
syship compart.project.save --path ship_loaded.zip
```

`compart.stability.evaluate` prints each condition of the [Stability criteria](compart/stability.md) with its status and values.

## Running a list of commands

Save the steps in a JSON file and run them in order with `syship run steps.json`. It stops at the first step that fails and tells you which one.

```json
[
  {"command": "compart.project.load", "args": {"path": "ship.zip"}, "yes": true},
  {"command": "compart.loading.set", "args": {"meshes": ["*C.O. TK*"], "fillPercent": 98}},
  {"command": "compart.stability.gz", "args": {"heelStepDeg": 2}},
  {"command": "compart.stability.evaluate"}
]
```

## Using it from Python

Add `--json` to get one line of JSON that a program can read:

```python
import json, subprocess

out = subprocess.run(["syship", "compart.stability.gz", "--includePoints", "false", "--json"],
                     capture_output=True, text=True, encoding="utf-8")
result = json.loads(out.stdout)
if result["ok"]:
    print(result["data"]["variables"]["GM0"])
else:
    print(result["error"]["message"], result["error"].get("hint"))
```

The exit code is `0` on success, `1` when the command failed, `2` for a wrong command, wrong parameters or a missing `--yes`, and `3` when no SySHIP is running.

## Window-only commands

These need the SySHIP window: `app.screenshot --path view.png`, `app.stage.open`, `hull.tab.open`, `hull.view.layers`, `hull.cp-curve.open`, `compart.tab.open`, `compart.view.camera`, `compart.view.select`, `compart.view.equilibrium` and `compart.stability.criteria.open`.

## Using it with an AI agent

An AI agent that can run shell commands can use SySHIP on its own: it finds commands with `syship list` and `syship describe`, runs them, and checks the result with read commands (`compart.project.summary`, `compart.loading.get`, `compart.stability.last`) or with `app.screenshot`. With the window open you see every step the agent takes.

!!! note "Coverage"
    The command line currently covers HULL (import, variations, export, projects) and COMPART (projects, modeling commands and scripts, layers, categories and compartment factors, loading, damage and stability). COMPART Hydro and DIMENSION commands will follow.
