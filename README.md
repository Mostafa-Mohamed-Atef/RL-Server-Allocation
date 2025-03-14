# Server Allocation Reinforcement Learning Project

This project implements a reinforcement learning (RL) solution for a server allocation problem using the Proximal Policy Optimization (PPO) algorithm from the `stable-baselines3` library. The goal is to dynamically allocate servers to handle incoming demand while optimizing resource utilization and minimizing queue length.

## Table of Contents
1. [Overview](#overview)
2. [Code Structure](#code-structure)
3. [Environment Details](#environment-details)
4. [Custom Neural Network](#custom-neural-network)
5. [Training Process](#training-process)
---

## Overview

The project simulates a server allocation scenario where the RL agent must decide how many servers to activate or deactivate based on incoming demand. The environment is designed to mimic real-world server allocation challenges, such as fluctuating demand patterns and the need to balance resource utilization and queue length.

The key components of the project include:
- A custom Gymnasium environment (`ServerAllocationEnv`) that simulates server allocation dynamics.
- A custom neural network (`CustomNetwork`) for feature extraction in the PPO algorithm.
- A PPO model configured with a linear learning rate schedule and custom hyperparameters.

---

## Code Structure

The code is organized as follows:
- **Environment (`ServerAllocationEnv`)**: Defines the server allocation problem, including state representation, action space, and reward function.
- **Custom Neural Network (`CustomNetwork`)**: A feature extractor with separate policy and value networks.
- **PPO Model**: Configured with custom hyperparameters and trained in phases.
- **Utility Functions**: Includes a linear learning rate scheduler.

---

## Environment Details

### `ServerAllocationEnv`
This Gymnasium environment simulates a server allocation scenario with the following features:
- **State Space**: A 4-dimensional vector representing:
  1. Normalized number of active servers.
  2. Normalized queue length.
  3. Normalized current demand.
  4. Normalized demand trend.
- **Action Space**: Discrete actions:
  - `0`: Decrease the number of active servers.
  - `1`: Maintain the current number of active servers.
  - `2`: Increase the number of active servers.
- **Reward Function**: Balances server utilization, queue length, and server activation costs.
- **Demand Patterns**: Supports Poisson and sinusoidal demand patterns.

### Key Methods:
- `reset()`: Resets the environment to its initial state.
- `step(action)`: Executes an action and returns the next state, reward, and termination flags.
- `_get_state()`: Computes the current state of the environment.
- `_generate_demand()`: Generates demand based on the selected pattern.
- `_calculate_reward()`: Computes the reward based on utilization, queue length, and server activation.

---

## Custom Neural Network

### `CustomNetwork`
A custom feature extractor for the PPO algorithm, consisting of two separate networks:
1. **Policy Network**: Extracts features for the policy head.
2. **Value Network**: Extracts features for the value head.

Both networks use fully connected layers with ReLU activation and a Tanh output layer.

---

## Training Process

The PPO model is trained in three phases, each consisting of 200,000 timesteps. Key configurations include:
- **Learning Rate**: A linear schedule from `3e-4` to `1e-5`.
- **Batch Size**: 256.
- **Number of Epochs**: 10 per update.
- **Gamma**: 0.99 (discount factor).
- **GAE Lambda**: 0.98 (Generalized Advantage Estimation).
- **Clip Range**: 0.3 (clipping parameter for PPO).
- **Entropy Coefficient**: 0.02 (encourages exploration).
- **Value Function Coefficient**: 1.5 (weight for value loss).

Intermediate models can be saved after each phase, and the final model is saved at the end of training.

---
