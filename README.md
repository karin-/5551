# Using Docker for Turtlebot Simulator

If you're not able to run linux and directly install ROS on your machine, you can use Docker to set up an ubuntu container that runs ROS.

# 1. Install Docker
Install the community edition of Docker ([installation instructions](https://docs.docker.com/install/)).


After installing, make sure Docker can properly pull and run an image:
```sh
$ docker run hello-world
```
Docker should respond with something like
```
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
ca4f61b1923c: Pull complete
Digest: sha256:ca0eeb6fb05351dfc59c20733c91def84cb8007aa89a5bf606bc8b315b9fc7
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.
...
```

### Pull a ROS image

The Turtlebot Simulator we want to use runs on the indigo ROS distribution. To pull the indigo distro, run
```sh
$ docker pull ros:indigo
```
(If you're interested, you can view the list of official ROS docker images  [here](https://hub.docker.com/_/ros/).)

### Getting started with your ROS container
Now that you've pulled a ROS image, you can spin up a container:
```sh
$ docker run -it ros:indigo
```
The `-it` flag opens up an interactive session in your ROS container. Now you can run
```
$ roscore
```
within the interactive session to launch the ROS master process inside your container.
At this point, you need to open a new bash session with your ROS container to enter more commands. To do this, open a new terminal on your machine and find the name of your ROS docker container by running
```
$ docker ps -l
```
This will output information about the most recent Docker container you've spun up. Notice the last column gives the name of the container:
```
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
8967c0ed6c78        ros:indigo          "/ros_entrypoint.sh …"   3 minutes ago   14 seconds ago                        mystifying_pike
```
In my case, my container is named `mystifying_pike`. To open up a new interactive session with my container, I can run
```
$ docker exec -it mystifying_pike bash
```
Next, you need to set up your environment with 
```
$ source /opt/ros/<distro>/setup.bash
```
(If you don't know what ROS distro your container has, just check `ls /opt/ros`.)
To make sure everything is working correctly, run
```
$ rostopic list
```
and you should see 
```
/rosout
/rosout_agg
```

### Setting up Turtlebot Simulator (aka installing 500 packages)
First, update apt-get to prepare for all the `sudo apt-get install`'s that are about to happen
```
$ sudo apt-get update
```
We'll need to install the ROS indigo desktop:
```
$ sudo apt-get install ros-indigo-desktop-full
```
and then various Turtlebot things:
```
$ sudo apt-get install ros-indigo-turtlebot ros-indigo-turtlebot-apps ros-indigo-turtlebot-interactions ros-indigo-turtlebot-simulator ros-indigo-kobuki-ftdi ros-indigo-rocon-remocon ros-indigo-rocon-qt-library ros-indigo-ar-track-alvar-msgs
```
[Source](http://wiki.ros.org/turtlebot/Tutorials/indigo/Turtlebot%20Installation)

### Using GUIs with your container
If you don't already have X11/XQuartz on your machine, [install here](https://www.xquartz.org/).
