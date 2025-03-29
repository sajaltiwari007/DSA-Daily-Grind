## Problem Link 
https://www.geeksforgeeks.org/problems/job-sequencing-problem-1587115620/1?_gl=1*167iuvk*_up*MQ..*_gs*MQ..&gclid=Cj0KCQjwkZm_BhDrARIsAAEbX1GQW1Wvp-bp2Vw9WSgfRsQlRn2KNOxAP10qdX2dDo71kJ9zBjAyhnEaAtlwEALw_wcB
## Problem Statment
You are given two arrays: deadline[], and profit[], which represent a set of jobs, where each job is associated with a deadline, and a profit. Each job takes 1 unit of time to complete, and only one job can be scheduled at a time. You will earn the profit associated with a job only if it is completed by its deadline.
Your task is to find:
The maximum number of jobs that can be completed within their deadlines.
The total maximum profit earned by completing those jobs.

Examples :
Input: deadline[] = [4, 1, 1, 1], profit[] = [20, 10, 40, 30]
Output: [2, 60]
Explanation: Job1 and Job3 can be done with maximum profit of 60 (20+40).

Input: deadline[] = [2, 1, 2, 1, 1], profit[] = [100, 19, 27, 25, 15]
Output: [2, 127]
Explanation: Job1 and Job3 can be done with maximum profit of 127 (100+27).

Input: deadline[] = [3, 1, 2, 2], profit[] = [50, 10, 20, 30]
Output: [3, 100]
Explanation: Job1, Job3 and Job4 can be completed with a maximum profit of 100 (50 + 20 + 30).

Constraints:
1 ≤ deadline.size() == profit.size() ≤ 105
1 ≤ deadline[i] ≤ deadline.size()
1 ≤ profit[i] ≤ 500

## Approach
The Job Sequencing Problem is a classic Greedy Algorithm problem where we aim to maximize total profit while scheduling jobs within their respective deadlines.
This implementation:
Stores Jobs as (Deadline, Profit) Pairs:
The deadline[] and profit[] arrays are converted into a 2D array where each row represents (deadline, profit).
Sorts Jobs in Descending Order of Profit:
To maximize profit, the job with the highest profit is considered first.
Uses a TreeSet to Efficiently Find the Earliest Available Slot:
The TreeSet<Integer> stores available time slots (days).
When scheduling a job with deadline d, we check if a slot ≤ d is available (using set.floor(d), which finds the largest available time ≤ d).
If a valid slot is found, it is removed from TreeSet to ensure it's not reused.

## Algorithm 
Store jobs as (deadline, profit) pairs.
Sort jobs in descending order of profit.
Find the maximum deadline maxi.
Use a TreeSet to store available time slots from 1 to maxi.
Iterate over sorted jobs:
Find the latest available time slot (set.floor(deadline)).
If a valid slot is found, schedule the job, increase profit, and remove the used slot from TreeSet.
Return the total count of scheduled jobs and the total profit.

## Complexity
Sorting Jobs:
Sorting takes O(n log n) time.
TreeSet Operations:
set.floor(d) (finding the latest available slot) takes O(log maxi).
set.remove(lower) (removing the used slot) takes O(log maxi).
Since each job is processed once, total TreeSet operations take O(n log maxi).
Overall Complexity:
Sorting: O(n log n)
Scheduling (TreeSet operations): O(n log maxi)
Total: O(n log n + n log maxi) ≈ O(n log n) (since maxi ≤ n in most cases)

## key Takeaway -:
TreeSet Search in less complexity.

## My Java Solution


        
    class Solution {
    public ArrayList<Integer> jobSequencing(int[] deadline, int[] profit) {
        int n = profit.length;
        int[][] arr = new int[n][2];
        for(int i=0;i<n;i++){
            arr[i][0]=deadline[i];
            arr[i][1]=profit[i];
        }
        Arrays.sort(arr, (a,b)->Integer.compare(b[1],a[1]));
        int maxi=-1;
        for(int i:deadline) maxi = Math.max(i,maxi);
        TreeSet<Integer> set = new TreeSet<>(); // Important data Struture
         for(int i=1;i<=maxi;i++) set.add(i);
         
         int count=0;
         int prof=0;
         for(int i=0;i<arr.length;i++){
             
             int end = arr[i][0];
             Integer lower = set.floor(end); // returs the greatest value lesser than equal to end in log time.
             
             if(lower!=null){
                 count++;
                 prof+=arr[i][1];
                 set.remove(lower);
             }
         }
         ArrayList<Integer> list = new ArrayList<>();
         list.add(count);
         list.add(prof);
         
         return list;
        
       
    }}
