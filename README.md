# Live-Image-processing-on-Jetson-Nano-Developer-Kit
* flash Jetpack image on jetson nano developer kit through SD card
  
# Steps to do after image flashing

*   sudo apt-get update
*   sudo apt-get install git cmake libpython3-dev python3-numpy
*   git clone --recursive https://github.com/dusty-nv/jetson-inference
*   cd jetson-inference/
*   mkdir build
*   cd build
*   cmake ../
*   cd ..
*   cd tools
*   ./download-models.sh
    
#   models to select for download 3 5 14 16 18 19 24 25 27 29 31 33 along with preselected models

*   cd ..
*   cd build
*   make
*   sudo make install
*   sudo ldconfig
*   cd aarch64/bin
*   ls
*   ls images
*   ./detectnet.py images/peds_0.jpg output_0.jpg   (to see whether all steps are correct and sample images generated correctly within files)
*   v4l2-ctl --list-devices  (to list camera devices ,and note down whether video0 or video1 to use in python code)
*   v4l2-ctl --device /dev/video0 --list-formats-ext
*   cd ~
   
#   after above step open notepad and rename untitled file as my-detection.py,then write the below code and save

* import jetson.inference
* import jetson.utils

* net= jetson.inference.detectNet("ssd-mobilenet-v2", threshold=0.5)
* camera = jetson.utils.gstCamera(1280, 720, "/dev/video1")
* display = jetson.utils.glDisplay()

* while display.IsOpen():
* img, width, height = camera.CaptureRGBA()
* detections = net.Detect(img, width, height)
* display.RenderOnce(img, width, height)
* display.SetTitle("Object Detection | Network {:.0f} FPS".format(net.GetNetworkFPS()))

# after coding save then the following steps
*   ls
*   python my-detection.py

# after this code we can see the live image processing in a popup window
