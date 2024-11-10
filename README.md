# kaggler-pub-GCI-titanic

このリポジトリは11/9に作成されました。

# コンペ概要

松尾研GCI講義でのコンペである。内容はそこまで難しくない。初期段階で公開ノートブックがかなり強い。（0.ipynb）

期限は11/18の午後５時まで

本家titanicコンペと比較してcolumnsは同じでデータ量も同じだが、内容が違う。これは、本家のtrain_dataとtest_dataをごちゃまぜにしたのか？
ー＞だとしたら、本家コンペのprivate_scoreが高いものを引っ張ってこれば解決しそう。

もしくは、train_dataとtest_dataに重複がないかを検討

# 試したいことリスト

~~これはもう試しました~~

~~年齢をビン化処理をする~~ してみた。rfcはスコアが下がるがxgbが大幅にスコア向上した。

他の学習機でのスコア向上

optunaを用いたイパーパラメーターチューニング（max_depth棟が大きくなってしまって過学習が起きてしまうのをどうするか要件等）

~~sex　*　Fareの列を作成するのがもしかしたら有効かもしれない~~ -> rfc以外でのスコア低減につながった。

0_8.ipynbについてrfc以外の学習機に対してもハイパーパラメータ最適化を施したい。

苗字によって生き残ったかどうかというのはかなり制の相関があるとすれば、それをdictで保管しておいて、同一名があったときに関しては同じ名前にしてしまうのも手かと思う。これはEDAをして相関があるかどうかを確認する必要あり。

~~MrsがMrに含有されていることを把握した~~ 11/9以前に作成されたものに関しては要改善

Ticket列に関して何かしらのデータを抽出できないか探ってみる。

~~SCQ列の削除~~　9.ipynbのスコアで確認する

閾値の最適化をする

GDBCの実装



# 変更内容

## 11/9土曜日

### 0.ipynbについて、

運営から配布されたデータ。

LBで0.765程度のスコアが出る。

### 0_8.ipynbについて、

現在LBで過去最高のスコア0.8が出る。

pro.ipynbに比べてlrでの予測値をアンサンブルしただけ。

### 1.ipynbについて、

前処理は名前に対しての敬称列を作成。

Familyの数を表す列を作成。

Ageをビン化

models = ['rfc', 'svc', "xgb", "gbc"] 

これらのモデルをoptunaによってハイパーパラメーター最適化

LBでは0.778程度しか出ない。

private_scoreでは0.85程度を記録

### 2.ipynbについて

前処理で各列の積についても考慮に入れてみた。

importanceを取得してみると、sex * Fare の要素が非常に大きな重要度を持っていることが判明した。

pycaretでtop3モデルgbc、ridge、xgboostのアンサンブルを作った。

なぜか、まったくスコアが出ない。private_scoreは0.83程度。

LBでは0.76程度しか出ない

pycaretを使うという策は断念。

### 3.ipynbについて

0_8.ipynbの派生形ノートブックになっている。

これは前処理の段階でAgeをビン化処理を施した。

privete_scoreでは0.82を記録しているが、LBでは0.797となっている。

あまりよくない処理なのかもしれない

### 4.ipynbについて

3.ipynbの派生形

xgbでの予想もしてみたが、パラメータチューニングなしで0.8に届かなかった。

没。鬱。

ただ、lrも0.806しかないので、そんなに精度はかわらないんじゃないかなー

### 5.ipynbについて

0_8.ipynbの派生形

敬称を抽出している。

学習機に関してはグリッドサーチをしたrfcのみ。private_scoreは0.825

lrが案外いいスコア0.821を記録している。

LBでは0.791を記録。アンサンブルしたらいいスコアが出そう。

### 6.ipynbについて

5.ipynbの派生形

学習機にxgbを追加。さらにrfc、lr、gbc、のアンサンブルで作成した。

private_scoreはどれも0.8を超えている。

LB_scoreは0.794だった。

gbc混ぜたのが良くなかったのかも

Family列が生成できていなかったのでFamily列を作って再検証してみたい

## 11/10 日曜日

## 7.ipynbについて

6.ipynbの派生形

LBは0.806。過去最高

主変更点は、名前列について、重要度が高いMrのみについてtrue or Falseで判定する列を作った。

アンサンブルに関してlrを排除し、rfcとgbcのみのアンサンブル（重みなし）

今後は試したいことリストに残っているものを片っ端から試していく

### 8.ipynbについて

7.ipynbの継承。Sex*Fareの列を新たに作ってみた。重要度は1位になったが多重共線性の問題でスコアはあまり上昇せず。

LB＿scoreは不明。

private_scoreに関してrfcは0.001だけ上昇したが他のモデルが壊滅的状況。おそらくLBでもあまりいいスコアは出ないだろう。

断念。

### 9.ipynbについて

7.ipynbの継承

C, S, Q のノイズ除去をした。重要度を示すグラフに数値も表示されるように改善した。結果private_scoreではrfcとmlpcのスコアが上昇した。この結果を受け、rfc、mlpc、gbcのアンサンブルをしている。

LB_scoreに関しては~~11/10の午後三次に採点される見通し~~　0.794

なかなか、スコアが上がらなかった中、先0.806を記録してうれしくなっているが、0.85を記録するのは恐ろしく遠いことを確認して絶望している。どうすればいいんだろう。

また、締め切りまで１週間程度＋提出回数が少ないことから、そろそろハイパーパラメータのチューニングをしていきたいと思っている。

### 10.ipynbについて

9.ipynbの継承。Ageをビン化してみた。rfc、mlpcのスコアが減少した。xgbのスコアが上昇した。

そこで、rfc(0.833), gbc(0.821),xgb(0.832)のアンサンブルを作成した。LBでのscore確認は11/10の午後九時になる見通し。

正直な感想、スコアは上昇する気がしている（願望）とりあえず、これからのAHCに備えたいし、SMBCコンペのほうもsubmitできるノートブック作成に取り掛かりたい気もしている。そのためにはハイパーパラメータチューニングをしなきゃなーという気持ちになっている。


