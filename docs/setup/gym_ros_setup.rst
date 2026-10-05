.. _doc_gym_ros_setup:

Gym_Ros Setup
=============

This page describes how to set up Gym ROS integration for RoboRacer.

.. contents:: Table of Contents
   :local:
   :depth: 2

Overview
--------

This page provides the steps required to set up the gym_ros simulator environment for RoboRacer. It will use Docker and foxglove to keep the setup process similar over different OSes.

Requirements
------------

 - As Docker is being used, this setup is OS agnostic. It should work on Linux, Windows, and MacOS. The specs of the devices should not need to be too powerful to run the simulator. A modern laptop with 8GB of RAM and a quad-core CPU should be sufficient.
 - Go to foxglove.dev and create an account. You will need to log in to foxglove studio to view the gym_ros environment.

Windows setup
-------------

Here is the setup video to follow along with, or you can just follow along with the text commands:

.. raw:: html

   <div style="text-align: center; margin-bottom: 1.5em;">
      <video width="600" height="315" controls>
         <source src="../_static/videos/WindowsEditedSetup.mp4" type="video/mp4">
         Your browser does not support the video tag.
      </video>
   </div>

1️⃣ **Install Docker**

We will start by downloading Docker Desktop and getting it running. You can download Docker Desktop from the official website: https://docs.docker.com/desktop/setup/install/windows-install/

There are a few videos online that can help with the Docker installation process. Here's one: https://www.youtube.com/watch?v=TMx4ydYKCHw

Sign into the docker app, using any account is fine. Github or google accounts are the most convenient to use.

*Important Note:* Things may not work starting from step 3 in the Windows section if you do not run the Docker Desktop app before running the commands in step 3. Make sure to run the Docker Desktop app first, every time you start your computer and want to run the simulator.

2️⃣ **Repo Setup**

Open a terminal and navigate to the directory where you want the repository to be cloned to. For my example, I will use ``C:\Users\Student\Documents>``

Once you're in the desired directory, run the following commands in the terminal to clone the repo and enter its directory:

.. code-block:: bash

   git clone https://github.com/f1tenth/f1tenth_gym_ros.git
   cd f1tenth_gym_ros

You should now see your new directory path. For example, ``C:\Users\Student\Documents\f1tenth_gym_ros>``.
We will now swap to the dev-humble branch of the repository. Run the following commands in the terminal:

.. code-block:: bash

   git checkout dev-humble
   git status

You should see that you are now on the dev-humble branch.

3️⃣ **Finish docker setup in terminal**

If you haven't run the Docker Desktop app since the last time you booted your PC, start it, then run the command once it's up:

.. code-block:: bash

   docker compose up -d --build

It may take a few minutes, but the end of the command's output should look something like this:

.. code-block:: text

	✔ Image f1tenth_gym_ros               Built				2.2s
	✔ Container f1tenth_gym_ros-sim-1     Started    	    4.0s

.. note::
	You will have to run this command every time you start your computer, otherwise the docker won't be running and the following steps won't work.

If you see the proper output, you can now enter the docker container by running the following command:

.. code-block:: bash

   docker exec -it f1tenth_gym_ros-sim-1 /bin/bash

Now, instead of a filepath like ``C:\Users\Student\Documents\f1tenth_gym_ros>``, you should see a filepath like ``root@(numbers and letters):/sim_ws#``.

To ensure that you're on the correct branch once more, run the following command:

.. code-block:: bash

   echo $ROS_DISTRO
   ls /opt/ros

4️⃣ **Launch the gym_ros environment**

Both of those command should return "humble". If not, use the command "exit" and swap to the dev-humble branch, then follow the steps from that point again. Then, continue with this command:

.. code-block:: bash

   ros2 launch f1tenth_gym_ros gym_bridge_launch.py open_foxglove:=false

You'll see more lines of output, eventually stopping with something like

.. code-block:: text

   Advertising new channel ## for topic "/cmd_vel"

If that's what you see last, you're good to open your browser and go to this link, keeping the terminal open:

