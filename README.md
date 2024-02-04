# InstantNGP_with_backgroundremoval
The Implementation of InstantNGP (Instant Neural Graphics Primitives) with Background Removal pipeline for generating novel views of plastic containers. This implementation is based on the research done by NVIDIA on NeRF (Neural Radiance Field). 

# Special Thanks to 
## Disclaimer: Most of the pipeline is based on the InstantNGP pipeline provided by NVIDIA. I highly recommend checking out the main Github repo of InstantNGP (https://github.com/NVlabs/instant-ngp.git)
1. https://github.com/NVlabs/instant-ngp.git
2. https://github.com/danielgatis/rembg.git
3. https://github.com/colmap/colmap.git

## Special Thanks to My Supervisors:
1. Prof. Dr. Martin Storath
2. Prof. Dr. -Ing Pascal Meißner
3. Daniel Kawetzki MSc

# Hardware Requirements
## Requirements from InstantNGP NVIDIA (run on the following dependencies):
1. NVIDIA GPU
2. C++14 capable compiler. Windows Visual Studio 2019 or 2022
3. Windows: CUDA version 11.7 or higher
4. CMake v3.21 or higher
5. Python 3.9.17 

## Recording of the video:
1. Sony Handycam CX405 with tripod stands

## PC:
1. Intel i5-12400F
2. NVIDIA RTX 3060 12 GB VRAM
3. 32 GB RAM
4. Windows 11 OS

# Quick Guide on Running the Code
Here is the general guide if you are interested in running the code locally: 
1. Open the Visual Studio Code command prompt.
2. Clone the Project InstantNGP from NVIDIA and supporting scripts for preprocessing:
```shell
git clone --recursive https://github.com/nvlabs/instant-ngp
cd instant-ngp
git clone https://github.com/vincentw1997/InstantNGP_with_backgroundremoval
```
3. Use CMake to build the project. Use the developer command prompt from Visual Studio Code
```shell
cmake . -B build -DCMAKE_BUILD_TYPE=RelWithDebInfo
cmake --build build --config RelWithDebInfo -j
```
4. Create a conda environment (Anaconda Navigator) on a separate command prompt
```shell
conda create -n my_env python=3.9.17
```
5. Activate the conda environment
```shell
conda activate my_env
```
6. Navigate to the folder instant-ngp (cd instant-ngp)
7. Install all the dependencies needed for instantNGP
```shell
pip install -r requirements.txt
```
8. Install background removal dependency
```shell
pip install rembg[gpu,cli]
```
9. Need to navigate to a particular folder of your video recording
```shell
cd <video_path>
```
10. Do the COLMAP preprocessing for the recorded video. Note that the video must be in a folder. The output of this script will form a transform.json file that is required for further InstantNGP processes.
The path to colmap2nerf.py is located on the instant-ngp folder with the following path instant-ngp/scripts/colmap2nerf.py <br />
--video_in args need to be the path to the MP4 file <br />
--video_fps args determines how many frames you want to use per second from the video (usually 2 is enough) <br />
--aabb_scale args can be determine by powers of 2. e.g. 4, 8, 16, 32. Use smaller values to "crop" the background (usually for smaller objects). If background is important then use larger values <br />
--out args determines the output path for the transform.json file <br />
```shell
python <path_to_colmap2nerf.py> --video_in <path_to_video_MP4> --video_fps 2 --run_colmap --aabb_scale 8 --out <path_for_transform.json_file>
```
11.  Need to create a backup folder for the COLMAP feature extraction part. As COLMAP can have different results for every run. 
```shell
xcopy /e <original_folder_path> <backup_folder_path>
```
12.  Rename the generated folder from COLMAP from images to imageori for background removal step
```shell
cd <original_folder_path>
rename images imagesori
```
13. Remove the background of the images
```shell
rembg p ./imagesori ./images
```
14. Rename the frames from png to jpg. Using supporting scripts in /instant-ngp/support/png_to_jpg.py
```shell
python <path_to_png_to_jpg.py> --target_folder ./images 
```
15. Remove lower sharpness images in the dataset using supporting script in /instant-ngp/support/evaluate_sharpness.py. <br /> --percent_delete args determine how much of the dataset lowest sharpness are removed.
```shell
python <path_to_evaluate_sharpness.py> --path_folder <original_folder_path> --path_json <original_folder_path> --output_list <original_folder_path> --percent_delete 10.0
```
16. Manually delete similar images in the folder. (optional)
17. Create the train test dataset using supporting scripts in /instant-ngp/support/train_test_v4.py. Still need to be improved as currently it is done manually for every class
```shell
python <path_to_train_test_v4.py> --data_path <path_to_images_folder> --test_data_path_to_save <path_to_new_save> --n_jumps 10 --starting_point 10
```
18. Copy the original transform.json from COLMAP to all the folders
19. Start your training using the script in /instant-ngp/scripts/run.py
```shell
python <path_to_run.py> --scene <path_to_transform.json> --save_snapshot <path_to_save_the_model> --n_steps 5000
```
20. continue with transform_test edit
21. edit the transform_test based on the test dataset
22. render the novel images from the trained model
23. compare the rendered images with comparison metrics

# Future Work
Maybe need to improve the code pipeline overall and combine it into 1 Python script 
Add more features or change the U net model used for the background removal 
