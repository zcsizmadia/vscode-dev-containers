# Python Install Script

*Installs Python, PIPX, and common Python utilities.*

**Script status**: Stable

**OS support**: Debian 9+, Ubuntu 16.04+, and downstream distros.

**Maintainer:** The VS Code and GitHub Codespaces teams

## Syntax

```text
 ./python-debian.sh [Python Version] [Install path] [PIPX_HOME] [Non-root user] [Update rc files flag] [Install tools] [Use Oryx if available] [Optimize when building from source]
```

|Argument|Default|Description|
|--------|-------|-----------|
|Python Version|`latest`| Version of Python to install. Set to `none` to skip installing.|
|Python install path|`/usr/local/python`| Location to install Python.|
|Non-root user|`automatic`| Specifies a user in the container other than root that will use Python. A value of `automatic` will cause the script to check for a user called `vscode`, then `node`, `codespace`, and finally a user with a UID of `1000` before falling back to `root`.|
|PIPX_HOME|`/usr/local/py-utils`| Location PIPX should install Python utilities and related venvs.|
| Add to rc files flag | `true` | A `true`/`false` flag that indicates whether sourcing the PIPX_HOME and bin path should be added to `/etc/bash.bashrc` and `/etc/zsh/zshrc`.|
|Install tools | `true` | A `true`/`false` flag that indicates whether related Python utilities should be installed.|
|Use Oryx if available | `true` | A `true`/`false` flag that indicates whether the Oryx CLI in images like the default codespaces image should be used if available (like in mcr.microsoft.com/vscode/devcontainers/universal).|
|Optimize when building from source| `false` | A `true`/`false` flag that indicates whether Python should be optimized for performance when built from source. Adds significant time to the build process.|


## Usage

1. Add [`python-debian.sh`](../python-debian.sh) to `.devcontainer/library-scripts`

2. Add the following to your `.devcontainer/Dockerfile`:

    ```Dockerfile
    ARG PYTHON_PATH=/usr/local/python
    ENV PIPX_HOME=/usr/local/py-utils \
        PIPX_BIN_DIR=/usr/local/py-utils/bin
    ENV PATH=${PYTHON_PATH}/bin:${PATH}:${PIPX_BIN_DIR}
    COPY .devcontainer/library-scripts/python-debian.sh /tmp/library-scripts/
    RUN apt-get update && bash /tmp/library-scripts/python-debian.sh "3.8.3" "${PYTHON_PATH}" "${PIPX_HOME}"
    ```

That's it!
