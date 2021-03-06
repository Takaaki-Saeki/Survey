# Cross-gender voice conversion with constant f0-ratio and average background conversion model
###### tags: `survey`


## 1. 概要
差分スペクトル法を用いて異性間の声質変換を行う研究．2018年のVC challengeにおいて好成績(76%の類似スコア，MOS3.22を記録)を残した手法についての解説を行なっている．

## 2. 先行研究と比較して何がすごいのか
差分スペクトル法は変換元話者音声と変換先話者音声のスペクトル包絡成分の差分を与えるようなフィルタを推定し，それを変換元話者の音声に対して適用する事によって変換を行うため，f0の変換方法に関しては別途考える必要があった．これに対し，WORLDボコーダを事前に適用し，変換元話者音声のf0を一定割合変化させる事によって，音質劣化を防ぎながら異性間の声質変換を実現したことがcontributionであると考えられる．

## 3. 手法のキモ
![](https://i.imgur.com/GD9dPgl.jpg)
まず入力として変換元話者の音声を受け取り，WORLDを用いてf0を一定割合変化させた音声に変換する．ここで，vocoder処理による音質劣化を最小限にするため，平均f0値に基づき，一定割合のf0変換にとどめていることがポイントである．
さらにf0変換された音声と変換先話者の音声からMCEPを抽出し，変換関数を求める．最後に，変換によるover-smoothingを防ぐため，話者依存のMCEPの系列内変動を計算する．
時間方向のアラインメントを取るため，MCEP特徴量に対し3iterationのDTWを行う．
また，背景モデルを事前学習しておく事によって過学習を防ぐこともポイントであるとしている．

## 4. 実験・検証方法
VCC2018のデータセットは変換元話者，変換先話者それぞれ4話者からなっており，女性話者2名，男性話者2名となっている．それぞれの音声データはそれぞれ81文からなっており，サンプリングレート22.5kHz，16bitで量子化されたwavフォーマットである．話者間のf0比は0.49から2.18の間の値をとる．考えうる16通りの話者の組み合わせに対し最適なモデルを構築することがミッションである．

評価は「変換先話者との類似度」，「音質」の2軸である．
類似スコアは評価者のうち「Same, absolutely sure」と「Same, not sure」と答えた割合，音質はMOSで評価した．
評価結果としては，類似スコアは77%，MOSは3.35で，類似スコアに関してはVCC2018の基準よりも5%改善され，MOSに関しては0.22低かった．ただし，VCC2018ではvocoder-freeとvocoder-basedの2部門に分かれており，それぞれ同性間・異性間変換を目的とするが，この基準ラインは2部門をまとめたものであり，信憑性は低いとしている．

MOSと類似度の結果は以下の通り．
本手法はN08に対応している．
![](https://i.imgur.com/yXwlpEO.jpg)
また，詳細な結果は以下の通り．
![](https://i.imgur.com/pBi8LWq.jpg)


## 5. 議論
今回の結果を受けて，差分スペクトル法と一定割合のf0変換に基づく声質変換は，場合によっては音質の劣化を低減できると言える．WSOLAによるf0変換や完全なvocoder-basedな手法と比較すると高品質であると言えそうである．

また，話者性に関してはかなり良い性能を示している．特に，異性間変換で全体の基準より良いスコアになったのは特筆すべきことである．


## 6. 次に読むべき論文
K. Kobayashi et al., Speech Communication, vol. 99, pp. 211--220, May 2018.
