# PX4 SITL Gazebo Simulation with QGroundControl

This guide provides step-by-step instructions to set up a Software in the Loop (SITL) simulation environment on Ubuntu 20.04 LTS or Ubuntu 22.04, utilizing PX4-Autopilot, Gazebo 11, and QGroundControl.

## Prerequisites

- A machine running Ubuntu 20.04 LTS or Ubuntu 22.04.
- Internet connection for downloading necessary software packages.

## PX4 Installation

1. Open up a terminal on your system.

2. Navigate to your home directory:
    ```bash
    cd ~
    ```

3. Clone the PX4-Autopilot repository:
    ```bash
    git clone https://github.com/PX4/PX4-Autopilot.git --recursive
    ```

4. Navigate to the setup directory:
    ```bash
    cd PX4-Autopilot/Tools/setup
    ```

5. Run the setup script for Ubuntu:
    ```bash
    bash ubuntu.sh
    ```

6. If you are on Ubuntu 20.04, perform the following steps to install Gazebo manually (skip if on Ubuntu 22.04 as Gazebo is installed by default):
    ```bash
    sudo wget https://packages.osrfoundation.org/gazebo.gpg -O /usr/share/keyrings/pkgs-osrf-archive-keyring.gpg
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/pkgs-osrf-archive-keyring.gpg] http://packages.osrfoundation.org/gazebo/ubuntu-stable $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/gazebo-stable.list > /dev/null
    sudo apt-get update
    sudo apt-get install gz-garden
    ```

7. Update and upgrade your system:
    ```bash
    sudo apt update && sudo apt upgrade -y
    ```

8. Verify Java is not installed. The output should say `Command ‘java’ not found`:
    ```bash
    java -version
    ```

9. Install OpenJDK 18:
    ```bash
    sudo apt-get install openjdk-18-jdk
    sudo apt-get install openjdk-18-jre
    ```

10. Verify the Java installation. The output should be similar to `openjdk version "18...`:
    ```bash
    java -version
    ```

11. Modify the `ubuntu.sh` script to use OpenJDK 18 and update gazebo version references:
    ```bash
    nano PX4-Autopilot/Tools/setup/ubuntu.sh
    ```
    - Find and replace `java_version=` line, change 14 -> 18.
    - Replace `libgazebo$gazeboversion-dev` -> `libgazebo-dev` and `gazebo$gazeboversion` -> `gazebo`.
    - Save and exit the editor (press CTRL + O to write changes, and then CTRL + X to exit).

## Gazebo Installation

12. Install Gazebo:
    ```bash
    curl -sSL http://get.gazebosim.org | sh
    ```

13. Add the current user to the dialout group:
    ```bash
    sudo usermod -a -G dialout $USER
    ```

14. Remove the modemmanager:
    ```bash
    sudo apt-get remove modemmanager -y
    ```

15. Install necessary GStreamer plugins:
    ```bash
    sudo apt install gstreamer1.0-plugins-bad gstreamer1.0-libav gstreamer1.0-gl -y
    ```

16. Log out and log back in to apply the group changes.

## QGroundControl Installation

17. Download QGroundControl:
    [Download QGroundControl](https://qgroundcontrol.com/downloads/)

18. Navigate to the directory where QGroundControl was downloaded (assuming it's the Documents directory):
    ```bash
    cd ~/Documents
    ```

19. Make the QGroundControl.AppImage executable:
    ```bash
    chmod +x ./QGroundControl.AppImage
    ```

20. Run QGroundControl:
    ```bash
    ./QGroundControl.AppImage
    ```
## Running the Simulation

1. Navigate to the PX4-Autopilot directory:
    ```bash
    cd ~/PX4-Autopilot
    ```

2. Build and run the simulation:
    ```bash
    make px4_sitl gazebo
    ```

3. Run QGroundControl:
    ```bash
    ./QGroundControl.AppImage
    ```

4. QGroundControl should automatically connect to the simulated vehicle.

Enjoy your PX4 SITL Gazebo simulation with QGroundControl!

