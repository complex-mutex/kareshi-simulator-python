## スコア計算シミュレータ

### 使い方
`score.py` のmain部分の宣言を自分の設定に合わせて変更してください
```python
# BP(自然回復分，ミニキャンディ，キャンディ)
bp = BP(0, 1000, 1000)

# Stamina(自然回復分，チャージハーフ，チャージフル，レベル)
stamina = Stamina(0, 1000, 0, 150)

# 鈍器
special = SpecialBP(10)

# appael: アピール値 (BP1の値)
# strategy:攻撃戦略をクラス化したもの (詳しくわからなければ変更する必要なし)
simulator = Simulator(appeal=12675, bp=bp, stamina=stamina, special=special, strategy=AttackStrategy)
```
あとは端末から `python score.py` で実行すると自動でそれっぽいシミュレーションをして最終スコアを表示します．

なぜかappealはベースを `12765`にして調節するといい感じになります．カードなどのブーストがある場合，`12765 * (ブースト%)` をお勧めします．

### 攻撃戦略について
`AttackStrategy`クラスを継承する or 直接書き換えることで変更可能です．
##### 注意
必ず`AttackType.ATTACK3, AttackType.ATTACK1, AttackType.ATTACKSP`をすべて使用してください．
特に`AttackType.ATTACK1，AttackType.ATTACKSP`を使用していない場合，プログラムが無限ループに陥る可能性があります．

```python
"""
Args:
hp      : 敵の残りHP
bp1     : BP1での攻撃力 (アイテムが存在しない時はNone)
bp3     : BP3での攻撃力 (アイテムが存在しない時はNone)
special : 鈍器での攻撃力 (アイテムが存在しない時はNone)

Returns:
AttackType: ATTACK1 or ATTACK3 or ATTACKSPのいずれか
"""
def enemyXXX_attack(hp, bp1, bp3, special):
    if bp3 and hp > bp3:
        return AttackType.ATTACK3
    elif bp1:
        return AttackType.ATTACK1
    else:
        return AttackType.ATTACKSP
```

### シミュレーションについて
##### 行動
* スタミナが残っている -> 現状いける中で最も後のステージに参加．ただし土台構築終わっていない場合は終わってない方を優先する
* スタミナがない -> ひたすらデートを消化します

##### ドロップ率
土台によるドロップ率向上はあります．アイテムごとの違いには~~面倒なので~~未対応です．

##### その他数値
結構適当に作っています．最終的にそれっぽい数値が出るように微調整したので正しい値を反映していません．

##### チケット
未対応．そのうち対応します．

##### 鈍器について
5秒追加する機能はついていません．

##### 好感度について
好感度によるステージ制限のみついています．アイテムドロップは未対応です．

### バグ
一応削っていますが気づいたら教えてください．あと無限ループに陥る可能性もなくはないのであまりに遅かったら`Ctrl-c`で停止してください．
