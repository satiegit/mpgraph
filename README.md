# mpgraphR 

　**mpgraphR.sty** は， **LuaTeX（LuaLaTeX）** で **MetaPost** を直接 **TeX** 文書内に記載して作図する事を可能にする **luamplib.sty** パッケージを利用する際に, なるべくシンプルなコマンドで様々な図形を楽に描くために拵えた私的なマクロ集です。

　基本的には例えば三角形ABCを描く際には， 
```
\mpPointsDef A(4,3)B(0,0)C(5,0).% Aという点を（４,３）とし,Bを(0,0),Cは(5,0)と座標を設定する。
\mpGlines ABCA.                 % ABCAの順に点を線分で繋いで描く
\mpGptlabels A(3)B(7)C(-1).     % ABCの点に点のラベルを描く。位置は右を0として反時計回りに12等分で指定。
```
とすれば， 座標で定義された3点ABCを線分で結んだ三角形ABCAが文書に現れるというマクロです。

![20210120forGit](https://user-images.githubusercontent.com/77713927/105143300-0206cf00-5b3f-11eb-91d9-b74b99476280.png)

　ただし， **LuaLaTeX** での利用が（多分）前提で， プリアンブルには例えば，
```
\usepackage{luamplib}% LuaTeX で MetaPost を利用する為のライブラリパッケージ
\usepackage{mpgraphR}% 私的な MetaPost の Graphic 用のマクロ集パッケージ
```
が必要（もちろん **mpgraphR.sty** が **TeX** の読み込めるディレクトリに置いてあり， **mktexlr** などが施されていて **TeX** が認識できる状態になっていることが前提）であり， 其の上で， 本文中に
```
\begin{mplibcode}% LuaTeX文書内でMetaPostのコードを書いて図を描く環境<開始>
\mpBaseSize(5mm)%  描画の基本単位を指定するマクロ。見れば分かるが5mmに指定している
...
\end{mplibcode}%   LuaTeX文書内でMetaPostのコードを書いて図を描く環境<終了>
```
という **mplibcode** という **MetaPost** のコードを書く環境を用意した上で其の中に先程の **mpgraphR.sty** で定義さえたマクロなコードを書いて利用します。まだ説明してない **\mpBaseSize(5mm)** というコマンドは雰囲気でわかると思いますが, 点や線を描く際の座標の基本の長さを **5mm** にするというマクロで先程提示した三角形ABCであれば， 底辺にあたるBCは25mmの長さで描画されることになります。

　別段 **mpgraphR.sty** のコマンドが必要なわけではないので， **mplibcode** 環境内では好きに **MetaPost** のコードを書いて利用することができます。

　例えば， 素の **MetaPost** で
```
\begin{mplibcode}% LuaTeX文書内でMetaPostのコードを書いて図を描く環境<開始>
\mpBaseSize(5mm)%  描画の基本単位を指定するマクロ。見れば分かるが5mmに指定している
\mpPointsDef A(4,3)B(0,0)C(5,0).% Aという点を（４,３）とし,Bを(0,0),Cは(5,0)と座標を設定する。
\mpGlines ABCA.                 % ABCAの順に点を線分で繋いで描く
\end{mplibcode}%   LuaTeX文書内でMetaPostのコードを書いて図を描く環境<終了>
```
と同じことを実現する為には， 
```
\begin{mplibcode}% LuaTeX文書内でMetaPostのコードを書いて図を描く環境<開始>
numeric unitwidth;unitwidth=5mm;
pair pointA,pointB,pointC;pointA=(4,3) scaled unitwidth;pointB=(0,0) scaled unitwidth;pointA=(5,0) scaled unitwidth;
draw pointA--pointB--pointC--cycle;
\end{mplibcode}%   LuaTeX文書内でMetaPostのコードを書いて図を描く環境<終了>
```
のように書けば良い。説明しなくても済むような変数名にしているので無駄にソースが長いけど， そこは工夫や加減で短くできると思う。
**numeric** は数値変数の宣言で， **pair** は点の宣言， **=** で値を設定できる事は普通だとして， 点の座標はスケールを5mmで設定しているということ。
**draw** で三角形ABCを描くのだけど， 点を線分で繋ぐ場合は **--** で結び, 最後に **--cycle** とすれば閉じてくれる。
各コマンドは最後に **;** で締めること。別段難しくもないのだけど煩雑だし図を描くことに専念したいというのがマクロ集を作成した動機。
