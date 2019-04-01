# CVFX-HW3

1.

簡介

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

Dissection 解開

先講結論：如果該 Unit 與某個類別是相關的，那麼其 Feature map(想像成熱力圖)會與語意分割後該類別的 Mask 相似。

透過語意分割模型將特定的類別切割出來，下圖將樹(Tree)的部分切割出來。

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/%E6%93%B7%E5%8F%963.JPG)

接下來我們再將某個 Unit 的 Feature map 與其分割出的 Mask 做比對，看看相不相似，下圖為架構的一部分。

![image](https://github.com/willy-lo/CVFX-HW3/blob/master/%E6%93%B7%E5%8F%964.JPG)

透過這方式一一比對，當 Feature map 與某個類別的 Mask 長得夠像的時候，我們可以認定這個 Unit 與某類別之間是有較大關聯性的。

2.Generate images with GANPaint:

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

2.Dissect GAN model and analyze

3.Compare with other method
