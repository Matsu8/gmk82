---
title: "3 - Drawing polygons in 3D"
tags: ""
---

3Dポリゴン描画<br>
<br>
古い方法での描画の問題は、スプライトまたはポリゴンが常に xy 平面にあることです。
つまり、すべての角が同じ深さになります。<br>
<br>
真の 3D の場合、頂点を異なる深さにできるようにする必要があります。<br>
<br>
ここからは、深度ではなく z 座標について説明します。<br>
したがって、座標を (x,y,z) タプルとして指定します。<br>
<br>
このために、高度な描画機能の特別なバージョンがあります。<br>
<br>
・`d3d_primitive_begin(kind)` <br>
　指定された種類の 3D プリミティブを開始します:<br>
　pr_pointlist、pr_linelist、pr_linestrip、pr_trianglelist、pr_trianglestrip または pr_trianglefan。<br>
<br>
<br>
たとえば、頂点が z = 200 で z=0 平面上に立つ四面体 (三面ピラミッド) を描画するには、次のコードを使用できます。
```
 d3d_primitive_begin(pr_trianglelist);
    d3d_vertex(100,100,0);
    d3d_vertex(100,200,0);    
    d3d_vertex(150,150,200);
    d3d_vertex(100,200,0);
    d3d_vertex(200,200,0);    
    d3d_vertex(150,150,200);
    d3d_vertex(200,200,0);
    d3d_vertex(100,100,0);    
    d3d_vertex(150,150,200);
    d3d_vertex(100,100,0);
    d3d_vertex(100,200,0);    
    d3d_vertex(200,200,0);
  d3d_primitive_end();
```
これを使用すると、四面体の頂点が視点から見て背後にあるため、画面に三角形が表示される可能性が高くなります。<br>
また、1色だけでは顔の違いがわかりにくい。<br>
<br>
以下に、視点を変更する方法を示します。<br>
色の割り当ては、頂点間に `draw_set_color(col)` 関数呼び出しを追加することで、以前と同じように行うことができます。<br>
<br>
<br>
テクスチャ ポリゴンを 3D で使用することもできます。<br>
ドキュメントの高度な描画機能で説明されているのとまったく同じように機能します。<br>
<br>
しかし今回は、基本機能の 3D バリアントが必要です。<br>
あなたが認識しなければならない1つのこと。<br>
<br>
テクスチャでは、位置 (0,0) は左上隅です。<br>
しかし、多くの場合、プロジェクションを使用する場合 (以下に示すように)、左下隅は (0,0) です。<br>
<br>
そのような場合、テクスチャを垂直方向に反転する必要があるかもしれません。<br>
<br>
・`d3d_primitive_begin_texture(kind,texid)` <br>
　指定された種類の 3D プリミティブを、指定されたテクスチャで開始します。<br>
<br>
・`d3d_vertex_texture(x,y,z,xtex,ytex) `<br>
　テクスチャ内の位置 (xtex,ytex) でプリミティブに頂点 (x,y,z) を追加し、<br>
　前に設定した色とアルファ値とブレンドします。<br>
<br>
・`d3d_vertex_texture_color(x,y,z,xtex,ytex,col,alpha)` <br>
　テクスチャ内の位置 (xtex,ytex) で頂点 (x,y,z) をプリミティブに追加し、<br>
　独自の色とアルファ値とブレンドします。<br>
<br>
・`d3d_primitive_end()` <br>
　プリミティブの記述を終了します。<br>
　この関数は実際に描画します。<br>
<br>
<br>
したがって、たとえば、次のコードを使用して、遠くに消える背景画像を描画できます
```
  var ttt;
  ttt = background_get_texture(back);
  d3d_primitive_begin_texture(pr_trianglefan,ttt);
    d3d_vertex_texture(0,608,0,0,0);
    d3d_vertex_texture(800,608,0,1,0);    
    d3d_vertex_texture(800,608,1000,1,1);
    d3d_vertex_texture(0,608,1000,0,1);
  d3d_primitive_end();
```
三角形には表と裏があります。<br>
<br>
前側は、頂点が反時計回りに定義されている側として定義されます。<br>
通常、両側が描かれます。<br>
<br>
でも閉じた形にすると三角形の裏側が見えなくなるのでもったいないです。<br>
<br>
この場合、背面カリングをオンにすることができます。<br>
これにより、描画時間が約半分節約されますが、ポリゴンを正しい方法で定義する作業が残ります。<br>
<br>
次の関数が存在します。<br>
<br>
・`d3d_set_culling(cull)` <br>
　背面カリングを開始するか (true)、背面カリングを停止するか (false) を示します。
