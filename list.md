# List CLI Utility

## Create command line interface (CLI) utility to list files. Name it `list`.

### Requirements:

- By default `list` should display list of files in current directory.

    ```shell
    ❯ list
    docker-compose.yml
    Dockerfile
    Dockerfile.prod
    entrypoint.sh
    Makefile
    manage.py
    Pipfile
    Pipfile.lock
    project
    pyproject.toml
    README.md
    release.sh
    ```

- `list` should accept argument path to a directory which content to display (it might be last argument).

    ```shell
    ❯ list project/
    __init__.py  api  config.py  db  tests
    ```

- `list` should accept CLI the following options:

     - -l, --list: display files in simple list (default behaviour)

     - -r, --reverse: display files in revers order

        ```shell
        ❯ list --reverse
        release.sh
        README.md
        pyproject.toml
        project
        Pipfile.lock
        Pipfile
        manage.py
        Makefile
        entrypoint.sh
        Dockerfile.prod
        Dockerfile
        docker-compose.yml
        ```

     - -d, --dirs: display directories with trailing slach '/' (like `project/`):

        ```shell
        ❯ list -d
        docker-compose.yml
        Dockerfile
        Dockerfile.prod
        entrypoint.sh
        Makefile
        manage.py
        Pipfile
        Pipfile.lock
        project/
        pyproject.toml
        README.md
        release.sh
        ```

     - -g, --grid: display files in grid maximizing horizontal space (quite complicated leave it for later):

        ```shell
        ❯ list -g
        docker-compose.yml  Dockerfile.prod  Makefile   Pipfile       project         README.md
        Dockerfile          entrypoint.sh    manage.py  Pipfile.lock  pyproject.toml  release.sh
        ```

- options should be composible:

     - more simple, support composition of long options:

        ```shell
        ❯ list --dirs --reverse
        release.sh
        README.md
        pyproject.toml
        project/
        Pipfile.lock
        Pipfile
        manage.py
        Makefile
        entrypoint.sh
        Dockerfile.prod
        Dockerfile
        docker-compose.yml
        ```

     - more complicated, support composition of short options:
        ```shell
        ❯ list -dr
        release.sh
        README.md
        pyproject.toml
        project/
        Pipfile.lock
        Pipfile
        manage.py
        Makefile
        entrypoint.sh
        Dockerfile.prod
        Dockerfile
        docker-compose.yml
        ```

### Hints

For implementation of grid display you need to know how wide is terminal window. Use `shutil` modules for this:

```python
>>> import shutil
>>> term_size = shutil.get_terminal_size()
>>> term_size
os.terminal_size(columns=120, lines=95)
```
`term_size.columns` gives you number of characters terminal is able to display at the moment.

To get access for arguments you can use `sys.argv`:

`main.py`:
```python
import sys
print(sys.argv)
```

```shell
python main.py --grid
['main.py', '--grid']
```

More advenced arguments parsing can be done with https://docs.python.org/3/library/argparse.html

To list directory content use `os.listdir`.

To check if it's directory use `os.path.isdir`.

