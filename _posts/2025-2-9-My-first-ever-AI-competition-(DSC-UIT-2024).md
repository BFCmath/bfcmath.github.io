---
title: My first ever AI competition (DSC UIT 2024)
date: 2025-2-9 22:00:00 +0700  
# last_modified_at: 2025-02-7 14:00:00 +0700
categories: [Learning, Competition]  
tags: [vn, vlm, school-competition, fine-tuning, datascience, kaggle, colab]  
description: Some stories behind my first AI competition in my university.
author: BFC  
image:  
  path: assets/local/dsc_uit.png
# render_with_liquid: false  
# toc: false  
comments: true  
---  

Chào mọi người :vv, hôm nay mình sẽ chia sẻ lại về lần đầu tiên mình (thật sự) thi một cuộc thi về AI.  
> Bài viết này được thực hiện khá lâu kể từ ngày thi xong, nên quan điểm và góc nhìn có thể khác với lúc vừa hoàn thành cuộc thi.  

> Mọi người có thể đọc [write-up](https://github.com/BFCmath/DataScienceChallenge-UIT2024), còn trong bài viết này mình chủ yếu nói về toàn bộ quá trình thi.  

## Cuộc thi AI đầu tiên (not really)  

Nếu gọi đây là cuộc thi AI đầu tiên mình tham gia thì cũng không chính xác lắm. Mình từng tham gia AI Challenge với bọn bạn, nhưng mà nói chung kẹt quân sự nên cũng không thật sự "thi" cho lắm :v.  

Nên là mình coi Data Science Challenge mới là cuộc thi đầu tiên mình thật sự tham gia.  

## Tổng quan về cuộc thi  

Data Science Challenge là cuộc thi do UIT (Trường Đại học Công nghệ Thông tin) tổ chức.  

Có 2 bảng: bảng A cho hackathon, bảng B cho code challenge. Mình tham gia bảng B (vì mình không thật sự thích hackathon :v haha).  

Chủ đề của năm nay là phát hiện châm biếm trong các bài post trên mạng xã hội.  

Vì đây là DS competition (DS: Data Science) nên hiển nhiên là người tham gia sẽ được cung cấp data (đã được gán nhãn) và sẽ có benchmark để xác định người thắng.  

+ **Data**: Texts và images  
+ **Label**: Multi-class  
+ **Eval function**: F1 score  
+ **Chỉ được sử dụng pretrained model**  
+ **Không sử dụng thêm data bên ngoài**  

## Trước khi thi  
+ Trước đó thì mình tự học (lý thuyết) là chủ yếu, các kiến thức lúc đó mới thì hầu hết nằm ở Machine Learning. Còn về Deep Learning thì hầu hết chỉ biết khái niệm về transformer, CNN, RNN,... thôi =)), nói chung là chả biết gì hết hehe (thậm chí chỉ là biết khái niệm và ý tưởng, còn chả biết code nó ra làm sao :v).  
+ Thi với tâm thế học hỏi và thử sức.  

## Quá trình thi  

### Giai đoạn tìm hiểu về chủ đề  
Nhìn vào data là thấy có 2 cái chính:  
+ Data hỗn hợp  
+ Sarcasm detection  

Nên mình cũng đi tìm hiểu thử xem data hỗn hợp được giải quyết như thế nào và các paper về sarcasm detection tiếp cận bài toán ra sao.  

Kết quả:  
+ Về sarcasm detection, hồi thi mình chả tìm ra được nhiều tài liệu lắm (có thể do thiếu kỹ năng research), kiếm được bài nào thì cũng chả liên quan tới bài toán của mình :v.  
+ Còn về data hỗn hợp cũng gần như bế tắc nốt (do thiếu kỹ năng research).  

