# End-to-End Synthetic LIDAR Point Cloud Data Generation & Deep Learning Validation!

Generation of synthetic and segmented Point Cloud LiDAR data and Object detection is a custom implementation of the paper from PIXOR. It represents the driving scene using lidar data in the Birds' Eye View (BEV) and uses a single stage object detector to predict the car


# Project Overview

![image](https://user-images.githubusercontent.com/83134822/115970366-c863ac80-a50f-11eb-9f56-1c3133d4cf33.png)

## Virtual LiDAR Simulator(Synthetic Point cloud generation) 

a)	Step 1: Once you download and unzip the folder, run the “Virtual LiDAR Simulator.exe”

![image](https://user-images.githubusercontent.com/83134822/115970476-8b4bea00-a510-11eb-9dbd-753a01b2ac0b.png)

b)	Step 2: Once the application loads (after the Unity splash Screen), it will lead you to the Credits Panel showcasing our research team. Please select the X button to move to Main Menu.

![image](https://user-images.githubusercontent.com/83134822/115970463-7d966480-a510-11eb-9171-5872fb41e6a5.png)

c)	Step 3: After selecting the X button this will lead to Main Menu scene. To start the simulation select the Start Button

![image](https://user-images.githubusercontent.com/83134822/115970490-9ef75080-a510-11eb-9158-db6ca381e2fc.png)

d)	(Step 4: Options button leads to a new panel where users can select different settings for the virtual LiDAR simulator. This feature is currently under development. Select Back to return to Main Menu.)

![image](https://user-images.githubusercontent.com/83134822/115970498-af0f3000-a510-11eb-839f-d97a4e132f83.png)

e)	Step 5: After starting the simulator, this will lead us to the main scene. The main controls to move around with the vehicle controller that also contains the LiDAR, are:
            a.	W: move forward
            b.	S: move backwards
            c.	A: turn left
            d.	D: Turn right
            e.	Escape: Returns to Main Menu
Tip: At very beginning of the main scene -even without moving at all-, by pressing the key button “P” (it stands for “PointCloud”) the sampling process starts (there are car gameObjects on the right side) 

![image](https://user-images.githubusercontent.com/83134822/115970511-c5b58700-a510-11eb-8d5f-449087c8f331.png)

f)	Step 6: After starting the sampling process, the vehicle controller immobilizes until it finishes collecting all the points. You can recover the control when everything is done. At the same time, after finishing, you will need the visit the “..\Virtual LiDAR Simulator_Data\StreamingAssets” subfolder of the application. In this folder you will find the automatically created dataset. This is a csv file with the name: “LiDAR_Dataset”. The csv contains 4 columns (XYZ info + a randomly created reflectance value ranging from 0.3 - 0.6)

![image](https://user-images.githubusercontent.com/83134822/115970529-eaa9fa00-a510-11eb-9d18-d5cab3b88d30.png)

g)	Step 7: A screenshot from a sample synthetic dataset.

![image](https://user-images.githubusercontent.com/83134822/115970538-fe556080-a510-11eb-937b-7bfa07fe81a2.png)


## Steps to Run Object Detection for Point cloud data

Create a folder “LidarProject” in your google drive.
Upload the project “PIXOR.zip” and “34epoch” into that location, and run the below command

```
from google.colab import drive
drive.mount('/content/drive')
```
Then you have to compile the c-library of the project using the below command

```
!pip install shapely numpy matplotlib
!unzip -qq /content/drive/MyDrive/LidarProject/PIXOR.zip
%cd PIXOR/srcs/preprocess
!make
!ls
%cd ..
```

You need KITTI library for this, to save time we have uploaded the library into google drive as well. If you just want to give it a try then you can use the following commands to get the library in google colab

```
!wget https://s3.eu-central-1.amazonaws.com/avg-kitti/data_object_velodyne.zip
!wget https://s3.eu-central-1.amazonaws.com/avg-kitti/data_object_label_2.zip
!unzip -qq data_object_velodyne.zip
!unzip -qq data_object_label_2.zip
!ls 

```
While in this operation you might find the below pop-up, you need not have to worry about it and choose “Ignore”, as we will not take full memory for our test run

![image](https://user-images.githubusercontent.com/83134822/115968633-cba66a80-a506-11eb-8d55-7b59841e17de.png)

Note: This will take a while, so to avoid this time delay as mentioned above we uploaded both the datasets on google drive in the folder that we created and ran the below command just to unzip
```
!unzip -qq /content/drive/MyDrive/LidarProject/data_object_velodyne.zip
!unzip -qq /content/drive/MyDrive/LidarProject/data_object_label_2.zip
!ls

```
Project expects files in certain folder so let’s go ahead and create them now

```
!mkdir data_object_velodyne
!mkdir test
!python3 copyfolder.py

```
Let’s now take the Point cloud data that we have created from unity and place it “test” folder marked in yellow box(you can drag and drop) as Project expects the *.csv file in that location. (You can find the test data in the project folder under “TestData”) run the command below

![image](https://user-images.githubusercontent.com/83134822/115968708-2770f380-a507-11eb-988a-f8f45fc11a9a.png)

```
!python3 csv2bin.py

```
Run the below command and you should find the output in the “srcs” folder (try refresh if you don’t see the Prediction.png file) 
```
!python3 object_detector.py
```

## Evaluation Results

### 3 car Detection
#### Simulator Input
![Simulator Input](https://user-images.githubusercontent.com/83134822/115968756-57b89200-a507-11eb-86f4-7cf9646ecb5d.png)
#### PIXOR Detection
![PIXOR Detection](https://user-images.githubusercontent.com/83134822/115969164-9c452d00-a509-11eb-84cd-de3b3288857c.png)

### 4 car Detection
#### Simulator Input
![Simulator Input](https://user-images.githubusercontent.com/83134822/115968776-6f901600-a507-11eb-929c-a91e33b00096.png)
#### PIXOR Detection
![PIXOR Detection](https://user-images.githubusercontent.com/83134822/115968779-7454ca00-a507-11eb-9e20-ac8fb1d9069e.png)





