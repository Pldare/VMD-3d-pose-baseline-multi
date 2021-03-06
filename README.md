# VMD-3d-pose-baseline-multi

このプログラムは、[VMD-Lifting](https://github.com/errno-mmd/VMD-Lifting) \(errno-mmd様\) を miu(miu200521358) がfork して、改造しました。

動作詳細等は上記URL、または [README-errno-mmd.md](README-errno-mmd.md) および [README-original.md](README-original.md) をご確認ください。

## 機能概要

- [miu200521358/VMD-3d-pose-baseline-multi](https://github.com/miu200521358/VMD-3d-pose-baseline-multi) で生成された3D関節データから、vmd(MMDモーションデータ)ファイルを生成します

## 準備

詳細は、[Qiita](https://qiita.com/miu200521358/items/d826e9d70853728abc51)を参照して下さい。

### 依存関係

python3系 で以下をインストールして下さい

- [Tensorflow](https://www.tensorflow.org/)
- [OpenCV](http://opencv.org/)
- python-tk (Tkinter)
- PyQt5

## 実行方法

1. [miu200521358/3d-pose-baseline-vmd](https://github.com/miu200521358/3d-pose-baseline-vmd) で生成された3D関節データ(pos.txt)、2D関節データ(smoothed.txt)を用意する
1. [miu200521358/3dpose_gan_vmd](https://github.com/miu200521358/3dpose_gan_vmd) で生成された3D関節データ(pos_gan.txt)、2D関節データ(smoothed_gan.txt)を用意する
1. [miu200521358/FCRN-DepthPrediction-vmd](https://github.com/miu200521358/FCRN-DepthPrediction-vmd) で生成された深度推定データ(depth.txt)を用意する
1. [3DToVmd.bat](3DToVmd.bat) を実行する
1. `3D解析結果ディレクトリパス` が聞かれるので、1.の結果ディレクトリパスを指定する
1. `ボーン構造CSVファイル` が聞かれるので、トレース先のモデルのボーン構造CSVファイルパスを指定する
    - センター移動の計算に身長を使用する
    - ボーン構造CSVファイルの出力方法は、[born/README.md](born/README.md)参照
    - 未指定の場合、デフォルトで[born/あにまさ式ミクボーン.csv](born/あにまさ式ミクボーン.csv)が読み込まれる
1. `足をIKで出力するか` 聞かれるので、出す場合、`yes` を入力する
    - 未指定 もしくは `yes` の場合、IKで出力する
    - `no` の場合、FKで出力する
1. `踵位置補正` が聞かれるので、踵のY軸補正値を数値(小数可)を指定する
    - マイナス値を入力すると地面に近付き、プラス値を入力すると地面から遠ざかる。
    - ある程度は自動で補正するが、ヒールのある靴や厚底などで補正しきれない場合に設定
    - 未指定の場合、「0」で補正を行わない
1. `センターXY移動倍率` が聞かれるので、センターのXY移動時の移動幅を入力する
    - 値が小さいほど、XYの移動量が少なくなり、大きいほど、移動量が増える
    - 未指定の場合、デフォルトで「30」とする
1. `センターZ移動倍率` が聞かれるので、センターのZ移動時の移動幅を入力する
    - 値が小さいほど、Zの移動量が少なくなり、大きいほど、移動量が増える
    - 未指定の場合、デフォルトで「5」とする
    - 「0」を指定した場合、センターZ移動を行わない
1. `グローバルX軸補正角度` が聞かれるので、適当な角度(-180～180度の整数のみ)を指定する
    - 未指定の場合、デフォルトで17度指定
    - モーションにより、微妙に異なる場合があるので、適宜調整
1. `円滑化度数` が聞かれるので、適当な正の整数を指定する
    - 未指定の場合、デフォルトで1回指定
1. `センター移動間引き量` が聞かれるので、センター間引き時の移動量(小数可)を指定する
    - 指定された移動量範囲内のセンターキーフレームを間引く
    - 未指定の場合、デフォルトで「0.5」とする
    - 「0」が指定された場合、間引きを行わず、以下`IK移動間引き量`、`間引き角度`、`間引きキー揃え`はスキップする
1. `IK移動間引き量` が聞かれるので、IK間引き時の移動量(小数可)を指定する
    - 指定された移動量範囲内のIKキーフレームを間引く
    - 未指定の場合、デフォルトで「1.5」とする
1. `間引き角度` が聞かれるので、回転系ボーン間引き時の回転角度(-180～180度の整数のみ)を指定する
    - 回転が指定された角度範囲内であれば、キーフレームを間引く
    - 未指定の場合、デフォルトで「20」とする
1. `間引きキー揃え`が聞かれるので、yes か no を入力する
    - 未指定 もしくは `yes` の場合、キーの間引きをパーツごと（例: 右肩・右腕・右ひじ等）に揃える
    - `no` の場合、キーの間引きを揃えない（注：あまり精度はよくないです）
1. `詳細なログを出すか` 聞かれるので、出す場合、`yes` を入力する
    - 未指定 もしくは `no` の場合、通常ログ
1. 処理開始
1. 処理が終了すると、1. の結果ディレクトリ以下に vmdファイルが出力される
1. MMDを起動し、モデルを読み込んだ後、モーションを読み込む

## ライセンス
GNU GPLv3

以下の行為は自由に行って下さい

- モーションの調整・改変
- ニコニコ動画やTwitter等へのモーション使用動画投稿
- モーションの不特定多数への配布
    - **必ず踊り手様や各権利者様に失礼のない形に調整してください**

以下の行為は必ず行って下さい。ご協力よろしくお願いいたします。

- クレジットへの記載を、テキストで検索できる形で記載お願いします。

```
ツール名：モーショントレース自動化キット
権利者名：miu200521358
```

- モーションを配布する場合、以下ライセンスを同梱してください。 (記載場所不問)

```
LICENCE

モーショントレース自動化キット
【Openpose】：CMU　…　https://github.com/CMU-Perceptual-Computing-Lab/openpose
【Openpose起動バッチ】：miu200521358　…　https://github.com/miu200521358/openpose-simple
【Openpose→3D変換】：una-dinosauria, ArashHosseini, miu200521358　…　https://github.com/miu200521358/3d-pose-baseline-vmd
【Openpose→3D変換その2】：Dwango Media Village, miu200521358：MIT　…　https://github.com/miu200521358/3dpose_gan_vmd
【深度推定】：Iro Laina, miu200521358　…　https://github.com/miu200521358/FCRN-DepthPrediction-vmd
【3D→VMD変換】： errno-mmd, miu200521358 　…　https://github.com/miu200521358/VMD-3d-pose-baseline-multi
```

- ニコニコ動画の場合、コンテンツツリーへ [トレース自動化マイリスト](https://www.nicovideo.jp/mylist/61943776) の最新版動画を登録してください。
    - コンテンツツリーに登録していただける場合、テキストでのクレジット有無は問いません。

以下の行為はご遠慮願います

- 自作発言
- 権利者様のご迷惑になるような行為
- 営利目的の利用
- 他者の誹謗中傷目的の利用（二次元・三次元不問）
- 過度な暴力・猥褻・恋愛・猟奇的・政治的・宗教的表現を含む（R-15相当）作品への利用
- その他、公序良俗に反する作品への利用

## 免責事項

- 自己責任でご利用ください
- ツール使用によって生じたいかなる問題に関して、作者は一切の責任を負いかねます
