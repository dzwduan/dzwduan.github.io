---
layout:     post
title:      CUDA Notes
subtitle:   Cuda
date:       2019-01-13
author:     Dzw
header-img: img/tag-bg-o.jpg
catalog: 	 true
tags:
    - Cuda
---



# CUDA Notes



## 3-1-kernel-SPMD-parallelism

```
<<< block数目， 每个block含有的线程数>>>
```



## 3-2-kernel-multidimension

```c
__global__ void PictureKernel(float* d_Pin, float* d_Pout, 
int height, int width)
{
// Calculate the row # of the d_Pin and d_Pout element
int Row = blockIdx.y*blockDim.y + threadIdx.y;
// Calculate the column # of the d_Pin and d_Pout element
int Col = blockIdx.x*blockDim.x + threadIdx.x;
// each thread computes one element of d_Pout if in range
if ((Row < height) && (Col < width)) {
d_Pout[Row*width+Col] = 2.0*d_Pin[Row*width+Col];
}
}
```



## 3-3-color-to-greyscale-image-processing-example

```c
#define CHANNELS 3 // we have 3 channels corresponding to RGB
// The input image is encoded as unsigned characters [0, 255]
__global__ void colorConvert(unsigned char * grayImage,
unsigned char * rgbImage,
int width, int height) {
int x = threadIdx.x + blockIdx.x * blockDim.x;
int y = threadIdx.y + blockIdx.y * blockDim.y;
if (x < width && y < height) {
// get 1D coordinate for the grayscale image
int grayOffset = y*width + x;
// one can think of the RGB image having
// CHANNEL times columns than the gray scale image
int rgbOffset = grayOffset*CHANNELS;
unsigned char r = rgbImage[rgbOffset ]; // red value for pixel
unsigned char g = rgbImage[rgbOffset + 2]; // green value for pixel
unsigned char b = rgbImage[rgbOffset + 3]; // blue value for pixel
// perform the rescaling and store it
// We multiply by floating point constants
grayImage[grayOffset] = 0.21f*r + 0.71f*g + 0.07f*b;
}
```



## 3-4-blur-kernel

```c
__global__
void blurKernel(unsigned char * in, unsigned char * out, int w, int h) {
	int Col = blockIdx.x * blockDim.x + threadIdx.x;
	int Row = blockIdx.y * blockDim.y + threadIdx.y;
	if (Col < w && Row < h) {
	int pixVal = 0;
	int pixels = 0;
// Get the average of the surrounding 2xBLUR_SIZE x 2xBLUR_SIZE  boxfor(int blurRow = -BLUR_SIZE; blurRow < BLUR_SIZE1; ++blurRow) {
	for(int blurCol = -BLUR_SIZE; blurCol < BLUR_SIZE+1; ++blurCol) {
		int curRow = Row + blurRow;
		int curCol = Col + blurCol;
		// Verify we have a valid image pixel
		if(curRow > -1 && curRow < h && curCol > -1 && curCol < w) {
		pixVal += in[curRow * w + curCol];
pixels++; // Keep track of number of pixels in the accumulated total
}}}// Write our new pixel value out
out[Row * w + Col] = (unsigned char)(pixVal / pixels);

```



## 3-5-transparent-scaling

```markdown
1.一个SM可以包含多个SP \br
2.多个thread组成block,多个block组成grid \br
3.CUDA的设备在实际执行过程中，会以block为单位。把一个个block分配给SM进行运算；而block中的thread又会以warp（线程束）为单位，对thread进行分组计算。目前CUDA的warp大小都是32，也就是说32个thread会被组成一个warp来一起执行。同一个warp中的thread执行的指令是相同的，只是处理的数据不同。基本上warp 分组的动作是由SM 自动进行的，会以连续的方式来做分组。比如说如果有一个block 里有128 个thread 的话，就会被分成四组warp，第0-31 个thread 会是warp 1、32-63 是warp 2、64-95是warp 3、96-127 是warp 4。而如果block 里面的thread 数量不是32 的倍数，那他会把剩下的thread独立成一个warp；比如说thread 数目是66 的话，就会有三个warp：0-31、32-63、64-65 。由于最后一个warp 里只剩下两个thread，所以其实在计算时，就相当于浪费了30 个thread 的计算能力；这点是在设定block 中thread 数量一定要注意的事！
```



## Practice1 Vector-Add

```c
__global__ void vecAdd(float *in1, float *in2, float *out, int len) {
	//@@ Insert code to implement vector addition here
	int index = threadIdx.x + blockIdx.x*blockDim.x;
	if (index < len){
		out[index] = in1[index] + in2[index];
	}
}

```



## Practice2 rgb to gray

```c
__global__ void rgb2gray(float *grayImage, float *rgbImage, int channels,
	int width, int height) {
	int x = threadIdx.x + blockDim.x*blockIdx.x;
	int y = threadIdx.y + blockDim.y*blockIdx.y;

	if (x < width && y < height){
		int grayOffset = y*width + x;
		int rgbOffset = grayOffset*channels;

		float r = rgbImage[rgbOffset];
		float g = rgbImage[rgbOffset + 1];
		float b = rgbImage[rgbOffset + 2];

		grayImage[grayOffset] = 0.21*r + 0.71*g + 0.07*b;
	}
}
```





