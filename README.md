# DP
https://www.techiedelight.com/?problem=ActivitySelectionProblem

一般來說用Greedy Algorithmn

'''java
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
'''
