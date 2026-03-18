# generate_fr3_controller.py

A code-generation script that scaffolds a new FR3 controller plugin from a single command.

## What it generates

| Output | Location |
|---|---|
| Header file | `include/fr3_controller/<snake_name>.hpp` |
| Source file | `src/<snake_name>.cpp` |

## What it patches

| File | Change |
|---|---|
| `CMakeLists.txt` | Adds `src/<snake_name>.cpp` to the `add_library` block |
| `fr3_plugin.xml` | Adds a `<class>` entry for the new plugin type |
| `config/fr3_ros_controllers.yaml` | Adds `left_<name>`, `right_<name>`, `dual_<name>` controller entries with the chosen `manipulator_command_interface` |

## Usage

```bash
cd fr3_controller
python3 generate_fr3_controller.py <ControllerName> --control_mode <mode> [--force]
```

### Arguments

| Argument | Required | Description |
|---|---|---|
| `name` | yes | Controller name in any style (e.g. `MyNewController`, `my_new_controller`). Converted to `snake_case` for files and `CamelCase` for the class. |
| `--control_mode` | yes | Command interface type: `position`, `velocity`, or `effort` |
| `--force` | no | Overwrite existing `.hpp` / `.cpp` files if they already exist |

### Examples

```bash
# Effort-mode controller
python3 generate_fr3_controller.py MyNewController --control_mode effort

# Position-mode controller, overwrite if exists
python3 generate_fr3_controller.py MyNewController --control_mode position --force
```

## After generation

1. Implement your control algorithm in the `update()` TODO block inside `src/<snake_name>.cpp`.
2. Rebuild the package:
   ```bash
   colcon build --packages-select fr3_controller
   ```
3. Launch with the new controller name:
   ```bash
   ros2 launch fr3_controller fr3_controller.launch.py \
     robot_side:=left controller_name:=<snake_name>
   ```

The script prints the generated plugin type and YAML controller names on completion:

```
Controller class : fr3_controller::<ClassName>
Plugin type      : fr3_controller/<ClassName>
YAML controllers : left_<snake_name>, right_<snake_name>, dual_<snake_name>
Control mode     : <mode>
```
