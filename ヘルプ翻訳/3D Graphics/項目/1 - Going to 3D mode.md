---
title: "1 - Going to 3D mode"
tags: ""
---

3Dモードを始める

3D モードを使用する場合は、まずゲーム メーカーを 3D モードに設定する必要があります。<br>
必要に応じて、後で 2D モードに戻すことができます。<br>
そのために、次の 2 つの関数が存在します。

・`d3d_start()`<br>
　3D モードの使用を開始します。<br>
　成功したかどうかを返します。

・`d3d_end()` <br>
　3D モードの使用を停止します。<br>
　成功したかどうかを返します。
<br>
<br>
3D モードに関連するすべての関数は d3d_ で始まることに注意してください。
<br>
<br>
<br>
3D モードを開始すると、次の変更が行われます。
<br>
まず、深度テスト (Z バッファリングとも呼ばれます) がオンになります (24 ビットの Z バッファを使用)。<br>
これは、画面上の各ピクセルに対して、最小の z 値 (= 深度値) を持つ描画のみが描画されることを意味します。<br>
<br>
インスタンスの深度が同じ場合、Z ファイティングと呼ばれるものが発生します。<br>
これは、2 つのインスタンスが同じレベルで描画しようとしたときに発生し、<br>
Z 値が非常に近いためにレンダリングが不正確になり、醜い結果を引き起こします。<br>
<br>
これを回避するには、重複する可能性のあるインスタンスの深さの値が同じでないことを確認してください。<br>
<br>
<br>
<br>
次に、通常の正射図法が透視図法に置き換えられます。<br>
<br>
これは次のことを意味します。<br>
通常、画面上のインスタンスのサイズはその深さに依存しません。<br>
透視投影では、深さが大きいインスタンスは小さく表示されます。<br>
<br>
深度が 0 の場合、元のサイズに等しくなります (投影を変更しない限り、以下を参照してください)。<br>
<br>
カメラの視点は、部屋の上の距離に配置されます。<br>
 (この距離は部屋の幅と同じです。これにより、適切なデフォルトの投影が得られます。)<br>
<br>
 カメラの前にあるインスタンスのみが描画されます。<br>
そのため、深度が 0 より小さいインスタンスを使用しないでください<br>
 (または、少なくとも -w より小さくないことを意味します。ここで、w は部屋またはビューの幅です)。<br>
<br>
<br>
<br>
3 番目に、垂直方向の y 座標が反転されます。<br>
通常、(0,0) の位置はビューの左上にありますが、<br>
3D モードでは (0,0) の位置は 3 次元ビューの場合と同様に左下の位置になります。<br>
<br>
<br>
<br>
実際には、以下の関数を使用して、隠面削除と透視投影のオンとオフを切り替えることができます。<br>
<br>
・`d3d_set_hidden(enable)` <br>
　深度テストを有効 (true) または無効 (false) にします。<br>
<br>
・`d3d_set_perspective(enable)` <br>
　透視投影の使用を有効にする (true) または無効にする (false)。<br>
