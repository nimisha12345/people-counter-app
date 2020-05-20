# Project Write-Up

You can use this document as a template for providing your project write-up. However, if you
have a different format you prefer, feel free to use it as long as you answer all required
questions.

## Explaining Custom Layers

The rundown of supported layers from prior legitimately identifies with whether a given layer is a custom layer. Any layer not in that rundown is naturally named a custom layer by the Model Optimizer. To really include custom layers, there are a couple of contrasts relying upon the first model system. In both TensorFlow and Caffe, the principal alternative is to enroll the custom layers as augmentations to the Model Optimizer. 

For Caffe, the subsequent choice is to enroll the layers as Custom, at that point use Caffe to figure the yield state of the layer. You'll require Caffe on your framework to do this choice. 

For TensorFlow, its subsequent choice is to really supplant the unsupported subgraph with an alternate subgraph. The last TensorFlow alternative is to really offload the calculation of the subgraph back to TensorFlow during derivation. 

You'll get an opportunity to rehearse this in the following activity. Once more, as this is a propelled point, we won't dig a lot of more profound here, yet don't hesitate to look at the connected documentation on the off chance that you need to know more.
The process behind converting custom layers involves the following.

-Generate the Extension Template Files Using the Model Extension Generator
-Using Model Optimizer to Generate IR Files Containing the Custom Layer
-Edit the CPU Extension Template Files
-Execute the Model with the Custom Layer
I used ssd_mobilenet_v2_coco_2018_03_29 model for person detection. First of all i used wget to download the model from the repository via link http://download.tensorflow.org/models/object_detection/ssd_mobilenet_v1_coco_2018_01_28.tar.gz. It is Supported Frozen Topology from TensorFlow Object Detection Models Zoo. after that i unzipped the tar using Tar -xvf command. 
using tar -xvf ssd_mobilenet_v2_coco_2018_03_29.tar.gz
after that for converting model to IR i  downloaded SSD MobileNet V2 COCO model's .pb file using the model optimizer using the command.
python /opt/intel/openvino/deployment_tools/model_optimizer/mo.py --input_model frozen_inference_graph.pb --tensorflow_object_detection_api_pipeline_config pipeline.config --reverse_input_channels --tensorflow_use_custom_operations_config /opt/intel/openvino/deployment_tools/model_optimizer/extensions/front/tf/ssd_v2_support.json

In above command opt/intel/openvino/deployment_tools/model_optimizer/mo.py path is path to model optimizer file. The next argument input_model frozen_inference_graph.pb is the input model for conversion which is in pb format. next is configuration file . this is pipe line config file. I also reveresed the input channel. At last fed in the json file.the conversion was sucessful, it formed .xml file and .bin file. The Execution Time was about around 82 seconds.

The Generated IR model files are :

XML file: /home/workspace/ssd_mobilenet_v2_coco_2018_03_29/./frozen_inference_graph.xml
BIN file: /home/workspace/ssd_mobilenet_v2_coco_2018_03_29/./frozen_inference_graph.bin

Finally i run the program using command:
for video file: python main.py -i resources/Pedestrian_Detect_2_1_1.mp4 -m "/home/workspace/intel//ssd_mobilenet_v2_coco_2018_03_29/frozen_inference_graph.xml -l /opt/intel/openvino/deployment_tools/inference_engine/lib/intel64/libcpu_extension_sse4.so -d CPU -pt 0.6 | ffmpeg -v warning -f rawvideo -pixel_format bgr24 -video_size 768x432 -framerate 24 -i - http://0.0.0.0:3004/fac.ffm

## Comparing Model Performance

My method(s) to compare models before and after conversion to Intermediate Representations
were 
The difference between model accuracy pre- and post-conversion was that, SSD MobileNet V2 IR detects less no of people with a high accuracy but fails to continuously track subject that is idle.

The size of the model pre- and post-conversion was almost the same. The SSD MobileNet V2 COCO model .pb file is about 66.4 MB and the IR bin file is 64.1 MB.

The inference time of the model pre- and post-conversion was 70ms. I tested both pre-trained model and the converted model, where turns out that, the pre-trained model from openzoo had a lesser inference time that the converted model. Also, the detection was so accurate with the pre-trained model.

## Assess Model Use Cases

Some of the potential use cases of the people counter app are, at the retail to keep a track of the people based on their interest, and at the traffic signal to make sure that people crosses safely.Monitor passenger traffic flow in air port and train station.and Assign staff deployment based on demand.

Each of these use cases would be useful because, it allows us to improve marketing strategy of the retail and as well as safety of the pedestrian.
## Assess Effects on End User Needs

Lighting, model accuracy, and camera focal length/image size have different effects on a deployed edge model. The potential effects of each of these are as follows:

-Lighting and Focal Length of the camera depends on the system installed. A bad lighting can seriosly reduces the accuracy of the model.

-One thing to notice is that, the camera's angle plays an important role that has affects on both the lighting as well as model accuracy.

-The camera image size should be compatible with the model for proper detection. The model accuracy is calculated using the confusion matrix which gives the details about the occurance of false postivites and negatives which degrades the accuracy of the model.

## Model Research

[This heading is only required if a suitable model was not found after trying out at least three
different models. However, you may also use this heading to detail how you converted 
a successful model.]

In investigating potential people counter models, I tried each of the following three models:

- Model 1: [Name]
  - [Model Source]
  - I converted the model to an Intermediate Representation with the following arguments...
  - The model was insufficient for the app because...
  - I tried to improve the model for the app by...
  
- Model 2: [Name]
  - [Model Source]
  - I converted the model to an Intermediate Representation with the following arguments...
  - The model was insufficient for the app because...
  - I tried to improve the model for the app by...

- Model 3: [Name]
  - [Model Source]
  - I converted the model to an Intermediate Representation with the following arguments...
  - The model was insufficient for the app because...
  - I tried to improve the model for the app by...