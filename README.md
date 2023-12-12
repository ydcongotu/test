# Todo List
- ラベリングの定義を変える（ｍの値だけ残す）
- モデルの後に，符号化を特定した４パターンの制約式を入れる．
- 範囲モデルの中では，不等式を使う(支持符号化は直接符号化に入る)
- 基本の４パターンの実験結果を載せる　時間制限:10min,　全て解が得れなくなるまで
- 鳩の巣，背景は放置

木曜日　10時　ミーテイング

* 楊さんとの打合せメモ 2023.09.29

- 研究テーマ
  - 頂点数nのグラフにおける最長の最短遷移長の独立集合遷移問題を生成す
    る方法  

- 基本アプローチの制約
  - G=(V,E) where |V|=n
  - 変数
    - i \in {0 to k} の p_{u,i} は頂点 u に状態 i でトークンがあること
      を表す命題変数
    - u,v \in {1 to n} の x_{u,v} (u != v) はグラフにu,vに辺があること
      を表す命題変数
  - \Psi^k を以下の制約の連言とする
    - 辺制約: x_{u,v} -> -p_{u,i} v -p_{v,i}
    - 遷移制約: i と j でトークンの移動が1つだけ
    - カーネル制約 (i=0) : -p_{u,i} -> \bigwedge_{v \in adj(u)} -p_{v,i}

- 基本アプローチのアルゴリズム1
  1. i <- 1 to k でループ
     - \Psi_k を構成する
     - isSAT(\Psi_k)
       - SAT: next
       - UNSAT: break (return 最後のSATのiをk'として返す) *k'も大きい*
  2. model <- getModels(\Psi_k') でループ *Modelが出過ぎる*
     - model で得られた初期トークン集合と目標トークン集合で最短遷移長を
       計算して d を得る
     - k' == d
       - true: return model (終了)
       - false: next
  3. k' = k' -1 して手順2を実行する

- Todo [1/4]
  - [X] 関連研究 (2022の優勝チームのdescription) を読む
  - [ ] 基本アプローチの実装
  - [ ] 水曜日に言ったこと
    - ハミング距離 (H) と遷移長 (D) の関係
      - (済) H=1 <-> D=1
      - (済) H=2 -> D >= 2
      - (済) H=n -> D >= n
      - D=2 -> H=2
      - D=3 -> H=?
      - D=3 -> H=?
      - D=12 -> H=6 であるような例が存在する
  - [ ] 前調査
    - 最遠ペアを出力する厳密解法を実装する
    - 最遠ペアを出力する解空間グラフを観察する

- model の列挙
  - clasp -n 0 hoge.cnf (前回列挙する) 解の数が10万程度
  - bddminisat 1億でもOK https://jpn01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fwww.disc.lab.uec.ac.jp%2Ftoda%2Fcode%2Fallsat.html&data=05%7C01%7C%7Cc45fc3101f194d43d34008dbc089f2ee%7C20ee4c8087bd422ca5063a2b0aca0615%7C0%7C0%7C638315470944280158%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C3000%7C%7C%7C&sdata=L0%2BxciUU4XUlJwYygSS9tlODBJQ%2Bd5OrUr3qV%2F3QV%2BU%3D&reserved=0
- グラフの直径を調べるアルゴリズム
  - https://jpn01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fsaturncloud.io%2Fblog%2Falgorithm-for-diameter-of-a-graph-explained-for-data-scientists%2F&data=05%7C01%7C%7Cc45fc3101f194d43d34008dbc089f2ee%7C20ee4c8087bd422ca5063a2b0aca0615%7C0%7C0%7C638315470944280158%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C3000%7C%7C%7C&sdata=%2Bx2a8qiqw5yjoTG0hxT2UwdrbaeIvS7NzgtpAF2eUQI%3D&reserved=0

