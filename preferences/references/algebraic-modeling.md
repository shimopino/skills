# 代数的にモデリングする

## ルール

1. **業務概念は型の代数で表す**。「A または B または C」は直和（Java: `sealed interface` + `record`）、「A かつ B」は直積（`record`）。フラグや `type` 文字列による分岐で代用しない。
2. **業務上の失敗は値で返す**。残高不足、重複登録、期限切れなどは sealed な結果型の一枝。呼び出し側は `switch` のパターンマッチで全枝を扱い、網羅性チェックに漏れを見つけさせる。
3. **例外はバグと基盤障害だけ**。`NullPointerException` や `IllegalStateException` はバグの信号、DB 断・I/O 失敗は基盤障害。業務ルールを例外で表現しない。
4. **ドメイン内で null を使わない**。欠損は直和の一枝（`Absent` 等）か、戻り値の `Optional`。フィールドに `Optional` は置かない（欠損を型に昇格させる）。
5. **振る舞いは型の上に置く**。直和の各枝に固有の振る舞いは、枝ごとの `record` のメソッドか、`switch` で一箇所に集める。共通インタフェースへの逃げ（全枝に空実装）は避ける。

## Java での形

```java
sealed interface PaymentResult permits Paid, InsufficientBalance, CardExpired {}
record Paid(ReceiptId id) implements PaymentResult {}
record InsufficientBalance(Money shortfall) implements PaymentResult {}
record CardExpired(YearMonth expiredAt) implements PaymentResult {}
```

呼び出し側は `switch (result) { case Paid p -> …; case InsufficientBalance i -> …; case CardExpired c -> …; }`。

## 緩める条件

- Java 17 以前、あるいは sealed/record を使えない言語バージョン → references/project-decisions-win.md に従い、`enum` + 手動網羅か、既存の慣習に合わせる。
- チームが例外ベースで統一している → 結果型は持ち込まず、例外の階層を sealed 的に設計する提案に留める。

## 参考

pstack の `Model the Domain`（散らばった条件分岐を一つの明示的構造に）と方向が同じ。
