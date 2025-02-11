---
title: My first ever AI competition (DSC UIT 2024)
date: 2025-2-9 22:00:00 +0700  
last_modified_at: 2025-02-11 22:00:00 +0700
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


## Tìm models lớn hơn 
Sau vài ngày thử nghiệm với ViLT, mình nhìn lại bảng xếp hạng thì thấy bị bỏ khá xa (cao nhất lúc đó của mình là 0.39 trong khi các thí sinh khác đã gần tới 0.41). Nên mình quyết định sử dụng chiến thuật cũ để tìm finetune script từ các model khác để test. 

### Vấn đề về GPU
Trong quá trình mình tìm, có rất nhiều models lớn lên tới hơn mấy tỷ tham số như: InternVL, LLaVa, Mistral và Qwen-VL. 

Mình cũng khá hào hứng thử từng cái, do là model lớn, nên cộng đồng cũng lớn, kèm theo rất nhiều script, video và blog có thể học hỏi được. 

Tuy vậy, có vài vấn đề xuất hiện khiến mình không thể dùng được các model trên:
+ Cần GPU hopper, cụ thể hơn, các model này sử dụng flash attention 2.0(?) và với các nguồn GPU miễn phí thì không thể nào dùng được flash attention.
+ Không hỗ trợ tiếng Việt.
+ Cần quá nhiều VRAM, các models với 2B và 7B tham số, mình không tài nào nhét vừa được với các mức GPU free.

### Kaggle
Tới đây, mình cũng nhận thấy sự quan trọng của GPU, và bắt đầu sử dụng một nguồn GPU Khác ngoài Google Colab, đó là Kaggle.

Kaggle thật sự tuyệt vời nhưng không dễ để dùng tối ưu ngay lần đầu sử dụng.

