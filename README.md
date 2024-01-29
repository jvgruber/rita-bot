# R.I.T.A.
The **R**eactive **I**ndependent **T**ask **A**chiever robot is our DIY mobile robot project.

## Milestones
- [x] Setting up the development environment (GitHub, Docker, ROS-Package)
- [ ] Simulating the robot with URDF
- [ ] Controlling the simulated robot with an XBOX Controller through docker
- [ ] Running the Robot on the Raspberry Pi
- [ ] Controling the GPIO pins on the Raspberry via ROS
- [ ] Mapping the controlls from the XBOX controller to the GPIO pins

## Documentation
### Create the package
First create an empty workspace and the file structure:
```bash
mkdir -p dev_ws/src
cd dev_ws
colcon build --symlink-install
```

Then change to `src`. Inside create the package with
```bash
ros2 pkg create --build-type ament_cmake rita-bot
```
or clone the repo:
```bash
git clone https://github.com/jvgruber/rita-bot.git
```

Build the package from the `dev_ws` directory
```bash
colcon build --package-select rita-bot
```
Alternatively you can use `colcon build` for building the entire workspace.

After that make sure to `source install/setup.bash` or `source install/setup_local.bash`.


### Write custom nodes
see the ROS2 tutorial on https://docs.ros.org/en/foxy/Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Cpp-Publisher-And-Subscriber.html

### Write launch files
see the ROS2 tutorial on https://docs.ros.org/en/foxy/Tutorials/Intermediate/Launch/Creating-Launch-Files.html

### Set up Controller in Docker
Here is the in-depth youtube video: https://www.youtube.com/watch?v=uf4zOigzTFo&t=1s
On the host machine we can check all input peripherals:
```bash
ls -l /dev/input/by-id
```
You can also try running `jstest-gtk`.

We can use devices in our docker by adding some arguments to `docker run ros-iron-rita:latest`:
```bash
--device=/dev/input/js0 # for the controller only (the controller needs to be connected before starting the container!)
-v /dev/input:/dev/input --device-cgroup-rule='c 13:* rmw' # for the input device group (input has id 13)
-v /dev:/dev --device-cgroup-rule='c *:* rmw' # for all device groups
```

### Set up RaspberryPi in Docker
