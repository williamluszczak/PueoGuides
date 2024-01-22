# Root Installation Guide  
This tutorial focuses on building the necessary tools from source files in order to get root compiled (and hopefully working). This tutorial follows closely to the one provided to me by William Luszczak, however, I focus on setting up root on the virtual Ohio Super Computer environment. The OSC environment is a useful tool to access your workspace from anywhere, as long as you have internet.

To set up your OSC account go to https://ondemand.osc.edu and register for an account. You should use your name.#@buckeyemail.osu.edu. You'll have to reach out to your advisor to get added to a project. Once this is completed you should be able to login, navigate to *My Interactive Sessions -> Pitzer Desktop*. Fill out the number of hours you plan to use and the cores you'd like to request. I'd recommend giving yourself an hour additional to what you think you'd use. 

----

### CMake 3.21.2

The source file can be found here: https://cmake.org/download/.

I downloaded my source file to `/users/PAS0654/mjackson/pueosim/cmake`. 

Then untar your source file using 

`tar -xzvf cmake-3.22.1.tar.gz`

`mkdir install`

`cd cmake-3.22.1`

We want to configure our build to end up in our `install` directroy.

`./configure --prefix=/users/PAS0654/mjackson/pueosim/cmake/install` 

`make`

`make install`

After this is complete I recommend making a shell script to incorporate this new directory to your `$PATH` variable.

Create a new document and name it something like `load_env.sh` and then add the following text using your appropriate pwd:

`export PATH=/users/PAS0654/mjackson/pueosim/cmake/install/bin:$PATH`

Save this and then run `source load_env.sh`. 

To make sure this has worked correctly run `cmake --version`:

    cmake version 3.22.1

    CMake suite maintained and supported by Kitware (kitware.com/cmake).
 ---   
### gcc 11.1.0

The source file can be found here: https://github.com/gcc-mirror/gcc/tags.

Once again, make an appropriate directory and untar the source file.

`tar -xzvf gcc-releases-gcc-11.1.0.tar.gz`

`cd gcc-releases-gcc-11.1.0`

Then we need to download the prerequisites for gcc

`contrib/download_prerequisites`

After installation of prerequisites, follow a similar procedure to CMake:

`cd ../`

`mkdir build`

`cd build`

`../gcc-releases-gcc-11.1.0/configure -v --prefix=/users/PAS0654/mjackson/pueosim/gcc-11.1.0 --enable-checking=release --enable-languages=c,c++,fortran --disable-multilib --program-suffix=-11.1`

Once completed, compile gcc:

`make -j $(nproc)` 

`make install`

Here `$(nproc)` is the number of threads you want to use to compilate. I used 10 threads, and it took ~30mins or so. 

After gcc has been built successfully go ahead and open your `load_env.sh` file to edit and add more environment variables:

```
export PATH=/users/PAS0654/mjackson/pueosim/gcc-11.1.0/bin:$PATH

export LD_LIBRARY_PATH=/users/PAS0654/mjackson/pueosim/gcc-11.1.0/lib64:$LD_LIBRARY_PATH`

export CC=/users/PAS0654/mjackson/pueosim/gcc-11.1.0/bin/gcc-11.1

export CXX=/users/PAS0654/mjackson/pueosim/gcc-11.1.0/bin/g++-11.1

export FC=/users/PAS0654/mjackson/pueosim/gcc-11.1.0/bin/gfortran-11.1

```

Go ahead and run `source load_env.sh` and check that `gcc-11.1.0` has been properly installed:

`gcc-11.1 --version`

```
gcc-11.1 (GCC) 11.1.0

Copyright (C) 2021 Free Software Foundation, Inc.

This is free software; see the source for copying conditions.  There is NO

warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

