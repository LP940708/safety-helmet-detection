# Helmet Dataset + Helmet Object Detection
Dataset from: https://github.com/wujixiu/helmet-detection

## Getting Started alias Training a Network
```bash
git clone
cd helmet-dataset
```

Download the dataset and move it into the `helmet-dataset` folder. You will need to rename the dataset folder to `data`.

```bash
docker build - < Dockerfile --tag helmet-dataset-train

nvidia-docker run -v /helmet-dataset:/helmet-dataset -it helmet-dataset-train bash
cd /helmet-dataset

./download_checkpoints.sh --network_type mobilenet_v2_ssd --train_whole_model false
create_dataset.sh

./retrain_detection_model.sh
```
Right now, there is only Mobilenetv2 and Mobilenetv1 both with SSD (`mobilenet_v2_ssd` and `mobilenet_v1_ssd`) available. You can train the whole model or just the last layers. In order to change the number of steps, you will need to change value of `num_steps` inside the `pipeline.config` file which is in the `ckpt` folder after downloading the checkpoint.

## Deploying
Get the `models` folder on a device where the `edgetpu-compiler` is installed (Follow https://coral.withgoogle.com/docs/edgetpu/compiler/ to install the compiler)
```
cd models/
edgetpu_compiler output_tflite_graph.tflite 
```
This will create `output_tflite_graph_edgetpu.tflite` which can be used with the Google Coral Sticks!

## Running the models
Demo codes from: https://coral.withgoogle.com/docs/accelerator/get-started/#set-up-on-linux-or-raspberry-pi

To detect on a single image run:
```bash
python detect_image.py --model="./models/output_tflite_graph_edgetpu.tflite \
		       --label="./models/labels.txt" \
		       --input="./path/to/image"
```

To detect on images from a webcam run:
```bash
python detect_camera.py --model="./models/output_tflite_graph_edgetpu.tflite \
			--labels="./models/labels.txt"
```

## Examples

![](https://raw.githubusercontent.com/wujixiu/helmet-detection/master/hardhat-wearing-detection/imgs/00163.jpg)

![](https://raw.githubusercontent.com/wujixiu/helmet-detection/master/hardhat-wearing-detection/imgs/00197.jpg)

![](https://raw.githubusercontent.com/wujixiu/helmet-detection/master/hardhat-wearing-detection/imgs/00250.jpg)
