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
##  3. 磁軌移動序列 
##  4. 開啟寶盒 