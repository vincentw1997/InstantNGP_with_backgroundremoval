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
2. Clone the Project InstantNGP from NVIDIA:
```shell
git clone --recursive https://github.com/nvlabs/instant-ngp
cd instant-ngp
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
The path to colmap2nerf.py is located on the instant-ngp folder with the following path instant-ngp/scripts/colmap2nerf.py /n
--video_in args need to be the path to the MP4 file
--video_fps args determines how many frames you want to use per second from the video (usually 2 is enough)
--aabb_scale args can be determine by powers of 2. e.g. 4, 8, 16, 32. Use smaller values to "crop" the background (usually for smaller objects). If background is important then use larger values
--out args determines the output path for the transform.json file
```shell
python <path_to_colmap2nerf.py> --video_in <path_to_video_MP4> --video_fps 2 --run_colmap --aabb_scale 8 --out <path_for_transform.json_file>
```
11.  

