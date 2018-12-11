Title: Unity上實作坦克炮塔的瞄準以及轉向音效
Date: 2018-12-11
Category: csharp
Tags: csharp, tutorial, unity, game
Slug: unity-tank-aim-control
Author: kuoteng

# 瞄準

這學期的計算機圖型課程有一份作業是要我們使用 Unity 寫出一款坦克遊戲，一想到坦克遊戲就想到之前在玩的 War thunder ...

<div class="youtube youtube-4x3">
<iframe src="https://www.youtube.com/embed/TD2GcubWX_k"></iframe>
</div>

## 修正座標系

裡面有個操控是在第一人稱和第三人稱視角時，都能以鼠標指向來操控砲塔指向，如果我們要實作這個操控，必須先查看砲塔物件的座標軸是否是正確的 ( z 軸正向應該是物件正向)，所以在砲塔這個物件中

為此，我們在砲塔物件中加入以下 script (在 update 這個方法中)：

```c#
    Debug.DrawRay(transform.position, transform.forward*10f,Color.blue);
    Debug.DrawRay(transform.position, transform.up*10f,Color.green);
    Debug.DrawRay(transform.position, transform.right * 10f, Color.red);
```

![tank-01](https://i.imgur.com/MulJSb2.png)

這樣會畫出砲塔的座標軸方向，對應到藍色、綠色、紅色

![tank-02](https://i.imgur.com/Prrg584.png)

可以看到座標軸是錯的，所以我們應該要修正座標軸，這邊要怎麼修正已經加入 Unity 場景中的物件呢？

- 我在 Unity 的官方論壇上找到了[解答](https://answers.unity.com/questions/62675/redefine-axis-of-an-object.html)：

    1. 先新增一個新的空 GameObject (隨便命名，像我就設置成`TowerAgent`)
    2. 將 GameObject 置於需要修正物件下，使之成為其子物件
    3. Reset 空 GameObject 的座標，這個空物件的座標應該會顯示為 (1, 1, 1)，但他實際上已經是母物件的座標了
    4. 將空 GameObject 拉出修正物件，並且設置對應的座標轉向
    5. 設置轉向後將修正物件置於空 GameObject 下，使修正物件成為其子物件

## 計算出轉向角

Unity 中有一好用的函式 `ScreenPointToRay` 可以計算出場景照相機到場景某一點的向量，剛好我們有第一人稱視角的攝影機 Camera1

![Camera1](https://i.imgur.com/UT76C11.png)

![Camera1-1](https://i.imgur.com/DHtAwa5.png)

（記住 Camera1不能在砲塔中，否則照相機會跟著我們砲塔轉動，而永遠不停止，我這邊卡超久><）


我們就以此照相機來計算 (你必須先將 camera1 設置成 `Camera1`) 方向向量 dir，並以黃線畫出，在沒有偵測到滑鼠與場景的交點時將此方向向量設置為物件正向 (以下 code 置於 FixedUpdate 方法中)

```csharp
Ray ray = camera1.ScreenPointToRay(Input.mousePosition);
RaycastHit hit;
Vector3 dir;

if (Physics.Raycast(ray, out hit))
{
    Vector3 hitpos = new Vector3(hit.point.x, transform.position.y, hit.point.z);
    dir = (hitpos - transform.position).normalized;
}
else {
    dir = transform.forward;
}
Debug.DrawRay(transform.position, dir*10f, Color.yellow);

float angle = Vector3.Angle(transform.forward, dir);
Quaternion rotate = Quaternion.RotateTowards(transform.rotation,
                                            Quaternion.LookRotation(dir,transform.up),
                                            RotateSpeed * Time.deltaTime);
this.transform.rotation = rotate;
```

## 視角轉換

但我們並不只有第一人稱視角相機，所以我們將原本的相機設為當前使用的場景攝影機 `Camera.current`:

```csharp
Ray ray = Camera.current.ScreenPointToRay(Input.mousePosition);
```

最後結果：

![](https://i.imgur.com/xRbrhGI.gifv)

# 音效

而砲塔轉向有音效，人工轉動的砲塔如 KV 系列會有手搖輪桿的聲音，而電動砲塔則有電動傳帶的聲音

我們可以觀看目前物件的正向 (物件的 z 軸正向) 與轉動向量的夾角度數是否小於一定值，如果是則在 FixedUpdate 方法中改變 private 布林變數 is_looping

```csharp
if(Vector3.Angle( transform.forward, dir) < 0.2) {
    is_looping = false;
}
else {
    is_looping = true;
}
```

當然...在 Start 方法中要記得初始化這個布林值
```csharp
is_looping = true
```

最後在 Update 方法中處理音訊的播放與停止與否，其中 `loopSound` 是 public AudioSource，必須先預設

```csharp
if(is_looping) {
    if (!loopSound.isPlaying){
        loopSound.Play();
    }
}
else {
    loopSound.Stop();
}
```

大功告成！
