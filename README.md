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
# Hash Function using 2 modulos
```cpp
	int add(int a,int b,int mod){
	int res = (a+b)%mod;
	if(res<0)
		return res+mod;
	return res;
}
int mult(int a,int b,int mod){
	int res = (a*1LL*b)%mod;
	return res<0 ? res+mod : res;
}

int pow(int a, int b , int mod){
	int res=1;
	while(b){
		if((b&1)==1){
			res = mult(res,a,mod);
		}
		a = mult(a,a,mod);
		b>>=1;
	}

	return res;
}


class Hash {
private:
	vector<int>p_inv1,p_inv2;
	vector<pair<int,int>>hash;
	int p,M1,M2;
public:
	Hash(int n,int pValue,int modValue1,int modValue2): p_inv1(n),p_inv2(n),hash(n),p(pValue),M1(modValue1),M2(modValue2) {
		int pw_inv1 = pow(p,M1-2,M1);
		int pw_inv2 = pow(p,M2-2,M2);
		p_inv1[0]=1;
		p_inv2[0]=1;
		for(int i=1;i<n;i++){
			p_inv1[i] = mult(p_inv1[i-1],pw_inv1,M1);
			p_inv2[i] = mult(p_inv2[i-1],pw_inv2,M2);
		}
	}
	void Build(string &str){
		int p_pow1=1,p_pow2=1;
		for(int i=0;i<str.size();i++){
			int h1 = add((i==0) ? 0 : hash[i-1].first,mult(str[i]-'a'+1,p_pow1,M1),M1);
			int h2 = add((i==0) ? 0 : hash[i-1].second,mult(str[i]-'a'+1,p_pow2,M2),M2);
			// deb2(h1,h2);
			hash[i]={h1,h2};
			p_pow1 = mult(p_pow1,p,M1);
			p_pow2 = mult(p_pow2,p,M2);
		}	
	}

	pair<int,int> getHash(int x,int y){
		int res1 = add ( hash[y].first , (x==0) ? 0 : -hash[x-1].first , M1 );
		int res2 = add ( hash[y].second , (x==0) ? 0 : -hash[x-1].second , M2 );
		res1 = mult (res1, p_inv1[x] , M1 );
		res2 = mult (res2, p_inv2[x] , M2);
		return {res1,res2};
	}
};
```
