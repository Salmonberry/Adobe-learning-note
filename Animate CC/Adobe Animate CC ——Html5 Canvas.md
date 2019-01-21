# Adobe Animate CC ——Html5 Canvas
## Question:

### **Q1：**

- <span style="font-size:20px">**通過點擊事件，跳轉到指定幀，本該停止播放狀態的影片剪輯會自動播放**</span>

![2018-12-18_10-32-06](.\src\img\2018-12-18_10-32-06.gif)

- **Slove**:
    - **why：**主要 原因是該影片剪輯在時間軸上的時間長度沒有足夠長到<span style="color:red;font-weight:bold">againButton</span>的所在時間的位置上。

    - **What:**
      - **時間域：**整個影片的所在時間軸上占用的時間

      - **時間域範圍：**該影片剪輯控件在時間軸上所占的時間長度的有效作用域，若該影片剪輯在播放的過程中保持某一時刻狀態，那麽只要在該時間軸上的有效作用域内，該狀態（在沒有設置循環播放 的情況下）保持一直不變，但是如果在該影片剪輯所在時間作用外，通過代碼跳轉的方式重新進入到該影片剪輯的有效作用範圍内，該影片剪輯就會被再次調用（該過程，與通過new的方式，實例化一個重新初始化對象類似，所以影片就會被重新調用）
    - **how:**調整影片的時間長度，讓影片的時間長度與整個影片動畫的時間長度一致，那麽就相當于一個全局對象，不會被重新被初始化調用
    ![img_02](.\src\img\img_02.png)

- **Summary：**
  - An的動畫<span style="color:green">**影片長度**</span>相當與程序的由運行到退出的<span style="color:green">**生命周期長度**</span>；而要對單一控件進行事件綁定控制調用，應保證該控件的時間長度是與An動畫長度是一致的，從而確定該控制的狀態作用于整個生命周期,就是編程技術上說的<span style="color:green">**全局對象**</span>；
  - 相反來説，若對某一控件多次復用，設置不同的時間長度，就相當於<span style="color:#0798C2">**實例化多個來自同一模板的同一控件**</span>，這裏和Unity中的prfab的應用一致，是一種實例化多個對象的方式。

----

**Q2：**

- <span style="font-size:20px">**設置點擊事件無響應**</span> 

![2018-12-19_14-30-36](.\src\img\2018-12-19_14-30-36.gif)

- **Slove:**
  - **why:**主要原因是==<span>**控件命名不唯一**</span>==，儅有控件被使用重複命名時，控件就無法有響應。
  - **what:**控件的name命名，在一個An的動畫中是全局de 唯一id，不允許重複的。
  - **how:**修改控件的命名，保證其唯一性。

![2018-12-19_14-36-05](.\src\img\2018-12-19_14-36-05.png)
<span></span>
![2018-12-19_14-34-59](.\src\img\2018-12-19_14-34-59.png)

- **Summary:**
  -  在An中控件的命名時唯一的id名，不可以用同樣的命名賦予給多個控件
  -  在重複命名了。由於An不是專業的IDE也不會有相關的信息提示，只能通過綁定事件時，無法觸發相關的事件進行推測。

-----

**Q3：**

- <span style="font-size:20px">**由EaselJS 引起的設置點擊事件無響***</span>

![2018-12-19_14-30-36](.\src\img\2018-12-20_9-56-30.gif)

- **Slove:**
  - **why:**
    - 在 EaselJS 中，<span style="color:green">帧编号从 0 开始而不是从 1 开始</span>。这会影响一些调用，如 <span style="color:red">**gotoandstop() **</span>和<span style="color:red">**gotoAndPlay()**</span> 调用。
    - 如果从本地文件系统运行同时具有位图和按钮的内容，一些浏览器可能会生成本地安全性错误。
  - **what:**

  ```javascript
  //action217幀----EaselJS216幀
  this.tufuButton.addEventListener("click", fl_ClickToGoToAndStopAtFrame_6.bind(this));
  function fl_ClickToGoToAndStopAtFrame_6()
  {
   this.gotoAndStop(218);
  }
  
  //action218幀----EaselJS217幀
  this.kkButton.addEventListener("click", fl_MouseClickHandler_3.bind(this));
  function fl_MouseClickHandler_3()
  {
  	// 开始您的自定义代码
  	// 此示例代码在"输出"面板中显示"已单击鼠标"。
  	alert("已单击鼠标");
  	// 结束您的自定义代码
  }
  ```
  這裏正是由於Animate與EaselJS中對幀數的定義不同而引起的歧義問題

  - **how:**
    - 對於所跳轉的兩個幀畫面，都爲控件關於 <span style="color:red">**gotoandstop() **</span>和<span style="color:red">**gotoAndPlay()**</span> 调用，應遵循EaseJs規範來設置幀數。確保調用<span style="color:red">**gotoandstop() **</span>和<span style="color:red">**gotoAndPlay()**</span> 方法時，所設置的幀數必須為An轉場畫面的第一楨的前一幀

![2018-12-19_14-36-05](.\src\img\2018-12-20_10-14-06.png)
<span></span>
![2018-12-19_14-34-59](.\src\img\2018-12-20_10-13-33.png)

- **Summary:**

  -  AnimateCC是根據CreateJS而驅動的H5模式，在涉及編程時，應遵循CreateJS的規範；而對於動畫的製作則可以按照原AnimateCC的套路來做
  -  EaselJS 中，<span style="color:green">帧编号从 0 开始而不是从 1 开始</span>。这会影响一些调用，如 <span style="color:red">**gotoandstop() **</span>和<span style="color:red">**gotoAndPlay()**</span> 调用。

  ![createJs](.\src\img\createJs.png)

 ### **Q4：**

- <span style="font-size:20px">**代碼片段設置在有效時間段非初始幀而導致的交互無效**</span>

![2018-12-18_10-32-06](.\src\img\2018-12-21_14-39-54.gif)

- **Slove**:
    - **why：**<span style="color:green">一個有效的時間片段，由初始時間到結束時間，是一個功能狀態的變化</span>，若初始就不存在相關的功能，後續添加相關的代碼也是無效（中間幀或末幀的狀態出現，並是一個狀態的變化過程量）。

    - **What:**
      - **有效的狀態變化：**應該從一個有效時間區間的初始幀開始設置

    - **how:**將末幀的代碼片段設置在初始幀上
      ![img_02](.\src\img\create.png)

- **Summary：**
  - AN中的代碼交互是與時間軸上的有效時間區段息息相關的，一個有效的代碼時間區段是從初態,發生態，末態的變化過程
  - 而在一個時間區段的生命周期都起作用的代碼片段，必須從初態（初始幀）上設置代碼片段，而對於到某一時間段停止的（單端在某一狀態下發生效果的，並沒有在整個生周期中具有全局性意思的），則可以在特定的幀上設置代碼片段。

----

### **Q5：** 
- <span style="font-size:20px">**部分圖片在渲染的時候無法顯示**</span>
- **Slove**:
    - **why：**An在轉換為Html5 Canvas時，其圖片的默認轉換格式為**<span style="color:green">svg</span>**，若工程文件中使用的是Jpg,Jpeg,png格式的文件在轉換為svg會出現異常，在工程中觀察是沒有異常 ,，但在渲染中會無法渲染
    - **how:**將庫中的原圖片替換轉換出錯的文件，就可以解決
- **Summary：**
  - n在轉換為Html5 Canvas時,Jpg,Jpeg,png格式的文件在轉換為svg會出現異常
  - 修改時，盡可能在庫中的相應元件中更改，原因在庫中的元件是一個prefab