.. code-block:: text

   https://app.foxglove.dev/?ds=foxglove-websocket&ds.url=ws://localhost:8765

If prompted by your browser to allow foxglove to interact with other apps, click "Allow". You should see a page that says "foxglove studio" and prompts you to sign in. Sign in with the account you created earlier.

The sim should now be running in your browser. Go to the ``Simulator Window Setup`` section of this page to find the steps to ensure the sim windows are set up properly.

Linux setup
-----------

Here is the setup video to follow along with, or you can just follow along with the text commands:

.. raw:: html

   <div style="text-align: center; margin-bottom: 1.5em;">
      <video width="600" height="315" controls>
         <source src="../_static/videos/WindowsEditedSetup.mp4" type="video/mp4">
         Your browser does not support the video tag.
      </video>
   </div>

1️⃣ **Install Docker**

We will begin by installing Docker. To ensure that no older version of Docker are installed that may interfere with getting the sim running, we will start by removing any older versions of Docker. Open a terminal and run the following commands:

.. code-block:: bash

   sudo apt remove docker.io docker-doc docker-compose podman-docker containerd runc

   sudo apt update

   sudo apt install lsb-release

   sudo apt install ca-certificates curl gnupg (yes when prompted)

   sudo install -m 0755 -d /etc/apt/keyrings

   curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg (this command may not work on the school wifi network, so if not, try a different one)
   sudo chmod a+r /etc/apt/keyrings/docker.gpg

   echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null 

   sudo apt update

   sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin (yes when prompted)

Finish the installation by running the following command:

.. code-block:: bash

   sudo usermod -aG docker $USER

Once you run that, log out of your computer and log back in. You can verify that Docker is installed correctly by running the following command:

.. code-block:: bash

   docker run hello-world

If you see a message that says "Hello from Docker!", then Docker is installed correctly. If not, try restarting your computer instead of just logging out, then running the last command again.

2️⃣ **Install git and Set Up the Repo**

Next we'll get git running and set up the repo. Run the following commands in the terminal:

.. code-block:: bash

   sudo apt update
   sudo apt install git

   git clone https://github.com/f1tenth/f1tenth_gym_ros.git
   cd f1tenth_gym_ros

   git checkout dev-humble
   git status

You should now be on the dev-humble branch in the repo's new directory. This is very important, so if git status returns a different branch, make sure you get swapped before continuing.

3️⃣ **Finish docker setup in terminal**

If you're on dev-humble, run these commands to finish setting up the docker container:

.. code-block:: bash

   docker compose up -d --build
   
The output of that command should end with text that looks like this:

.. code-block:: text

  ✔ Image f1tenth_gym_ros               Built				2.2s
  ✔ Container f1tenth_gym_ros-sim-1     Started    	    4.0s

.. note::

	You will have to run this command every time you start your computer, otherwise the docker won't be running and the following steps won't work.

Once that's finished, run these commands:

.. code-block:: bash

   docker exec -it f1tenth_gym_ros-sim-1 /bin/bash
   echo $ROS_DISTRO
   ls /opt/ros

After the first command above, your filepath should change to something like ``root@(numbers and letters):/sim_ws#``. The second and third commands should both return "humble". If not, use the command "exit" and swap to the dev-humble branch, then follow the steps from step 3 again.

4️⃣ **Launch the gym_ros environment**

Continue with this command:

.. code-block:: bash

   ros2 launch f1tenth_gym_ros gym_bridge_launch.py open_foxglove:=false

The output of that command should end with text that looks like this:

.. code-block:: text

   Advertising new channel ## for topic "/cmd_vel"

If this is what you see last, you're good to open your browser and go to this link, keeping the terminal open:

.. code-block:: text

   https://app.foxglove.dev/?ds=foxglove-websocket&ds.url=ws://localhost:8765

The sim should appear as running. Go to the ``Simulator Window Setup`` section of this page to find the steps to ensure the sim windows are set up properly.

MacOS setup
-----------




Simulator Window Setup
-----------------------

This section will show you how to set up the simulator windows so that they are easy to view and use. The steps are the same for all OSes.

