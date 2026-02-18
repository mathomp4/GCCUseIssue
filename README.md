# GCC Use Issue Reproducer

## Requirements

To run this reproducer you will need:

- A Fortran compiler (e.g. `gfortran`)
- Python 3
- Jinja2 (Python package)
- CMake 3.20 or newer

## Creating a reproducible test case

To create a reproducible test case, use the `create_reproducer.py` script:
```bash
usage: create_reproducer.py [-h] [--max-modules MAX_MODULES] [--num-subs NUM_SUBS] [--random-names]

Create Fortran modules for testing

options:
  -h, --help            show this help message and exit
  --max-modules MAX_MODULES
                        Maximum number of modules to create (must be a multiple of 10) (default: 50)
  --num-subs NUM_SUBS   Number of subroutines per module (default: 10)
  --random-names        Use random module names (default: False)
```

As can be seen it has three options:
- `--max-modules`: Maximum number of modules to create (must be a multiple of 10)
- `--num-subs`: Number of subroutines per module
- `--random-names`: Use random module names

The script will create a directory with the following structure:
```
Modules_{max-modules}-Subs_{num-subs}/
```

In that directory will be a series of directories:
```
Modules_10
Modules_20
...
Modules_{max-modules}
```
and a `build.sh` script that builds and will output the timings. These will be on screen
and in a file called `build_times.txt`

## Results

These were constructed with:
```bash
./create_reproducer.py --max-modules 50 --num-subs NUM --random-names
```
where `NUM` is the number of subroutines per module (200, 500, or 1000).

### IFX 2025.3

| Num Mods in base.F90 | 200 Mods/Mod | 500 Mods/Mod | 1000 Mods/Mod |
|----------------------|--------------|--------------|---------------|
| 10                   | 0.0768018    | 0.094695     | 0.125834      |
| 20                   | 0.0877472    | 0.139161     | 0.178307      |
| 30                   | 0.1          | 0.152556     | 0.238049      |
| 40                   | 0.128098     | 0.220568     | 0.298084      |
| 50                   | 0.125313     | 0.217045     | 0.444799      |

### NAG 7.2.36

| Num Mods in base.F90 | 200 Mods/Mod | 500 Mods/Mod | 1000 Mods/Mod |
|----------------------|--------------|--------------|---------------|
| 10                   | 0.0767738    | 0.114828     | 0.192035      |
| 20                   | 0.0958743    | 0.183339     | 0.319912      |
| 30                   | 0.127438     | 0.271527     | 0.457094      |
| 40                   | 0.163695     | 0.325619     | 0.587367      |
| 50                   | 0.188272     | 0.396058     | 0.73524       |

### GCC 15.2.0

| Num Mods in base.F90 | 200 Mods/Mod | 500 Mods/Mod | 1000 Mods/Mod |
|----------------------|--------------|--------------|---------------|
| 10                   | 0.13394      | 0.486648     | 1.65297       |
| 20                   | 0.367937     | 1.95448      | 6.37326       |
| 30                   | 0.728392     | 4.2855       | 15.6758       |
| 40                   | 1.28305      | 6.63747      | 27.6166       |
| 50                   | 2.13779      | 11.6106      | 40.6979       |

### Flang 22.1.0-rc.3

| Num Mods in base.F90 | 200 Mods/Mod | 500 Mods/Mod | 1000 Mods/Mod |
|----------------------|--------------|--------------|---------------|
| 10                   | 0.402891     | 0.94408      | 1.84692       |
| 20                   | 0.759075     | 1.81249      | 3.6862        |
| 30                   | 1.1175       | 2.7201       | 5.37281       |
| 40                   | 1.28407      | 3.59948      | 7.1061        |
| 50                   | 1.99816      | 4.53051      | 8.9507        |