Và mình có hẳn 1 [repo](https://github.com/BFCmath/FinetuneAI_Learning) share cách mình sử dụng Kaggle và Colab sao cho hiệu quả nhất. 

Chắc mình sẽ lên 1 bài về việc này sau (nếu rảnh), còn trong bài viết này xin phép chỉ đề cập là mình đã chuyển sang để tận dụng GPU của Kaggle.

### Vintern 1B 

Và với việc chuyển sang Kaggle, và sử dụng chiến thuật dò tìm model của mình. Rất may mắn là mình đã tìm ra được 1 model đủ to và có thể dùng trong Kaggle (2 GPUs). Model đó là của 1 team Việt Nam! 

> Trích từ abstract của [paper](https://arxiv.org/pdf/2408.12480): In this report, we introduce Vintern-1B (Vietnamese-InternVL2-1B), a reliable 1-billion-parameters multimodal large language model (MLLM) for Vietnamese
 language tasks
 
Model trên vừa đủ để giải quyết các vấn đề trước đó mình đề cập ở các model khác:
+ Chỉ sử dụng GPU T4 cũng có thể chạy được
+ Hỗ trợ tiếng Việt
+ Vừa đủ VRAM (Kaggle)

Và ngoài ra, authors còn rất tâm lý khi ghi hẳn một cái [finetune script](https://colab.research.google.com/drive/1bK6fpWfResjv9UxWoKHDStXQ8bop3a6Z?usp=sharing) cho người dùng, và solution chính của mình cũng chỉ xoay quanh script này thôi. 
> Một lần nữa cám ơn 5CD-AI team, hehe

> Qua vụ này thì một lần nữa nhấn mạnh tầm quan trọng của GPU, và việc phụ thuộc vào script từ tác giả đã hạn chế thế nào về solution của mình.

## Các thử nghiệm với Vintern
### Các vấn đề trước khi thật sự chạy được Vintern
Mặc dù nói lại thì nghe đơn giản, nhưng mà lúc đó mình vẫn rất đau khổ với GPU để có thể fine-tune được Vintern:
+ Quá trình sử dụng được Kaggle
+ Điều chỉnh hyperparemeters để có thể fit hoàn toàn model vào GPUs
+ Điều chỉnh script để có thể khớp với data của bản thân

> Bài học: pipeline script rất quan trọng và nên được ưu tiên trước trong đa số trường hợp.

### Instruction tuning
Một vấn đề rất lớn nữa, đó là model của Vintern, mình không biết cách chỉnh head của nó để biến nó từ generation task sang classification task
> Non =)), chả biết nói gì hơn hehe

Chính vì thế, nên mình có đi tìm hiểu thử cách để finetune generation task dạng này. Và hiển nhiên là mình đi kiếm tài liệu đọc về dụ này rồi. Một trong những tài liệu mình cực kì thích đó là: [The Ultimate Guide to Fine-Tuning LLMs from Basics to Breakthroughs: An Exhaustive Review of Technologies, Research, Best Practices, Applied Research Challenges and Opportunities](https://arxiv.org/pdf/2408.13296v1)

Mọi người có thể ghé qua đọc bảng [tóm tắt](https://github.com/BFCmath/FinetuneAI_Learning/tree/main/others/papers/UltimateGuideFromBasicsToBreakthrough) của mình nếu thấy ngán với cái tài liệu này.

Nhìn chung thì mình học được 2 thứ:
+ PEFT
+ Instruction tuning
> Ngoài ra còn nhiều thứ khác mà đến lần đọc thứ 2, thứ 3 mình mới học được.

Nhìn chung, mình sử dụng LoRA (PEFT) cho bài toán này (GPU issues).
Còn về instruction tuning, đó chính là kĩ thuật thêm vào 1 lời dẫn (như mình đã làm với ViLT) để khiến model hiểu rõ hơn về task và đưa ra output. 

Output đối với generation task kiểu này mình để hẳn luôn là "tên" class.

Ví dụ: 
+ Input: "`Đây là một bài viết trên Facebook có thể chứa hàm ý mỉa mai. Đây là tiêu đề: <caption>, đây là ảnh của bài post <image>. Trả về cho tôi 1 trong 4 câu trả lời sau: ...`"
+ Output: "`multi-sarcasm`"
> Tất nhiên generation model cho classification task thì sẽ bị nhiều vấn đề rồi, nhưng mà biết gì dùng nấy thôi :>

### Kĩ thuật train trên Kaggle

Vì vấn đề GPU, mình không thể train với >= 2 epoch được, nên mình đã viết script cho:
+ train epoch đầu tiên từ pretrained weights
+ lấy weights từ epoch trước đó để train cho các epoch sau

Việc train từng epoch như vậy cũng giúp mình kiểm soát overfitting.
> Trong quá trình này mình cũng khám phá ra được data stream của Kaggle rất mạnh

### Kết quả lần đầu submit Vintern
Mặc dù đau khổ như vậy, nhưng kết quả thì không phụ lòng mình. 

Mình quay lại top 2 với điểm khoảng 0.40 f1 score (nên nhớ là chưa hyper-tuning).
### More instruction and hyperparameter tuning
Với đà này thì mình đã tạo thêm nhiều instruction mới, bao gồm các kĩ thuật như COT...

Ngoài ra thì cũng chơi trò giảm dần learning rate cho các epoch sau

Kết quả cao nhất là khoảng 0.41 f1 score (top 3)
### Các vấn đề (mà sau này mình mới nhận ra)
+ Mình đã không kiểm soát được eval func, chỉ có loss func thôi (phụ thuộc vào script)
+ Generation model cho classification task rõ là không phù hợp
+ Model to hạn chế nhiều thứ, sử dụng model nhỏ sẽ tốt hơn ở nhiều vấn đề
+ Một vấn đề nữa là mình đã sử dụng các từ như multi-sarcasm, sau khi tokenize ra thì có thể đây là 1 [UNK] (cùng với các class khác). Rất may mắn là điều đó không xảy ra.

## Ensemble learning
Với kiến thức của mình từ Machine Learning, mình cũng biết tới các ensemble learning. 

Và cụ thể trong bài này, tận dụng các hyperparemeter và instruction khác nhau từ Vintern, mình đã **stack** các output lại để đạt được kết quả tốt hơn.

Có vài điều cần chú ý: 
+ F1 score sử dụng là loại macro, nghĩa là điểm số cao sẽ phản ánh khả năng dự đoán chính xác đối với các lớp có số lượng mẫu ít.
+ Imbalance data
+ Các mô hình Vintern của mình gặp khó khăn trong việc phát hiện các lớp có số lượng mẫu ít.

Chính vì thế nên chỉ đơn giản lấy trung bình sẽ khó mà hiệu quả, nên mình đã viết các luật (sau đây xin phép gọi là rules) để ưu tiên cho các lớp có số lượng mẫu ít này. 

Có 2 class cần quan tâm:
+ image-sarcasm
+ text-sarcasm

Các model của mình không hề detect được text-sarcasm, nên mình đã không thể làm gì hơn ngoại trừ bỏ class này.

Còn đối với image-sarcasm thì mình ưu tiên tuyệt đối, tức chỉ cần 1 model dự đoán 1 mẫu là image-sarcasm thì kết quả cuối cùng của mẫu đó sẽ là image-sarcasm.

Mình ban đầu chỉ stack 2 models vintern lại với nhau, 1 model có kết quả cao nhất trên public leaderboard (lb) và 1 model có khả năng detect được nhiều image-sarcasm.

Kết quả lên tới khoảng 0.415 

Sau đó mình stack thêm 1 model nữa có kết quả cao nhất trên tập validation thì kết quả cao nhất lên tới 0.423 trên lb. Và đây cũng là kết quả cao nhất của mình trên tập public test (top 4 trên public lb).

Ngoài ra mình còn test thêm meta-learner cho ensemble learning nhưng không hiệu quả. 
> Nếu các bạn có đọc write-up thì lúc đó mình còn không biết tên của kĩ thuật này là meta-learning :)) 

## Kết quả cuối cùng trên private test set
Mặc dù top 4 public, nhưng mà điểm private của mình lại khá ảm đạm, cao nhất tầm 0.405 thôi.

Giải thích cho việc này thì có các lí do chính mà bây giờ mình mới nhận ra như sau:
+ Thiếu kĩ năng sử dụng Cross-Validation set
+ Không nhận biết overfitting trên tập test
+ Model thiếu đa dạng (chỉ có 1 mình Vintern)

## Suy nghĩ cuối và lời cám ơn
Đây không hẳn là 1 cuộc thi lớn (so về số lượng team tham gia), nhưng nó lại rất phù hợp với kiến thức và mục tiêu của mình lúc đó. 

Mình gửi lời cám ơn tới BTC của DSC-UIT 2024 (nếu mấy anh/chị/thầy/cô có đọc được post này).

Về bản thân thì mình đã học được rất nhiều thứ mới, và qua bài đọc chắc các bạn cũng nhận ra điều đó.

Đây là một cuộc thi đặt nền tảng cho những hướng đi (mục tiêu) học tập trên con đường tự học của mình (kể cả khi đang soạn post này). 

Cám ơn các bạn đã đọc tới đây :)), hơi dài dòng và lê thê nhưng mà mình rất muốn chia sẻ câu chuyện này đến với mọi người. Nếu có gì sai sót và cần phải sửa đổi, mọi người hãy để lại 1 bình luận nhé!
