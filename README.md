# DP
https://www.techiedelight.com/?problem=ActivitySelectionProblem

### 一般來說用Greedy Algorithmn
> 1. 對活動進行升序排序。
> 2. 然後，我們loop每個活動, 
>      選擇開始時間大於或等於先前選擇的活動的結束時間的第一個活動。 
>       我們繼續這個過程，直到我們選擇了所有的活動。 


```java
import java.util.*;
class Solution {
    public static Set<Activity> selectActivity(List<Activity> activities) {
        // Sort the activities based on their finish time in ascending order
        Collections.sort(activities, Comparator.comparing(Activity::getFinish));

        Set<Activity> selectedActivities = new HashSet<>();

        Activity previousActivity = null;
        for (Activity currentActivity : activities) {
            if (previousActivity == null || currentActivity.getStart() >= previousActivity.getFinish()) {
                // If the start time of the current activity is greater than or equal to the finish time of the previous activity, select the current activity
                selectedActivities.add(currentActivity);
                previousActivity = currentActivity;
            }
        }

        return selectedActivities;
    }
}
```

### 用Dynamic Programming 

To solve this problem using dynamic programming, we can use a similar approach as the greedy approach, but with memoization. We can define a 1D array dp of size n, where n is the number of activities. The value of dp[i] will represent the maximum number of activities that can be performed from the first i activities.

We can initialize `dp[0]` to 1, since the first activity can always be performed. Then, for each subsequent activity j, we can check if it overlaps with any of the previous activities i (i.e., if the finish time of activity i is greater than or equal to the start time of activity j). If it doesn't overlap, then the maximum number of activities that can be performed from the first j activities is equal to the maximum of dp[i] for all i less than j, plus 1 for the current activity. If it does overlap, then we can't perform the current activity, so the maximum number of activities that can be performed from the first j activities is equal to the maximum of `dp[i]` for all i less than j.

Finally, the maximum number of activities that can be performed from all the activities is equal to the maximum value in the dp array.

Here's the implementation of the selectActivity method using dynamic programming in Java:


```java
import java.util.*;

class Solution {
    public static Set<Activity> selectActivity(List<Activity> activities) {
        // Sort the activities based on their start time in ascending order
        Collections.sort(activities, Comparator.comparing(Activity::getStart));

        int n = activities.size();
        int[] dp = new int[n];
        dp[0] = 1;

        // Initialize the dp array
        for (int i = 1; i < n; i++) {
            int max = 0;
            for (int j = 0; j < i; j++) {
                if (activities.get(j).getFinish() <= activities.get(i).getStart()) {
                    max = Math.max(max, dp[j]);
                }
            }
            dp[i] = max + 1;
        }

        // Find the maximum number of activities that can be performed
        int maxActivities = Arrays.stream(dp).max().orElse(0);

        // Reconstruct the selected activities
        Set<Activity> selectedActivities = new HashSet<>();
        int currentActivities = maxActivities;
        for (int i = n - 1; i >= 0 && currentActivities > 0; i--) {
            if (dp[i] == currentActivities) {
                selectedActivities.add(activities.get(i));
                currentActivities--;
            }
        }

        return selectedActivities;
    }
}
```
