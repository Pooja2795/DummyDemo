# Advanced Web Technology - Deep Encode 2
### Team Members
Team 2:
Pooja Suryaprakash Pathak, Mandana Saberdest, Shreyank Jitendra Saparia

### Motivation

Due to increase demand of video streaming, it is important for high defintion videos to provide best quality To improvise the qulaity videos, there is a need of video quality measurements.
In this project, we compare the quality metrics PSNR and SSIM which are widely used metrics with the help of 
different video codecs H.264 and Vp9. For this purpose, high resolution raw videos are being used from different video categories (cartoon, nature, sports) were selected. These metrics were further calculated for each video codecs with different bitrates from low to high. And finally the graphs are being created for visualization. These graphs depicts the individual results of each quality metrics for each video codecs using the different bitrates.

### Project Setup

For the encoding part, we used FFmpeg developers and a computer with windows as an operating system. 
We created a shell scripts for encoding purpose for each raw videos. The script includes selected video names, resoltuions, bitrates. It is using FFmpeg command lines to encode the raw videos according to the resolutions and bitrates given and with one single command line , it is able to upscale the videos. 
Further we also created the shell scripts for calculating the quality metrics (PSNR and SSIM) and save the results in a text file. 
These results were then read and analyzed using Python. In a program, it reads the files and saves in each variable. This process is done for both video codecs and quality metrics. Further comparison of quality metrics with each resolution and bitrates are plotted in a graph. Comparision of video codecs along with quality metrics are also plotted in graph.

### Code Example for Encoding using FFmpeg

To encode the original raw video, first select the source file and a video codec and also choose the resolutions and bitrates in the same command line.

Example:

```
ffmpeg -i Nature1.mp4 -vf scale=4096x2160 -vcodec libx264 -acodec aac -b:v 3000k nat1.mp4

```
Here a source file is taken as an input and it will be encoded with the video codec H.264 to a 4K resolution and a bitrate with 3000 kbps. Once the encoding will be done, it will save the video as mp4 file named "nat1.mp4". We performed these steps for other video codecs too (FullHD and HD) and for 9 bitrates from low to high. The reason behind choosing 3 video codecs and 9 bitrates was to observe if there any common patterns between these video codecs to further evaluate the quality metrics.
With ``-vf scale`` you can set the resolution , with ``-vcodec`` you can choose the video codecs, with ``-acodec`` you can choose the audio compression codec and with ``-b:v`` you can set the bitrates in kbps. This same process is done for both the video codecs.

### PSNR and SSIM calculation using FFmpeg

We created shell scripts for calculation of quality metrics PSNR and SSIM. One line FFmpeg command was written in a while query to generate 9 PSNR and SSIM files for the 9 different bitrates and the video codecs. The original video and the encoded video are compared to calculate those metrics. This comparison works fine only if the original video and the encoded video have the same resolution. For that you need to upscale the encoded video to the original resolution. After that the format , flags , fps are set. Further the results are stored in textfile.

```

ffmpeg -i aac$x.mp4 -i Nature1.mp4 -filter_complex "[0:v]scale=4096x2160:flags=bicubic[main]; [1:v]scale=4096x2160:flags=bicubic[ref]; [main][ref]psnr=1 > aac$x.txt"

```

In the above example, we used most widely used flag which is bicubic flag for upscaling since it keeps edges smooth. it is done for both main as well as reference files.

### Example of a Text File
The formatted text files are generated after calculating the metrics from all frames.

Example of PSNR calculation:
```
n:1 mse_avg:1.78 mse_y:2.53 mse_u:0.28 mse_v:0.29 psnr_avg:45.62 psnr_y:44.10 psnr_u:53.62 psnr_v:53.57 
n:2 mse_avg:1.81 mse_y:2.56 mse_u:0.29 mse_v:0.30 psnr_avg:45.56 psnr_y:44.04 psnr_u:53.48 psnr_v:53.43 
n:3 mse_avg:2.26 mse_y:3.24 mse_u:0.31 mse_v:0.30 psnr_avg:44.59 psnr_y:43.03 psnr_u:53.27 psnr_v:53.29 
n:4 mse_avg:2.41 mse_y:3.45 mse_u:0.31 mse_v:0.31 psnr_avg:44.32 psnr_y:42.75 psnr_u:53.17 psnr_v:53.19 
n:5 mse_avg:2.73 mse_y:3.92 mse_u:0.35 mse_v:0.34 psnr_avg:43.77 psnr_y:42.20 psnr_u:52.73 psnr_v:52.82 
n:6 mse_avg:2.69 mse_y:3.86 mse_u:0.35 mse_v:0.34 psnr_avg:43.84 psnr_y:42.27 psnr_u:52.71 psnr_v:52.77  

```
In the above example, mse(Mean Squared Error) and psnr values are being caluculated for y,u,v chrominance color transformation and average values. 

