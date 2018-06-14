---
title: Day 4 Notes
layout: note
---

# Day 4 Notes

## CartPole learning agent

```
# from: https://medium.com/@tuzzer/cart-pole-balancing-with-q-learning-b54c6068d947

import gym
import numpy as np
import random
import math
from time import sleep


## Initialize the "Cart-Pole" environment
env = gym.make('CartPole-v0')

## Defining the environment related constants

# Number of discrete states (bucket) per state dimension
NUM_BUCKETS = (1, 1, 6, 3)  # (x, x', theta, theta')
# Number of discrete actions
NUM_ACTIONS = env.action_space.n # (left, right)
# Bounds for each discrete state
STATE_BOUNDS = list(zip(env.observation_space.low, env.observation_space.high))
STATE_BOUNDS[1] = [-0.5, 0.5]
STATE_BOUNDS[3] = [-math.radians(50), math.radians(50)]
# Index of the action
ACTION_INDEX = len(NUM_BUCKETS)

## Creating a Q-Table for each state-action pair
q_table = np.zeros(NUM_BUCKETS + (NUM_ACTIONS,))

## Learning related constants
MIN_EXPLORE_RATE = 0.01
MIN_LEARNING_RATE = 0.1

## Defining the simulation related constants
NUM_EPISODES = 1000
MAX_T = 250
STREAK_TO_END = 120
SOLVED_T = 199
DEBUG_MODE = True

def simulate():

    ## Instantiating the learning related parameters
    learning_rate = get_learning_rate(0)
    explore_rate = get_explore_rate(0)
    discount_factor = 0.99  # since the world is unchanging

    num_streaks = 0

    for episode in range(NUM_EPISODES):

        # Reset the environment
        obv = env.reset()

        # the initial state
        state_0 = state_to_bucket(obv)

        for t in range(MAX_T):
            env.render()

            # Select an action
            action = select_action(state_0, explore_rate)

            # Execute the action
            obv, reward, done, _ = env.step(action)

            # Observe the result
            state = state_to_bucket(obv)

            # Update the Q based on the result
            best_q = np.amax(q_table[state])
            q_table[state_0 + (action,)] += learning_rate*(reward + discount_factor*(best_q) - q_table[state_0 + (action,)])

            # Setting up for the next iteration
            state_0 = state

            # Print data
            if (DEBUG_MODE):
                print("\nEpisode = %d" % episode)
                print("t = %d" % t)
                print("Action: %d" % action)
                print("State: %s" % str(state))
                print("Reward: %f" % reward)
                print("Best Q: %f" % best_q)
                print("Explore rate: %f" % explore_rate)
                print("Learning rate: %f" % learning_rate)
                print("Streaks: %d" % num_streaks)

                print("")

            if done:
               print("Episode %d finished after %f time steps" % (episode, t))
               if (t >= SOLVED_T):
                   num_streaks += 1
               else:
                   num_streaks = 0
               break

            #sleep(0.25)

        # It's considered done when it's solved over 120 times consecutively
        if num_streaks > STREAK_TO_END:
            break

        # Update parameters
        explore_rate = get_explore_rate(episode)
        learning_rate = get_learning_rate(episode)


def select_action(state, explore_rate):
    # Select a random action
    if random.random() < explore_rate:
        action = env.action_space.sample()
    # Select the action with the highest q
    else:
        action = np.argmax(q_table[state])
    return action


def get_explore_rate(t):
    return max(MIN_EXPLORE_RATE, min(1, 1.0 - math.log10((t+1)/25)))

def get_learning_rate(t):
    return max(MIN_LEARNING_RATE, min(0.5, 1.0 - math.log10((t+1)/25)))

def state_to_bucket(state):
    bucket_indice = []
    for i in range(len(state)):
        if state[i] <= STATE_BOUNDS[i][0]:
            bucket_index = 0
        elif state[i] >= STATE_BOUNDS[i][1]:
            bucket_index = NUM_BUCKETS[i] - 1
        else:
            # Mapping the state bounds to the bucket array
            bound_width = STATE_BOUNDS[i][1] - STATE_BOUNDS[i][0]
            offset = (NUM_BUCKETS[i]-1)*STATE_BOUNDS[i][0]/bound_width
            scaling = (NUM_BUCKETS[i]-1)/bound_width
            bucket_index = int(round(scaling*state[i] - offset))
        bucket_indice.append(bucket_index)
    return tuple(bucket_indice)

if __name__ == "__main__":
    simulate()

```

