---
layout:     post
title:      Cuda Learn day 1
subtitle:   cuda lesson
date:       2018-12-30
author:     Dzw
header-img: img/cuda/1.png
catalog: 	 true
tags:
    - cuda
---



# Cuda Learn day1



## kernel functions and threading

![](/image/cuda/1.png)

## kernel launch

![](/image/cuda/2.png)





```
__global__ void vecAddKernel(float* A, float*B, float*C, int c){
	int i = blockDim.x *blockIdx.x + threadIdx.x;
	if (i < n)
		c[i] = A[i] + B[i];
}

int vecAdd(float* A, float *B, float *C, int c){
	vecAddKernel << <ceil(n / 256.0), 256 >> >(d_A)
}

//__global__,executed on device,only callable from host
//__host__  ,both executed and callable with device
//__device__,both executed and callable with 

//“ << <” number of  blocks, number of threads in each block “ >> >” 


```



```
Quiz Questions for Module 2

1 .If we want to allocate an array of v integer elements in CUDA device global memory, what would be an appropriate expression for the second argument of the cudaMalloc() call?

(A) n

(B) v

(C) n * sizeof(int)

(D) v * sizeof(int)

 

Answer: (D)

Explanation: This one should be self-evident. cudamalloc(void **devPtr,size_t size)

 

2 .f we want to allocate an array of n floating-point elements and have a floating-point pointer variable d_A to point to the allocated memory, what would be an appropriate expression for the first argument of the cudaMalloc() call? 

(A) n

(B) (void *) d_A

(C) *d_A

(D) (void **) &d_A

Answer: (D)  cudamalloc(void **devPtr,size_t size)

Explanation: &d_A is pointer to a pointer of float. To convert it to a generic pointer required by cudaMalloc() should use (void **) to cast it to a generic double-level pointer.

3 .If we want to copy 3000 bytes of data from host array h_A (h_A is a pointer to element 0 of the source array) to device array d_A (d_A is a pointer to element 0 of the destination array), what would be an appropriate API call for this in CUDA?

(A) cudaMemcpy(3000, h_A, d_A, cudaMemcpyHostToDevice);

(B) cudaMemcpy(h_A, d_A, 3000, cudaMemcpyDeviceTHost);

(C) cudaMemcpy(d_A, h_A, 3000, cudaMemcpyHostToDevice);

(D) cudaMemcpy(3000, d_A, h_A, cudaMemcpyHostToDevice);

Answer: (C)

Explanation: See Lecture 2.2 slides.
reference from library:
cudaError_t cudaMemcpy	(	void * 	dst,
const void * 	src,
size_t 	count,
enum cudaMemcpyKind 	kind	 
)	


4 .How would one declare a variable err that can appropriately receive returned value of a CUDA API call?

(A) int err;

(B) cudaError err;

(C) cudaError_t err;

(D) cudaSuccess_t err;

Answer: (C)

Explanation: See Lecture 2.2 slides.2.8 EXERCISES
```





## Cs344 assingment1

```
__global__
void rgba_to_greyscale(const uchar4* const rgbaImage,
                       unsigned char* const greyImage,
                       int numRows, int numCols)
{
  //TODO        m color to greyscale
  //the mapping from components of a uchar4 to RGBA is:
  // .x -> R ; .y -> G ; .z -> B ; .w -> A
  //
  //The output (greyImage) at each pixel should be the result of
  //applying the formula: output = .299f * R + .587f * G + .114f * B;
  //Note: We will be ignoring the alpha channel for this conversion

  //First create a mapping from the 2D block and grid locations
  //to an absolute 2D location in the image, then use that to
  //calculate a 1D offset
	int pixel = 15;
	//grid上x维度上的线程块索引 * 一个线程块x维度上的线程数量 + x维度上的线程索引
	unsigned int x = blockIdx.x*blockDim.x + threadIdx.x;
	unsigned int y = blockIdx.y*blockDim.y + threadIdx.y;
	if (x<numCols && y<numRows)
		greyImage[y*numRows + x] =  0.228f * rgbaImage[y*numRows + x].x + 
									0.587f * rgbaImage[y * numRows + x].y + 
									0.114f * rgbaImage[y * numRows + x].z;


}

void your_rgba_to_greyscale(const uchar4 * const h_rgbaImage, uchar4 * const d_rgbaImage,
                            unsigned char* const d_greyImage, size_t numRows, size_t numCols)
{
  //You must fill in the correct sizes for the blockSize and gridSize
  //currently only one block with one thread is being launched
  const int blockWidth = 32;
  const dim3 blockSize(blockWidth,blockWidth ,1 );  //TODO

  int blocksX = (numRows-1) / blockWidth + 1;
  int blocksY = (numCols-1) / blockWidth + 1;
  const dim3 gridSize( blocksX, blocksY, 1);  //TODO
  rgba_to_greyscale<<<gridSize, blockSize>>>(d_rgbaImage, d_greyImage, numRows, numCols);
  
  cudaDeviceSynchronize(); checkCudaErrors(cudaGetLastError());

}
```