Example of SSIM caluculation:
```
n:1 Y:0.993755 U:1.000000 V:1.000000 All:0.995837 (23.805846)
n:2 Y:0.993755 U:1.000000 V:1.000000 All:0.995837 (23.805784)
n:3 Y:0.993749 U:1.000000 V:1.000000 All:0.995833 (23.801620)
n:4 Y:0.993750 U:1.000000 V:1.000000 All:0.995833 (23.802241)
n:5 Y:0.993748 U:1.000000 V:1.000000 All:0.995832 (23.800750)
n:6 Y:0.994305 U:0.999949 V:0.999999 All:0.996195 (24.196474)

``` 
In the above example, Y, U, V chrominance color transformation and All values are being calculated for SSIM. 

### Python code to read and plot the comparison graph

To show the comparison between quality metrics (PSNR and SSIM), we plotted the graphs using matplotlib library in python. 
First we read the result files generated after the calculation of PSNR and SSIM according to the video codecs H.264 and Vp9 for different bitrates and store it different variable. Further we separate the average values of PSNR and SSIM values in a separate variable which will be used for the plotting of graphs.

Example of PSNR:
```
##### for 4096x2160 with bitrates 3000k, 5000k, 7000k

    football_h264_psnr1 = pd.read_csv('1  sportsH264psnr1.txt', sep =' ', header = None, names = columns)
    football_h264_psnr2 = pd.read_csv('1  sportsH264psnr2.txt', sep =' ', header = None, names = columns)

##### separating Average PSNR values per frame to separate column

    football_h264_psnr1["PSNR Avg"]  = football_h264_psnr1['psnr_avg'].str.split(':').apply(lambda x: float(x[1]))
    football_h264_psnr2["PSNR Avg"]  = football_h264_psnr2['psnr_avg'].str.split(':').apply(lambda x: float(x[1]))

```
Example of SSIM:
```
##### for 4096x2160 with bitrates 3000k, 5000k, 7000k

    football_h264_ssim1 = pd.read_csv('1  sportsH264ssim1.txt', sep =' ', header = None, names = columns)
    football_h264_ssim2 = pd.read_csv('1  sportsH264ssim2.txt', sep =' ', header = None, names = columns)

##### separating SSIM values per frame to separate column

    football_h264_ssim1["SSIM"]  = football_h264_ssim1['5'].str.split(':').apply(lambda x: float(x[1]))
    football_h264_ssim2["SSIM"]  = football_h264_ssim2['5'].str.split(':').apply(lambda x: float(x[1]))

```
Then further we do the comparison of PSNR and SSIM values with respect to video codecs, resolutions and bitrates.

Example of PSNR: 
```
##### Comparision of PSNR values of a video encoded at 4k resolution with 3000k, 5000k, 7000k bitrate respective.
##### x axis is the frame number of the video, y axis is the psnr value

    ax1.plot(x, football_h264_psnr1["PSNR"], label = 'h264 PSNR at 4096x2160 with 3000k bitrate')
    ax1.plot(x, football_h264_psnr2["PSNR"], label = 'h264 PSNR at 4096x2160 with 5000k bitrate')

```
Example of SSIM:
```
##### Comparision of SSIM values of a video encoded at 4k resolution with 3000k, 5000k, 7000k bitrate respective.
##### x axis is the frame number of the video, y axis is the psnr value

    ax1.plot(x, football_vp9_ssim1["SSIM"], label = 'vp9 SSIM at 4096x2160 with 3000k bitrate')
    ax1.plot(x, football_vp9_ssim2["SSIM"], label = 'vp9 SSIM at 4096x2160 with 5000k bitrate')

```
Finally we compare the video codecs(H.264, Vp9) along with quality metrics (PSNR and SSIM) at different resolutions and bitrates.

```
Examples for PSNR and SSIM:

    ax1.plot(x, football_h264_psnr1["PSNR Avg"], label = 'h264 PSNR at 4096x2160 with 3000k bitrate')
    ax1.plot(x, football_vp9_psnr1["PSNR Avg"], label = 'vp9 PSNR at 4096x2160 with 3000k bitrate')

    ax1.plot(x, football_h264_ssim1["SSIM"], label = 'h264 SSIM at 4096x2160 with 3000k bitrate')
    ax1.plot(x, football_vp9_ssim1["SSIM"], label = 'vp9 SSIM at 4096x2160 with 3000k bitrate')
```
By performing above steps, the graphs are plotted which can be visualized. The results can be seen in the provided documentation.
