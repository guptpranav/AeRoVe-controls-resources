# ArduPilot SITL

Given below are the instructions for installing Ardupilot SITL. The following instructions assume that you have Ubuntu 20 and ROS Noetic already installed in your system.

## Cloning the Repository

```bash
git clone https://github.com/ArduPilot/ardupilot --recurse-submodules
cd ardupilot
git submodule update --init --recursive
```

The following command will install all the required dependencies and add the binaries to your ```$PATH```

```bash
Tools/environment_install/install-prereqs-ubuntu.sh -y
export PATH=$PATH:$HOME/ardupilot/Tools/autotest
```

Once completed, log out and log in again to make changes permanent

## Building the Repository

```bash
./waf clean
./waf configure --board sitl
./waf copter
```

After the build is successful, you can test if the sitl is working using following commands

```bash
cd ~/ardupilot/ArduCopter
../Tools/autotest/sim_vehicle.py --map --console
```

## Gazebo Environment

### Installing Gazebo Plugin

```bash
git clone --recurse https://github.com/khancyr/ardupilot_gazebo
cd ardupilot_gazebo
mkdir build
cd build
cmake ..
make -j4
sudo make install
```

### Launching Gazebo

Start Gazebo in one terminal

```bash
gazebo --verbose worlds/iris_arducopter_runway.world
```

Start SITL in another terminal

```bash
cd ~/ardupilot/ArduCopter
../Tools/autotest/sim_vehicle.py -f gazebo-iris --console --map
```

In the SITL terminal, run the following command:

```bash
STABILIZE> mode GUIDED
GUIDED> arm throttle
GUIDED> takeoff 5
```

and the iris drone in the gazebo window should move to a stable hover at $(0,0,5)$
