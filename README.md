# uvenv
A global virtual environment management script for Python uv virtual environment management

由于uv 没有提供类似conda env 这样的功能，因此自己手动实现了一个。

使用方法: 
source uvenv.sh

或将上面一行代码添加到~/.bashrc


### Commands

-   **`list`:** List all created environments.
-   **`create <name> [options]`:** Create a new environment named `<name>`.  Any additional options are passed directly to `uv venv`.
-   **`delete <name>`  or  `del <name>`:** Delete the environment named `<name>`.
-   **`activate <name>`  or  `act <name>`:** Activate the environment named `<name>`.
-   **`deactivate`  or  `deact`:** Deactivate the currently active environment.
-   **`help`:** Display this help message.

### Options

-   **`name`:** The name of the virtual environment (required for `create` and `delete`).

## Examples

```bash
# Create a new environment named "my-project"
uvenv create my-project

# Create a new environment with a specific Python version (example, if uv supports it)
# uvenv create my-project -p python3.9  

# List all environments
uvenv list

# Activate the "my-project" environment
uvenv activate my-project
uvenv act my-project

# Deactivate the current environment
uvenv deactivate
uvenv deact

# Delete the "my-project" environment
uvenv delete my-project
uvenv del my-project
