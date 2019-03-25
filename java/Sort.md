# Interface
````
import java.util.Comparator;  
/** 
 * 排序器接口(策略模式: 将算法封装到具有共同接口的独立的类中使得它们可以相互替换) 
 * 
 */  
public interface Sorter {  
    
/** 
  * 排序 
  * @param list 待排序的数组 
  */  
  public <T extends Comparable<T>> void sort(T[] list);  
   
/** 
 * 排序 
 * @param list 待排序的数组 
 * @param comp 比较两个对象的比较器 
 */  
  public <T> void sort(T[] list, Comparator<T> comp);  
}  
````

# Bubble Sort Java
````
import java.util.Comparator;  

public class BubbleSorter implements Sorter {  
   
   @Override  
   public <T extends Comparable<T>> void sort(T[] list) {  
      boolean swapped = true;  
      for(int i = 1; i < list.length && swapped;i++) {  
        swapped= false;  
        for(int j = 0; j < list.length - i; j++) {  
           if(list[j].compareTo(list[j+ 1]) > 0 ) {  
              T temp = list[j];  
              list[j]= list[j + 1];  
              list[j+ 1] = temp;  
              swapped= true;  
           }  
        }  
      }  
   }  
   
    @Override  
    public <T> void sort(T[] list,Comparator<T> comp) {  
      boolean swapped = true;  
      for(int i = 1; i < list.length && swapped; i++) {  
        swapped = false;  
        for(int j = 0; j < list.length - i; j++) {  
           if(comp.compare(list[j], list[j + 1]) > 0 ) {  
              T temp = list[j];  
              list[j]= list[j + 1];  
              list[j+ 1] = temp;  
              swapped= true;  
           }  
        }  
      }  
   }   
}  
````

# Quick Sorter
````
import java.util.Comparator;  
   
/** 
 * 快速排序 
 * 快速排序是使用分治法（divide-and-conquer）依选定的枢轴 
 * 将待排序序列划分成两个子序列，其中一个子序列的元素都小于枢轴， 
 * 另一个子序列的元素都大于或等于枢轴，然后对子序列重复上面的方法， 
 * 直到子序列中只有一个元素为止 
 * 
 */  
public class QuickSorter implements Sorter {  
   
  @Override  
  public <T extends Comparable<T>> void sort(T[] list) {  
     quickSort(list, 0, list.length- 1);  
   }  
   
   @Override  
	public <T> void sort(T[] list, Comparator<T> comp) {  
      quickSort(list, 0, list.length- 1, comp);  
   }  
   
   private <T extends Comparable<T>> void quickSort(T[] list, int first, int last) {  
      if (last > first) {  
        int pivotIndex = partition(list, first, last);  
        quickSort(list, first, pivotIndex - 1);  
        quickSort(list, pivotIndex, last);  
      }  
   }  
    
   private <T> void quickSort(T[] list, int first, int last,Comparator<T> comp) {  
      if (last > first) {  
        int pivotIndex = partition(list, first, last, comp);  
        quickSort(list, first, pivotIndex - 1, comp);  
        quickSort(list, pivotIndex, last, comp);  
      }  
   }  
   
   private <T extends Comparable<T>> int partition(T[] list, int first, int last) {  
      T pivot = list[first];  
      int low = first + 1;  
      int high = last;  
	   
      while (high > low) {  
        while (low <= high && list[low].compareTo(pivot) <= 0) {  
           low++;  
        }  
        while (low <= high && list[high].compareTo(pivot) >= 0) {  
           high--;  
        }  
        if (high > low) {  
           T temp = list[high];  
           list[high]= list[low];  
           list[low]= temp;  
        }  
      }  
	   
      while (high > first&& list[high].compareTo(pivot) >= 0) {  
        high--;  
      }  
      if (pivot.compareTo(list[high])> 0) {  
        list[first]= list[high];  
        list[high]= pivot;  
        return high;  
      }  
      else {  
        return low;  
      }  
   }  
   
