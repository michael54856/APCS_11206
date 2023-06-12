##  1. 路徑偵測 
用一個變數dir來代表目前所朝向的方向<br>
這邊用0,1,2,3來分別代表上、下、左、右<br>
面朝某一個方向時都有4種的移動方式(上、下、左、右)<br>
所以總共要做出16個判斷<br>
<img src="https://github.com/michael54856/APCS_11206/blob/main/image/directionDiscription.jpg" width="50%" />
<br>

```cpp
#include <iostream>
using namespace std;


int main()
{
    int n;
    cin >> n;

    int dir = 3;

    int x = 0;
    int y = 0;

    //dir = 0 : 上
    //dir = 1 : 下
    //dir = 2 : 左
    //dir = 3 : 右

    int ans[3] = {}; //左轉，右轉，迴轉

    for(int i = 0; i < n; i++)
    {
        int nextX,nextY;
        cin >> nextX >> nextY;

        if(dir == 0) //現在面朝上面
        {
            if(nextY - y > 0) //上
            {

            }
            else if(nextY - y < 0) //下
            {
                ans[2]++;
                dir = 1;
            }
            else if(nextX - x < 0) //左
            {
                ans[0]++;
                dir = 2;
            }
            else if(nextX - x > 0) //右
            {
                ans[1]++;
                dir = 3;
            }
        }
        else if(dir == 1) //現在面朝下面
        {
            if(nextY - y > 0) //上
            {
                ans[2]++;
                dir = 0;
            }
            else if(nextY - y < 0) //下
            {

            }
            else if(nextX - x < 0) //左
            {
                ans[1]++;
                dir = 2;
            }
            else if(nextX - x > 0) //右
            {
                ans[0]++;
                dir = 3;
            }
        }
        else if(dir == 2) //現在面朝左邊
        {
             if(nextY - y > 0) //上
            {
                ans[1]++;
                dir = 0;
            }
            else if(nextY - y < 0) //下
            {
                ans[0]++;
                dir = 1;
            }
            else if(nextX - x < 0) //左
            {

            }
            else if(nextX - x > 0) //右
            {
		ans[2]++;
                dir = 3;
            }
        }
        else if(dir == 3) //現在面朝右邊
        {
             if(nextY - y > 0) //上
            {
                ans[0]++;
                dir = 0;
            }
            else if(nextY - y < 0) //下
            {
                ans[1]++;
                dir = 1;
            }
            else if(nextX - x < 0) //左
            {
		ans[2]++;
                dir = 2;
            }
            else if(nextX - x > 0) //右
            {

            }
        }

        x = nextX;
        y = nextY;

    }
    cout << ans[0] << " " << ans[1] << " " << ans[2];
}
```

##  2. 特殊位置 
檢查每一個點是否都是特殊位置<br>
比較好的做法應該是用BFS和其他技巧，但考試不考慮複雜度，用最暴力的方法即可<br>
我們會先選出某一個點，再看距離這個點曼哈頓距離為X內的所有格子<br>
但直接找出這些點需要用到BFS，因此我們不會直接找這些點<br>
我們會框出他的範圍，再去檢查這個範圍內的各個點，曼哈頓距離是否在X內<br>
都符合條件才會加到總和中<br>
另一個要注意的點是，因為我們一開始要先輸出特殊位置有幾個<br>
因此得到的答案要先存起來，最後再輸出<br>

藍色是陣列範圍，紅色是對於這一格所要檢查的範圍<br>
<img src="https://github.com/michael54856/APCS_11206/blob/main/image/grid_description1.png"/>

有可能我們要看的範圍是超出陣列範圍的，因此要把看的界線強行更改到陣列範圍內<br>
<img src="https://github.com/michael54856/APCS_11206/blob/main/image/grid_description2.png"/>

```
#include <iostream>
using namespace std;

bool check(int a[50][50], int y, int x, int m, int n)
{
	//定義左上角的座標與右下角的座標
    int startY = y - a[y][x];
    int startX = x - a[y][x];
    int endY = y + a[y][x];
    int endX = x + a[y][x];

	//進行正規化，避免超出陣列範圍
    if(startY < 0)
    {
        startY = 0;
    }
    if(startX < 0)
    {
        startX = 0;
    }
    if(endY >= m)
    {
        endY = m-1;
    }
    if(endX >= n)
    {
        endX = n-1;
    }

    int sum = 0;

    for(int i = startY; i <= endY; i++)
    {
        for(int j = startX; j <= endX; j++)
        {
            if(abs(i-y) + abs(j-x) <= a[y][x]) //檢查曼哈頓距離
            {
                sum += a[i][j];
            }
        }
    }

    if(sum % 10 == a[y][x])
    {
        return true;
    }
    return false;

}


int main()
{
    int a[50][50];
    int m,n;
    cin >> m >> n;
    for(int i = 0; i < m; i++)
    {
        for(int j = 0; j < n; j++)
        {
            cin >> a[i][j];
        }
    }

    int ans[2500][2];
    int ansCount = 0;

    for(int i = 0; i < m; i++)
    {
        for(int j = 0; j < n; j++)
        {
            if(check(a,i,j,m,n) == true) //符合就存下來
            {
                ans[ansCount][0] = i;
                ans[ansCount][1] = j;
                ansCount++;
            }
        }
    }
	
	//輸出
    cout << ansCount << endl;
    for(int i = 0; i < ansCount; i++)
    {
        cout << ans[i][0] << " " << ans[i][1] << endl;
    }
}
```

##  3. 磁軌移動序列 

##  4. 開啟寶盒 
待補充.......