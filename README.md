# My test fixtures

These are the fixtures I use for my minimal Food Computer setup.
The hardware components/capabilities that I intend to start with are:
+ am2315 temperature/humidity sensor
+ mhz16 CO<sup>2</sup> sensor
+ an exhaust fan
+ multiple air circulation fans

### Prequisites

The [OpenAg default.json](https://github.com/OpenAgInitiative/openag_brain/tree/master/fixtures) fixture was loaded before fixtures in this repo.

## Setup 

These are the commands I used to setup the PI and ultimately apply the fixture(s). 
These steps overlap the Food Computer setup instruction. 
I capture the commands here as a point in time example of what I did to get the fixture working.

#### Core setup

These commands come from [here](http://wiki.openag.media.mit.edu/openag_brain/installing/installing_the_os) and [here](http://wiki.openag.media.mit.edu/openag_brain/installing/installing_with_docker).

`sudo apt-get update`

`sudo apt-get install git`

`git clone http://github.com/OpenAgInitiative/openag_brain_docker_rpi`

`cd openag_brain_docker_rpi`

`sh install_docker.sh`
	
`docker-compose up -d`

#### Enter into the docker container's scope

`docker exec -it openagbraindockerrpi_brain_1 bash`

+ This command returns these two lines. I don't know if this is an issue or not. But it appears to be unimportant.
  ```
  bash: -e: command not found
  bash: -e: command not found
  ```

`source catkin_ws/devel/setup.bash`

Flash the default fixture. The default fixture has the core *module types*. 
The *module types* are the templates for each instance of a *modules* are build upon. 
There can be multiple instances of a *module* although this will not be the case in my simple setup.
The `default.json` fixture file has more *module types* then I will be using, but this does not appear to cause any negative side effects. 

`rosrun openag_brain flash -F ./catkin_ws/src/openag_brain/fixtures/default.json`

+ **NOTE** If the flash does not succeed and instead (after many screens of data) returns a series of timeouts like so (the quantity of imbedded # appear to randomly change):
    ```
   	#avrdude: stk500v2_ReceiveMessage(): timeout
	#avrdude: stk500v2_ReceiveMessage(): timeout
	##avrdude: stk500v2_ReceiveMessage(): timeout
	#avrdude: stk500v2_ReceiveMessage(): timeout
    ```
    then I unplug the Arduino for 30 seconds. 
    Plug it back in and run the command again. 
    Before I figured out the *unplug* trick, I happened upon a [*reset* trick](https://www.youtube.com/watch?v=tAzjO4v7oF4) that didn't work for me. Maybe it works for certain Arduino models.

#### Download my specific fixtures

`mkdir ymerej`  

`cd ymerej`  

`git clone https://github.com/ymerej/openag-fixtures.git`  

`rosrun openag_brain flash -F ~/ymerej/openag-fixtures/am2315_sensor.json`

### Later I will add fixtures for the CO<sup>2</sup> sensor and fans