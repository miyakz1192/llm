# LLMのファインチューニングについて調べてみる

## 段取り的な

LLMのファインチューニングを調べたい。今まで持っている知識だと。。。。

1. データセットを用意する
2. トレーニングのコードを書く

2のトレーニングについては、LoRAという方法が良いようだ。モデル本体の重みに手を加えるのではなく、追加学習分の重みを学習して、それをモデル本体の重みに足しこむことで、追加学習を実現する学習速度と、学習データ保存効率性に優れた方法となる。

## データセットの用意

### databricks-dolly-15k-ja

URLはここ。

https://github.com/kunishou/databricks-dolly-15k-ja?tab=readme-ov-file

そして、このURLの中の以下を見ると、

https://huggingface.co/datasets/kunishou/databricks-dolly-15k-ja

概ね以下のようなノリでデータを用意すれば良いことがわかる。基本的には人間が用意するのがベースとなるらしい。

category:closed_qa

output:
ヴァージン・オーストラリア航空は、2000年8月31日にヴァージン・ブルー航空として、2機の航空機で単一路線の運航を開始しました

input:
ヴァージン・オーストラリア航空（Virgin Australia Airlines Pty Ltd）はオーストラリアを拠点とするヴァージン・ブランドを冠する最大の船団規模を持つ航空会社です。2000年8月31日に、ヴァージン・ブルー空港として、2機の航空機、1つの空路を運行してサービスを開始しました。2001年9月のアンセット・オーストラリア空港の崩壊後、オーストラリアの国内市場で急速に地位を確立しました。その後はブリスベン、メルボルン、シドニーをハブとして、オーストラリア国内の32都市に直接乗り入れるまでに成長しました。	

instruction:
ヴァージン・オーストラリア航空はいつから運航を開始したのですか？	

index:
0


このように、
https://github.com/kunishou/databricks-dolly-15k-ja/blob/main/databricks-dolly-15k-ja.json

のようなjsonを作るのが基本形らしい。それでは、これをどうやって、LLMに学習させるのか？？

## 学習方法

### OpenAIスタイル

このURLによれば、簡単にfine tuning出来るAPIがすでに用意されている。

https://qiita.com/ksonoda/items/b9fd3e709aeae79629ff 

データを送りつければ勝手にOpenAI側で学習タスクが進む様子。

### OSSスタイル

以下のYoutube動画がカナリ参考になる。

https://www.youtube.com/watch?v=P6eweyxbTb0

次にこのURL。ここに掲載されているスクリプトがそのまま利用できるそうな。

https://github.com/tloen/alpaca-lora

このfinetune.pyを見ると、LLamaベースに最適化されているような気がする。LLama専用みたい。
だとすると、ElyzaやSwallowでも使える気がする。

このYoutube動画でも、上のスクリプト(finetune.py)を使い、databricks-dolly-15k-ja.jsonを使ったそうな。

## LoRAについて

以下のURLがわかりやすい。

https://xtech.nikkei.com/atcl/nxt/keyword/18/00002/120800245/

理論的な所はここが入り口か。

https://blueqat.com/yuichiro_minato2/d2a03601-2f3e-4535-980d-33c79add516c

少し詳し目。

https://speakerdeck.com/hpprc/lun-jiang-zi-liao-lora-low-rank-adaptation-of-large-language-models?slide=53
