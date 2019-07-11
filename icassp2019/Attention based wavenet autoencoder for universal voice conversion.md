# Attention based wavenet autoencoder for universal voice conversion
###### tags: `survey`

## 1. 概要
WaveNet autoencoderに基づく声質変換の研究．各話者の発話タイミングを反映した声質変換のために，入力と出力の発話タイミングを変えるようなattention要素を付加している．


## 2. 先行研究と比較して何がすごいのか
1. WaveNet autoencoderを用いて高品質な声質変換を行えることを示した．
2. Universal voice conversion(入力に依らず，所望の話者の音声に変換できる声質変換器)に向けた手法を提案した．
3. 話者の一時的な発話パターンにfitするようなattention構造をもつWaveNet autoencoderのアーキテクチャを提案した．
4. 新たな話者の発話訓練データを必要としない，WaveNetに基づくText-to-Speech手法を提案した．

## 3. 手法のキモ
![](https://i.imgur.com/hzMkTXM.png)
図のように，1つのencoderを通った入力が，k人の話者に対応する経路を通り，それぞれのWaveNet decoderでsoftmax reconstruction lossを最小化するように学習される．入力音声は，encoderによって潜在空間に移された後，confusion networkによってk人の話者のいずれかに識別される．この時，encoderは，符号化が話者特有のものとならないように，confusion matrixに対して敵対的に学習される．
WaveNet autoencoderのアーキテクチャは[A Universal Music Translation Network](https://arxiv.org/abs/1805.07848)に基づいている．
さらに，変換先話者に合わせて発話パターンを変化させるようなAttention構造を導入している．Inputを時間方向に縮めたり引き伸ばしたりしたものがEncodeされ，そこからInputそのものをEncodeしたものを復元するようにAttention networkが学習される．ここで， dot-productに基づくものより安定なGMMに基づくAttention構造を用いている．各々のAttention networkはLSTMからなる自己回帰モデルである．

## 4. 実験・検証方法
提案法をText-to-Speechに応用し，実験的評価を行なっている．
この手法は他話者から所望の話者に変換を行うので，既存のText-to-Speechでテキストを音声に変換し，その音声を本手法で変換することにより，Text-to-Speechを実現できるとしている．
なお，Speech-to-Speechの音声サンプルは[変換音声サンプル](https://voiceconversion.github.io)から聞くことができる．

用いたデータセットはLJ，Blizzard Challenge2011，Blizzard Challenge2013のもの．Tacotron2やVoice Loopなどと比較し，変換先話者そのものであるGround Truthとも比較した．この手法でattention構造を用いたものと用いなかったものについても比較を行った．

評価尺度としては，客観的な尺度と主観的な尺度の両方を用いた(以下の1~3)．
1. Mel Cepstral Distortion (MCD)．評価能力に限界はあるが，2つの音声データのスペクトル間の距離を測ることのできる客観的な尺度．変換音声と変換先話者の自然音声の時間方向長さは揃っていないため，MCD DTWにより調節した．
2. 変換音声と変換先話者の自然音声との時間的な距離を評価するため，MCD DTWの計算でどれだけ削除したか，どれだけ挿入を行なったかという評価をした．
3. Mean Opinion Scoreで評価した．

結果は以下の通り．
![](https://i.imgur.com/emVOns2.png)

## 5. 議論
結果的に，Tacotron2やVoice Loopと比較すると高いMOSスコアを示した．また，特にtable3のDTWによる評価で，attentionを導入することによってスコアが改善されることが確認できる．

## 6. 次に読むべき論文
- Noam Mor, Lior Wolf, Adam Polyak, and Yaniv Taigman, “A universal music translation network,” arXiv preprint arXiv:1805.07848, 2018.
- TACOTRON: TOWARDS END-TO-END SPEECH SYNTHESIS
- VoiceLoop: Voice Fitting and Synthesis via a Phonological Loop