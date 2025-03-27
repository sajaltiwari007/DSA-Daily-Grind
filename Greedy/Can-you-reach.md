## Probelm Link - 
https://www.codechef.com/problems/FRMN?tab=statement

## Probelm Appraoch-:
1. Identifying Extreme Peaks
A peak at index i is a maxima if H[i] > H[i-1] and H[i] > H[i+1].
A peak at index i is a minima if H[i] < H[i-1] and H[i] < H[i+1].
A peak is considered extreme if it is either a maxima or a minima.

2. Observing Movement Restrictions
Since adjacent differences in height are always 1, it is impossible to cross an extreme peak.
To cross a maxima, the movement would be x → x+1 → x, which is not allowed.
This restriction implies that movement is blocked at extreme peaks.

3. Understanding Reachability
Consider two positions i and j (i < j):
If there are no extreme peaks between i and j, they can meet directly.
If there is exactly one extreme peak strictly between i and j, they can still meet by moving toward that peak.
If there are two or more extreme peaks between i and j, they cannot meet at all because:
i cannot cross the nearest extreme peak to its right.
j cannot cross the nearest extreme peak to its left.
Since these peaks are different, i and j can never reach each other.

4. Efficient Calculation Using next[]
Define an array next[i], where next[i] stores the next extreme peak to the right of i.
This can be computed in O(N) time:
If i+1 is an extreme peak, set next[i] = i+1.
Otherwise, inherit the value from next[i+1].

5. Counting Valid Pairs
For each position i, find the number of positions j (j > i) that can be reached:
The first extreme peak to the right of i is at next[i].
The second extreme peak to the right of i is at next[next[i]].
All positions in the range [i+1, next[next[i]]] are reachable.
Add the size of this range to the final count.

Time Complexity
Identifying extreme peaks: O(N)
Precomputing next[]: O(N)
Counting valid pairs: O(N)
✅ Total Complexity: O(N) per test case.

## MY JAVA SOLTUION


		
		class Codechef
     {
	public static void main (String[] args) throws java.lang.Exception
	{
		Scanner sc = new Scanner(System.in);
    int t = sc.nextInt();
    for(int i=0;i<t;i++){
		    
		    int n = sc.nextInt();
		    int[] arr = new int[n];
		    
		    for(int j=0;j<n;j++) arr[j] = sc.nextInt();
		    
		    System.out.println(helper(n,arr));
		}

	}
	static long helper(int n , int[] arr){
	    
	    boolean[] extrem = new boolean[n];
	    
	    for(int i=1;i<=n-2;i++){
	        
	        if((arr[i]>arr[i-1] && arr[i]>arr[i+1])||(arr[i]<arr[i-1] && arr[i+1]>arr[i]))
	        extrem[i] = true;
	    }
	    
	    long[] next = new long[n];
	    Arrays.fill(next , n);
	    for(int i=n-2;i>=0;i--){
	        
	        next[i] = extrem[i+1]?(long)i+1:next[i+1];
	    }
	    
	    long answer=0;
	   // System.out.println(Arrays.toString(next));
	    
	    for(int i=0;i<=n-2;i++){
	        
	        long second = next[i]!=(long)n?next[(int)next[i]]:(long)n;
	        if(second==n)
	        answer+=(long)(second-i-1);
	        else
	        answer+=(long)(second-i);
	    }
	    
	    return answer;
	}}
