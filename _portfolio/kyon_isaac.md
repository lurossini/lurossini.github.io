---
title: "kyon_isaac"
excerpt: "Isaac Lab simulation and training environment for the KYON bi-manual quadruped robot.  
        The project contains several environments for:

        - Legged locomotion

        - Wheeled locomotion
        
        - Loco-manipulation
        
        - Sim-to-sim and sim-to-real 
        

        <img src='/images/500x300.png'>"
collection: portfolio
---

The [GitHub repository](https://github.com/lurossini/kyon_isaac/tree/locomotion) contains an Isaac Lab extension complete with assets, environments, mdp, and tasks to train and evaluate locomotion and loco-manipulation skills for the KYON bi-manual quadruped robot, designed and developed at the Humanoid and Human Centered Mechatronics laboratory (HHCM).  

## Legged Locomotion
Legged locomotion environment is contained in the [`source/kyon_isaac/kyon/isaac/tasks/locomotion/velocity/config/kyon/flat_env_cfg.py`](https://github.com/lurossini/kyon_isaac/blob/locomotion/source/kyon_isaac/kyon_isaac/tasks/locomotion/velocity/config/kyon/flat_env_cfg.py) script. The robot is trained to track a 2D command velocity randomly sampled every 10 s. The network used is an asymettric actor-critic. The observation used are listed in the following table:

|          | ObsTerm          | Description                                      |
|----------|----------------  |--------------------------------------------------|
| Actor    | \\(^B\omega\\)   | Angular velocity in base frame                   | 
|          | \\(^B g\\)       | Gravity vector in base frame                     |
|          | \\(^B v_{cmd}\\) | Velocity command in base frame                   |
|          | \\(\Delta q\\)   | Joint position relative to default configuration |
|          | \\(\Delta q_H\\) | Joint position history (\\(H=3\\))               |
|          | \\(\dot{q}\\)    | Joint velocity                                   |
|          | \\(a_{t-1}\\)    | Last action                                      |
| Critic   | \\(^B v\\)       | Linear velocity in base frame                    |
|          | \\(^B a\\)       | Linear acceleration in base frame                |
|          | \\(F_c\\)        | Contact forces                                   |
|          | \\(\tau\\)       | Joint effort                                     |

Actor's observation terms are corrupted with Gaussian noise. The critic augments all the denoised actor's observation terms with some privileged information coming from the simulation ([link](https://github.com/lurossini/kyon_isaac/blob/3204ae837f0f58b6c3dda57f08f9a23821c12a50/source/kyon_isaac/kyon_isaac/tasks/locomotion/velocity/config/kyon/flat_env_cfg.py#L77)).

The action space includes joint position variations around the default configuration only of the legs' joints \\(a_t = \Delta\hat{q} = q_{trg} - q_{def} \in \mathbb{R}^{12}\\) ([link](https://github.com/lurossini/kyon_isaac/blob/3204ae837f0f58b6c3dda57f08f9a23821c12a50/source/kyon_isaac/kyon_isaac/tasks/locomotion/velocity/config/kyon/flat_env_cfg.py#L56)).

Rewards include tracking terms both for the linear and angular velocity, reward terms for following a specific gait (i.e., trotting) with a specific clearance while maximizing the air time, penalty terms for base motion around the not-commanded axis and for contact slip, and some regularization terms for variations in joint position, velocity, accelerations and action smoothness ([link](https://github.com/lurossini/kyon_isaac/blob/3204ae837f0f58b6c3dda57f08f9a23821c12a50/source/kyon_isaac/kyon_isaac/tasks/locomotion/velocity/config/kyon/flat_env_cfg.py#L243)).

Each episode is terminated after a body collision with the base of the thig of the robot ([link](https://github.com/lurossini/kyon_isaac/blob/3204ae837f0f58b6c3dda57f08f9a23821c12a50/source/kyon_isaac/kyon_isaac/tasks/locomotion/velocity/config/kyon/flat_env_cfg.py#L338)), and curriculum learning is used on more and more challenging terrains to improve the adaptability of the policy.

The training is performed using the `rsl_rl` PPO algorithm whose parameters are contained in [`source/kyon_isaac/kyon/isaac/tasks/locomotion/velocity/config/kyon/agents/rsl_rl_ppo_cfg.py`](https://github.com/lurossini/kyon_isaac/blob/locomotion/source/kyon_isaac/kyon_isaac/tasks/locomotion/velocity/config/kyon/agents/rsl_rl_ppo_cfg.py). The environment is registered under the task name `Isaac-Velocity-Flat-Kyon-v0` and `Isaac-Velocity-Flat-KyonFull-v0` whether the user aims to train on KYON with and without the arms, respectively.

ADD TRAINING CURVES HERE AND VIDEO OF THE INFERENCE

## Wheeled locomotion
For the wheeled locomotion, the action space in enlarged to include the velocity of the continuous wheel joints resulting in \\(a_t = (\Delta\hat{q}, \dot{q}_{wheels}) \in \mathbb{R}^{16}\\) ([link](https://github.com/lurossini/kyon_isaac/blob/3204ae837f0f58b6c3dda57f08f9a23821c12a50/source/kyon_isaac/kyon_isaac/tasks/locomotion/velocity/config/kyon/wheel_flat_env_cfg.py#L57)). 

While observation terms are the same as the legged case, the reward terms are reduced to not consider gait tracking, contact air time, and contact slip, since we are not anymore interested in taking steps. The wheeled locomotion environment is in the [`source/kyon_isaac/kyon/isaac/tasks/locomotion/velocity/config/kyon/wheel_env_cfg.py`](https://github.com/lurossini/kyon_isaac/blob/locomotion/source/kyon_isaac/kyon_isaac/tasks/locomotion/velocity/config/kyon/wheel_flat_env_cfg.py#L57) script. The environment is registered under the task name `Isaac-Velocity-Flat-Kyon-Wheel-v0`

ADD TRAINING CURVES AND VIDEO OF THE INFERENCE

## Loco-manipulation (WIP)
Training loco-manipulation skills includes the biggest part of customization of the Isaac Lab library. The source code of the environment is contained  [`source/kyon_isaac/kyon/isaac/tasks/locomanipulation/config/kyon/loco_manipulation_env.py`](https://github.com/lurossini/kyon_isaac/blob/locomotion/source/kyon_isaac/kyon_isaac/tasks/locomanipulation/config/kyon/loco_manipulation_env_cfg.py). The main idea behind the loco-manipulation environment, is to train the robot to track randomly generated Cartesian poses with the left arm end-effector.

Also in this case, we use an asymetric actor-critic, avoiding the necessity of a base estimation module. The observation are collected in the following table ([link](https://github.com/lurossini/kyon_isaac/blob/3204ae837f0f58b6c3dda57f08f9a23821c12a50/source/kyon_isaac/kyon_isaac/tasks/locomanipulation/config/kyon/loco_manipulation_env_cfg.py#L146)):

|          | ObsTerm          | Description                                                               |
|----------|----------------  |---------------------------------------------------------------------------|
| Actor    | \\(^B\omega\\)   | Angular velocity in base frame                                            | 
|          | \\(^B g\\)       | Gravity vector in base frame                                              |
|          | \\(^B p_{cmd}\\) | Command pose for the left end-effector in base frame                      |
|          | \\(\Delta q\\)   | Joint position relative to default configuration (left arm's joints only) |
|          | \\(\Delta q_H\\) | Joint position history (\\(H=3\\)) (left arm's joints only)               |
|          | \\(\dot{q}\\)    | Joint velocity (left arm's joints only)                                   |
|          | \\(a_{t-1}\\)    | Last action                                                               |
|          | \\(^B p_{EE}\\)  | Left end-effector pose in base frame                                      |
| Critic   | \\(^B v\\)       | Linear velocity in base frame                                             |

The action space is made of the 2D base velocity and the left arm's joint positions \\(a_t = (^Bv, \Delta\hat{q}_{LA}) \in \mathbb{R}^{8}\\) ([link](https://github.com/lurossini/kyon_isaac/blob/3204ae837f0f58b6c3dda57f08f9a23821c12a50/source/kyon_isaac/kyon_isaac/tasks/locomanipulation/config/kyon/loco_manipulation_env_cfg.py#L74)). The base velocity action is mapped in joint position reference for the legs' actuators using the frozen legged locomotion policy previously trained.

Following the idea presented in [[1]](https://arxiv.org/pdf/2412.03012), the reward tracking is based on a hierarchy of rewards to enable the arm pose tracking only when the goal is in the workspace of the robotic arm:

The environment is registered under the task name `Isaac-LocoManipulation-Kyon-v0`

## Sim-to-sim and sim-to-real
To bridge the gap between Isaac Lab simulators and other simulators, as well as the gap between simulation and real robots, I implemented the _domain randomization_ technique ([link](https://github.com/lurossini/kyon_isaac/blob/3204ae837f0f58b6c3dda57f08f9a23821c12a50/source/kyon_isaac/kyon_isaac/tasks/locomotion/velocity/config/kyon/flat_env_cfg.py#L142)).
Specifically, during training, at the start-up of each environment the friction between rigid bodies, the actuator gains, the actuators' friction model, and the base mass are randomized in a user defined range. Moreover, the robot base pose and velocity is randomized at every environment reset that occurs after a termination or a truncation. Furthermore, to accomodate for delays in communication and safety filter, which are usually implemented on robots to avoid fast and dangerous change in reference, the Articulation uses DelayedPDActuators with a delay ranging in the frequency range of the low-pass filter used on the real robot. The assets used for training and inference are [`source/kyon_isaac/kyon_isaac/assets/kyon_train.py`](https://github.com/lurossini/kyon_isaac/blob/locomotion/source/kyon_isaac/kyon_isaac/assets/kyon_train.py) and [`source/kyon_isaac/kyon_isaac/assets/kyon_play.py`](https://github.com/lurossini/kyon_isaac/blob/locomotion/source/kyon_isaac/kyon_isaac/assets/kyon_play.py), respectively.

Sim-to-sim and sim-to-real are tested using a custom environment that enables a ZMQ communication with the [XBot2](https://advrhumanoids.github.io/xbot2/master/index.html) middleware [2], contained in [`source/kyon_isaac/kyon_isaac/env/manager_based_xbot2_env.py`](https://github.com/lurossini/kyon_isaac/blob/locomotion/source/kyon_isaac/kyon_isaac/env/manager_based_xbot2_env.py). The idea behind this environment is to create a class, inherited from the Isaac Lab's `ArticluationCfg` class, that uses a custom `ArticulationData` to override the robot datas used in the `mdp.func` using the XBot2 API at each environment `update()`. In this way, there is no need for an hardcoded mapping between the XBot2 API methods and the observation space, and the inference can be executed simply calling the usual Isaac Lab script, specifying the registered task.


### Reference
[1] K. Jiang, Z. Fu, J. Guo, W. Zhang and H. Chen, "Learning Whole-Body Loco-Manipulation for Omni-Directional Task Space Pose Tracking With a Wheeled-Quadrupedal-Manipulator," in IEEE Robotics and Automation Letters, vol. 10, no. 2, pp. 1481-1488, Feb. 2025

[2] A. Laurenzi, D. Antonucci, N.G. Tsagarakis, and L. Muratore, "The XBot2 real-time middleware for robotics", in Robotics and Autonomous Systems, vol. 163, 2023.