# OpenAI Galaxian-v5 DeepRL training using PPO
## Loading the model from HuggingFace
```python
from huggingface_sb3 import load_from_hub

repo_id = "Amankankriya/ppo-CarRacing-v2"  # The repo_id
filename = "ppo-CarRacing-v2.zip"  # The model filename.zip

checkpoint = load_from_hub(repo_id, filename)
model = PPO.load(checkpoint, print_system_info=True)
```

## Understanding the Environment
**Actions:**<br />
Galaxian has the action space of Discrete(6) with the table below listing the meaning of each action’s meanings.  
0 - Noop  
1 - Fire  
2 - Right  
3 - Left  
4 - RightFire  
5 - LeftFire  


**Observation Space:**<br />
- obs_type="rgb" -> observation_space=Box(0, 255, (210, 160, 3), np.uint8)  
- obs_type="grayscale" -> Box(0, 255, (210, 160), np.uint8), a grayscale version of the “rgb” type

## Parameters
<table>
  <tr>

| Parameter | Value |
|--|--|
| Policy | CnnPolicy |
| Learning rate | 0.00005 |
| batch_size  | 128 |
| n_epochs | 4 |
| Timesteps | 1e7 |

</tr>
</table>

## Training

<div style="display: flex; justify-content: center;">
  <div>
    <p align="center">Before Training</p>
    <video src="https://github.com/bantu-4879/Atari_Games-Deep_Reinforcement_Learning/assets/75673216/96c99b92-77e5-46d3-8cba-708f5cb3f297" width="400" height="300" controls/>
  </div>
  <div>
    <p align="center">After Training</p>
    <video src="https://github.com/bantu-4879/Atari_Games-Deep_Reinforcement_Learning/assets/75673216/3e5d93b2-0dd1-438b-8dec-e62c134bb80c" width="400" height="300" controls/>
  </div>
</div>

## At different parameters

| Learning Rate | Mean Reward | Deviation |
|---------------|-------------|-----------|
| 0.00001       | -59.55538536| 25.310341824182267 |
| 0.00005       | 289.0281644 | 120.81766432392448 |
| 0.0001        | 81.42063832 | 76.9650508453757   |
| 0.0002        | 65.2997484  | 72.21204323808004  |
| 0.0005        | -14.38487644| 61.22441748132813  |

# Problem
I have tried variety of parameters and figured out that this combination is working in right direction and also fast enough. But after certain training stage, it is not able to learn further. I understand that it is struck at local minimum, but it is not even a acceptable local minimum. If you are reading this please help me figure out how should i get out of that local minimum?



 
