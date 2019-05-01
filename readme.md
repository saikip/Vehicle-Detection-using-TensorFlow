Environment:
OS: Windows x64
Vitual Env used: No
Year: 2019
NOTE: ONLY those files are present in this repository which have been modified

Reference Links:
1. https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/installation.md
2. https://github.com/BurntSushi/nfldb/wiki/Python-&-pip-Windows-installation
3. https://www.tensorflow.org/install/pip
4. https://github.com/protocolbuffers/protobuf/blob/master/cmake/README.md
5. https://github.com/protocolbuffers/protobuf/blob/master/README.md#protocol-compiler-installation
6. https://github.com/tensorflow/models
7. http://www.covingtoninnovations.com/mc/winforunix.html

#Installation:
1. Install MS visual studio, CMake, cygwin, Git (optional), Python (link 2) and Pip (link 2)
     Helpful:   pip install --upgrade pip
				pip freeze
				pop list
2. Open any cmd in admin mode (VS2013 x64 Native Tools Command Prompt or cygwin or powershell)
3. Install tensorflow (link 1 and link 3).
		pip install tensorflow
		pip install tensorflow-gpu # For GPU
   Install specific version of tf:
   pip install tensorflow==1.5.0
   Force reinstall:
   pip install tensorflow --upgrade --force-reinstall
   Force reinstall specific version:
   pip install tensorflow==1.5.0 --upgrade --force-reinstall
   Install specific version using pip but no matching version issue :
   python -m pip install --upgrade https://storage.googleapis.com/tensorflow/mac/cpu/tensorflow-1.12.0-py3-none-any.whl

4. Install dependencies (link 1)
		pip install Cython
		pip install contextlib2
		pip install jupyter
		pip install matplotlib
		pip install pillow
		pip install lxml
5. Install protobuf 
	For windows, just download binary and add bin that has protoc.exe to PATH (link 5)
	Otherwise do complete installation (link 4)
6. Clone the required models to your <path_to_tensorflow> from git link (link 6)
	Add below paths to PATH
	<path_to_tensorflow>/models
	<path_to_tensorflow>/models/research
	<path_to_tensorflow>/models/research/slim
7. COCO API installation (link 1) 
	Go to desired installation location and run the below in VS cmd as step#2
	Edit the setup.py to change line#12 (See Bug#3)
	git clone https://github.com/cocodataset/cocoapi.git
	cd cocoapi/PythonAPI
	make
	cp -r pycocotools <path_to_tensorflow>/models/research/ 
8. 	Protobuf compilation (link 1)
	# From tensorflow/models/research/
	protoc object_detection/protos/*.proto --python_out=.
9. Test tensorflow object detection is working or not:
	python object_detection/builders/model_builder_test.py
10. Detect objects 
	jupyter notebook object_detection\object_detection_tutorial.ipynb
	
	Here add your images to the tests_images folder and name them image1.jpg, 
	image2.jpg, imageN.jpg, etc. In the notebook modify the line under the 
	detection heading to

		TEST_IMAGE_PATHS = [ os.path.join(PATH_TO_TEST_IMAGES_DIR, 'image{}.jpg'.format(i)) for i in range(1, N+1) ]
	
	N being the last number of image.
	
	Now in Jupyter, do "run all" in cells tab
	
11.	To run vehicle detection on a video, follow this link:
	https://pythonprogramming.net/video-tensorflow-object-detection-api-tutorial/?completed=/introduction-use-tensorflow-object-detection-api-tutorial/
	
12.	To run vehicle detection on custom dataset, follow this links:
	https://pythonprogramming.net/custom-objects-tracking-tensorflow-object-detection-api-tutorial/?completed=/video-tensorflow-object-detection-api-tutorial/
	https://towardsdatascience.com/real-time-and-video-processing-object-detection-using-tensorflow-opencv-and-docker-2be1694726e5
	
13. To harness computational power of IBM Watson Machine Learning and Annotation Tool, follow the below link:
	https://github.com/bourdakos1/Custom-Object-Detection
	 
	Tip: DUring the installatiin, config file is found here: C:\cygwin64\home\PSaikia\config.yaml

#Common Bugs during installation:
1. Bug (protobuf manual): broken cmake/RC Pass 1 failed to run.
   Fix: 
	1. Install windows XP for C++ support from VS installer in VS
	2. Add rc path to path after step 1 
	3. My rc paths: 
			C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\lib
			C:\Program Files (x86)\Microsoft SDKs\Windows\v7.1A\Bin
			C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\SDK\ScopeCppSDK\SDK\bin\rc.exe
			C:\Program Files (x86)\Windows Kits\10\bin\10.0.17763.0\x64\rc.exe

2. Bug (cocoapi): invalid numeric argument '/Wno-cpp' 
   Fix: Windows issue: https://github.com/cocodataset/cocoapi/issues/51
   Replace line#12 in setup.py from
   extra_compile_args=['-Wno-cpp', '-Wno-unused-function', '-std=c99'].
   to
   extra_compile_args=[],

3. Bug (cocoapi): unix-based errors such as rm -rf, make(e=2), cp -r not recognized etc.
   Fix: Use cygwin as cmd or use link 7 to use corresponding windows commands

4. Bug: object_detection/protos/*.proto: No such file or directory
   Fix: Use cygwin or manually compile each proto file as below (see proto2pb.txt file)
   protoc object_detection/protos/<filename>.proto --python_out=.
  
5. Bug: ImportError: No module named nets
   Fix: Add slim path to PATH and copy nets folder to sitepackages object detection folder
   
6. Bug: Any other ImportError: No module named XXX
   Fix: DO clean tf reinstall (pip install tensorflow --upgrade --force-reinstall)
 
7. Bug: no such command/file etc even after installation
   Fix: Add bin path of respective program to PATH


