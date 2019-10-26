# CYCLEGAN-VC2: IMPROVED CYCLEGAN-BASED NON-PARALLEL VOICE CONVERSION

###### tags: `survey`

## 1. 概要
ノンパラレル声質変換手法であるCycleGAN-VC2についての論文．
既存手法であるCycleGAN-VCを改良したモデルを提案．


## 2. 先行研究と比較して何がすごいのか
主観評価の結果，同性間・異性間を含む全ての話者対で，自然性・話者性の両面でCycleGAN-VC2がCycleGAN-VCを上回った．


## 3. 手法のキモ
従来法のCycleGAN-VCと異なる点は主に以下の通り．
1. cycle-consistency loss によるoversmoothing効果を低減するため，two-step adversarial lossを導入した．
2. Generatorのアーキテクチャを変更し，2-1-2D CNNとした．
3. Discriminatorとして，FullGANではなくPatchGANを用いた．


## 4. 実験・検証方法

従来法であるCycleGAN-VCと，提案法であるCycleGAN-VC2を，以下の方法で実験的に比較した．
1. メルケプ歪み(MCD)と変長スペクトル距離(MSD)を用いた客観評価
2. 自然性・話者性に関する主観評価


## 5. 議論
全ての話者対について，MCD，MSDの両面でstate-of-the-artな結果を出した．
また，主観評価の結果，同性間・異性間を含む全ての話者対で，自然性・話者性の両面でCycleGAN-VC2がCycleGAN-VCを上回った．
さらに，vocoderはworldなので，Neural Vocoderを用いることによる自然性の向上が期待できる．

## 6. 次に読むべき論文
- ATTS2S-VC: SEQUENCE-TO-SEQUENCE VOICE CONVERSION WITH ATTENTION AND CONTEXT PRESERVATION MECHANISMS
- Parallel-Data-Free Voice Conversion Using Cycle-Consistent Adversarial Networks