## LunarLander learning agent

```
# This code is inspired by Matthew Chan's solution to the Cart-Pole problem:
# https://gist.github.com/tuzzer/90701191b50c2e7bafca167858fcb234

import gym

env = gym.make('LunarLander-v2')

# Nop, fire left engine, main engine, right engine
ACTIONS = env.action_space.n

# Landing pad is always at coordinates (0,0). Coordinates are the first two numbers in state vector.
# Reward for moving from the top of the screen to landing pad and zero speed is about 100..140 points.
# If lander moves away from landing pad it loses reward back. Episode finishes if the lander crashes or
# comes to rest, receiving additional -100 or +100 points. Each leg ground contact is +10. Firing main
# engine is -0.3 points each frame. Solved is 200 points.

import numpy as np
import random

def discretize_state(state):
    dstate = list(state[:5])
    dstate[0] = int(0.5*(state[0]+0.7)*10/2.0) # pos x
    dstate[1] = int(0.5*(state[1]+0.5)*10/2.0) # pos y
    dstate[2] = int(0.5*(state[2]+1.5)*10/3.0) # vel x
    dstate[3] = int(0.5*(state[3]+2)*10/3.0) # vel y
    dstate[4] = int(0.5*(state[4]+3.14159)*10/(2*3.14159)) # angle
    if dstate[0] >= 5: dstate[0] = 4
    if dstate[1] >= 5: dstate[1] = 4
    if dstate[2] >= 5: dstate[2] = 4
    if dstate[3] >= 5: dstate[3] = 4
    if dstate[4] >= 5: dstate[4] = 4
    if dstate[0] < 0: dstate[0] = 0
    if dstate[1] < 0: dstate[1] = 0
    if dstate[2] < 0: dstate[2] = 0
    if dstate[3] < 0: dstate[3] = 0
    if dstate[4] < 0: dstate[4] = 0
    return tuple(dstate)

def run(num_episodes, alpha, gamma, explore_mult):
    max_rewards = []
    last_reward = []
    qtable = np.subtract(np.zeros((5, 5, 5, 5, 5, ACTIONS)), 100) # start all rewards at -100
    explore_rate = 1.0
    for episode in range(num_episodes):
        s = env.reset()
        state = discretize_state(s)
        
        for step in range(10000):
            #env.render()

            # select action
            if random.random() < explore_rate:
                action = random.choice(range(ACTIONS))
            else:
                action = np.argmax(qtable[state])

            (new_s, reward, done, _) = env.step(action)
            new_state = discretize_state(new_s)

            # update Q
            best_future_q = np.amax(qtable[new_state]) # returns best possible reward from next state
            prior_val = qtable[state + (action,)]
            qtable[state + (action,)] = (1.0-alpha)*prior_val + alpha*(reward + gamma * best_future_q)
            state = new_state
            
            if done or step == 9999:
                last_reward.append(reward)
                break
        
        if explore_rate > 0.01:
            explore_rate *= explore_mult    
        max_rewards.append(np.amax(qtable))
        
    return (max_rewards, last_reward[-50:], qtable) # return rewards from last 50 episodes


num_episodes = 200
for alpha in [0.05, 0.10, 0.15]:
    for gamma in [0.85, 0.90, 0.95]:
        (max_rewards, last_reward, _) = run(num_episodes=num_episodes, alpha=alpha, gamma=gamma, explore_mult=0.995)
        print("alpha = %.2f, gamma = %.2f, mean last 50 outcomes = %.2f, q max: %.2f, q mean: %.2f" % (alpha, gamma, np.mean(last_reward), np.max(max_rewards), np.mean(max_rewards)))

(max_rewards, last_reward, qtable) = run(num_episodes=200, alpha=0.1, gamma=0.95, explore_mult=0.995)
print("mean last 50 outcomes = %.2f, q max: %.2f, q mean: %.2f" % (np.mean(last_reward), np.max(max_rewards), np.mean(max_rewards)))
np.save('qtable.npy', qtable)

# Use best qtable to play the game (no learning anymore)
import gym
import numpy as np
env = gym.make('LunarLander-v2')
qtable = np.load('qtable.npy')
for i in range(100):
    s = env.reset()
    state = discretize_state(s)
    for step in range(10000):
        env.render()

        # select action
        action = np.argmax(qtable[state])

        (new_s, reward, done, _) = env.step(action)
        new_state = discretize_state(new_s)

        if done or step == 9999:
            break

        state = new_state
```