```
---
### FFTW 3.3.9

The source file can be found here: http://www.fftw.org/download.html. 

It is important to not follow the given installation instructions on the website. Go ahead and untar your source file in an appropriate directory:

`tar -xzvf fftw-3.3.9.tar.gz`

The follow the following build instructions: 

`mkdir build`

`cd build`

`cmake -DCMAKE_INSTALL_PREFIX=/users/PAS0654/mjackson/pueosim/fftw/build/ -DBUILD_SHARED_LIBS=ON -DENABLE_OPENMP=ON -DENABLE_THREADS=ON ../fftw-3.39`

`make install -j $(nproc)`

Then once this is completed we will remove everything except a few directories and then we will rebuild with an additional flag.

`rm *` 

`rm -r CMakeFiles`

`cmake -DCMAKE_INSTALL_PREFIX=/users/PAS0654/mjackson/pueosim/fftw/build/ -DBUILD_SHARED_LIBS=ON -DENABLE_OPENMP=ON -DENABLE_THREADS=ON ../fftw-3.39`

`make install -j $(nproc)`

To double check, your fftw install directory should have the following files: 

```
 include/fftw3.f  
 include/fftw3.f03
 include/fftw3.h  
 include/fftw3l.f03  
 include/fftw3q.f03 
 lib64/libfftw3f.so          
 lib64/libfftw3f_threads.so.3      
 lib64/libfftw3_omp.so.3.6.9  
 lib64/libfftw3_threads.so
 lib64/libfftw3f_omp.so        
 lib64/libfftw3f.so.3        
 lib64/libfftw3f_threads.so.3.6.9  
 lib64/libfftw3.so            
 lib64/libfftw3_threads.so.3
 lib64/libfftw3f_omp.so.3      
 lib64/libfftw3f.so.3.6.9    
 lib64/libfftw3_omp.so             
 lib64/libfftw3.so.3          
 lib64/libfftw3_threads.so.3.6.9
 lib64/libfftw3f_omp.so.3.6.9  
 lib64/libfftw3f_threads.so  
 lib64/libfftw3_omp.so.3           
 lib64/libfftw3.so.3.6.9
```

Now, you guessed it, go ahead and edit your `load_env.sh` file to include:

`export FFTWDIR=/users/PAS0654/mjackson/pueosim/fftw/build`

---
### gsl 2.7.1

The source file can be found here: https://www.gnu.org/software/gsl/. 

 Go ahead and untar your source file in an appropriate directory and follow the installation instructions:
 
 `tar -xzvf gsl-latest.tar.gz`
 
 `mkdir build`
 
 `cd gsl-2.7.1`
 
 `./configure --prefix=/users/PAS0654/mjackson/pueosim/gsl/build`
 
 `make`
 
 `make check`
 
 `make install`
 
 After this has completed, go ahead and edit your `load_env.sh` to include the following:
 
```
export GSL_ROOT_DIR=/users/PAS0654/mjackson/pueosim/gsl/build/
export PATH=/users/PAS0654/mjackson/pueosim/gsl/build/bin:$PATH
export LD_LIBRARY_PATH=/users/PAS0654/mjackson/pueosim/gsl/build/lib:$LD_LIBRARY_PATH
```
 ---
 
## ROOT 6.24.00
 
It is finally time to install ROOT! 

The source file can be found here: https://root.cern/install/all_releases/.
 
Go ahead and untar the file in an appropriate directory and follow the below instructions:
 
`tar -xzvf root_v6.24.00.source.tar.gz`

`mkdir build install`

`cd build`

`cmake -DCMAKE_INSTALL_PREFIX=/users/PAS0654/mjackson/pueosim/root/install/ /users/PAS0654/mjackson/pueosim/root/root-6.24.00/ -Dfortran=ON -Dminuit2=ON -Dmathmore=ON -Dxrootd=OFF`

`cmake --build . --target install -j $(nproc)`

If this completes and everything is working correctly then to finish setup run:

`source ../install/bin/thisroot.sh`

Then open root simply by running `root` : 


``` 
[mjackson@p0353 install]$ root
   ------------------------------------------------------------------
  | Welcome to ROOT 6.24/00                        https://root.cern |
  | (c) 1995-2021, The ROOT Team; conception: R. Brun, F. Rademakers |
  | Built for linuxx8664gcc on Apr 14 2021, 14:33:50                 |
  | From tags/v6-24-00@v6-24-00                                      |
  | With g++-11.1 (GCC) 11.1.0                                       |
  | Try '.help', '.demo', '.license', '.credits', '.quit'/'.q'       |
   ------------------------------------------------------------------

root [0] 

```

---








