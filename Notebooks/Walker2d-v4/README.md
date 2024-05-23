# OpenAI CarRacing-v2 DeepRL training using PPO
## Loading the model from HuggingFace
```python
from huggingface_sb3 import load_from_hub

repo_id = "Amankankriya/ppo-Walker2d-v4"  # The repo_id
filename = "ppo-Walker2d-v4.zip"  # The model filename.zip

checkpoint = load_from_hub(repo_id, filename)
model = PPO.load(checkpoint, print_system_info=True)
```

## Understanding the Environment
The walker is a two-dimensional two-legged figure that consist of seven main body parts - a single torso at the top (with the two legs splitting after the torso), two thighs in the middle below the torso, two legs in the bottom below the thighs, and two feet attached to the legs on which the entire body rests. The goal is to walk in the in the forward (right) direction by applying torques on the six hinges connecting the seven body parts.  

**Actions:**<br />
 <img width="732" alt="Screenshot 2024-05-23 at 4 05 39 PM" src="https://github.com/bantu-4879/Atari_Games-Deep_Reinforcement_Learning/assets/75673216/f3021910-ee6d-4588-ae1f-282f45f57123">


**Observation Space:**<br />
By default, observation is a Box(-Inf, Inf, (17,), float64) 

**Rewards:**<br />
The reward consists of three parts:
- healthy_reward: Every timestep that the walker is alive, it receives a fixed reward of value healthy_reward,
- forward_reward: A reward of walking forward which is measured as forward_reward_weight * (x-coordinate before action - x-coordinate after action)/dt. dt is the time between actions and is dependeent on the frame_skip parameter (default is 4), where the frametime is 0.002 - making the default dt = 4 * 0.002 = 0.008. This reward would be positive if the walker walks forward (positive x direction).
- ctrl_cost: A cost for penalising the walker if it takes actions that are too large. It is measured as ctrl_cost_weight * sum(action2) where ctrl_cost_weight is a parameter set for the control and has a default value of 0.001

The total reward returned is reward = healthy_reward bonus + forward_reward - ctrl_cost and info will also contain the individual reward terms



