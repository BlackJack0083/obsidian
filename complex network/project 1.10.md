`strength`代码：
```c++
void Net::Strength() {  
    int i, j, l, n, m;  
    int f, t;  
    long double sum;  
    char strengthfilename[512];  
    FILE* fout;  
    sprintf(strengthfilename, "strength.txt");  
    fout = fopen(strengthfilename, "a");  
        for (i = 0; i < edgeNumber; i++) {  
        edge[i].strength = 0;  
    }  
        for (i = 0; i < edgeNumber; i++) {  
        sum = 0; //count the number of triangles passing through through node i  
        f = edge[i].edgeIndexfrom;  
        t = edge[i].edgeIndexto;  
                for (j = 0; j < node[f].k; j++) {  
            m = node[f].nodeIndex[j];  
            if (m > -1) {  
                for (l = 0; l < node[t].k; l++) {  
                    n = node[t].nodeIndex[l];  
                    if (n > -1) {  
                        if (n == m)sum++;  
                    }  
                }  
            }  
        }  
        if (min(node[f].k, node[t].k) == 1) edge[i].strength = 0;  
        else edge[i].strength = sum / double(min(node[f].k, node[t].k) - 1);  
                fprintf(fout, "%Lf\n", edge[i].strength);  
        fflush(fout);  
    }  
    fclose(fout);  
        maxstrength = 0;  
    for (i = 0; i < edgeNumber; i++) {  
        if (maxstrength < edge[i].strength)maxstrength = edge[i].strength;  
    }  
    cout << "maxstrength " << maxstrength << endl;  
}
```
关键部分修改为：
```c++
if (min(node[f].k, node[t].k) == 1) edge[i].strength = 0;  
        else edge[i].strength = sum / double(min(node[f].k, node[t].k) - 1);  
                fprintf(fout, "%Lf\n", edge[i].strength); 
```

匹配$$s_{ij}=\frac{n_{ij}}{\min \{k_i,k_j\}-1}$$

`spread`部分修改：
- 不考虑信息衰减： $p$ 始终为1
- $c_{ij}= (\delta + s_{ij}^\alpha$)
- $c_0 += c_{ij}$
```c++
void Net::Spread(long double lambda1, long double decay1, long double alpha1, long double ct1) {  
    int i, j, n, r;  
    int x, y, f;  
    int judge;  
    long double pp, c_xy;  
    //int clusternumber;  
        judgeo = 0;  
    for (r = 0; r < Round && judgeo == 0; r++) {  
        for (i = ins; i > 1; i--) {      //shuffle the list - randomlization of the infective list  
            pp = genrand64_real1();  
            j = int(pp * i);  
            n = ig[j];  
            ig[j] = ig[i - 1];     // mg+j <--> mg+n-1 to ensure every node in the infective list will be shuffled  
            ig[i - 1] = n;  
        }  
        ///////传播过程  
        for (i = 0; i < ins; i++) {  
            x = ig[i]; ////randomly select a node  
            for (n = 0; n < node[x].k; n++) {  
                y = node[x].nodeIndex[n];  
                f = node[x].edgeIndex[n];  
                if (node[y].s == -1) {  
                    if (lambda1 > genrand64_real1()) {  
                        c_xy = pow(edge[f].strength, alpha1);  
                        node[y].content += c_xy + delta;  
                    }  
                                        if (node[y].content > ct1) {  
                        node[y].s = 0; ///the node is infected by epidemic l  
                        judge++;  
                    }  
                }  
            }  
        }  
        //////////////////////////////////////////////////////////////////////  
        for (i = 0; i < nodeNumber; i++) {  
            if (node[i].s == 0) {  
                if (recover > genrand64_real1()) {  
                    node[i].s = 1; ///the node recovers from an infection with epidemic j after one  
                    node[i].time = 0;  
                }  
            }  
        }  
                ins = 0;  
        for (i = 0; i < nodeNumber; i++) {  
            if (node[i].s == 0) {  
                ig[ins] = i;  
                ins++;  
                node[i].time++;  
            }  
        }  
                if (judge > 0)realround++;  
        if (ins == 0)judgeo = 1;  
    }  
        steadytime = r;  
    rfinal = 0;  
    for (i = 0; i < nodeNumber; i++) {  
        if (node[i].s > 0)rfinal += 1.0;  
    }  
    rfinal = rfinal / double(nodeNumber); /////统计系统内的恢复比例  
}
```
关键部分修改为：
```c++
for (i = 0; i < ins; i++) {  
	x = ig[i]; ////randomly select a node，x为感染的结点
	for (n = 0; n < node[x].k; n++) {  
		y = node[x].nodeIndex[n];   //y为x的邻居节点 
		f = node[x].edgeIndex[n];   //f为xy的便强度
		if (node[y].s == -1) {  
			if (lambda1 > genrand64_real1()) {  
				c_xy = pow(edge[f].strength, alpha1);  
				node[y].content += c_xy + delta;  
			}  
			if (node[y].content > ct1) {  
				node[y].s = 0; ///the node is infected by epidemic l  
				judge++;  
			}  
		}  
	}  
}
```


