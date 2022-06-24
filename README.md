# Stockroom
This repository contains the full autonomy stack (implemented on ROS) for fetch robot to navigate through a mock stockroom environment to fetch and deliver items from the specified bin in a stockroom and deliver it to the counter.
![stockroom_gazebo](https://user-images.githubusercontent.com/68220390/175476967-30af0247-79c6-4092-aa7a-e7350fe6eb18.png)
# Implementation
# Gazebo Stockroom World
* A Bin model is created (details are in model/bin/model.sdf) 
* Using createMarker from ar_track_alvar reprositary (git clone -b noetic-devel https://github.com/machinekoder/ar_track_alvar) and alvar.py ,12  ALVAR marker tags for 12 bins in stockroom are created along with materials.
* In order to automate rather than individual repetition of the stockroom creation with 12 bins, 12 ALVAR tags and walls EmPy XML(stockroom.world.em) is used.
* empy stockroom.world.em >stockroom.world command creates the stockroom.world with 12 bins ,ALVAR tags and walls.
* To stock the 12 bins with products at a particular position in the bins stock_products.py was written, coke model was used.
* stockroom_bot.launch file which contains the stockroom.world and fetch.launch.xml(https://github.com/fetchrobotics/fetch_gazebo.git) was used to launch the stockroom world and fetch from the command line.
# Map 
* To create the map of the stock room , fetch is teleoperated using teleop_twist_keyboard.py from the teleop_twist_keyboard reprositary(https://github.com/ros-teleop/teleop_twist_keyboard.git).
* The /base_scan and /tf data was recorded into stockroom_bot.bag using rosbag record.
* gmapping slam was run to create the map of the stockroom when the recorded rosbag was played back.
* By saving it using map_server/map_saver the created map along with map.yaml were saved.
# Navigation
* Map file and initial localization(initial_localization.py) of the robot were passed to the navigation stock(nav.launch).
* Using move_base action interface, navigation goals are feed to the navigation stack, this was implemented in go_to_bin.py.
* go_to-bin.py takes the bin number(0-11) at the command line as an argument and convert that bin number into the coordinates in the room(as we know the stockroom dimensions) and pass them as goal pose to the move_base. 
* look_at_bin.py was used to point the robot's head so that it is aiming the bin.
* Even though go_to_bin.py sends the robot to the specified bin, there would be some errors like initial localization errors , which cause robot to go near rather than exact opposite of the bins, so ALVAR Marker tags (markers.launch) are used to guide the robot so that it can pick the correct items.
* Using the transforms generated by the ALVAR marker detection system, we can com‐
mand the robot to reach out and grasp an item that is in a known position relative to
the ALVAR marker at the back of the bin. pick_up_item.py is intended to run once the
robot is close enough to a bin to detect its ALVAR marker, after which it will generate
an arm motion planner goal that is relative to the bin and MoveIt is used for motion planing and octomap package was also used.
![Pick up from 2nd bin](https://user-images.githubusercontent.com/68220390/175505162-0eadb7ca-5df1-429e-b104-5e3e075ed440.jpg)
                       Picking up the item from the 2nd bin (0th bin ,1st bin)
* To deliver the item to the customer counter outside the stockroom,the robot has to navigate to a position behind the counter, extend the arm, open the gripper to drop the object, and
then retract the arm and return to the stockroom, which was implemented in deliver_to_counter.py
![Screenshot (221)](https://user-images.githubusercontent.com/68220390/175506960-3b4cda4a-7320-47f7-b91e-a292a5c5969d.png)










