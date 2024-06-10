# OpenAI Half Cheetah v4 DeepRL training using DDPG and custom network architecture

## Loading the model from HuggingFace
```python
from huggingface_sb3 import load_from_hub

repo_id = "Amankankriya/DDPG-HalfCheetah-v4"  # The repo_id
filename = "DDPG-HalfCheetah-v4.zip"  # The model filename.zip

checkpoint = load_from_hub(repo_id, filename)
model = PPO.load(checkpoint, print_system_info=True)
```

## Understanding the Environment
This environment is based on the work by P. Wawrzyński in “A Cat-Like Robot Real-Time Learning to Run”. The HalfCheetah is a 2-dimensional robot consisting of 9 body parts and 8 joints connecting them (including two paws). The goal is to apply a torque on the joints to make the cheetah run forward (right) as fast as possible, with a positive reward allocated based on the distance moved forward and a negative reward allocated for moving backward. The torso and head of the cheetah are fixed, and the torque can only be applied on the other 6 joints over the front and back thighs (connecting to the torso), shins (connecting to the thighs) and feet (connecting to the shins).
<br />
<br />
**Actions:**<br />
<img width="740" alt="Screenshot 2024-06-10 at 2 44 16 PM" src="https://github.com/bantu-4879/Atari_Games-Deep_Reinforcement_Learning/assets/75673216/a17a9270-4fd3-4b07-8a90-7d79c367c223">
<br />
<br />
**Observation Space:**<br />
By default, observation is a Box(-Inf, Inf, (17,), float64)
<br />
<br />
**Rewards:**<br />
The reward consists of two parts:

- forward_reward: A reward of moving forward which is measured as forward_reward_weight * (x-coordinate before action - x-coordinate after action)/dt. dt is the time between actions and is dependent on the frame_skip parameter (fixed to 5), where the frametime is 0.01 - making the default dt = 5 * 0.01 = 0.05. This reward would be positive if the cheetah runs forward (right).

- ctrl_cost: A cost for penalising the cheetah if it takes actions that are too large. It is measured as ctrl_cost_weight * sum(action2) where ctrl_cost_weight is a parameter set for the control and has a default value of 0.1

The total reward returned is reward = forward_reward - ctrl_cost and info will also contain the individual reward terms
