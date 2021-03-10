## This file lists several files that we need to modify or create in MIT CHEETAH project for our own quadrupedal. Remember to check  corresponding include file of following source files. 

## Workload List
Created on 2020.03.01 by wyn
```
********/resources*********************************************
need to create model files:       done   doing    todo 
milab_body.obj                              *
milab_hip.obj                               *
milab_upper_link.obj                        *
milab_upper_link_mirror.obj                 *
milab_lower_link.obj                        *


********/config************************************************
need to create milab robot and user parameters for QT simulator:
milab-robot-defaults.yaml          *
milab-user-defaults.yaml           *

********/common************************************************
******/src**                     done   doing    todo 
****/Dynamics**
Quadruped.cpp                               *
FloatingBaseModel.cpp                       * 
******/include**
cppTypes.h                         *
****/Dynamics**
need to create file according to MiniCheetah.h:
Milab.h                                    * todo
****/ControlParameters
SimulatorParameters.h              *

********/robot*************************************************
******/src**                     done   doing    todo  
RobotRunner.cpp                    * todo
main_helper.cpp                    *
HardwareBridge.cpp                 *
******/include**
HardwareBridge.h                   *

********/sim***************************************************
******/src**                     done   doing    todo  
Simulation.cpp                     * todo
SimControlPanel.cpp                * todo
SimControlPanel.ui                 *
RobotInterface.cpp                 *
Graphics3D.cpp                     *
DrawList.cpp                                * todo


********/user**************************************************
******/MIT_Controller**          done   doing    todo 
****/Controllers**
**/convexMPC**
RobotState.cpp                     *
ConvexMPCLocomotion.cpp                            *
**/WBC_Ctrl** 
*/LocomotionCtrl
LocomotionCtrl.cpp                                 *
*/TaskSetbranch
BodyOriTask.cpp                                    *
BodyPosTask.cpp                                    *
LinkPosTask.cpp                                    *
****/FSM_States**
FSM_State.cpp                      * todo
FSM_State_Locomotion.cpp           * todo
FSM_State_RecoveryStand.cpp                        *
FSM_State_StandUp.cpp              * todo
FSM_BalanceStand.cpp               * todo
SafetyChecker.cpp                  *
```

## Cheetah-Software
This repository contains the Robot and Simulation software project.  For a getting started guide, see the documentation folder.

The common folder contains the common library with dynamics and utilities
The resources folder will contain data files, like CAD of the robot used for the visualization
The robot folder will contain the robot program
The sim folder will contain the simulation program. It is the only program which depends on QT.
The third-party will contain *small* third party libraries that we have modified. This should just be libsoem for Cheetah 3, which Pat modified at one point.

## Build
To build all code:
```
mkdir build
cd build
cmake ..
./../scripts/make_types.sh
make -j4
```

If you are building code on your computer that you would like to copy over to the mini cheetah, you must replace the cmake command with
```
cmake -DMINI_CHEETAH_BUILD=TRUE
```
otherwise it will not work.  If you are building mini cheetah code one the mini cheetah computer, you do not need to do this.

This build process builds the common library, robot code, and simulator. If you just change robot code, you can simply run `make -j4` again. If you change LCM types, you'll need to run `cmake ..; make -j4`. This automatically runs `make_types.sh`.

To test the common library, run `common/test-common`. To run the robot code, run `robot/robot`. To run the simulator, run `sim/sim`.

Part of this build process will automatically download the gtest software testing framework and sets it up. After it is done building, it will produce a `libbiomimetics.a` static library and an executable `test-common`.  Run the tests with `common/test-common`. This output should hopefully end with

```
[----------] Global test environment tear-down
[==========] 18 tests from 3 test cases ran. (0 ms total)
[  PASSED  ] 18 tests.
```
## Run simulator
To run the simulator:
1. Open the control board
```
./sim/sim
```
2. In the another command window, run the robot control code
```
./user/${controller_folder}/${controller_name} ${robot_name} ${target_system}
```
Example)
```
./user/JPos_Controller/jpos_ctrl 3 s
```
3: Cheetah 3, m: Mini Cheetah
s: simulation, r: robot

## Run Mini cheetah
1. Create build folder `mkdir mc-build`
2. Build as mini cheetah executable `cd mc-build; cmake -DMINI_CHEETAH_BUILD=TRUE ..; make -j`
3. Connect to mini cheetah over ethernet, verify you can ssh in
4. Copy program to mini cheetah with `../scripts/send_to_mini_cheetah.sh`
5. ssh into the mini cheetah `ssh user@10.0.0.34`
6. Enter the robot program folder `cd robot-software-....`
7. Run robot code `./run_mc.sh` 



## Dependencies
- Qt 5.10 - https://www.qt.io/download-qt-installer
- LCM - https://lcm-proj.github.io/ (Please make it sure that you have a java to let lcm compile java-extension together)
- Eigen - http://eigen.tuxfamily.org
- `mesa-common-dev`
- `freeglut3-dev`
- `libblas-dev liblapack-dev`

To use Ipopt, use CMake Ipopt option. Ex) cmake -DIPOPT_OPTION=ON ..

