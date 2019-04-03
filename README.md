# CVFX-HW3
大綱:

1.Generate images with GANPaint

2.簡單概念的介紹

3.Dissect GAN model and analyze

4.Compare with other method

**********************************************************************************************************************************




Generate images with GANPaint:

我們這組選了兩張照片來做所有情況的比較

原圖:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/%E5%8E%9F%E6%AA%94.PNG)

結果:

brick:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/brick(mix).png)

                            before                                                           after

cloud:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/cloud(mix).png)

                            before                                                           after

dome:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/dome(mix).png)

                            before                                                         after

door:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/door(mix).png)

                            before                                                           after

grass:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/grass(mix).png)

                            before                                                           after

sky:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/sky(mix).png)

                            before                                                           after

tree:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/tree(mix).jpg)

                            before                                                           after

原圖:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/%E5%8E%9F%E5%9C%96(2).PNG)
   
結果:   

brick:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/brick(2_mix).png)

                            before                                                           after

cloud:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/cloud(2_mix).png)

                            before                                                           after

dome:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/dome(2_mix).png)

                            before                                                         after

door:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/door(2_mix).png)

                            before                                                           after

grass:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/grass(2_mix).png)

                            before                                                           after

sky:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/sky(2_mix).png)

                            before                                                           after

tree:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/tree(2_mix).png)

                            before                                                           after

***************************************************************************************************************************

簡單概念的介紹:

GAN 近年來發展得非常火熱，但 GAN 伴隨著幾個問題 e.g., 生成的品質不夠好， 訓練不夠穩定。

而上面這兩個問題是許多人的研究方向，但較少人在探討 GAN 內部的神經元以及每個 layer 都在學些什麼？

因此使用視覺化的方式探討 GAN 神經元。以圖片生成的任務為例，我們希望他會生成出特定場景的圖片。

而可視化能帶來什麼好處呢？

(1)可以知道 GAN 是在學習什麼樣的東西。

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/%E6%93%B7%E5%8F%96.JPG)

(2)尋找哪些神經元導致輸出的結果不好。

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/%E6%93%B7%E5%8F%961.JPG)

(3)知道哪些神經元導致輸出結果不好，那我們就讓那些神經元的輸出設為0。並且提出一個可互動的框架來調整神經元，可達到新增或是移除指定類別的功能。

概念

我們可以知道 CNN 是有所謂的空間相關性的。

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/%E6%93%B7%E5%8F%962.JPG)

那我們經過上面這個示意圖能夠知道，每個 pixel（Feature map） 都是對應到前面 Feature map 的某區域。

Dissection 解開:

先講結論：如果該 Unit 與某個類別是相關的，那麼其 Feature map(想像成熱力圖)會與語意分割後該類別的 Mask 相似。

透過語意分割模型將特定的類別切割出來，下圖將樹(Tree)的部分切割出來。

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/%E6%93%B7%E5%8F%963.JPG)

接下來我們再將某個 Unit 的 Feature map 與其分割出的 Mask 做比對，看看相不相似，下圖為架構的一部分。

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/%E6%93%B7%E5%8F%964.JPG)

透過這方式一一比對，當 Feature map 與某個類別的 Mask 長得夠像的時候，我們可以認定這個 Unit 與某類別之間是有較大關聯性的。

Intervention 介入:

透過上面的方式，我們能夠知道哪些 Unit 與某類別是相關的，但是我們卻不知道更動這些 Unit 的 Feature map 與最終產生出來的圖片(Output)有什麼關聯。

可能這個 Unit 與該類別雖然有關聯，但是實際上對產生出來的圖片影響不大，透過這個方式我們可以將其類別的 Unit 進行排序，排名(Rank)出對其類別最有影響力的 Unit，此實驗是對每個類別取 1…n（n=20） Units。

整體想法:
我們要先透過 Dissection 得知哪些 Unit 是與該類別相關，再經由 Intervention 明白改動這個 Unit 到底對最終輸出的該類別有著多大的影響。因此我認為 Dissection 與 Intervention 的關係可以用 “方向” 與 “能量大小” 的關係作比喻。

方法:

此文提出的架構如下，上半部為 Dissection 解開，下半部為 Intervention 介入

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/%E6%93%B7%E5%8F%965.JPG)

先看 G 的架構，對於圖像生成的任務，通常會輸入雜訊 z，然後生成圖片 x，而之所以可以生成 x 是因為 z 再經過一層一層的 layer 慢慢的轉變成圖片，這邊的想法是我們提取出第 r 層，來探討他每個 Unit 的 Feature map，而他的 Feature map 會繼續影響著後續的 f(layers) 最終生成圖片。

而我們的最終目的是希望找出哪些 Unit 是對結果有顯著影響的。這邊我們可以這樣想像，假設我們要生出辦公室的場景，那可能有些 Unit 各自負責桌子、椅子、人。

但是剛好這張圖片生成出來後，只有桌子和椅子，我們希望找到著重於桌子和椅子的 Unit，這樣就能對他做移除/修改的動作。

因此我們定義下面這公式

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/%E6%93%B7%E5%8F%966.JPG)

