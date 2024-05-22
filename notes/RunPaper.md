# Reproduce the paper

## Setup the project

### Download the pretrained Instance Diffusion:

```bash
cd pretrained

wget https://huggingface.co/runwayml/stable-diffusion-v1-5/resolve/main/v1-5-pruned-emaonly.ckpt
wget "https://huggingface.co/xudongw/InstanceDiffusion/resolve/main/instancediffusion_sd15.pth?download=true" -O instancediffusion_sd15.pth

cd ../
```

### Start an interactive session:
Run the following and wait until your session starts:
```bash
srun  --partition=gpu --gres=gpu:1 --cpus-per-task=9 --gpus=1 --job-name=Interactive --ntasks=1 --time=01:00:00 --mem=32000M --pty /bin/bash
```

You can exit it any time with:
```bash
exit
```

### Activate the environment:
```bash
# Load the correct modules
module purge
module load 2023
module load Anaconda3/2023.07-2 #
module load CUDA/12.1.1

# Activate the conda environment
source activate instdiff1
```
## InstanceDiffusion Inference Demons

(sad hack to use the correct python)

```bash
/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 8 \
  --output ./output/ \
  --input_json demos/demo_cat_dog_robin.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_box.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4
```

### Let's Get Everybody Turning Heads!
```bash
/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 8 \
  --output ./output/ \
  --input_json demos/demo_eagle_left.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_box.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.2 \
  --cascade_strength 0.4
```

### Image Generation Using Single Points
```bash
/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 8 \
  --output ./output/ \
  --input_json demos/demo_corgi_kitchen.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_point.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.2 \
  --cascade_strength 0.4
```

### Iterative Image Generation
```bash
/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 8 \
  --output ./output/ \
  --input_json demos/demo_iterative_r1.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_box.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.2 \
  --cascade_strength 0.4
```

## Model Quantitative Evaluation on MSCOCO (Zero-shot)

### Get COCO dataset

Execute the following (you don't need an interactive session for this):
```bash
mkdir datasets
cd datasets
mkdir coco
mkdir annotations

# Download validation images
wget http://images.cocodataset.org/zips/val2017.zip

# Extract validation images
unzip val2017.zip -d coco/images

# Download annotations
wget http://images.cocodataset.org/annotations/annotations_trainval2017.zip

# Extract the annotations
unzip annotations_trainval2017.zip -d coco/annotations

# Move the files to the correct location
mv coco/annotations/annotations/* coco/annotations/

# Clean up
rm -r datasets/coco/annotations/annotations

# Download the customized instances_val2017.json (as per documentation)
wget "https://drive.google.com/uc?export=download&id=1sYpb7jRZJyBJYPFHyjxosIDaiQhkrEhU" -O annotations/instances_val2017.json

cd ../
```

--- 
***ToDo: The COCO dataset is too big. We have to use only part of it.***

Once you have organized the data, proceed with executing the following commands (you need an interactive session for this):
```bash
CUDA_VISIBLE_DEVICES=0 /home/scur0411/.conda/envs/instdiff1/bin/python eval_local.py \
    --job_index 0 \
    --num_jobs 1 \
    --use_captions \
    --save_dir "eval-cocoval17" \
    --ckpt_path pretrained/instancediffusion_sd15.pth \
    --test_config configs/test_mask.yaml \
    --test_dataset cocoval17 \
    --mis 0.36 \
    --alpha 1.0
```

```bash
/home/scur0411/.conda/envs/instdiff1/bin/pip install ultralytics
mv datasets/coco/images/val2017 datasets/coco/images/val2017-official
ln -s generation_samples/eval-cocoval17 datasets/coco/images/val2017
/home/scur0411/.conda/envs/instdiff1/bin/yolo val segment model=yolov8m-seg.pt data=coco.yaml device=0
```

### Attribute Binding
```bash
test_attribute="colors" # colors, textures
CUDA_VISIBLE_DEVICES=0 /home/scur0411/.conda/envs/instdiff1/bin/python eval_local.py \
    --job_index 0 \
    --num_jobs 1 \
    --use_captions \
    --save_dir "eval-cocoval17-colors" \
    --ckpt_path pretrained/instancediffusion_sd15.pth \
    --test_config configs/test_mask.yaml \
    --test_dataset cocoval17 \
    --mis 0.36 \
    --alpha 1.0
    --add_random_${test_attribute}

# Eval instance-level CLIP score and attribute binding performance
/home/scur0411/.conda/envs/instdiff1/bin/python eval/eval_attribute_binding.py --folder eval-cocoval17-colors --test_random_colors
```
## InstanceDiffusion Model Training

*ToDo*
