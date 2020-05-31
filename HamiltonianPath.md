```cpp
#include <bits/stdc++.h>
using namespace std;
int dp[11][1<<11];
bool solve(vector<vector<int>> &graph,int node,int visited,int allVisited)
{
    if(visited==allVisited)
    {
        // if we want to check for hamiltonian cycle then we add one more condition to check if there exist a path between node and source
        return true;
    }
    if(dp[node][visited]!=-1)
        return dp[node][visited];
    
    int v = graph.size();
    
    
    for(int i=0;i<v;i++)
    {
        if((visited&(1<<i)) == 0 && graph[node][i])
        {
            if(solve(graph,i,(visited|(1<<i)),allVisited))
                return dp[node][visited]=true;
        }
        
    }
    
    return dp[node][visited] = false;

    
}

int main() {
    
    memset(dp,-1,sizeof dp);
    int n,m;
    cin>>n>>m;
    vector<vector<int>> graph(n,vector<int>(n));
    for(int i=0;i<m;i++)
    {
        int from,to;
        cin>>from>>to;
        graph[from-1][to-1] = 1;
        graph[to-1][from-1] = 1;
    }
    int allvisited = (1<<n)-1;

    
    bool flag = false;
    for(int i=0;i<n;i++)
    {
        // marking node i as visited and cheking if hamiltonian path starts from i
        if(solve(graph,i,1<<i,allvisited))
        {
            flag = true;
            break;

        }

    }

    if(flag==true)
        cout<<true<<endl;
    else
        cout<<false<<endl;

	return 0;
}
