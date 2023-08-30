# reusable-codes


# Inversion count

```cpp
#include <bits/stdc++.h> 
long long merge(long long *arr,int l,int mid,int r){
    int n1 = mid - l + 1;
    int n2 = r - mid;
    long long inv = 0;
    long long a[n1],b[n2];
    for(int i=0;i<n1;i++)
        a[i] = arr[l+i];
    for(int j=0;j<n2;j++)
        b[j] = arr[mid + 1 + j];
    

    int i=0,j=0,k=l;
    while(i<n1 && j<n2){
        if(a[i]<b[j]){
            arr[k++] = a[i++];
        }
        else{
            inv += n1 - i;
            arr[k++] = b[j++];
        }
    }
    while(i<n1)
        arr[k++] = a[i++];
    while(j<n2)
        arr[k++] = b[j++];

    return inv;
    
    
}
long long countInversion(long long *arr,int l,int r){
   while(l<r){
        long long inv = 0;
        int mid = l + (r - l)/2;
        inv += countInversion(arr,l,mid);
        inv += countInversion(arr,mid+1,r);
        inv += merge(arr,l,mid,r);
        return inv;
   }
   return 0;
}
long long getInversions(long long *arr, int n){
    return countInversion(arr,0,n-1);
}
```

# Segment Tree
```cpp
    class SGTree {
		vector<int>seg;
		bool isMaxm = false; // minimum by default;
	public:
		SGTree(int n,bool isMaxm){
			seg.resize(4*n+1);
			this->isMaxm = isMaxm;
		}
		void build(int ind,int low,int high,int *arr){
			if(low == high){
				seg[ind] = arr[low];
				return;
			}
			int mid = low + (high-low)/2;
			build(2*ind+1,low,mid,arr);
			build(2*ind+2,mid+1,high,arr);
			seg[ind] = isMaxm ? max(seg[2*ind+1],seg[2*ind+2]) : min(seg[2*ind+1],seg[2*ind+2]);
		}

		int query(int ind,int low,int high,int l,int r){
			if(r < low || l>high) return isMaxm ? INT_MIN : INT_MAX;
			if(low >= l && high <= r) return seg[ind];
			int mid = low + (high-low)/2;
			return isMaxm ? max(query(2*ind+1,low,mid,l,r),query(2*ind+2,mid+1,high,l,r)) : min(query(2*ind+1,low,mid,l,r),query(2*ind+2,mid+1,high,l,r));
		}

		void update(int ind,int low,int high,int i,int val){
			if(low == high){
				seg[ind] = val;
				return;
			}
			int mid = low+(high-low)/2;
			if(i<=mid) update(2*ind+1,low,mid,i,val);
			else update(2*ind+2,mid+1,high,i,val);
			seg[ind] = isMaxm ? max(seg[2*ind+1],seg[2*ind+2]) : min(seg[2*ind+1],seg[2*ind+2]);
		}
};
    ```
