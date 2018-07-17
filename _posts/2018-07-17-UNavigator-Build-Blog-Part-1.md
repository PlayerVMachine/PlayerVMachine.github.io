---
title: Ultrasonic Mapping and Navigation Robot
categories: hardware
date: 2018-07-17 16:00:00 -1700
---

### Build Log (Part 1)

A couple years ago now I started wondering about the feasibilty of near real-time piloting of unmanned vehicles on the Moon and Mars. I think it would be a more visceral experience to sitdown and put on a VR headset and feel like you're at the weel of a vehicle on another planet than looking at maps and plotting the vehicles next 8 moves and waiting to find out how those moves panned out. In video game terms I think it would be interesting to transition the game of remote exploration from turn-based startegy to a first-person perspective. 

The most significant barrier to that transition is the communication delay between Earth and the Moon, Mars or other areas of interest. This handy [chart](https://www.spaceacademy.net.au/spacelink/commdly.htm) from the Australian Space Academy shows that the delay in one way communication between the Earth and the Moon for example is about 1.3s, doing some quick math that puts the round trip delay at 2.6s. Now as anyone who's played an online game can tell you lag starts affecting gameplay around the 100-300ms range and at 800ms the game is downright unplayable and if you hit the 1s or greater mark most games will kick you from the server for having a connection that's too slow. So a round trip delay of 2.6s would make a first-person game unplayable... Against other people, however the Moon is currently a PvE (Player Versus Environment) zone not a PvP (Player versus Player) zone. PvE zones can often be much more stable and slower paced than PvP zones. If the Moon's environment in the vehicle's vicinity is unlikely to change significantly in 2.6s then maybe a first-person game mode is viable afterall.

The next major barrier is telemetry, livestreaming video from the Moon or further away back to Earth would take _huge_ bandwidth drawing massive amounts of power, which is undesirable when there are no Tesla charging stations in range of the vehicle and it needs to be as light as possible so that we can launch it from Earth to its destination on an existing rocket. What if then instead of livestreaming video we could send back smaller packets of data every 1.3s that contained reliable telemetry for the entire area the vehicle could travel in a 1.3s span. Just like in games where to lower computing requirements only your immediate area is rendered and the direction you move in gets rendered as you progress. That takes away the livestream requirement but even still sending high resolution photos requires a lot of bandwidth then requires a lot of processing to map a 2D photo into a 3D environment for the player. Instead of sending an image back from the vehicle to Earth what if we sent back just the data required for a computer to render a 3d representation of the environment the vehicle can travel in 1.6s, essentially just sending back x,y,z coordinates for every object in the vehicle's field of operation. That requires less bandwidth than a video and at the least is less computationally expensive than a photo to translate into a 3D environment for the player.

This sounds good on paper to me at least but I need to see if this holds up in the real world at all. The first step is to generate the 3d coordinate data and try to plot it on a computer. I imagine that LIDAR would be well suited to this task but I don't have a LIDAR sensor sitting around so I'll start with an ultrasonic sensor and see what I can get out of that.

Here's my inital setup:
[IMAGE 1]

An HC-SR04 Ultrasonic Sensor mounted on two HS-422 Servo Motors to pitch and yaw the sensor all controlled by an Arduino UNO powered by USB via my laptop to get started.

Since I can control the pitch and yaw of the sensor I figured reading in the data as spherical coordinate points then converting to cartesian for the graphing makes the most sence. The yaw of the sensor is theta, the pitch is phi and the distance measured by the sensor is ro. Due to the way the servo motors and the mounting brackets work together the scan area is too constrained to get a full 360 degree 3D scan but I don't need that to get started. So I chose a range of 20 - 140 degrees theta and 160 - 180 degrees phi to give a 20 degree by 120 degree scan area.

I created a few different sample 'terrains' for the sensor to scan and to try plotting them in Matlab:
<img src="SetupSamples.png" width="512">

A simple Python script reads the data fro the Arduino into a .csv file:
```python
import socket, csv, serial

with open ('scan.csv', 'w') as csvfile:
	writer = csv.writer(csvfile, delimiter=',')
	with serial.Serial('/dev/cu.usbmodem1411', 9600) as ser:
		while 1:	
			data = ser.readline().decode('ascii')
			raw = data.split(',', 3)
			values = [int(raw[0]), int(raw[1]), int(raw[2])]
			print(values)
			writer.writerow(values)
```

Then I import the files into Matlab as matrices, converted from spherical to cartesian coordinates and started plotting the results:
<img src="First-Scan.png" width="512">

I was excited that the collection and plotting had gone so smoothly however the results seem pretty noisy and I certainly wouldn't want to drive anything with them as my map. It's possible to get a sense of what the sensor scanned in a side by side comparison as you can see below, but that's not what I'm going for. So I set about trying to clean up the data and get those outliers in check.
<img src="Side-by-side-comparison.png" width="512">

Just by setting a threshold on the ro values you get something a bit more workable:
<img src="scan2-sph-max80.png" width="512">

Then realizing I was missing a Matlab toolbox filter signals I extended my Python code using numpy, scipy, and matplotlib to smooth and graph the scan data after reading it in (code included at the end). The smoothing function was based off of a scipy cookbook example which in my case seems to have smoothed the data out beyond recognition:
<img src="Too-smooth.png" width="512">

To try things out without having to scan data in each run I moved back to Matlab where I tried to average out the scan data by comparing each point to the mean of the point to the left and right of it and if it was greater then setting it to the mean if not leaving it as is:
```matlab
Radius1 = zeros(121, 21);
for j = 1:21
    for i = 1:121
        if (i > 1 && i < 121 && j > 1 && j < 21)
           %avg = (Radius(i-1,j) + Radius(i+1,j) + Radius(i, j-1) + Radius(i, j+1) + Radius(i-1,j-1) + Radius(i-1, j+1) + Radius(i+1, j+1) + Radius(i+1, j-1)) / 8;
           avg = (Radius(i-1,j) + Radius(i+1,j)) / 2;
           if (Radius(i, j) > (avg * 1.25))
               Radius1(i, j) = avg * 1.25;
           else
               Radius1(i, j) = Radius(i, j);
           end
        end
    end
end
```
The result is a little cleaner but still rather spiky overall and difficult to decipher:
<img src="Better-but-not-good.png" width="512">

I'll wrap up this first log entry here as I continue to search for a way to clean up the plotting of the scan data. Have any ideas? Feel free to let me know at [playervm@pm.me](mailto:playervm@pm.me) or on Discord PlayerVMachine#7577

#### Ardunio Code
```c
#include <Servo.h>

#define trigPin 13
#define echoPin 12

Servo phiServo;
Servo thetaServo;

int phi = 0;    // variable to store the servo position
int theta = 0;

void measure(int phi, int theta) {
  long duration, distance;

  //Send the trigger signal
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  //Recieve return pulse
  duration = pulseIn(echoPin, HIGH);
  
  distance = (duration/2) / 29.1;
  Serial.print(distance);
  Serial.print(',');
  Serial.print(theta);
  Serial.print(',');
  Serial.println(phi);
}

void setup() {
  Serial.begin (9600);
  
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  
  phiServo.attach(9);
  thetaServo.attach(10);
}

void loop() {
  delay(5000);
  for (theta = 160; theta <= 180; theta += 1) {
    thetaServo.write(theta);
    delay(15);
    
    if (theta % 2 == 0) {
      for (phi = 20; phi <= 140; phi += 1) {
        phiServo.write(phi);
        measure(phi, theta);
        delay(15);
      }
    } else {
      for (phi = 140; phi >= 20; phi -= 1) {
        phiServo.write(phi);
        measure(phi, theta);
        delay(15);
      }         
    }
  }
}
```

#### Python code
```python
import socket, serial
from scipy import *
from scipy import signal
import numpy as np
import matplotlib.pyplot as plt
import mpl_toolkits.mplot3d.axes3d as axes3d

points = 121 * 21
scanData = np.zeros((120, 20))

#Retrieve scan data
with serial.Serial('/dev/cu.usbmodem1411', 9600) as ser:
	count = 0
	while (count < points):
		data = ser.readline().decode('ascii')
		raw = data.split(',', 3)
		values = [int(raw[0]), int(raw[1]), int(raw[2])]
		scanData[values[2] - 21, values[1] - 161] = values[0]
		count = count + 1
	ser.close()

#modified scipy 2d smoothing coockbook example to attempt to clean noise in scan data
def gauss_kern(size):
	size = sizey = int(size)
	x, y = mgrid[-size:size+1, -sizey:sizey+1]
	g = exp(-(x**2/float(size)+y**2/float(sizey)))
	return g / g.sum()

def blur_image(im, n):
	g = gauss_kern(n)
	improc = signal.convolve(im, g, mode='valid')
	return(improc)

#smoothScan = blur_image(scanData, 5)

## TEST
smoothScan = np.zeros((120, 20))
for i in range(0,120):
	for j in range(0,20):
		if (scanData[i, j] > 80): 
			smoothScan[i, j] = 80
		else:
			smoothScan[i, j] = scanData[i, j]



#Setup spherical coordinates
theta, phi = np.linspace(np.pi/9, 7*np.pi/9, 120), np.linspace(7*np.pi/8, np.pi, 20)
THETAt, PHIt = np.meshgrid(theta, phi)
THETA, PHI = np.transpose(THETAt), np.transpose(PHIt)
R = smoothScan

#Convert to Cartesian
X = R * np.sin(PHI) * np.cos(THETA)
Y = R * np.sin(PHI) * np.sin(PHI)
Z = R * np.cos(PHI)

#setup 3d plot
fig = plt.figure()
ax = fig.add_subplot(1,1,1, projection='3d')
plot = ax.plot_surface(X, Y, Z, cmap=plt.get_cmap('jet'), linewidth=0, antialiased=False, alpha=0.5)

plt.show()
```