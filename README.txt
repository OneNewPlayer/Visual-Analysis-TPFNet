
## Date: 27th May 2020

## Setup and installation for TPGAN

1. Library version:
	-Python: 3.6.3
	-Tensorflow: 1.5.0
	-Keras: 2.1.3
	-GPU: GeForce GTX 1080 Ti (single)
	-TP-GAN is Developed on Anaconda 5.0.1, Windows10 64bit
	-Face landmarks extraction is executed on CentOS 7
	
	
2. Building dataset:
	-Purchase MULTI-PIE dataset from http://www.cs.cmu.edu/afs/cs/project/PIE/MultiPie/Multi-Pie/Home.html.
	
	-Convert MULTI-PIE target images into jpeg with same directory structures. misc/jpeg_converter.py can help this.
	
	-Extract landmarks as np.array from target images and store them into pickled dictionary file (landmarks.pkl). misc/extract_from_multipie.py and misc/landmark_convert.py can help this. 
	In this experiments, I used following python module on CentOS 7 for landmark extraction, though all other process are done in Windows 10. https://github.com/1adrianb/face-alignment

	
3. Fine-tuning LightCNN model:
	-Firstly, train Light CNN with MS-Celeb-1M 'ALIGNED' dataset with cleaned list. In this experiments I used Light CNN code at https://github.com/yh-iro/Keras_LightCNN. Please see this instructions.

	-Secondly, with trained extractor fixed, train new classifier with MULTI-PIE.

	-Thirdly, fine-tune the extractor with MULTI-PIE.

	
4. Running synthesis demo using downloaded trained models:
	-Please see TPGAN\Keras_TP-GAN-master\Keras_TP-GAN-master\misc\demo.py:
	-Baisc steps to run the demo.py:
		1. Update path of the images which is formatted as .png or .jpeg and the size of the image need to be less than 500KB. 
		2. input four points on the inputted image which is popped out by TPGAN: left and right eyes, tip of nose and centre of the mouth for the process in the global network.
		3. input four points again but use bounding box to screenshot four area for process in the local network.
		4. Press "s" which stands for "screenshot" and then press "p" or press "q". Where "p" which stands for "process" and "q" stands for "quit" 
		5. The generated images will be saved as pred.jpg in the folder of "misc" 


## Setup and installation for Facenet
1. Installation:[The same environment as TPGAN]
	-Use the same environment of TPGAN. In addition, instal mtcnn through pip:
		$ pip install mtcnn
		
	[This implementation requires OpenCV>=4.1 and Keras>=2.0.0 (any Tensorflow supported by Keras will be supported by this MTCNN package). ]
	
	- If this is the first time you use tensorflow, you will probably need to install it in your system:
		$ pip install tensorflow
	or with conda
		$ conda install tensorflow
	Note that tensorflow-gpu version can be used instead if a GPU device is available on the system, which will speedup the results.


2.Pretrained model:
	-place the pretrained model under the models directory:
		Dlib face calibration model: shape_predictor_5_face_signals.dat. Bz2
		FaceNet face recognition: model.10-0.0156.hdf5


3.To prepare data:
	-Download LFW database and put it in the data directory:
		$wget HTTP: / / http://vis-www.cs.umass.edu/lfw/lfw-funneled.tgz
		$tar - XVF LFW - funneled. TGZ
		$wget HTTP: / / http://vis-www.cs.umass.edu/lfw/pairs.txt
		$wget HTTP: / / http://vis-www.cs.umass.edu/lfw/people.txt


4.Evaluation of the script:(My Accuracy: 89.27 %.) 
		$python lfw_eval. Py


5.Data preprocessing:
	-Extract training images:
		$python pre - process. Py
	[Out of a total of 202,599 face images, 5,600 could not be calibrated by dlib.So 202599-5600 = 196,999 were used for training.]


6.training:
	$python "train". Py


7.To visualize the training process, execute the following commands:
	$tensorboard - logdir path_to_current_dir/logs


8.DEMO:
	$ python demo.py


