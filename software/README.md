Docker image, based on a Ubuntu 13.10 ssh server, that includes the neural network simulators used in brainscales: nest, brian, neuron, the language-indpendent simulator PyNN and the multi-simulation coordinator music. Once you have pulled the image (sudo docker pull brainscales/neural-networks:software), you can use it in two different ways:
- either with just the command line:
sudo docker run -i -t brainscales/neural-networks:software /bin/bash
- or with X11 forwarding in the following one-liner:
CONTAINER_ID=$(sudo docker run -d -p 22 brainscales/neural-networks:software /usr/sbin/sshd -D) && PORT_NUMBER=$(sudo docker PORT $CONTAINER_ID 22 | awk 'BEGIN {FS=":"} {print $2}')  && ssh -Y root@127.0.0.1 -p $PORT_NUMBER
The current password of root is: root
- if you are a first-time user: once inside the container, look at the README and launch the different commands proposed.
- Notes about X11 forwarding:
1. when you restart the computer the sshd daemon will be relaunched if it has not been killed
- Notes about docker:
1. if you kill the ssh client connection, you can relaunch it through: ssh -Y root@127.0.0.1 -p $PORT_NUMBER
2. commit regularly your work: sudo docker commit $CONTAINER_ID your-image-name:your-tag-name
3. if you have forgotten to commit before killing the sshd daemon or closing the /bin/bash container, don't panic, it is still there. You can retrieve it through: sudo docker ps -a
4. If you don't remember the local port (PORT_NUMBER), you can retrieve it using: sudo docker PORT $CONTAINER_ID 22
5. If you don't remember the container ID, you can retrieve it using: sudo docker ps
