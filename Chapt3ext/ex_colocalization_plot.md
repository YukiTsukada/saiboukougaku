## Co-Localization Plotの高速化

連載第三回で示した共局在プロットのImageJマクロ(<https://gist.github.com/cmci/7547786>)は、少々手直しすることで高速化が可能である。時間がかかっている部分がどこであるかを考えると、次の二点である。

1. ループ毎に表示されているチャネルをsetSliceで切り替えている。
2. ループが全ピクセル数の数だけ回る。

という2つの点にある。まずループの回数を減らすにはどうしたらよいだろうか。Javaのオブジェクトに直接アクセスできればかなり速くなるのであるが、マクロではできない。そこで、行ごとのピクセル値を配列で取得することを試みる。マクロの場合、まずlineRoiをセットし、その領域のピクセル値を配列で一気に取得する。この取得した配列を、次々にconcatenateすれば、画像をひとつだけの配列に変換することが可能である。


まずsetSliceを高速化する簡単な方法であるが、これはバッチ・モードを使うと良い。画像をディスプレイ上に表示するのはかなり時間のかかる過程である。この表示を行わないようにすることでかなりの高速化をはかることができる。

ループの高速化: 行ごとに配列を一気に取得する。

###さらに高速化する

Jythonを使うとさらに高速になる。というのも、全ピクセルを一次元の配列として一発で取得できるので、あとはそれをプロットすればよい、ということになるのである。以下にJyhtonのスクリプトを示す。
