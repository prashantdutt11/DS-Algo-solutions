```cpp
#include<limits>
long long dp[1000][1000];
int parent[1000][1000];
vector<int> answer;

void back(int l, int r,vector<int> &arr){
    //base case
    if(l>r)
        return;
    if(l==r)
    {
        answer.push_back(arr[parent[l][r]]);
        return;
    }
    

    //first choose parent of (l,r)
    answer.push_back(arr[parent[l][r]]);

    //call back recursively for two new segments
    //calling left segment first because we want lexicographically smallest
    
    back(l,parent[l][r]-1,arr);
    back(parent[l][r]+1,r,arr);
}
long long solve(vector<int> &arr,int i,int j)
{
    if(i>j)
    {
        
        return dp[i][j] = 0;
    }
        
    if(i==j)
    {
        parent[i][j] = i;
        return  dp[i][j] = arr[i+1]-arr[i] + arr[i]-arr[i-1];
    }
    
    if(dp[i][j]!=-1)
        return dp[i][j];
        
    int bestIndex;
    
    long long answer = LLONG_MAX;
    for(int k=i;k<=j;k++)
    {
        long long temp = (long long)arr[j+1] + (long long)arr[i-1] + (long long)solve(arr,i,k-1) + (long long)solve(arr,k+1,j);
        if(answer>temp)
        {
            answer = temp;
            bestIndex = k;
        }
        
    }

    parent[i][j] = bestIndex;
    
    return dp[i][j] = answer;
        
}


vector<int> Solution::rodCut(int N, vector<int> &arr) {
    if(N==1)
        return {};
    memset(dp,-1,sizeof dp);
    answer.clear();
    
    arr.insert(arr.begin(),0);
    arr.push_back(N);

    solve(arr,1,arr.size()-2);

    back(1,arr.size()-2,arr);
    return answer;
    
}
```
