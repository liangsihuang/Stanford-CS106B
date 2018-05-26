# Stanford-CS106B

## Lec14
Algorithm analysis  
time spent and memory used  
Evaluating performance  
Empirically-time with stopwatch  
Mathematically-analyze algorithm  
## Lec16
```cpp
int Partition(Vector<int> &arr, int start, int step)
{
    int 
}
void Quicksort(Vector<int> &arr, int start, int step)
{
    if (step>start){
        int pivot = Partition(arr, start, step);
        Quicksort(arr, start,);
    }
}
```
Quicksort code  
Partition is linear O(N)
T(N) = N + 2T(N/2) => O(NlogN)h
### Assuming bad 90/10 split  
still O(NlogN)
### Assuming worst N-1/1 split
Ack! O(N^2)  
already sorted!
* What to do?
  * Choose pivot differently
  * Compute actual median
  * "Median of three"
  * Random
为什么没有泡沫排序？更难写，易出错，又更慢  

### template 一劳永逸地写出排序
* A proliferation of Swap
* Function template 

