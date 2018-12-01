## IKidWorkSpace
```
├── GameController  GameController Node, communicate with game controller
├── GoogleDeps      Google Dependences, include part cartographer, protobuf and ceres
├── Perception      Get image from camera, detect objects and caculate objects distance
├── README.md
├── Shm             Share memory
└── WorldNode       Self locate and objects locate
```

### Preinstall
* Install ROS

    Ubuntu 18.04 follow [ROS wiki](http://wiki.ros.org/kinetic/Installation/Ubuntu)

    Other Ubuntu version install corresponded ROS version.
* Install CUDA and CUDNN

    Follow [cudnn-install](https://docs.nvidia.com/deeplearning/sdk/cudnn-install/index.html)

* Install libraries dependencies

      sudo apt install libgoogle-glog-dev libgflags-dev libncurses5-dev libsuitesparse-dev libeigen3-dev ninja-build libcairo2-dev python-catkin-tools

* Download ikid source

      sudo apt install python-wstool

      cd ~

      git clone https://github.com/b51/IKidReadme.git

      mkdir ~/IKidWorkSpace

      cd ~/IKidWorkSpace

      wstool init src ~/IKidReadme/ikid.rosinstall

* Generate GoogleDeps

      cd src/GoogleDepsGenerate

      ./deps_generater.sh

      cd .. && mv GoogleDepsGenerate ~/Templates/

* Generate libdarknet

  1. Download source code

      `git clone https://github.com/pjreddie/darknet`

  2. Checkout to yolo2

      `git checkout 80d9be`

  3. Modify some Options and make

      `vi Makefile`

      `GPU=0      ->  GPU=1`

      `CUDNN=0    ->  CUDNN=1`

      `OPENCV=0   ->  OPENCV=1`

      `OPENMP=0   ->  OPENMP=1`

      `make`

      `cp libdarknet.so IKidWorkSpace/src/Perception/lib/`

* Build and launch

  1. Build

      `cd IKidWorkSpace`

      `catkin build Perception WorldNode`

      `source devel/setup.bash`

  2. Launch Shm.launch to init shared memory

      `roslaunch Shm Shm.launch`

  3. Launch PerceptionNode.launch

        `Set camera_index at Perception/config/ConfigPerception.lua, if /dev/video0 -> camera_index = 0.`

        `roslaunch PerceptionNode PerceptionNode.launch`

        `If launch success, will display image get from camera at realtime.`

  4. Launch WorldNode.launch

        `roslaunch WorldNode WorldNode.launch`

### Issues

1. Launch PerceptionNode.launch/WorldNode.launch throw error with msg

   `terminate called after throwing an instance of 'boost::interprocess::interprocess_exception'`

   This issue caused by shm not initilized, need launch Shm.launch first.
