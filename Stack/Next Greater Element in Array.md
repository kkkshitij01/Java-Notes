
```java 
    public static int[] bruteForce(int arr[]) {

        int ans[] = new int[arr.length];

        ans[arr.length - 1] = -1;

        for (int i = 0; i < arr.length - 1; i++) {

            int idx = i + 1;

            while (idx < arr.length && arr[i] >= arr[idx]) {

                idx++;

            }

            ans[i] = idx == arr.length ? -1 : arr[idx];

        }
        return ans;

    }
```