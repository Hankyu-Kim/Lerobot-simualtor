# Lerobot Simulator

## Unity Files
Since the Unity project files are too large, I am currently exploring efficient ways to upload and share them.

---

# Task #1: Pick & Place

### Environment
In the simulator, we loaded the Lerobot URDF model.  
There are 4 blocks as distinguishable objects.  
For training, we do not manually move the robot, even though it is possible to control joint by joint.

### Collecting Datasets
Since the target block location is known, our code calculates the object coordinates, applies inverse kinematics (IK) to pick it up, and places it into the box.

https://github.com/user-attachments/assets/280587c1-ee26-4164-9d62-77098eaa6ef8

At this stage, we record videos from both the front camera and the top camera.

### Front-Camera & Top-Camera
<img width="640" height="480" alt="Image" src="https://github.com/user-attachments/assets/9c86d5bf-3e6f-43c3-8d92-82593125ff8f" />

<img width="640" height="480" alt="Image" src="https://github.com/user-attachments/assets/4bb6ecb1-da09-4200-8a37-b72cb77901f9" />

We split the videos frame by frame to build image-based datasets.

### Example Dataset Frames
<img width="942" height="602" alt="Image" src="https://github.com/user-attachments/assets/949e8997-d04d-480c-a21f-fde2af23f12a" />

<img width="942" height="602" alt="Image" src="https://github.com/user-attachments/assets/b1eebf10-11f3-4fbd-b7d5-568cf1486faf" />

### Training
We trained the models using Hugging Face Lerobot implementations: ACT, Diffusion Policy, and pi0.  
➡️ [Hugging Face Lerobot Models](https://huggingface.co/docs/lerobot/en/il_robots)

### Model Conversion
After training, we obtained the `model.safetensors` weight file and converted it into `model.onnx` using the `config.json`.  
Then, inference was performed with Unity Sentis or onnxruntime.

https://github.com/user-attachments/assets/0e19505c-c660-4c1a-b0ed-287cde2ed7eb

---

# Task #2: Sweep Block

This time, we collected real-world data using the physical Lerobot.  
Dataset reference: [Real Sweep Dataset](https://huggingface.co/spaces/lerobot/visualize_dataset?path=%2Fhank123456%2Freal_sweep_250821%2Fepisode_0)

We applied different models for comparison:

### 1. ACT (20,000 steps)
- Achieved over 90% success rate at specific poses.  
- However, if the block position shifted slightly (~5cm), the arm often passed above or below the block, leading to a steep performance drop.

### 2. Diffusion Policy (20,000 steps)
- Motions were choppy and slow.  
- No significant progress was observed once the arm approached the block.

### 3. Pi0Fast (20,000 steps)
- Catastrophic behavior: sudden jerky motions at regular intervals.  
- Robot frequently collided with itself (unstable control).  
- Environment issue detected: required downgrading `transformers` version for PaliGemma integration.

### Additional Improvements
1. Since the task involves many repetitive postures, the model tended to overfit and only performed well when sweeping from similar positions.  
   → To address this, we plan to introduce blocks of different colors and place them in varied positions across episodes.  

2. For Pi0Fast, a colleague achieved better performance by training far beyond 20,000 steps on an A100 GPU.  
   → Therefore, we also plan to conduct extended training sessions to improve stability and success rates.
