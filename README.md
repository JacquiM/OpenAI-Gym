# OpenAI-Gym
This repository contains a solution to the Mountain Car Demo as well as the Cart Poll Demo from the OpenAI Gym library

## Cart Pole (V1) ##
According to OpenAI (2018a), the CartPole problem involves a pole which is attached to a cart which moves along a frictionless track. The controls of the system apply force of either +1 or -1 to the cart. The pole starts upright and should not fall over. A reward of +1 is provided each time that the pole remains upright (per step) and the episode comes to an end when the pole is no longer less than 15 degrees from its starting vertical position. In a case where the cart moves more than 2.4 units from the center, the episode is ended too. The approach used in this solution was DDQN.

![](cartpole.gif)

### The step-for-step approach is outlined below: ###
- Load important parameters
- Initialise attributes
- Build online and target networks
  - Assign place holders for the input state, the input action and target
  - Assign online network variables
  - Assign target network variables
  - Calculate the Q function and the loss
  - Train operations
      - Sample from memory
      - Generate targets
      - Train and write summaries
      - Decay epsilon
- Merge summaries
- Initialise variables and summary writer
- Synchronise online and target network
  - Transfer online network values into the target network

### The main algorithm follows the following logical flow: ###
- Starts a loop through the number of episodes
- Chooses an action using Epsilon greedy policy which:
    - Passes forward
    - Retrieves the maximum
    - Depending on whether the pass is greater than the maximum, a new choice is either explored or an old choice is exploited
- Takes an action by stepping through the environment
- If the state is bad, it is penalized
- Old choices, actions, rewards, new actions and done status are memorized
    - Convert action to hot representation
    - Fix dimensions
    - Add into replay buffer and if necessary pop oldest memory
- Train
    - Sample from memory
    - Generate targets
    - Train and write summaries
    - Decay epsilon
- Move forward
- Append results and check to see if it has been solved

### The following factors support the choice to use DDQN to solve this activity: ###
- Handles the problem of overestimating Q-values
- Increases training speed

The average number of episodes is 81 episodes before a stable solution is found and no more training is to be done. The diagram below is a plot that shows the number of steps in relation to the episode number.

## Mountain Car (V0) ##
According to OpenAI (2018b), the MountainCar problem is one that involves a one-dimensional track with a car that is positioned on this track, between two mountains. The goal of this problem is to get the car on top of the mountain on the right with the constraint of the car’s engine not being strong enough to successfully make the trip. Thus, the care needs to drive back and forth to build enough momentum to reach the goal. The approach used in this solution is a Deep-Q learning model

![](mountaincar.gif)

### The step-for-step approach is outlined below: ###
- Load important parameters
- Initialise attributes
- Build model
  - Make the model sequential
  - Add a Dense layer with two input nodes, using relu as the activation function
  - Add another dense layer using relu activation function
  - Add another dense layer using linear activation
- Update target model
  o Transfer the weights from the model to the target model
- Remember the state, action, reward, next state and done status
- Replay
- Save model

### The main algorithm follows the following logical flow: ###
- Starts a loop through the number of episodes
- Act
  o Select random action according to the state which is predicted and return the action
- Step through the environment
- Validate states and assign rewards
- If done, add one to the reward else subtract one from the reward
- Remember the state, action, reward, next state and done status
- Add the reward to the score
- Test whether done
  o If done, display score
  o Else replay
- Save the model

### The following factors support the choice to use DDQN to solve this activity: ###
- Reduces correlation between experiences in updating the DNN
- Increases learning speed with min-batches
- Reuses past transitions to avoid catastrophic forgetting
- Reduces complexity
- Reduces computation time needed for training

### Singh (2018) investigated the following approaches to dealing with the Mountain Car activity: ###
- Random movements: On 10 trail runs the maximum time of survival is 118 timesteps with an average survival time of roughly 21 steps
- Weight vector: On 10 trail runs the maximum score achieved was 762 with an average score of 315
- Deep neural networks: The model is only added to the training set if the cart successfully balances the pole for more than 100 time steps
- Deep Q Networks: The pole was balanced for more than 2000 timeframes

From the specifications above, it can be seen that the best approach would be to use Deep Q networks.

The adjustment to time yields a difference in number of episodes it takes to reach the top. The first attempt of training this model took quite a large amount of time, however, only seven episodes were needed for the car to reach the top of the mountain with a time range of 200 units.

There was a decrease in episodes when looking at a time range of 100 units instead of 10. Although the number episodes taken to reach the top decreases, the speed at which the model is trained increases exponentially.

There is a tradeoff between training time and number of episodes needed to complete the challenge. If one were to favor time over number of episodes, one should decrease the time range, however, if one were to favor number of episodes, the time range should be increased.
