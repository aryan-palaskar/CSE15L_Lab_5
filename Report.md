# Lab Report 5

## EdStem Post

**Student:** <br><br>
Hi, I am getting the following error after running tests on the `sort` method within the `Arrays.java` file. The `sort` method is supposed to sort the given array in ascending order. The failure inducing input is the array `{3,4,5,6,1,2}`. I think the error is in the implementation of the sort method but I am not able to find where exactly.

![Image](lab51.JPG) <br><br>

JUnit Test code for the method <br>
```

 @Test
  public void testSort() {
    int[] input1 = {3,4,5,6,1,2};
    int[] sorted = Arrays.sort(input1);
    assertArrayEquals(new int[]{1,2,3,4,5,6}, sorted);
  }

```

**TA:** <br><br>
Hi, I suggest running `jdb` (Java Debugger) on the test file and making a breakpoint at the line where `assertArrayEquals` is so you can inspect the contents of the sorted array at each index before its compared to the correct sorted array. After the inspection you should find the bug in the `sort` method. 

**Student:** <br><br>
After running `jdb` on the test file and inspecting the values of the sorted array, I found out that the value at the last index is not getting sorted. <br><br>
![Image](lab52.JPG) <br><br>
The code for the sort method is: 
```
 static int[] sort(int[] arr) {

    // Start replacing smallest values starting from the beginning of the array
    int arrayLength = arr.length;

    for(int i = 0; i < arrayLength-1; i++) {

      // Finds the minimum element after the index
      int minIndex= i;
      
      for(int j = minIndex+1; j < arrayLength-1; j++) {

        if(arr[j] < arr[minIndex]) {

          minIndex = j;
        }
      }

      // Swap the found minimum element with the first element 
      int temp = arr[i];
      arr[i] = arr[minIndex];
      arr[minIndex] = temp;
    }

    return arr;
  }
```
The bug was in the nested `for` loop where it checks the conditon if `j` is less than `arrayLength`. It should be `j < arrayLength` instead of `j < arrayLength-1` which resulted in skipping the last value in the array while sorting the array. <br><br>

## Information Needed for Setup

**File Structure**
```
lab3/

./lab3:
Arrays.class  Arrays.java  ArrayTests.class  ArrayTests.java  lib/  test.sh

./lab3/lib:
hamcrest-core-1.3.jar  junit-4.13.2.jar
```

**Contents of each file before fixing bug** <br>
 Arrays.java: <br> <br>
 ```
public class Arrays {

  // Changes the input array to be in reversed order
  static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length/2; i++) {
      int temp = arr[arr.length - i -1];
      arr[arr.length - i - 1] = arr[i];
      arr[i] = temp;
    }
  }

  // Uses selection sort to sort the given array

  static int[] sort(int[] arr) {

    // Start replacing smallest values starting from the beginning of the array
    int arrayLength = arr.length;

    for(int i = 0; i < arrayLength-1; i++) {

      // Finds the minimum element after the index
      int minIndex= i;
      
      for(int j = minIndex+1; j < arrayLength-1; j++) {

        if(arr[j] < arr[minIndex]) {

          minIndex = j;
        }
      }

      // Swap the found minimum element with the first element 
      int temp = arr[i];
      arr[i] = arr[minIndex];
      arr[minIndex] = temp;
    }

    return arr;
  }

}
  ```

ArrayTests.java: 
```
import static org.junit.Assert.*;
import org.junit.*;

public class ArrayTests {
	@Test 
	public void testReverseInPlace() {
    int[] input1 = {3,4,5,6,1,2};
    Arrays.reverseInPlace(input1);
    assertArrayEquals(new int[]{2,1,6,5,4,3}, input1);
	}


  @Test
  public void testSort() {
    int[] input1 = {3,4,5,6,1,2};
    int[] sorted = Arrays.sort(input1);
    assertArrayEquals(new int[]{1,2,3,4,5,6}, sorted);
  }
}
```

test.sh: 
```
javac -g -cp ".;lib/hamcrest-core-1.3.jar;lib/junit-4.13.2.jar" *.java

java -cp ".;lib/junit-4.13.2.jar;lib/hamcrest-core-1.3.jar" org.junit.runner.JUnitCore ArrayTests
```

**Command Lines to trigger the bug**

`$ bash test.sh` to run JUnit tests at the beginning before fixing the bug.<br>
`$ javac -g -cp ".;lib/hamcrest-core-1.3.jar;lib/junit-4.13.2.jar" *.java` compile the java files using `-g` option that adds information for the debugger in the class files. <br>
`$ jdb -classpath ".;lib/junit-4.13.2.jar;lib/hamcrest-core-1.3.jar" org.junit.runner.JUnitCore ArrayTests` running jdb on the test file `ArrayTests.java`. <br>
`> stop at ArrayTests:17` to create a breakpoint before checking if the `sorted` array is equal to expected result. 
`run` to run the debugger. 
`main[1] dump sorted` to print out the contents of the array after running the `sort` method. 
`main[1] exit` to exit the debugger. 
`vim Arrays.java` open the java file in vim to edit it. 
`25<J> 14<E> <x><X> :wq` the keys pressed to fix the bug in the java file and save it. 



