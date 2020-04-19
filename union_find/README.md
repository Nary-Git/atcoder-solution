# アルゴリズム概要

Union-Find とは，素集合データ構造に対して適用される効率的なアルゴリズム（操作）の名称です．  
•Union：2 つの集合を一つに統合する（2 つの木構造を一つにまとめる）．  
•Find：要素が属する集合の代表を見つける（木構造の根を見つける）．2 つの要素が同じ素集合に属するかの判定（同じ根を持つか）にも使われる．  

「グループを繋げる」，「グループに属するか」という操作を実装寄りに説明すると以上のようになります．素集合を上手く保持することで（素集合データ構造では），データに対して 2 つの操作を高速に行うことができます．素集合データ構造は，2 つの操作の名称から，Union-Find データ構造，Union-Find 木（Union-Find 森では？）とも呼ばれます  
集合の分割をモデル化したものであり，集合の分割を無向グラフの連結成分に対応づけることができます．例えば，競プロでよく見る無向グラフに適用することができます．無向グラフの頂点の個数が入力として与えられ，  
•新たな辺を追加し，連結成分を繋げる（グループを繋げる）
•2 つの頂点が同じ木に属するか（連結成分か）（ある要素が同じグループに属するか）
という 2 つのクエリに対して処理を高速に行うという問題は素集合データ構造の性質をそのまま使えます（つまり 無向グラフの問題に Union-Find を適用できる）．  


* [Union find(素集合データ構造)  from AtCoder Inc.](https://www.slideshare.net/chokudai/union-find-49066733) 

# code

```python
class UnionFind():
    def __init__(self, n):
        self.n = n
        self.parents = [-1] * n # 各要素の親要素の番号を格納するリストparents 
                                # 要素が根（ルート）の場合は-(そのグループの要素数)を格納する
    def find(self, x): # 要素xが属するグループの根を返す
        if self.parents[x] < 0:
            return x
        else:
            self.parents[x] = self.find(self.parents[x])
            return self.parents[x]
    def union(self, x, y): # 要素xが属するグループと要素yが属するグループとを併合する
        x = self.find(x)
        y = self.find(y)
        if x == y:
            return
        if self.parents[x] > self.parents[y]:
            x, y = y, x
        self.parents[x] += self.parents[y]
        self.parents[y] = x
    
    # 以下、便宜的に使えるメソッド。省略しても問題ない
    def size(self, x): # 要素xが属するグループのサイズ（要素数）を返す
        return -self.parents[self.find(x)]
    def same(self, x, y): # 要素x, yが同じグループに属するかどうかを返す
        return self.find(x) == self.find(y)
    def members(self, x): # 要素xが属するグループに属する要素をリストで返す
        root = self.find(x)
        return [i for i in range(self.n) if self.find(i) == root]
    def roots(self): # すべての根の要素をリストで返す
        return [i for i, x in enumerate(self.parents) if x < 0]
    def group_count(self): # グループの数を返す
        return len(self.roots())
    def all_group_members(self): # {ルート要素: [そのグループに含まれる要素のリスト], ...}の辞書を返す
        return {r: self.members(r) for r in self.roots()}
    def __str__(self): # print()での表示用 ルート要素: [そのグループに含まれる要素のリスト]を文字列で返す
        return '\n'.join('{}: {}'.format(r, self.members(r)) for r in self.roots())
```