參數定義：
r : 某層 Layer， 可看架構圖 generator(G) 的部分
U : 哪些 Unit 是對目前想要操作的類別有影響的，舉例我可能想要修改椅子而已，我們要找出哪些 units 對椅子有影響。
U- : U 的補數，即為我們不感興趣的 units.

Dissection 解開:
此處我們要找到哪些 Unit 對該類別有影響，找尋哪些 Unit 的 Feature maps 與我們 Semantic segmentation 的模型預測出來的 Mask 相近。

而此處使用 IoU 的方式!

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/%E6%93%B7%E5%8F%967.JPG)

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/%E6%93%B7%E5%8F%968.JPG)

參數定義：

↑ : 將 Feature maps Resize 成 x 的大小。

Sc : 給定 x 後，獲得 c 這個類別的 Mask， 架構圖segmentation黃色處。

t : threshold，這部分比較複雜，有興趣的人在自己去了解。

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/%E6%93%B7%E5%8F%969.JPG)

透過這方式，我們可以知道 r 的哪些 Unit 與某個類別有著高度相關性。

不同 layer 之間學習到的特徵不同

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/%E6%93%B7%E5%8F%9610.JPG)

第 1 層我們看到還是會有誤判的情形，而且除了天花板這類別之外，其他的類別都沒學到（可看右半部 Unit class distribution）但是第 4 層學習到最多的類別，可是到第 10 層過後，反而開始學到紋理、邊緣一些細節等等，個人覺得或許是因為這是圖像生成的任務，導致後面的 Layer 會認真學習優化一些細節。

透過視覺化“手動”改善 GAN 扭曲

這部分是我個人覺得這論文最有意思的地方了，我們都知道 GAN 雖然這幾年一直進步，但是 GAN 還是會產生出怪怪的圖片。

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/%E6%93%B7%E5%8F%9611.JPG)

那我們可以透過可視化找出扭曲部分所對應的 Unit，可是怎麼找呢？ 他又不能被 Sc 分到一類，因此這邊提出來的方法是先產生 1000 張圖片，然後我們可以看每個 Unit 對應到分數最高的前幾張圖片，透過這種方式我們可以發現有的 Unit 專門產出很奇怪的圖片，那我們就可以手動將該 Unit 的 Feature map “Off”，透過這方式就明顯地將詭異的部分改善了，而作者提出人工將 layer 4 （512個units） 挑出 20 個這種 Unit 將他 Off，只需要 10 分鐘，換句話說，只要10分鐘效能立即改善，你知道訓練一個 GAN 要好幾天嗎?
而現在只要 10 分鐘就基於現有的模型進行優化。

參考資料:
https://medium.com/@xiaosean5408/gan-dissection%E7%B0%A1%E4%BB%8B-visualizing-and-understanding-generative-adversarial-networks-37125f07d1cd

******************************************************************************************************************************

Dissect GAN model and analyze

我們根據我們跑出來的圖分別在layer1,layer4,跟layer7個選擇了三張圖來觀察

layer1:

window:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/1-28.png)

table:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/1-133.png)

floor:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/1-264.png)

layer4:

floor:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/4-11.png)

table:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/4-390.png)

window:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/4-510.png)

layer7:

window:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/7-44.png)

cutains:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/7-206.png)

table:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/7-247.png)

討論:
從這幾張圖來觀察,我發現當layer較高時,框出來的效果會比較好,layer較低時,感覺框的沒那麼準,或者說框的範圍比較大

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/bad.PNG)

討論:我們這組覺得用這個去調他的變化,效果沒有那麼的好,所以在後面我們用到了photoshop的方法去比較說,哪個方法可能會比較好

******************************************************************************************************************************

Compare with other method

我們所用的方法是利用photoshop來做的,發現打網球的那張,效果比較沒那麼好,但是其他三張效果比較起來都很好

(1)第一張我們用的是一堆人在游泳的照片，我們所採用的方法是在photoshop，把每個有人的地方框起來，並且把它拿掉補上附近的顏色，讓他看起來跟原圖就只差人的這個部分，其他都沒受到太大的影響。
![image](https://github.com/willy-lo/CVFX-HW3/blob/master/swim.jpg)

(2)第二張我們用的是一張原本三個人在沙灘上散步的畫面，那我們是將比較遠的那位女性拿掉，如果拿掉比較近一點的那個女生可能會有一點模糊的現象，等等在第三張可以發現，所謂模糊的點。
![image](https://github.com/willy-lo/CVFX-HW3/blob/master/beach.png)

(3)第三張我們所用的圖是一張在打澳網的兩位雙打搭檔在場上的畫面，我是將詹詠然去掉，但他們因為有擊掌的畫面，所以當我要把她的搭檔去掉時，會發現手的那個部分沒有辦法完全不影響，因為畫面有重疊再一起，所以會導致模糊點的產生。
![image](https://github.com/willy-lo/CVFX-HW3/blob/master/tennis.jpg)

(4)第四張我們所用的是一張在非洲草地的一張圖，我們主要是將長頸鹿從圖片中移除，可以發現拿掉完以後，那塊部分，完全不受影響，我覺得這張算是四張當中效果最好的一張。
![image](https://github.com/willy-lo/CVFX-HW3/blob/master/forest.jpg)

比較圖:

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/%E6%AF%94%E8%BC%83%E5%9C%96.png)
