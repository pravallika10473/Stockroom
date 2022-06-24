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