   private <T> int partition(T[] list, int first, int last, Comparator<T> comp) {  
      T pivot = list[first];  
      int low = first + 1;  
      int high = last;  
   
      while (high > low) {  
        while (low <= high&& comp.compare(list[low], pivot) <= 0) {  
           low++;  
        }  
        while (low <= high&& comp.compare(list[high], pivot) >= 0) {  
           high--;  
        }  
        if (high > low) {  
           T temp = list[high];  
           list[high] = list[low];  
           list[low]= temp;  
        }  
      }  
   
      while (high > first&& comp.compare(list[high], pivot) >= 0) {  
        high--;  
      }  
      if (comp.compare(pivot,list[high]) > 0) {  
        list[first]= list[high];  
        list[high]= pivot;  
        return high;  
	      }  
	      else {  
	        return low;  
	      }  
	   }      
}  
````

# Merge Sort
````
import java.util.Comparator;  
   
/** 
 * 归并排序 
 * 归并排序是建立在归并操作上的一种有效的排序算法。 
 * 该算法是采用分治法（divide-and-conquer）的一个非常典型的应用， 
 * 先将待排序的序列划分成一个一个的元素，再进行两两归并， 
 * 在归并的过程中保持归并之后的序列仍然有序。 
 * 
 */  
public class MergeSorter implements Sorter {  
   
   @Override  
   public <T extends Comparable<T>> void sort(T[] list) {  
	  T[] temp = (T[]) new Comparable[list.length];  
      mSort(list,temp, 0, list.length- 1);  
   }  
	    
   private <T extends Comparable<T>> void mSort(T[] list, T[] temp, int low, int high) {  
      if(low == high) {  
        return ;  
      }  
      else {  
        int mid = low + ((high -low) >> 1);  
        mSort(list,temp, low, mid);  
        mSort(list,temp, mid + 1, high);  
        merge(list,temp, low, mid + 1, high);  
      }  
   }  
   private <T extends Comparable<T>> void merge(T[] list, T[] temp, int left, int right, int last) {  
        int j = 0;   
        int lowIndex = left;   
        int mid = right - 1;   
        int n = last - lowIndex + 1;   
        while (left <= mid && right <= last){   
            if (list[left].compareTo(list[right]) < 0){   
                temp[j++] = list[left++];   
            } else {   
                temp[j++] = list[right++];   
            }   
        }   
        while (left <= mid) {   
            temp[j++] = list[left++];   
        }   
        while (right <= last) {   
            temp[j++] = list[right++];   
        }   
        for (j = 0; j < n; j++) {   
            list[lowIndex + j] = temp[j];   
        }   
   }  
   
   @Override  
   public <T> void sort(T[] list, Comparator<T> comp) {  
      T[]temp = (T[])new Comparable[list.length];  
      mSort(list,temp, 0, list.length- 1, comp);  
   }  
    
   private <T> void mSort(T[] list, T[] temp, int low, int high, Comparator<T> comp) {  
      if(low == high) {  
        return ;  
      }  
      else {  
        int mid = low + ((high -low) >> 1);  
        mSort(list,temp, low, mid, comp);  
        mSort(list,temp, mid + 1, high, comp);  
        merge(list,temp, low, mid + 1, high, comp);  
      }  
   }
   
   private <T> void merge(T[] list, T[]temp, int left, int right, int last, Comparator<T> comp) {  
        int j = 0;   
        int lowIndex = left;   
        int mid = right - 1;   
        int n = last - lowIndex + 1;   
        while (left <= mid && right <= last){   
            if (comp.compare(list[left], list[right]) <0) {   
                temp[j++] = list[left++];   
            } else {   
                temp[j++] = list[right++];   
            }   
        }   
        while (left <= mid) {   
            temp[j++] = list[left++];   
        }   
        while (right <= last) {   
            temp[j++] = list[right++];   
        }   
        for (j = 0; j < n; j++) {   
            list[lowIndex + j] = temp[j];   
        }   
   }    
} 
````