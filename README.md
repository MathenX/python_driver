# Mecademic Python API

A python module designed for Robot products from Mecademic. The module offers tools that give access to all the features of the Mecademic Robots such as MoveLin and MoveJoints. The module can be started from a terminal or a python application and controls the Mecademic Products. 

####Supported Robots

 * Meca500 R3

## Getting Started

These instructions will allow you to control the Robots from Mecademic through a python package and use the package for development and deployment purposes.

## Prerequisites

To be able to use the module without unexpected errors, the user must have a copy of python installed on their machine and it is recommended to use python version 3.6 or higher. [Python](https://www.python.org/) can be installed from its main website (a reboot will be require after the installation to complete the setup). 

## Downloading the package

There are two ways to download the python driver for the Mecademic Robots. The first option is to download the MecademicRobot.py module from the Mecademic Github. The module can then be used in projects just like any other python module. The other option is download and install it to your computer through pip. Pip will place the MecademicRobot package with the rest of your python packages in your path and can easily be imported to multiple projects. To install the package through pip, two methods can be used. The first is to download the wheel of the package from the dist folder of the Mecademic python_driver Github repository and run the following command from a terminal:
```
C:<path-to-package-wheel>> pip install  MecademicRobot-<current version>-<python version>-<license>-<os>.whl
```
The second method is to give pip the Mecadmic python_driver Github repository url. Pip will download the wheel and install it in the python package path on its own. This is down with the following command:

```
C:<any-path>> pip install git+https://github.com/Mecademic/python_driver
``` 
If the package is in your python package path, it can be imported into any of your python projects without having a copy in your directory.

## Running a Robot from Mecademic with the package

To run a Mecademic Robot with the MecademicRobot package, two options present itself. The first is to use it in an interactive terminal. The other is to use it as a dependency of a runnable script.

#### Interactive Terminal 

To use the MecademicRobot.py in an interactive terminal, the python file must be started with the -i modifier in a terminal as follows:

```
C:Users\admin> python -i <file-path>/MecademicRobot.py 
```
Or
```
C:<file-path>> python -i MecademicRobot.py 
```
If the package is in your python package path, you can also start a python shell and import it then.
```
C:<any-path>> python
>>> import MecademicRobot
```

This will open a python terminal with the module already imported. From there you need to connect to the Mecademic Robot before being able to perform actions. This is done by making an instance of the class MecademicRobot by passing the IP Address of the Robot as an argument and using the function Connect():
```
>>> robot = MecademicRobot.MecademicRobot('192.168.0.100')
>>> robot.Connect()
```

Once succesfully connected to the Robot, you are able to start performing actions. To get it ready for operations, it must be activated and homed or it will fall in error. To do so, the following functions are run:

```
>>> robot.Activate()
>>> robot.Home()
```

The Robot is now ready to perform operations. The user programming manual or the documentation in the module is sufficiant to be able to make the Robot perform actions and control the robot. When done with the Robot and desire to power it off, it must first be deactivated and disconnected to avoid issues and problems. It is done by two functions:

```
>>> robot.Deactivate()
>>> robot.Disconnect()
```

If during use the Robot encounters an error, it will go into error mode. In this mode, the module will block any command to the Robot unless its an error reset. To properly reset errors on the Robot, the following function must be run:

```
>>> robot.ResetError()
```

#### Runnable Script

For making a runnable script, the above functions calls remain the same in the script. Actions must be placed between activation and deactivation to avoid errors. Writing the script is like regular programming in python. It is recommended to have an error catcher to get the Robot out of error mode and not have the Robot stop working in the middle of operation. This can be creatively done by using the isInError() and action functions to catch the Robot immediately as it falls in error and bringing it back to operating condition. A method can be made to it like the following:
```py
def AutoRepair(robot):
    if(robot.isInError()):
        robot.ResetError()
    elif(robot.GetStatus()['Paused']==1):
        robot.ResumeMotion()
```
Note: Deactivating and reactivating the Robot is not necessary but can be helpful in power cycle recovery.

An example of a script for a Mecademic Robot would be:
```py
import MecademicRobot
robot = MecademicRobot.MecademicRobot('192.168.0.100')
robot.Connect()
robot.Activate()
robot.Home()
robot.SetBlending(0)
robot.SetJointVel(100)
while True:
	robot.MoveJoints(0,0,0,170,115,175)
	robot.MoveJoints(0,0,0,-170,-115,-175)
	robot.MoveJoints(0,-70,70,0,0,0)
	robot.MoveJoints(0,90,-135,0,0,0)
	robot.GripperClose()
	robot.MoveJoints(0,0,0,0,0,0)
	robot.MoveJoints(175,0,0,0,0,0)
	robot.MoveJoints(-175,0,0,0,0,0)
	robot.MoveJoints(175,0,0,0,0,0)
	robot.GripperOpen()
	robot.MoveJoints(0,0,0,0,0,0)
```
This will make the Robot perform a repetitive task until the program is terminated.

A more viable way to make long programs for the Mecademic Products is by using a string. In python, there are various ways to write strings and the string variable type is useful to make a program script by using the triple quotes string. This format can be spread across multiple lines and includes newlines into the string without placing them implicitly. This makes it easy to read and write the program you are trying to develop. The script can be written easily with the string format as follow:
```py
    	Program = """SetBlending(0)
                    SetJointVel(100)
                    MoveJoints(0,0,0,170,115,175)
                    MoveJoints(0,0,0,-170,-115,-175)
                    MoveJoints(0,0,0,170,115,175)
                    MoveJoints(0,0,0,-170,-115,-175)
                    MoveJoints(0,0,0,170,115,175)
                    MoveJoints(0,0,0,-170,-115,-175)
                    MoveJoints(0,0,0,170,115,175)
                    MoveJoints(0,0,0,-170,-115,-175)
                    MoveJoints(0,0,0,170,115,175)
                    MoveJoints(0,0,0,-170,-115,-175)
                    MoveJoints(0,-70,70,0,0,0)
                    MoveJoints(0,90,-135,0,0,0)
                    MoveJoints(0,-70,70,0,0,0)
                    MoveJoints(0,90,-135,0,0,0)
                    MoveJoints(0,-70,70,0,0,0)
                    MoveJoints(0,90,-135,0,0,0)
                    MoveJoints(0,-70,70,0,0,0)
                    MoveJoints(0,90,-135,0,0,0)
                    MoveJoints(0,-70,70,0,0,0)
                    MoveJoints(0,0,0,0,0,0)
                    MoveJoints(175,0,0,0,0,0)
                    MoveJoints(-175,0,0,0,0,0)
                    MoveJoints(175,0,0,0,0,0)
                    MoveJoints(-175,0,0,0,0,0)
                    MoveJoints(175,0,0,0,0,0)
                    MoveJoints(-175,0,0,0,0,0)
                    MoveJoints(175,0,0,0,0,0)
                    MoveJoints(-175,0,0,0,0,0)
                    MoveJoints(175,0,0,0,0,0)
                    MoveJoints(0,0,0,0,0,0)"""
```
Each line has a command with the arguments it requires. The commands are written in the same way as the functions in the module. Once the script is complete, it must be broken down into a list of actions. Python makes it easy by using the available string functions.
```py
Program = Program.replace(' ','')
movements = Program.split("\n")
```
The variable movements contains the list of actions to perform after getting rid of all the empty spaces and seperating the command by distinguishing commands by line. To go through the actions one by one, it is only required to make a loop that iterates through the actions. Using the __exchangeMsg__ function, it is easy to send the command to the Robot. The __exchangeMsg__ function is responsable for interpreting the commands and return expected messages back to the user. It is the backbone of most of the functions of the module.
```py
for action in movements:
	robot.exchangeMsg(action)
	if(robot.isInError()):
    	AutoRepair(robot)
```
If the script you wrote is one you wish the Robot to repeat until stopped by a user for whatever reason, the previous loop can be placed inside an infinite loop. Using all the information, building blocks and functions provided, you are fully equipped to control and program your Mecademic Robot for your project's requirements.

## Built With

* [Visual Studio Code](https://code.visualstudio.com/) - code editor

## Authors 

* **Mecademic** - *Continuous work*


