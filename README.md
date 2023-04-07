# pgzero-example

The [pgzero] example. Build executable file use [pyinstaller].

## Run hello.py

```sh
# If no python, install first https://www.python.org/downloads
# Install pgzero package
pip install pgzero

# Run hello.py
python hello.py
# or
pgzrun hello.py
```

## Build executable file

 ```sh
# Install pyinstaller pckage tools
pip install pyinstaller

 # Build executable file to the dist/
pyinstaller --onefile hello.py
 ```

## Running error

```sh
./dist/hello
    FileNotFoundError: [Errno 2] No such file or directory: '.../pgzero/data/icon.png'
```

### Solution 1: Remove `show_default_icon()` in pgzero/game.py

```sh
vi `python -c "import os, pgzero; print(os.path.join(os.path.dirname(pgzero.__file__), 'game.py'))"`
    def show_default_icon():
        """Show a default icon loaded from Pygame Zero resources."""
        # from io import BytesIO
        # from pkgutil import get_data
        # buf = BytesIO(get_data(__name__, 'data/icon.png'))
        # pygame.display.set_icon(pygame.image.load(buf))
```

2. Build package

```sh
pyinstaller --onefile --windowed hello.py
```

### Solution 2 Add icon file for pgzero/game.py

```sh
# Generate game.spec use console
pyi-makespec --onefile hello.py
# Generate game.spec use no-console
pyi-makespec --onefile --windowed hello.py

# Copy icon.png from pgzero library
mkdir -p pgzero/data
cp `python -c "import os, pgzero; print(os.path.join(os.path.dirname(pgzero.__file__), 'data/icon.png'))"` pgzero/data

# Config `datas` direcory
# sed -i "s/datas=\[\]/datas=\[\('pgzero', 'pgzero'\)\]/g" hello.spec
vi hello.spec
    # datas=[]
    datas=[('pgzero', 'pgzero')]

# Build executable file by config file
pyinstaller game.spec
```

## References

- [pgzero]
- [pygame]
- [pyinstaller]

[pgzero]: https://github.com/lordmauve/pgzero
[pygame]: https://github.com/pygame/pygame
[pyinstaller]: https://github.com/pyinstaller/pyinstaller
