# Summary

Path-planning is an important primitive for autonomous mobile robots that lets robots find the shortest(or otherwise optimal) path between two points. Otherwise optimal paths could be paths that minimize the amount of turning, the amount of braking or whatever a specific application requires. Algorithms to find a shortest path are important not only in robotics, but also in network routing, video games and gene sequencing.

## Basic Requirements

There are three basic requirements for motion planning:

- Safey: collision avoidance
- Smoothness: energy saving and comfortable 
- Kinodynamic feasibility: executable, controllable

## Methods for Motion Planning

The old-school pipeline of motion planing is:

- Front-end path finding
    - Search for an initial safe path
    - Low dimensional
    - Discrete space
- Back-end trajectory generation
    - Search for an executable trajectory
    - High dimensional
    - Continous space

And the general methods includes:

- Front-end: Path finding
    - Search-based Path Finding
        - Graph Search Basis
        - Dijkstra and A*
        - Jump Point Search
    - Sampling-Based Path Finding
        - Probabilistic Road Map
        - Rapidly-exploring Random Tree(RRT)
        - Optimal Sampling-Based Methods
        - Advanced Sampling-based Methods
    - Kinodynamic Path Finding
        - State-state Boundary Value Optimal Control Problem
        - State Lattice Search
        - Kinodynamic RRT*
        - Hybrid A*
- Back-end: Trajectory Generation
    - Minimum Snap Trajectory Generation
        - Differential Flatness
        - Minimum Snap Optimization 
        - Closed-form Solution to Minimum Snap
        - Time Allocation
        - Implementation in Practice
    - Soft and Hard Constrained Trajectory Optimization
        - Soft Constrained Trajectory Optimization
        - Hard Constrained Trajectory Optimization
- MDP & MPC
    - Markov Decision Process-based Planning
        - Uncertainties in Planning and MDP
        - Minimax Cost Planning and Excected Cost Minimal Planning
        - Value Iteration and Real-Time Dynamic Programming
    - Model Prediction Control for robotics Planning
        - Linear MPC
        - Non-linear MPC

## Reference

- [Motion Planning for Mobile Robots](https://www.shenlanxueyuan.com/course/268)
