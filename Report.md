# Lab Report 5

## EdStem Post

Student, <br><br>
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
<br>
TA, <br><br>
Hi, I suggest running jdb (Java Debugger) on the Test
