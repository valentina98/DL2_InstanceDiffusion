# Run tests

Prerequisites (described in RunPaper.md): 
- Setup the project
- Start an interactive session
- Activate the environment

## Generate images with points

*Idea: close points - ToDo*

### Attempt to generate a panda and balloons very close to each other

```bash
/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 3 \
  --output ./output_tests/ \
  --input_json demos/test_points_panda_balloons.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_points.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4
```

## Generate images with bounding boxes

*Idea: overlapping boxes*

### Attempt to generate an apple in front of a pear:

When the 2 fruits are specified at the same hight and same size, the model always assumes the pear in front of the apple. We attempted to provide instructions to contradict that. The 

```bash
/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 1 \
  --output ./output_tests/ \
  --input_json demos/test_bbox_apple_pear.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_box.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4 

/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 1 \
  --output ./output_tests/ \
  --input_json demos/test_bbox_pear_apple.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_box.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4
```

### Attempt to generate vase behind a flower:

The model understands perspective and tries to align the objects accordingly - if something is on the table and higher than the vase, it should be behind. 
However, we provide text instructions for the vase being behind the flower. 
Masks could be used for this use-case. *ToDo: test masks*

```bash
/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 1 \
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

*Idea: crossing scribbles*

### Attempt to generate scenes

We defined a scene with a bear, iceberg and an igloo. We attempted to use scribbles (a list of points inside the bounding box) to specify if the igloo or the iceberg is in the front. In the first example the igloo is behind the iceberg (no points are specified for the igloo at the overlapping area), and in the second example it is the other way around. 
This did not work. The same images got generated.
*ToDo: Why this didn't work?*

```bash
# /home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
#   --num_images 1 \
#   --output ./output_tests/ \
#   --input_json demos/test_bbox_polar_bear_iceberg_igloo.json \
#   --ckpt pretrained/instancediffusion_sd15.pth \
#   --test_config configs/test_scribble.yaml \
#   --guidance_scale 7.5 \
#   --alpha 0.8 \
#   --seed 0 \
#   --mis 0.36 \
#   --cascade_strength 0.4 

/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 4 \
  --output ./output_tests/ \
  --input_json demos/test_bbox_polar_bear_iceberg_igloo1.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_scribble.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4 

/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 4 \
  --output ./output_tests/ \
  --input_json demos/test_bbox_polar_bear_iceberg_igloo2.json \
  --ckpt pretrained/instancediffusion_sd15.pth \
  --test_config configs/test_scribble.yaml \
  --guidance_scale 7.5 \
  --alpha 0.8 \
  --seed 0 \
  --mis 0.36 \
  --cascade_strength 0.4 
```

## Generate concepts/instances turning heads


*Idea: more complex animal turning its head - ToDo*

### We attempt to generate a donkey turning its head along with its ears

```bash
/home/scur0411/.conda/envs/instdiff1/bin/python inference.py \
  --num_images 4 \
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
  --num_images 4 \
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