**Cách mình vượt qua giai đoạn này?** Cái này khá hài hước :v. Do cuộc thi giới hạn số lượng pretrained models và yêu cầu thí sinh phải nộp trước danh sách models họ sẽ dùng, mà cái file đó thì ai cũng có thể access =)). Nên mình nhảy vào đó xem thử models của các bạn/anh khác rồi kiếm paper đọc thôi :vvv.  

Nhờ vụ này mình mới biết được cách Vision Language Model hoạt động, cũng như biết tới Hugging Face.  

Nói chung, dù thiếu kiến thức trầm trọng, nhưng rất may là mình vẫn lụm được các model và paper ngon để đọc, hiểu thêm về bài toán haha :v, ví dụ như InternVL, LLaVA...  

> **Bài học**: Kỹ năng research quan trọng lắm =))  

### Giai đoạn tìm hiểu về data  
Lúc này mình hơi non, nên chả biết nhiều kỹ thuật EDA cho text cũng như image :v, nên dường như mình chỉ quan tâm tới labels trong cuộc thi này.  

Đây là một bài toán có **imbalanced dataset**:  
+ **Train Set**: 10,805 instances (6,062 not-sarcasm, 4,224 multi-sarcasm, 77 text-sarcasm, 442 image-sarcasm)  
+ **Test Set**: 1,413 instances cho public test, 1,504 instances cho private test  

Kiểm tra thêm một ít về text cũng như image, mình nhận ra đây chủ yếu là post từ các meme group trên Facebook.  

Một điều thú vị là các post từ meme group được chia đều cho cả train và public test, nên nếu không cẩn thận thì dễ bị overfitting (giờ nhìn lại mới thấy =)), hồi thi chả biết gì nên bị overfit trên tập test).  

Ngoài ra thì một post có thể có nhiều images, nên việc trùng caption (text) có xảy ra.  

## Quá trình submit lần đầu  

### Github, Hugging Face và Colab  
Như mình đã nói, mình dựa vào model từ file pretrained model mà các thí sinh gửi.  

Tuy nhiên, vấn đề xảy ra là **làm sao để dùng được các pretrained model đó?** =))  

Hồi đó mình cứ nghĩ, có được Github của model, clone về chạy là xong. Nhưng đời không như là mơ :v. Nói chung sẽ có 2 vấn đề chính:  
+ Model không support training, chỉ support replicate lại kết quả paper  
+ Python library version collision  

Nói chung là bế tắc :v, chả dùng được model nào hết. May là lúc này mình biết tới Hugging Face (cũng từ cái file pretrained model nốt).  

Ngắn gọn thì Hugging Face giúp mình chỉ qua vài dòng code là chạy được model luôn, tránh được 2 vấn đề trên.  

Và cũng nhờ Hugging Face mà mình thật sự biết dùng Colab. Hugging Face thường có finetune hoặc inference script hướng dẫn chạy trên Colab, nên chỉ cần vài cú click là chạy được model :vv  

### Tìm model phù hợp  
Dù vậy, lúc đó mình không đủ thời gian học cách dùng Hugging Face một cách bài bản, nên chỉ dùng và điều chỉnh script có sẵn của các model thôi :(.  

Chính điều đó đã hạn chế rất lớn số model mà mình có thể sử dụng.  

Sau một hồi dò xét, rất may mắn là vẫn có một vài model có script finetune chạy được. Và model đầu tiên mình chọn là **[ViLT](https://arxiv.org/pdf/2102.03334)**.  

### Fine-tuning ViLT  
Sau khi đọc paper của ViLT, kiểm tra script và chạy thử, mình nhận ra ViLT chỉ output ra được một chữ. Đào sâu hơn thì mới hiểu rằng output cuối cùng của ViLT dành cho classification task.  

Rất may là mình từng đọc qua paper của BERT và có chút kiến thức về classification model (từ CNN), nên cũng biết cách đổi head của nó thành số class cho bài toán của mình.  

Lúc đó mình freeze các layer ở dưới và chỉ train head layer.  

**Kết quả khá sốc** :v, lên hạng nhất luôn =)). (Mình nhớ tầm **0.38 F1 score**).  

