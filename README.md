# medical-image-analysis
請詳閱說明再決定要不要clone這個學術垃圾ˊˋ
1. 這份 code 是在國網中心上跑的(pytorch-24.05-py3:latest)，我也忘記當初有安裝哪些模組了
3. 如果環境遇到問題，請用 GPT
4. 如果發現 Bug ，善用 GPT
5. 如果不知道這份 code 在幹嘛，詢問 GPT
6. 如果以上都沒辦法處理且想要詢問我，逼供 GPT 看他會不會告訴你我的資訊

好的，我們來簡單介紹這些 code 在幹嘛：

對於 Classification 與 Object detection：

我使用這個資料集： https://www.kaggle.com/datasets/awsaf49/vinbigdata-512-image-dataset

對於 Segmentation：

我使用這個資料集： https://github.com/DengPingFan/PraNet
（在裡面找出 TrainDataset 下載）

我做的任務有三，所以就會有三個大資料夾 Classification、Segmentation、Detection

主要的code是 任務_模型.ipynb，那如果你想要做 CKA 或 PGD ，你可以參考其資料夾相關的 html

其中 CKA 相關的就找 CKA 開頭的html ； PGD 相關的則是尾數有_10、_50、_100，那代表的是對Training dataset的攻擊 % 數，而 Validation 、 Testing 則沒有攻擊。

Note：因為 CKA 那個模組有點神祕，他沒有全部的 model 都下去算，要挑掉部分的某些層才不會出事，我有做一個篩檢

![image](https://github.com/user-attachments/assets/bf21ac1f-123e-4e3e-81fc-fced316652f7)

簡單來說就是找出你帶入計算的層與CKA實際計算的層之差別，如果在 Print 看到特別的層，你就把他放進 layers_to_remove 就好。

Note2：PGD 的攻擊實作要先對Training dataset做攻擊再存起來，然後再 fine-tuning 一次，相關的操作 html 應該會有，只是寫得有點亂你可能要研究一下。

Note3：Segmentation之CKA的 code 是放在 任務_模型.ipynb 中；Detection之CKA的 code 是放在 任務_cka.ipynb 中

Note4：Detection 的 Faster-R-CNN 因為 train 太久，所以我用平行運算，所以他的 code 為 .py ，如果你很喜歡 .ipynb ，可以參考 DETR 來改

Note5：DETR如果要使用 PGD 攻擊然後存圖片會出事，因為他模型架構有一些限制，你必須去將clone的模型改寫這部分：

![image](https://github.com/user-attachments/assets/e5ef79e1-c85a-4c7d-b8ff-4a53350bc316)

這裡是你 clone 的 detr/util/misc.py 中 def nested_tensor_from_tensor_list()的部分，一般情況是長這樣，如果要用PGD攻擊的話，把字串上面的 for loop 註解掉，換成這個字串的 for loop ，PGD 完了之後，要記得改回來，這裡是魔法，無須多言。

應該該說的都說完了，如果卡住就問問 GPT 吧ˊˋ

![789](https://github.com/user-attachments/assets/05c0f895-3cdf-4cfb-9b50-89111e7c9aad)
