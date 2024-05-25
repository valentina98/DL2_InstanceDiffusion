# Run tests

Prerequisites (described in RunPaper.md): 
- Setup the project
- Start an interactive session
- Activate the environment

## Test different inputs

```bash
/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 3 \
  --output ./output_tests/ \
  --input_json demos/test_points_crogi_kitchen.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_point.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4

/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 3 \
  --output ./output_tests/ \
  --input_json demos/test_scribble_crogi_kitchen.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_scribble.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4\

/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 3 \
  --output ./output_tests/ \
  --input_json demos/test_bbox_crogi_kitchen.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_box.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4
  
/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 3 \
  --output ./output_tests/ \
  --input_json demos/test_mask_crogi_kitchen.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_mask.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4
```

## Generate images with points

*Idea: close points*

### Attempt to generate a panda and very close to each other balloons

```bash
/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 3 \
  --output ./output_tests/ \
  --input_json demos/test_point_panda_balloons1.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_point.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4
  
/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 3 \
  --output ./output_tests/ \
  --input_json demos/test_point_panda_balloons2.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_point.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4
```

## Generate images with bounding boxes

*Idea: overlapping boxes*

### Attempt to generate an apple in front of a pear:

When the 2 fruits are specified at the same hight and same size, the model assumes more often the pear in front of the apple (no matter which one is on the left or right).

```bash
/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 3 \
  --output ./output_tests/ \
  --input_json demos/test_bbox_apple_pear1.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_box.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4 

/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 3 \
  --output ./output_tests/ \
  --input_json demos/test_bbox_pear_apple1.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_box.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4
```

 We provide instructions to contradict that. Instead of seeing the apple at the front, we observe how the fruits blend together.

```bash
/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 3 \
  --output ./output_tests/ \
  --input_json demos/test_bbox_apple_pear2.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_box.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4 

/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 3 \
  --output ./output_tests/ \
  --input_json demos/test_bbox_pear_apple2.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_box.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4
```

Then we decide to compare with the output using the less restrictibe points:

```bash
/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 3 \
  --output ./output_tests/ \
  --input_json demos/test_point_apple_pear2.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_box.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4 

/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 3 \
  --output ./output_tests/ \
  --input_json demos/test_point_pear_apple2.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_box.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4
```

### Attempt to generate vase behind a flower:

*Idea: contradict perspective*

The model understands perspective and tries to align the objects accordingly - if something is on the table and higher than the vase, it should be behind it. 
However, we provide text instructions for the vase being to the front. 

```bash
/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 3 \
  --output ./output_tests/ \
  --input_json demos/test_bbox_vase_flower_table.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_box.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4
```

## Generate images with scribbles

*Idea: depth with scribbles*

### Attempt to generate scenes

We defined a scene with a bear, iceberg and an igloo. We attempted to use scribbles (a list of points inside the bounding box) to give the model a cue if the igloo or the iceberg should be in the front. In the first example we input the igloo behind the iceberg (we specified no points for the igloo at the overlapping area), and in the second example it is the other way around. 
This did not work. The same images got generated.

*Why this didn't work?*

In the UniFusion block different inputs are separately tokenized and fed to the encoder. It is described that adding more inputs improves the precision of the output, which we see in the demos, however, we notice that there is no effect in the case of adding scribbles.

```bash
/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 3 \
  --output ./output_tests/ \
  --input_json demos/test_scribble_polar_bear_iceberg_igloo1.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_scribble.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4 

/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 3 \
  --output ./output_tests/ \
  --input_json demos/test_scribble_polar_bear_iceberg_igloo2.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_scribble.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4 
```

```bash
/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 3 \
  --output ./output_tests/ \
  --input_json demos/test_scribble_polar_bear_iceberg_igloo3.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_scribble.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4 
  
/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 3 \
  --output ./output_tests/ \
  --input_json demos/test_scribble_polar_bear_iceberg_igloo4.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_scribble.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4 
  
/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 3 \
  --output ./output_tests/ \
  --input_json demos/test_scribble_polar_bear_iceberg_igloo5.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_scribble.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4 
```

## Generate concepts/instances turning heads

*Idea: experiment with a complex animal*

### We attempt to generate a donkey looking to the left and the same donkey looking to the right

```bash
/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 3 \
  --output ./output_tests/ \
  --input_json demos/test_direction_donkey_left.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_box.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4 

/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 3 \
  --output ./output_tests/ \
  --input_json demos/test_direction_donkey_right.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_box.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4 
```
