---
title: "Multi-contact planning and control for humanoid robots: Design and validation of a complete framework"
collection: publications
permalink: /publication/paper1
# excerpt: 'This paper is about the number 1. The number 2 is left for future work.'
date: 2023-08-01
venue: 'Robotics and Autonomous Systems'
# slidesurl: 'https://academicpages.github.io/files/slides1.pdf'
paperurl: 'https://raw.githubusercontent.com/lurossini/lurossini.github.io/master/_publications/paper1.pdf'
bibtexurl: 'https://raw.githubusercontent.com/lurossini/lurossini.github.io/master/_publications/paper1.bib'
# citation: 'L Rossini, P Ferrari, F Ruscelli, A Laurenzi, G Oriolo, N G Tsagarakis, E M Hoffman, "Multi-contact planning and control for humanoid robots: Design and validation of a complete framework", Robotics and Autonomous Systems, Volume 166, 2023'
---
In this paper, we consider the problem of generating appropriate motions for a torque-controlled humanoid robot that is assigned a multi-contact loco-manipulation task, i.e., a task that requires the robot to move within the environment by repeatedly establishing and breaking multiple, non-coplanar contacts. To this end, we present a complete multi-contact planning and control framework for multi-limbed robotic systems, such as humanoids. The planning layer works offline and consists of two sequential modules: first, a stance planner computes a sequence of feasible contact combinations; then, a whole-body planner finds the sequence of collision-free humanoid motions that realize them while respecting the physical limitations of the robot. For the challenging problem posed by the first stage, we propose a novel randomized approach that does not require the specification of pre-designed potential contacts or any kind of pre-computation. The control layer produces online torque commands that enable the humanoid to execute the planned motions while guaranteeing closed-loop balance. It relies on two modules, i.e., the stance switching and reactive balancing module; their combined action allows it to withstand possible execution inaccuracies, external disturbances, and modeling uncertainties. Numerical and experimental results obtained on COMAN+, a torque-controlled humanoid robot designed at Istituto Italiano di Tecnologia, validate our framework for loco-manipulation tasks of different complexity.