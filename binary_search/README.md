# アルゴリズム概要

* [Weblio辞書から引用](https://www.weblio.jp/content/2分探索法)
```
2分探索法とは、検索対象がソートされている場合に適用できる高速な検索手法のことである。

2分探索法は、具体的には次のようなアルゴリズムで検索を行う。まず、検索の開始は、真ん中に位置するデータと比べる。一致すればそれで終わりである。小さければ目的のデータは前半にあり、大きければ後半にある。これをデータが無くなるまで繰り返す。

この手法の胆は、検索範囲を2分割することで、検索対象が一気に絞り込まれるところにある。1000個のデータに対しては多くても10回程度の比較により結果が得られる。一般にN個のデータから検索する場合にはlog2N回のオーダーの比較で目的とするデータが得られる。

なお、この手法はデータが整列していることが前提のため、データの変更が多い状況では、その準備のためのソートにかかる時間により、かえって全体の効率が落ちる場合がある。
```

* アニメーション
  * [アルゴリズムビジュアル大事典](https://yutaka-watanobe.github.io/star-aida/1.0/algorithms/bst_simulation/anim.html)


# 例題
* 問題

```
リスト[1,3,4,5,6,7,9,10,13] があるとき、
要素の値が10のインデックスは？
```

* 解答

```python
def binary_search(list,taget):
    '''
    要素の値targetのindexを返す関数
    '''

    result = -1
    left = 0                        # leftとright（インデックス）を使ってリストの検索部分を追跡
    right = len(list) - 1           # はじめは、leftは0, rightはリストの最後のインデックスを設定

    while left <= right:            # 1つの要素に絞り込まれるまでの間は...
        center = (left + right)//2  # 真ん中の要素を調べる
        if list[center] == target:  # 真ん中の要素 = targetのとき（目標を検出したとき）
            result = center         # -> そのインデックスを返して抜ける
            break
        elif list[center] < target: # 真ん中の要素がtargetより小さかったとき
            left = center + 1       # -> leftをcenterより右に設定し直す。rightはそのまま
        elif list[center] > target: # 真ん中の要素がtargetより大きかったとき
            right = center - 1      # -> leftはそのまま。rightをcenterより左に設定し直す。

    if result == -1:
        return str(target) + "は見つかりませんでした"
    else:
        return "要素の値が" + str(target) + "のインデックス=>" + str(result)


example_list = [1,3,4,5,6,7,9,10,13,16,18,22,24,25,26] 
target = 6

example_list = sorted(example_list)
binary_search(example_list, target)
# -> 4
```