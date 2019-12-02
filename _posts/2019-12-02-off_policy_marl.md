---
layout: article
title: Off-policy Multi-agent Reinforcement Learning
tags: RL Multi-agent
mathjax: true
---

  *Multi-agent reinforcement learning has been one of my favoriate reseach topic, among all the sub-subjects, I am most interested in how to do off-policy multi-agent learning. Why off-policy is difficult in MARL setting? One obvious reason is that all the agent's policy are changing all the time, so it's hard to reuse the experience. You can no longer simply apply important sampling as other single agent off-policy methods do, because other agents' policies are causing non-stationarity. So how do we stablising the experience replay is one question we need to answer. Until now, the best paper I can find in this topic is <a href="https://arxiv.org/abs/1702.08887">Stabilising Experience Replay for Deep Multi-Agent Reinforcement Learning</a>. They introduced two methods of stabalising experiment replay. Here I want to share with you about the ideas. *


### <a href="https://arxiv.org/abs/1702.08887">Stabilising Experience Replay for Deep Multi-Agent Reinforcement Learning</a>

#### Method 1:  Multi-agent importance sampling

To tackle the non-stationarity of the env, they interpret the replay memory as the off-environment data.What is off-environment? If we treat other agents as part of the enviornment, and the environment is not stable, it changes all the time, so the experience you get some another "state" of the environment is called off-environment data. Then they augment the data with the probability of the joint action in the experience tuple. The tuples are stored in a replay buffer. 


Another problem in multi-agent off-policy is that we can hardly tell which observation with the action combination represent which state of the game, so that instead of using the original observation, they use the action trajectories as the state input, to eliminate the influence the changing policies in off-policy learning. Because we know that a certain history will always lead to the same state if your env is deterministic, even in multi-agent setting.  

This importance sampling method gives unbiased estimation of the true value, but has large and unbounded variance. In order to reduce the variance they introduced another method. 

#### Method 2: Multi-agent Fingerprints 

Importance sampling is unbiased but brings large and unbounded variance. So instead of correcting the non-stationarity like the above method, they proposed the method embraces the non-stationarity. 

The sequence of policies that generated the data in the buffer can be thought of as following a single, one-dimensional trajectory through the high-dimensional policy space. If you want to know how far you are away from an experience, the simplest way is to find out at which training iteration you got this slice of data, so that the iteration number could be a good choice of the fingerprint. Also besides the fingerprint, they also added the exploration rate as an augumentation to the observation which evoles smoothly during the training. 


#### More words: 

They have done different implementations, with RNN with simple Feed-forward, and the experiments results are quite intersting. The finger print idea sounds really simple but it works well even better than importance sample. The inspriation for us is that sometimes an diffult problem can be solved with just simple ideas, so don't criticise your idea too eary before really trying it out. 






