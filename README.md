# reusable-codes

`
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
`
