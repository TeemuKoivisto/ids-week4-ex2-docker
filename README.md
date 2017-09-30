# Docker installation for Introduction to Datascience week 4 exercise 2

For Windows users setting up virtualenv is kind of impossible since you can't just install Scipy with pip. Only way to install it is either with Anaconda or as pre-built package. What I myself did at first was install by hand the missing dependencies directly as I had already Scipy installed with Anaconda.

But I knew there was another way which I was curious to try out. So instead depending on Windows' quirks you could just run the whole thing as a container. And this is what I did.

# Shortened version

While reading through this thing the second time I came to conclusion it might be a bit too much if you haven't done anything like it before :sweat_smile:. So these are the short versions.

## Windows

1) If you have Windows Professional/Enterprise, install Docker. If you don't install Docker Toolbox. Also it might be you can't even install Docker if you have wrong type of CPU. https://www.docker.com/

2) Enable virtualization from BIOS if you don't have it enabled. If you have UEFI hold shift while pressing restart and then choose restart and in boot screen hit BIOS key(s). For me it was F12. Check it first before you try.

3) The image is kinda large, 4.5 GB. But can't do much about it. Put it to download on background using: `docker pull jupyter/tensorflow-notebook`

4) Create folder somewhere inside `C:\Users\teemu` eg. `C:\Users\teemu\datascience`. I don't know why but this is only way I got it to work with my Docker toolbox.

5) Download `Exercise 1.ipynb` and `HASYv2`-dataset to the folder. Uncompress the dataset. It might take a while.

6) After you have downloaded the image run the container using: `docker run -it -p 8888:8888 -v ///c/Users/teemu/datascience/week4:/home/jovyan/week4 jupyter/tensorflow-notebook` (Change the path accordingly). You should see a link similar to: `http://localhost:8888/?token=766f8bc899fa77a1adc177623c8db5a136c90b45532c27d2`

7) If you have Docker Toolchain Docker is actually running on Docker Machine. Type `docker-machine ip` to get its IP. For me it was `192.168.99.100`. So the notebook is running on `192.168.99.100:8888`.

8) And that's it! You should be able to see the folder from the notebook.

## MacOS (Linux should be similar)

1) Install Docker https://www.docker.com/

2) Create folder eg. `/Users/teemu/datascience` on MacOS or `/home/teemu/datascience` on Ubuntu.

3) Enable file sharing from Docker options (at least on MacOS) to eg. `/Users/teemu/datascience`

4) Download the `Exercise 1.ipynb` and `HASYv2`-dataset to the folder. Uncompress the dataset. It might take a while.

5) After the image download has finished run the container using: `docker run -it -p 8888:8888 -v /Users/teemu/datascience/week4:/home/jovyan/week4 jupyter/tensorflow-notebook` (Change the path accordingly). You should see a link similar to: `http://localhost:8888/?token=766f8bc899fa77a1adc177623c8db5a136c90b45532c27d2`

6) And that's it! You should be able to see the folder from the notebook.

# Prerequisites

1) You have to install either Docker or Docker Toolbox. If you have Linux, MacOS or Windows Professional/Enterprise you can (and should) install Docker. If you have Windows Home edition you have to install Docker Toolbox. Follow the instructions on their website. https://www.docker.com/

2) Also you should check if running Docker on your PC is even possible. You might have to turn on virtualization from your BIOS. For me it was going to the UEFI boot menu (hold shift while pressing restart) and then just clicking restart. Then in the start screen I hit F12 to enter the BIOS. The key might be different for your PC so verify it before you try.

And the image you're about to download is kinda large, 4.5 GB. I can't remember how much Docker is. So if you have slow internet or have no space to spare consider either alternatives or fixing those issues first.

# Docker

3) Now you have Docker installed and virtualization enabled. Open up Docker Quickstart terminal or look if you can find Docker somewhere on your PC. If there's an error consult the internet to see if you can fix it. Again it might not work on your PC if you don't have proper CPU.

You should probably read about Docker if you're interested. It's cool concept and I think about it as customizable minimal virtual OS.

4) First pull the Docker-image we are about to use: `docker pull jupyter/tensorflow-notebook`

5) This could take a while so meanwhile let's set up our environment. First create a folder somewhere on your PC which you'd want to be shared between your host OS and the container. For example `/Users/teemu/datascience` on MacOS or `/home/teemu/datascience` on Ubuntu. NOTE: if you have Windows you have to at least with Docker Toolchain set up the folder somewhere inside `C:\Users\teemu`. I don't know why but this is the only way I got it to work. So following previous examples maybe `C:\Users\teemu\datascience`. (Replacing of course my name with yours)

6) Go to that folder using your terminal or Quickstart-terminal if you have Docker Toolchain (for some reason `docker` command didn't work properly with my Windows' regular terminal). Then pull this repository or copy by yourself the `Exercise 1.ipynb` and download `HASYv2`-dataset into folder perhaps called `week4`. Then uncompress the dataset which might also take a while.

7) Now we are almost there, for MacOS users at least you have to enable file-sharing on your folder e.g. `/Users/teemu/datascience` from Docker options. For Ubuntu I don't know and for Windows you don't have to do anything.

# Running the container

8) When at last you have finally downloaded the image we can try running it: `docker run -it -p 8888:8888 -v ///c/Users/teemu/datascience/week4:/home/jovyan/week4 jupyter/tensorflow-notebook`

Replace the path to the folder with your own path. Also the weird triple slashes are for Windows users only. Now you should see the container booting up and displaying a link like so: `http://localhost:8888/?token=766f8bc899fa77a1adc177623c8db5a136c90b45532c27d2`

Again if you are with Docker Toolchain the actual host isn't your localhost but `docker-machine`. You can see the IP address for it when you type: `docker-machine ip`. For me the address was `192.168.99.100`. So the container is running on address: `192.168.99.100:8888`. For normal Docker users everything should be fine and you should be able to see the folder you just created as volume that you can edit and the changes are synced between your host PC and container.

NOTE: we didn't use the `requirements.txt` to download any dependencies as I think it should work just as it is. But if you do encounter problems you could just ssh into the container and install them by yourself:
```
docker ps -a # check first your container ID
docker exec -it <first two digits of container id> bash
cd ~/week4
pip install -r requirements.txt
```
# Docker commands

Incase something went wrong and you have to stop the container/whatever here's list of commands that might come in handy:

Lists installed images  
`docker images`

Lists running processes  
`docker ps`

Lists stopped and running processes  
`docker ps -a`

Creates a container using org/image and opens up a port from its port 8080 to localhost:8888 (or Docker machine)  
`docker run -it org/image -p 8888:8080`

Creates a container with volume attached to it  
`docker run -it -p 8888:8888 -v /Users/teemu/datascience:/usr/src/data jupyter/tensorflow-notebook`

Stops an image that has ID starting with 'fe4ad'  
`docker stop f4ead`

This one works too if there is no other container IDs starting with 'f'  
`docker stop f`

Removes/deletes stopped container  
`docker rm f4`

Ssh's into container with ID starting with f4 and opens up bash-interpreter  
`docker exec -it f4 bash`

Builds a new image from the current folder (you have to create Dockerfile first)  
`docker build -t teemu/ids-week4 .`

# Conlusion

I don't know why I did this but I have been planning on tweaking with containers for some time now. Today/last night I thought this was as good as chance it's ever going to get. Hopefully you learned something and managed to run your perhaps first Docker-container. It's cool technology so I think it will come very handy in the future also. Happy coding and get back to working on the exercise!
