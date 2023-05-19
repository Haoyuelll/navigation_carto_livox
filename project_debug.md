# Common problems and solutions

## 1. No such file or directory: 'numpy/arrayobject.h'

### Problem description

Cpython cannot find header file `xxx.h`: (here we take numpy as example)

```bash
fatal error: numpy/arrayobject.h: No such file or directory
   19 | #  include <numpy/arrayobject.h>
```

### Solution

The problem occurs in `LiDAR_IMU_Init` package, you can add the following lines to your `src/LiDAR_IMU_Init/CMakeLists.txt`

```cmake
message(PYTHON_INCLUDE_DIRS=${PYTHON_INCLUDE_DIRS})
```

You may get path like: `/home/[username]/anaconda3/include/python3.9/`

Then use the command `ls` to check whether `numpy/arrayobject.h` under this directory.

If not, use this command to copy numpy header files to the $PYTHON_INCLUDE_DIRS:

 ```bash
cp -r $CONDA_PREFIX/lib/python3.*/site-packages/numpy/core/include/numpy $CONDA_PREFIX/include/python3.*/
 ```

If you meet similar problems with other packages, use 

```bash
ls -R $CONDA_PREFIX/lib/python3.*/site-packages/[your_package_name] | grep ".h$"
```

to search for header files in this package. Then copy the header directory to `$CONDA_PREFIX/include/python3.*/`

##### Explanation

numpy header files are stored in `[your_numpy_dir]/core/include/numpy` and is supposed to be read with  `setup(include_dirs=[numpy.get_include()])` in `setup.py` for Cpython.

However, this setup() may not exist in the project and resulted in missing header files.

 