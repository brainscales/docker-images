Description
===========
Docker image, based on a Ubuntu 13.10 ssh server, that includes the neural network simulators used in brainscales: 
- nest, 
- brian, 
- neuron, 
- PyNN (0.7), the language-indpendent simulator (it uses as backend either nest, brian or neuron),
- music, the multi-simulation coordinator. 

I. Downloading the image
========================

Prerequisites
-------------
On the host computer, you only nedd to have the docker engine installed. If the host computer is a ubuntu machine, you should install lxc-docker:
```
sudo apt-get install lxc-docker
```

Downloading
-----------
To download the image, just launch from the command line:

```
sudo docker pull brainscales/neural-networks:software
```  
  
II. Using the image
===================

You can use it in two different ways, either using a bash terminal (without displat) or using X11 forwarding (with display):

1. through a bash terminal
--------------------------

In this mode, you can launch neural simulators but you can't display the results in figures. Launch the following command at the terminal:
```
sudo docker run -i -t brainscales/neural-networks:software /bin/bash
```

If you are a first-time user: once inside the container, look at the README and launch the different commands proposed:
```
ls
more README
```

To test the nest simulator:
```
cd /root//nest-2.2.2/examples/nest
nest brunel-2000.sli
```

To test nest, neuron and brian using the same PyNN (python) code:
```
cd /root/PyNN-0.7.5/examples
python brunel.py nest
python brunel.py neuron
python brunel.py brian
```

2. through X11 forwarding
-------------------------

In this mode, you connect to the machine using ssh -Y (that exports the X11 display). It permits to display your results

Prerequisites
-------------
On the host machine, you need to install a ssh client. For example, if the host machine runs on Ubuntu, install openssh-client:
```
sudo apt-get install openssh-client
```

Launching
---------

Copy-paste in your terminal the following one-liner:
```
CONTAINER_ID=$(sudo docker run -d -p 22 brainscales/neural-networks:software /usr/sbin/sshd -D) && PORT_NUMBER=$(sudo docker PORT $CONTAINER_ID 22 | awk 'BEGIN {FS=":"} {print $2}')  && ssh -Y root@127.0.0.1 -p $PORT_NUMBER
```
The current password of root is: root
```
username: root
passwd: root
```

If you are a first-time user: once inside the container, look at the README and launch the different commands proposed:
```
ls
more README
```

To test 'music' with 3D display:
```
cd /root/music/test
mpirun -np 4 /usr/local/bin/music /root/music/test/demo.music
```

To test 'brian' that gives a rasterplot:
```
python -c 'from brian import *; brian_sample_run()'
```

To launch the demo of 'neuron':
```
neurondemo
```

To test the nest simulator:
```
cd /root//nest-2.2.2/examples/nest
nest brunel-2000.sli
```

To test nest, neuron and brian using the same PyNN (python) code:
```
cd /root/PyNN-0.7.5/examples
python brunel.py nest
python brunel.py neuron
python brunel.py brian
```

3. Notes about X11 forwarding
--------------------------
- When you restart the computer, the sshd daemon will be relaunched if it has not been killed
- If you kill the ssh client connection, you can relaunch it through: 
```ssh -Y root@127.0.0.1 -p $PORT_NUMBER```
- If you don't remember the local port (PORT_NUMBER), you can retrieve it using: ```sudo docker PORT $CONTAINER_ID 22```
- If you don't remember the container ID, you can retrieve it using: ```sudo docker ps```

4. Notes about docker
---------------------
- Commit regularly your work: ```sudo docker commit $CONTAINER_ID your-image-name:your-tag-name```
- In the command above, once you have committed your work as your-image-name:your-tag-name, in the one-liners proposed above, you will replace the name ```brainscales/neural-networks:software``` with ```your-image-name:your-tag-name```
- If you have forgotten to commit before killing the sshd daemon or before closing the /bin/bash container, don't panic, it is still there. You can retrieve it using: ```sudo docker ps -a```
- If you don't remember the local port (PORT_NUMBER), you can retrieve it using: ```sudo docker PORT $CONTAINER_ID 22```
- If you don't remember the container ID, you can retrieve it using: ```sudo docker ps```
- To list all the images: ```sudo docker images```
