# PueoBuilder Tutorial 

This tutorial focuses on compiling pueoBuilder and completing a simulation using pueoSim. Before starting, please make sure that you have completed the Root Tutorial and have the following properly compiled and working:

- CMake 3.21.1
- gcc 11.1.0
- fftw 3.3.9
- gsl 2.7.1
- ROOT 6.24.00

## Accessing pueoBuilder Repository

To have access to the GitHub repository you must be added to the pueoCollaboration on GitHub. Afterwards, you'll have to use an SSH key to clone the repository. To do this open up your terminal and run the following: 

`ssh-keygen -t ed25519 -C "your_email@example.com"`

You'll be prompted to provide a file to save the key in, just hit enter. 

You'll be prompted to enter a passphrase - you can hit enter to skip this or insert a passphrase. 

Then go ahead and add the new SSH key to the ssh-agent: 

`eval "$(ssh-agent -s)"`

`$ ssh-add ~/.ssh/id_ed25519` 

Once you have this completed all you need to do is copy this SSH key and add it to your GitHub account. To do this open your file viewer, navigate to the top right corner, click on the three lines and scan for **show hidden files**. This will allow you to view your new .ssh folder created by generating the SSH key. 

Go ahead and navigate into this folder and copy the contents from your SSH key file called something along the lines of **'id_ed25519.pub'**.

Then on your GitHub account navigate to Settings -> SSH and GPG keys -> New SSH key. Paste your key, give it an appropriate name, and add it to your account. 

Now navigate back to your terminal, create an appropriate directory (for me `/users/PAS0654/mjackson/pueosim/pueoBuilder`) and clone pueoBuilder:

`git clone git@github.com:PUEOCollaboration/pueoBuilder.git`

After this has completed, open up your `load_env.sh` file and add the following information: 

```
export PUEO_BUILD_DIR=/users/PAS0654/mjackson/pueosim/pueoBuilder
export PUEO_UTIL_INSTALL_DIR=/users/PAS0654/mjackson/pueosim/pueoBuilder
export NICEMC_SRC=${PUEO_BUILD_DIR}/components/nicemc
export NICEMC_BUILD=${PUEO_BUILD_DIR}/build/components/nicemc
export PUEOSIM_SRC=${PUEO_BUILD_DIR}/components/pueoSim
export LD_LIBRARY_PATH=${PUEO_UTIL_INSTALL_DIR}/lib:$LD_LIBRARY_PATH
```

Please note $PUEO_BUILD_DIR and $PUEO_UTIL_INSTALL_DIR point to where you cloned pueoBuilder. After re-sourcing your shell script you should be able to run: 

`./pueoBuilder.sh`

# Testing pueoSim

Now that pueoBuilder has compiled the next step is to make sure everything is working correctly. To test this, go ahead and run the following: 

`./simulatePueo -i pueo.conf -o {output} -r 1`

Where simulatePueo is located in `./pueoBuilder/build/components/pueoSim/simulatePueo` and your {output} is your choice of location to place an output file. If you run this, you should see the following: 

```
[mjackson@p0085 pueoBuilder]$ ./build/components/pueoSim/simulatePueo -i pueo.conf -o /users/PAS0654/mjackson/pueosim/ -r 1
Changed input file to: pueo.conf
Changed output directory to: /users/PAS0654/mjackson/pueosim/
Settings.cc is searching for /users/PAS0654/mjackson/pueosim/pueoBuilder/components/pueoSim/config/pueo.conf in ./ 
Settings.cc is searching for /users/PAS0654/mjackson/pueosim/pueoBuilder/components/pueoSim/config/pueo.conf in ./config/ 
Settings.cc is searching for /users/PAS0654/mjackson/pueosim/pueoBuilder/components/pueoSim/config/pueo.conf in /users/PAS0654/mjackson/pueosim/pueoBuilder/components/nicemc/config/ 
Settings.cc is searching for /users/PAS0654/mjackson/pueosim/pueoBuilder/components/pueoSim/config/pueo.conf in // 
Found /users/PAS0654/mjackson/pueosim/pueoBuilder/components/pueoSim/config/pueo.conf in //!
INITIAL SEED is 65546
Using Crust 2.0 ice model.
Writing CreateHorizons input file.
Apply impulse response to digitizer path: 1
Apply impulse response to trigger path: 1
Use time-dependent thresholds: 1
Use dead time from flight: 0
Use noise from flight for digitizer path: 1
Use noise from flight for trigger path: 1
Loaded chain FlightPath::Anita3 with first good entry 0 at 1.41887e+09 and last good entry 30929 at 1.42078e+09
initializing and using preliminary PUEO payload geometry
AnitaVersion=4
nantennas is 108
Info in <TCanvas::P
```

This should run for < 1 minute and then provide a summary : 

``` 
~~~~~ Summary for electron neutrinos  ~~~~~~~~~~~~~~~ 
	Number simulated: 73
	Number passed (unweighted): 0
	Number passed (weighted): 0
	Effective volume: -1 km^3 sr
	Effective area: -1 km^2 sr


~~~~~ Summary for mu neutrinos  ~~~~~~~~~~~~~~~ 
	Number simulated: 61
	Number passed (unweighted): 0
	Number passed (weighted): 0
	Effective volume: -1 km^3 sr
	Effective area: -1 km^2 sr


~~~~~ Summary for tau neutrinos  ~~~~~~~~~~~~~~~ 
	Number simulated: 66
	Number passed (unweighted): 0
	Number passed (weighted): 0
	Effective volume: -1 km^3 sr
	Effective area: -1 km^2 sr


~~~~~ Summary for all neutrinos  ~~~~~~~~~~~~~~~ 
	Number simulated: 200
	Number passed (unweighted): 0
	Number passed (weighted): 0
	Effective volume: 0 km^3 sr
	Effective area: 0 km^2 sr

Elapsed runtime is 0:52 minutes
```

Congragulations, you have a working set of PUEO software! 






