---
excerpt: "        The basic idea of the system is very simple. We find the distance
  of every node in the network from two points inside the reference area. A line connecting
  these two points is taken as the reference x-axis. Based on these two distances
  one can apply simple Pythagoras theorem to obtain two equations with two unknown
  variables. These two equations are solved to obtain the x co-ordinate and y-coordinate
  of that node.\r\n\r"
categories: [page, project]
layout: page
title: WiFi based location tracking
created: 1268976013
---
The basic idea of the system is very simple. We find the distance of every node in the network from two points inside the reference area. A line connecting these two points is taken as the reference x-axis. Based on these two distances one can apply simple Pythagoras theorem to obtain two equations with two unknown variables. These two equations are solved to obtain the x co-ordinate and y-coordinate of that node.

The main program resides and runs in a computer that is called the “main server”. This server is positioned in a such a way that it can be considered as the origin. The program requires certain initialization. Administrator has to enter the IP of the “Helping Node”. It searches for this node and a socket connection is established. It tries to find out the distance at which this “Helping Node” is by using the iw program.

The distance between the main server and the helping node is recorded for future calculations. The program iw is run from a shell script which makes it to dump the details regarding the nodes connected to the network. It also redirects the data regarding each of the nodes to a file. The data stored in file are only a part of what iw dumps, that part which is needed to find the distance of the nodes and recognize them.

The data stored in file are directly read by the server program. This data is regarding the power of the signal that was received at the main server. It is also called RSSI or Received Signal Strength Index. This is expressed in decibel. It must be provided by the device as well as the device driver must provide this value to the kernel or user space. This value in decibels can be used to calculate the distance that the data has traveled to reach the particular node.

The program iw runs in helper node as well in the same manner. When the main server requests the helper node for data, it is provided using the file that a similar script in the helper node (as in the server) redirects the “grep”ed output into. This is obtained in decibel. Thus we have obtained the power at which the data has been received at the “Helper Node”. This is used to measure the distance using a formula mention later. This file is transfered to the main server via a socket connection.

We have thus distance to a point from two different location. We also know where these locations are with respect to each other. Now to find out the location of the point all we need is simple mathematics.

<img src=/data/wifi_img.jpg>

GUI Implementation
------------------
To implement the GUI and to visualize the data we obtained, an image is drawn that is updated every time a change is seen. I used Cairo for this purpose. The UI was designed using Glade and used normal GTK library.

Cairo is a software library used to provide a vector graphics-based, device-independent API for software developers. It is designed to provide primitives for 2-dimensional drawing across a number of different backends. Cairo is designed to use hardware acceleration when available. Although written in C, there are bindings for using the cairo graphics library from many other programming languages, including Factor, Haskell, Lua, Perl, Python, Ruby, Scheme, Smalltalk and several others. Dual licensed under the GNU Lesser General Public License and the Mozilla Public License, cairo is free software.

The cairo API provides operations similar to the drawing operators of PostScript and PDF. Operations in cairo including stroking and filling cubic Bézier splines, transforming and compositing translucent images, and antialiased text rendering. All drawing operations can be transformed by any affine transformation (scale, rotation, shear, etc.)

The code will be soon available in my git-hub account.

(Update 10-04-10)The code available isn't complete. Some shell scripts are missing. Dont bother the comments in the gui code. The code has some copy-paste from thenon-gui code. Will do the corrections.

(C, Cairo lib, GTK)
