# Lerobot-simualtor

## Since Unity file is too large, I'm looking for some way to upload.

---

### Environment
In simulator, we loaded Lerobot URDF model.
There are 4 blocks which is the object to distinguish.
For training, we do not manually move the robot even we can move joint by joint.

### Collecting datasets
Since we know where the block we want is, our code calculate the coordinate of the object and apply IK for picking up and draw it into the box.

https://github.com/user-attachments/assets/280587c1-ee26-4164-9d62-77098eaa6ef8

At this moment, we record videos of front camera and top camera

### front-camera & top-camera
<img width="640" height="480" alt="Image" src="https://github.com/user-attachments/assets/9c86d5bf-3e6f-43c3-8d92-82593125ff8f" />

<img width="640" height="480" alt="Image" src="https://github.com/user-attachments/assets/4bb6ecb1-da09-4200-8a37-b72cb77901f9" />

We split it into frame by frame and collect dataset as an images.

### front-camera & top-camera datasets
<img width="942" height="602" alt="Image" src="https://github.com/user-attachments/assets/949e8997-d04d-480c-a21f-fde2af23f12a" />

<img width="942" height="602" alt="Image" src="https://github.com/user-attachments/assets/b1eebf10-11f3-4fbd-b7d5-568cf1486faf" />

### Training
We train the model by using hugging face lerobot models : ACT, Diffusion Policy, pi0.
(https://huggingface.co/docs/lerobot/en/il_robots)

### Converting model
After getting weight file(model.safetensors), we convert it into model.onnx using config.json file.
And then, we made the inference code to run the model.onnx file using sentis package or onnxruntime.

https://github.com/user-attachments/assets/0e19505c-c660-4c1a-b0ed-287cde2ed7eb
