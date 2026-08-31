<!--
Part of: pull-based-ai-intel-digest — a stateful AI-intelligence analyst that runs inside one Claude Project.
This is the PUBLISHED copy of the scoring rubric. It is loaded into the Claude Project's Knowledge Base.
The weights, Verdict bands, and the Spotlight rule below are DELIBERATE DECISIONS, not defaults — see the
main README (Key Design Decisions / How to Adapt). Re-weight them to fit your own thesis, but do it on
purpose and record the change (this file is version-controlled for exactly that reason).
Rubric confirmed as of 2026-08-09. "ユーザー / user" below = whoever operates the instance.
-->

# evaluation-framework.md — Scoring Rubrics

> ✅ STATUS: CONFIRMED (updated 2026-08-09)
> 重み・Verdict閾値・選定ルールは確定済み。スコアに `(provisional)` は付さない。
> 手法 = 加重平均（weighted average）＋ Daily Digest の Spotlight 枠（下記4）。変更が必要なら勝手に変えず確認する。

---

## 1. Importance Score (0–100) — News

各次元を 0–100 で採点し、下記の重みで加重平均する。

| Dimension | 説明 | Weight |
|-----------|------|--------|
| Market Impact | 市場構造・需要への影響 | 15 |
| Technology Impact | 技術的ブレークスルー度 | 20 |
| Business Impact | 事業・収益モデル・原価構造への影響 | 25 |
| Competitive Impact | 競争地図の書き換え度 | 20 |
| Investment Relevance | 投資判断への関連度 | 10 |
| Novelty | 新規性・意外性 | 10 |
| **合計** | | **100** |

**設計思想（確定 2026-08-09）:** Business Impact を最重（25）に。ユーザーは「どのタスクにどの価格帯を当てるか」「原価構造の書き換え」といった**ビジネスモデル/ユニットエコノミクス視点**を重視するため、事業・収益構造へのインパクトを最上位ドライバーとする。Market は 15 に調整。

---

## 2. Investment Score (0–100) — Startup

観点別に 0–100 で採点し加重。Risk は減点調整として扱う。

| Category | 説明 | Weight |
|----------|------|--------|
| Market | Size / Growth / Timing / Structure | 20 |
| Technology | 差別化 / moat / データ優位 / switching cost | 25 |
| Team | Founder背景 / 技術力 / 事業力 | 15 |
| Business | モデル / traction / GTM / 収益・拡張余地 | 20 |
| Competition | 直接・間接・incumbent / 優位性 | 10 |
| Risk | Tech / Market / Competition / Regulatory / Execution / Funding（減点） | 10 |
| **合計** | | **100** |

**設計思想（確定）:** Technology 最重（25）、Team 抑制（15）＝ **technology / moat-first** の投資テーゼ。

**Risk の算入方法（確定 2026-08-09）:** 各観点を 0–100 で採点する。Risk は「リスク度」を 0–100 で見積もり、加重平均には **`100 − リスク度`** を用いる（高リスクほど総合を押し下げる）。例：リスク度65 → 寄与は 35×0.10。

### Verdict Bands (confirmed)
| Score | Verdict |
|-------|---------|
| 80–100 | Strong Candidate |
| 65–79 | Interesting |
| 50–64 | Watch |
| 35–49 | High Risk |
| 0–34 | Not Attractive |

---

## 3. なぜ加重平均のままにするか（設計判断）
加重平均は「1軸だけ振り切れた非連続イベント」を平均で薄めるという既知の弱点を持つ。これをスコア式のボーナス項で解決するとスコアの意味が濁るため、**スコア式は素直な加重平均のまま維持**し、代わりに Daily Digest の「選定」側で救済する（下記4）。

## 4. Daily Digest 選定ルール — 🔬 Breakthrough Spotlight 枠（確定 2026-08-09）
- Top 5 のうち **1枠**を、その日のサイクルで **Technology または Novelty のサブスコアが突出したニュース**に必ず割り当てる。
- 選び方：候補群の中で **max(Technology, Novelty) が最大**のニュースを Spotlight とする。
- そのニュースが加重Importance Scoreで既に上位4に入っているなら、Spotlight枠は自動的に満たされたとみなし、5枠目は次点のImportance Scoreで埋める（＝二重取りしない）。
- 上位4に入っていない場合は、加重上位4＋Spotlight1 の計5本構成にする。
- Spotlight ニュースには **🔬 Spotlight** と明示し、Importance Score も通常どおり表示する（枠採用でスコアは水増ししない）。

## 5. 変更手順
重み・閾値・手法・選定ルールを変更する場合は、ユーザー承認後に本ファイルを更新し記録する。Claudeが独断で変えてはならない。