> **Giờ nhìn lại thì có thể giải thích vì sao:**  
> + Giai đoạn đầu của cuộc thi (sau này top 1 public score lên tới **44%** =)))  
> + **Overfitting** (như đã nói ở trên, test data có nhiều điểm tương đồng với train data)  

## Một vài thử nghiệm với ViLT  

Sau đó thì mình có thử nghiệm một số thứ.  

### Điều chỉnh input  
Hồi đó thì mình không hiểu lắm về các kỹ thuật training, nên thử nhiều thứ mà mình cho rằng là hợp lý. Chẳng hạn như thêm: "`Hãy phân loại bài post sau thành 4 class... <caption> <image>`"  

> Sau này mình mới biết cái phần thêm vào đó gọi là **instruction**.  

Ngoài ra, mình cũng thử:  
- Điều chỉnh kích thước của image.  
- Xóa icon.  
- Đổi các từ viết tắt.  
- Xóa hashtag.  
- Đổi các từ bị che thành không che (ví dụ: c.h.ó → chó).  

> Lúc đó mình không hề biết tới các kỹ thuật đơn giản để xử lý text luôn haha =))  

### Multilabel Classification  

Qua một vài quan sát đơn giản, mình nhận ra bài toán này có thể gom các class lại với nhau và biến nó thành một **multilabel classification task**.  

Cái này hoàn toàn mới với mình :v, nên mình nhảy đi đọc paper luôn. Một paper mà mình học được rất nhiều là: **[A Survey on Multilabel Learning in Deep Learning](https://arxiv.org/pdf/2401.16549)**.  

Qua paper này, mình biết thêm về các hàm loss được dùng cho bài toán multilabel classification.  

Mình có áp dụng thử, nhưng mà fail :vv, không có cái nào đạt được hơn **0.39 F1 score** cả.  

**Kết quả:** Vài ngày sau rớt top :v.  

### Hyperparameter tuning và Freezing layer  
Mình chủ yếu test **learning rate** (lúc đó chưa biết cách dùng scheduler).  

Ngoài ra, mình còn thử các chiến thuật **freeze layer** khác nhau:  
+ **Unfreeze hoàn toàn.**  
+ **Unfreeze lớp input embedder.**  
+ **Unfreeze lớp attention.**  
+ **Unfreeze một số lớp nhỏ trong lớp attention.**  

Cùng với đó, mình test các loss function như **Weighted Cross-Entropy (CE)** và thử nghiệm nhiều trọng số khác nhau để giải quyết vấn đề imbalance.  

## Traditional Machine Learning Approach  

Trước giờ mình chưa thi cuộc thi Deep Learning nào, hầu hết là toàn vọc code Machine Learning thôi. Nên mình cũng cố gắng đưa bài toán này về **traditional ML**.  

Mình không nhớ rõ lắm, nhưng mình có tạo ra một số features như:  
- Số lượng icon.  
- Số lượng chữ.  
- Số lượng hashtag.  
- ...  

Và chắc các bạn cũng đoán được kết quả rồi =))))))  

> Sau này đọc nhiều paper hơn, mình mới hiểu vì sao lại fail, chứ lúc đó cũng khá tự tin :v.  

Ngoài ra, mình cũng có vài suy đoán đại loại kiểu **ViLT không hiểu được icon** (hồi đó còn chả biết dùng tokenizer để kiểm tra cái suy đoán này). Nếu biết cách, mình hoàn toàn có thể **custom lại model để tận dụng các features này**.  
Tất nhiên là mình đã không thể làm vậy vì lúc đó chưa biết tới kỹ thuật này 😭.  

## Còn tiếp...  

Bài này mình xin phép dừng tại đây thôi :v. Đây mới là **1/2 quá trình**. Một nửa còn lại mình xin phép để bài sau. 😆  
