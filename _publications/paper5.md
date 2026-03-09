---
title: "A Real-Time Approach for Humanoid Robot Walking including Dynamic Obstacles Avoidance"
collection: publications
date: 2023-12-12
venue: 'IEEE-RAS 22nd International Conference on Humanoid Robots (Humanoids)'
paperurl: 'https://raw.githubusercontent.com/lurossini/lurossini.github.io/master/_publications/paper5.pdf'
bibtexurl: 'https://raw.githubusercontent.com/lurossini/lurossini.github.io/master/_publications/paper5.bib'

---
This paper proposes a novel approach to online re-plan the walking trajectory of a biped humanoid robot to avoid unexpected interactions and impacts with dynamic obstacles that may compromise the balance of the humanoid robot. The proposed method adjusts the position of the contacts of a pre-planned global trajectory according to the position of moving obstacles and the robot's dynamic properties. The methodology includes a graph-based footstep planner to generate a footstep sequence aware of possible changes in a dynamic environment, a Model Predictive Controller based on the Single-Rigid Body Dynamics to track the computed footsteps, and a final Whole-Body Control layer to compute proper joint torque commands. Preliminary results using the proposed approach are presented to demonstrate the effectiveness of the proposed framework in simulated scenarios with the DRACO3 humanoid bipedal platform.