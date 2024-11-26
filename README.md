# ROS Tutorials  

This repository contains a step-by-step guide and Dockerfiles for setting up learning environments for both **ROS 1 (Noetic)** and **ROS 2 (Humble)**. By using Docker, you can quickly and easily get started with ROS without worrying about system dependencies or installation conflicts.

---

## Table of Contents

1. [Prerequisites](#prerequisites)  
2. [Dockerfiles](#dockerfiles)  
3. [Building the Docker Images](#building-the-docker-images)  
4. [Running the Docker Containers](#running-the-docker-containers)  
5. [ROS 1 Tutorials](#ros-1-tutorials)  
6. [ROS 2 Tutorials](#ros-2-tutorials)  

---

## Prerequisites

Ensure you have Docker installed on your system. Follow the installation guide from [Docker's official site](https://www.docker.com/products/docker-desktop).

---

## Dockerfiles

This repository includes Dockerfiles for setting up both ROS environments:  

- **ROS 1 (Noetic)**: `Dockerfile.ros1-noetic`  
- **ROS 2 (Humble)**: `Dockerfile.ros2-humble`  

---

## Building the Docker Images

### Building the ROS 1 (Noetic) Image

Run the following command to build the ROS 1 Docker image:
```bash
docker build -t ros1 -f Dockerfile.ros1-noetic .
```

### Building the ROS 2 (Humble) Image
Run the following command to build the ROS 2 Docker image:

```bash
docker build -t ros2 -f Dockerfile.ros2-humble .
```

### Running the Docker Containers
Running the ROS 1 (Noetic) Container
Start the container for ROS 1 using the following command:
```bash
docker run -it --name ros1 ros1
```

### Running the ROS 2 (Humble) Container
Start the container for ROS 2 using the following command:
```bash
docker run -it --name ros2 ros2
```

# ROS 1 Tutorials
After entering the ROS 1 container, you can follow these steps to begin:

## Step 1: Initialize ROS Environment
Run the following command to set up the ROS environment:

```bash
source /opt/ros/noetic/setup.bash
```
## Step 2: Create a ROS Workspace
```bash
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws
catkin_make
source devel/setup.bash
```
## Step 3: Write a Simple Publisher Node
```bash
source /opt/ros/noetic/setup.bash
```

1. Navigate to the src folder:
```bash
cd ~/catkin_ws/src
```
2. Create a package:
```bash
catkin_create_pkg beginner_tutorials std_msgs rospy roscpp
cd ..
catkin_make
source devel/setup.bash
```

3. Create a Python script for a publisher in beginner_tutorials:

```bash
nano src/beginner_tutorials/scripts/talker.py
```

Add the following content:

```python
#!/usr/bin/env python3
import rospy
from std_msgs.msg import String

def talker():
    pub = rospy.Publisher('chatter', String, queue_size=10)
    rospy.init_node('talker', anonymous=True)
    rate = rospy.Rate(10)  # 10hz
    while not rospy.is_shutdown():
        hello_str = "hello world %s" % rospy.get_time()
        rospy.loginfo(hello_str)
        pub.publish(hello_str)
        rate.sleep()

if __name__ == '__main__':
    try:
        talker()
    except rospy.ROSInterruptException:
        pass

```
4. Make the script executable
```bash
chmod +x src/beginner_tutorials/scripts/talker.py
```
5. Run the script
```bash
rosrun beginner_tutorials talker.py
```



# ROS 2 Tutorials
After entering the ROS 2 container, you can follow these steps to begin:

## Step 1: Initialize ROS 2 Environment
Run the following command to set up the ROS 2 environment:

```bash
source /opt/ros/humble/setup.bash
```
## Step 2: Create a ROS Workspace
```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws
colcon build
source install/setup.bash
```
## Step 3: Write a Simple Publisher Node
1. Navigate to the src folder:
```bash
cd ~/ros2_ws/src
```

1. Create a package:
```bash
cd ~/catkin_ws/src
```
2. Create a package:
```bash
ros2 pkg create --build-type ament_python beginner_tutorials
```

3. Navigate to the package:
```bash
cd beginner_tutorials
```
4. Edit talker.py inside the beginner_tutorials package:
```bash 
nano beginner_tutorials/talker.py
```
Add the following content:

```python
import rclpy
from rclpy.node import Node
from std_msgs.msg import String

class MinimalPublisher(Node):
    def __init__(self):
        super().__init__('minimal_publisher')
        self.publisher_ = self.create_publisher(String, 'topic', 10)
        timer_period = 0.5  # seconds
        self.timer = self.create_timer(timer_period, self.timer_callback)

    def timer_callback(self):
        msg = String()
        msg.data = 'Hello, ROS 2!'
        self.get_logger().info('Publishing: "%s"' % msg.data)
        self.publisher_.publish(msg)

def main(args=None):
    rclpy.init(args=args)
    minimal_publisher = MinimalPublisher()
    rclpy.spin(minimal_publisher)
    minimal_publisher.destroy_node()
    rclpy.shutdown()

if __name__ == '__main__':
    main()
```
5. Make it executable and run:
```bash
python3 beginner_tutorials/talker.py
```

5. Run the script
```bash
rosrun beginner_tutorials talker.py
```