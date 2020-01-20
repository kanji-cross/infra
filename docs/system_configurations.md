```uml
@startuml
skinparam shadowing false
left to right direction
database 熟語DB {
  component 熟語table [
    熟語名
    頻出度（正確にはポピュラー具合、難易度にリンク）
  ]
}

database 問題DB {
  component 問題table [
    id
    難易度
    問題(json?)
    抜かれた文字
  ]
}

[熟語インサータ] -do-> [mecab-ipadic-NEologd]: 熟語の一覧を取り出す
[熟語インサータ] -le-> [検索エンジン？]: 熟語の頻出度を調べる 
[熟語インサータ] -> 熟語DB

熟語DB <-do- [問題原案作成サーバ]: 難易度に応じた特定の頻出度以上の熟語を取り出す
note top {
  漢字が埋まった状態の表を作成する
}

[問題原案作成サーバ] -do-> [問題作成サーバ]: json?
note bottom {
  特定の漢字を抜き出す
}

[問題作成サーバ] -> 問題DB

actor クライアント

クライアント -> 問題DB: 難易度指定して取得

クライアント -> [ヴァリデータ]: 解答を提出
[ヴァリデータ] -ri-> 熟語DB: 構成する熟語の存在をチェック
@enduml
```