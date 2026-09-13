# 不正状態を表現できなくする

## ルール

1. **Parse, don't validate**。外部入力（HTTP、DB、ファイル、環境変数）は境界で 1 回だけドメイン型に変換する。変換に成功した値は以後どこでも正しいと信じ、再検証しない。変換失敗は境界で結果型として返す（references/algebraic-modeling.md）。
2. **プリミティブを裸で通さない**。ID、金額、メールアドレス、数量などは `record` の値オブジェクトで包む。**単なる ID も例外にしない**。`OrderId` と `CustomerId` を取り違えるバグは型で消す。
3. **不変条件はコンストラクタで守る**。値オブジェクトの `record` はコンパクトコンストラクタで不変条件を検査し、不正なら生成させない。生成できた値は常に有効。
4. **状態は型で分ける**。状態を持つ概念（注文、申請、契約）は `Draft` / `Submitted` / `Approved` のように状態ごとの型にし、遷移メソッドの戻り値型で許される遷移だけを表現する。`status` フィールドと `if` で分岐しない。
5. **状態遷移表が設計図**。`examples` が作った状態遷移表の「拒否」セルは、対応するメソッドが存在しないことで表す。

## Java での形

```java
record OrderId(UUID value) {
  OrderId { Objects.requireNonNull(value); }
}

sealed interface Order permits Draft, Submitted, Shipped {}
record Draft(OrderId id, List<Line> lines) implements Order {
  Submitted submit(Clock clock) { … }   // Draft からだけ submit できる
}
record Submitted(OrderId id, List<Line> lines, Instant submittedAt) implements Order {
  Shipped ship(TrackingNumber t) { … }
}
record Shipped(OrderId id, TrackingNumber tracking) implements Order {}
```

`Shipped` に `submit` は存在しない。不正遷移はコンパイルエラー。

## 緩める条件

- 既存コードが `status` enum で統一され、変更範囲が局所的 → references/project-decisions-win.md。状態ごとの型は新規の概念にだけ適用する提案に留める。
- 永続化層（ORM エンティティ）はフレームワークの制約で緩めてよいが、ドメイン型との変換は境界の 1 箇所に閉じ込める。

## 参考

pstack の `Type System Discipline`（不正状態を表現しにくく）と `Boundary Discipline`（外部データは境界で検証）に対応。
