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

```cpp
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

這題使用遞迴的方式來解，這邊所有的變數都是共用的，除了head,tail是每層一個<br>
遇到T的話比較簡單，只要串上去就好，這邊我們需要head跟tail，如果head = -1，代表這是第一個數字<br>
遇到L就繼續進行遞迴，遞迴結束之後，我們可以得到下一段的cost是多少，也能知道其head與tail<br>
所以我們就能夠計算出cost = (下一段的cost x 有幾次) + (loop之間的串接) + (目前的tail串到下一段head的cost) <br>
只是要小心可能會有沒有head的狀況，就會少(目前的tail串到下一段head的cost)這一個<br>
直接看下面的code可能比較好理解<br>

```cpp
#include <iostream>
using namespace std;

long long int calculateCost(string &s, int &i, int &head, int &tail, int &len) //這邊s,i,len是共用參數，head,tail每層不同
{
    long long int cost = 0;
    for( ; i < len; )
    {
        if(s[i] == 'T')
        {
            int num = ((s[i+1]-'0')*10) + (s[i+2]-'0'); //計算數字
            i += 3; //移動指標
            if(head == -1) //檢查是否有頭，沒有頭的話不用加cost
            {
                head = num;
                tail = num;
            }
            else  //有頭的話就要加cost
            {
                cost += abs(tail-num);
                tail = num;
            }
        }
        else if(s[i] == 'L')
        {
            int times = (s[i+1]-'0'); //看次數
            i+=2;  //移動指標
            int nexthead = -1;
            int nextTail = -1;
            long long int recursionCost = calculateCost(s, i, nexthead, nextTail,len);  //繼續做遞迴，會回傳下一段遞迴的cost與頭跟尾分別是多少

            if(recursionCost == 0) //代表只有一個數字
            {
                if(head == -1) //沒有頭，不用加cost
                {
                    head = nexthead;
                    tail = nextTail;
                }
                else //有頭，需要加cost
                {
                    cost += abs(tail-nexthead);
                    tail = nextTail;
                }
            }
            else  //有cost需要計算
            {
                if(head == -1)
                {
                    cost += recursionCost*times;                  //計算中間那段重複的cost
                    cost += abs(nextTail-nexthead) * (times-1);   //計算遞迴中間進行連接的cost
                    head = nexthead;
                    tail = nextTail;
                }
                else
                {
                    cost += recursionCost*times;                 //計算中間那段重複的cost
                    cost += abs(nextTail-nexthead) * (times-1);  //計算遞迴中間進行連接的cost
                    cost += abs(tail-nexthead);                  //計算這個頭連接到下一段的cost
                    tail = nextTail;
                }

            }

        }
        else if(s[i] == 'E')
        {
            i++;
            return cost;
        }

    }
    return cost;
}

int main ()
{
    string s;
    cin >> s;
    int len = s.length();
    int i = 0;
    int head = 10;
    int tail = 10;
    long long int ans = calculateCost(s, i, head, tail,len);
    cout << ans;

}
```
##  4. 開啟寶盒 
待補充.......