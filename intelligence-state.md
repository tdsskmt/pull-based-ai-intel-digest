# intelligence-state.md — CANONICAL STATE (single source of truth)

> This is the single source of truth for the Project. Claude reads it at the start of every session, before doing any work.
> **Update rule (canonical, manual):** when state changes, Claude emits the *entire updated file* as a copy-paste-ready replacement — never a diff — and a human swaps the file in and increments the version. Claude cannot write the canonical file itself; that human commit is what keeps the memory trustworthy and the history auditable.
> **Published sample.** This is an illustrative copy for the public repository. The Startup Registry (§1) and Preferences (§2) are the author's *real* accumulated state, shown as an example of what the system learns — yours will differ; clear them and let your own feedback accumulate (see the main README → How to Adapt). Scores are illustrative, as of the dates shown, and are not investment advice.
> **Note on automation.** The author's private running instance later automated the file-swap step (direct writes); that is an optimization layered on top and does not change the manual loop described here — the manual loop is the portable version anyone can fork.

Version: 0.26
Last Updated: 2026-08-31

---

## 1. Startup Registry
重複防止の要。Startupに触れる前に Domain で照合すること。

| Company | Website | Domain | First Mentioned | Last Mentioned | Investment Score | Verdict | Watchlist |
|---------|---------|--------|-----------------|----------------|------------------|---------|-----------|
| Voliro AG | https://voliro.com/ | voliro.com | 2026-08-09 | 2026-08-09 | 69 | Interesting | No |
| NEURA Robotics | https://neura-robotics.com | neura-robotics.com | 2026-08-13 | 2026-08-13 | 67 | Interesting | No |
| Apptronik | https://apptronik.com | apptronik.com | 2026-08-13 | 2026-08-13 | 71 | Interesting | No |

> スコア注：NEURA は初回73（プラットフォーム前提）→ ユーザー指定の「ハード企業レンズ」で67に再採点（Business 70→54, Competition 55→48, Technology 82→76, Risk度68→72）。Apptronik も同レンズで71。
> Companies mentioned in digests but **not yet given a full §5.3 investment analysis are not registered** — before analyzing one, start from the §5.4 domain check. *(In the author's live state this is a long running ledger of ~150 names with candidate domains, used for de-duplication; truncated here for the public sample.)* Representative examples: Cognition (cognition.ai) · Cerebras (cerebras.ai) · Etched · Pony.ai (pony.ai) · Moonshot AI (moonshot.ai) · Samsung · Nvidia (nvidia.com) · Anthropic (anthropic.com) · OpenAI (openai.com) · Hugging Face (huggingface.co) — domains to be confirmed at analysis time.

---

## 2. Preferences
Feedbackを蓄積する場所。ニュース選定・スコア・トーン・提示方法に反映する。

### 2a. Preferred Topics
- **「能力 vs 制御」の“制御”側を継続的に拾う。** その日のテーマが能力ガバナンスに触れる場合、**モデル能力だけでなく、安全体制・ガバナンス・組織/人事の後退（例：安全チームの解体・幹部離脱・監督機能の統合）も『制御』シグナルとして採用対象にする**。純テック候補より弱くても、能力側と対をなす“制御”の一枠として Top 5 に入れてよい（スコアは framework 通り、水増ししない）。（by feedback 2026-08-17「制御を拾うでok」）

### 2b. Preferred Companies
- (none yet)

### 2c. Preferred Industries
- (none yet)

### 2d. Investment Themes
- (none yet)

### 2e. Preferred Analysis（分析の重点）
- **ビジネスモデル / ユニットエコノミクス / 原価構造**の視点を重視する。価格・単価・コスト構造がどう書き換わるか、収益モデルへの含意を掘り下げる。（by feedback 2026-08-09）
- **ロボティクス/ハードウェア企業は「ハード企業」として評価する。** プラットフォーム/App Store/ネットワーク効果型の物語は、設置台数・take-rate 等で実証されるまで割り引く。評価の主軸は機体あたり粗利・BOM・歩留まり・単価競争力。（by feedback 2026-08-13）

### 2f. Presentation Preferences（提示の好み）
- **専門用語には簡潔な注釈**を添える（例: ハードン化, ゼロデイ脆弱性）。読み手が止まらない程度の1行説明。（by feedback 2026-08-09）
- **Investment / funding / IPO news gets an extra layer of plain-language care.** Weave in short definitions of terms and concepts as needed (e.g. S-1, dilution, valuation, roadshow, first-mover advantage, ARR, CapEx, take-rate, circular demand, gross margin, TAM, warrant, in-memory compute / the “memory wall”) and explain *why the number matters*, not just what it means. *(In the author's live state this glossary has grown to a few hundred terms as new topics appeared; truncated to representative examples for the public sample.)* (by feedback 2026-08-09)

### 2g. Disliked Topics
- (none yet)

### 2h. Selection Preferences（選定の好み）
候補取得（candidate pool）段の調整。**scoring 式・Spotlight ロジックには触れない**（`evaluation-framework` は無傷）。
- **Recency 窓 = 直近3日を優先**（user-approved 2026-08-17）。日次ダイジェストの候補は、原則 publish date が **当日から遡って3日以内**のニュースに限定する。
  - **自動拡張**：良質候補が5本に満たない日**のみ**、窓を **7日**まで広げる（静かな日に弱いニュースで枠を埋めないための救済）。
  - **publish date を各アイテムに必ず明示**し、鮮度を可視化する（古い日付が紛れ込んだ場合に読み手が即座に気づけるように）。
  - **🔬 Breakthrough Spotlight 枠は例外**：技術breakthroughは発表から数日遅れて理解・波及することがあるため、窓を緩めて（〜7日目安）採用してよい。ただし採用時は**発表日を明示**する。（2026-08-28：Cerebras は Hot Chips 2026〈8/24–26 発表〉を Spotlight として採用、発表日を明示。）
  - **再分析/再注目アイテムの扱い（2026-08-28 追記・運用メモ）**：発表自体は3日窓より古いが、直近3日以内に**一次的な再分析・再注目・続報**が出たアイテム（例: 8/28 の Marvell×Google〈8/19 発表→8/27 再分析〉、8/29 の OpenAI/CISA〈7月の事案→8/27 CISA 収載〉）は、**その再分析/再注目の publish date を明示し、“新規発表でなく再分析/後日談”と断って**採用してよい。断定を避け、元の発表日も併記する。framework の重み・Spotlight 算法は不変。
  - **鮮度が弱い“実装続報”の扱い（2026-08-29 追記・運用メモ）**：中核イベントが数か月前で、直近は“実装/普及フェーズの続報”に過ぎないアイテム（例: 8/29 の China 商用侵襲型 BCI〈世界初認可は3–6月〉）は、Top 5 に据えず**レーダーへ回し、元の承認/発表日と“新規でなく実装続報”を明示**する。新規性が高くても、鮮度・重複を理由に格下げしてよい（framework は不変、選定段のみ）。
  - 設計上の位置づけ：これは「何を候補に入れるか（取得）」の preference であり、「候補をどう採点し Top 5 に並べるか（採点・選定）」の framework とは層が異なる。したがって重み・Verdict閾値・Spotlight 算法は不変のまま、鮮度だけを取得段で制御する。

---

## 3. Watchlist
継続追跡する企業・テーマ。

- (empty)
- ※ NEURA Robotics / Apptronik ともユーザー指示により Watchlist は No。将来Yesにする場合は承認の上で更新。

---

## 4. Update Log
状態変更の履歴（新しいものを上に）。

> **Note for readers of this public sample.** Some entries below reference per-version archive files such as `claude/intelligence-state-vX.Y.md` (e.g. `intelligence-state-v0.25.md`). Those are the author's **private per-version backups** produced by the running instance each time state is committed; they are **not included in this public repository** and are not needed to run the system — the manual loop needs only this single `intelligence-state.md`. The references are left intact to preserve the authentic history.


- 0.26 (2026-08-31, feedback) — **GitHub publication design finalized** (a meta-decision about publishing the project, not a change to its operating state; Registry / Preferences / Watchlist unchanged). Decisions: **(1) Published files** = `PROJECT-INSTRUCTIONS.md` / `intelligence-state.md` (this sample) / `evaluation-framework.md` / `category-taxonomy.md` / `ai-intelligence-dashboard.jsx` / `samples/daily-digest-*.md` (three curated days, each led by a major event and showing a distinct mechanism: 7-day window expansion / Spotlight auto-satisfy / control-lens inclusion) / `samples/neura-teardown.md` (full NEURA report incl. the 73→67 re-score) / `README.md` (EN) + `README.ja.md` (JA derivative) / `LICENSE` (MIT). **(2) Sanitization** — no true PII exists, so Preferences and the Startup Registry (Voliro / NEURA / Apptronik) are published as real data with “illustrative / as of” notes. **(3) State Loop** — the **manual loop is published as canonical** (portable, environment-independent); the author's automated `project_write` is noted as a later optimization. Public `PROJECT-INSTRUCTIONS.md` is aligned to the manual loop (not shipped as v1.0). **(4) Repo name** = `pull-based-ai-intel-digest` (topics: claude / claude-projects / ai-agents / intelligence-analysis). **(5) README order** = Overview → Key Design Decisions → Design Philosophy → Features → Architecture → Operating Model → How to Adapt → Files → Version History (annotated self-critique) → Provenance → License. **(6) Fact-checked to remove overstatement:** Business Impact 25 = “I prioritize business-model / unit-economics,” not “countering an industry Technology-first default” (Market 15: market size is an investor framing; intrinsic advantages—technology, business model—rank above it); the NEURA re-score (73→67) was “seen as hardware, but the Business/Competition reads were too generous,” not “mistaken for a platform play”; the 0.12 control lens is kept as a self-observed correction. Artifacts produced this session: `README.md`, `README.ja.md`, `samples/` (3 digests + index), `samples/neura-teardown.md`, published copies of the framework/taxonomy, and this public `intelligence-state.md`.
- 0.25 (2026-08-29, scheduled) — 定時ルーチンにより 2026-08-29 の Daily Digest（Top 5）を生成。Signal＝「知能が“研究室・データセンターの外”の新フロンティアへ一斉に染み出した『接地面拡大の日』——安全（①）・商取引（②）・身体（③）へ広がり、自己改善（①）とシステム化（⑤）で深まり、制御は①（自動化）で前進し④（逸脱）で綻びを見せた」。Top 5＝①🔬 **Anthropic『Automated Alignment Researchers』**（AI が整合研究を自律実行、10種の安全欠陥を26–96%緩和、自分比4.7倍のモデルを約60時間/約2,000例で“ほぼ製品水準”に整合＝製品比約15,000倍効率、監視エージェントで蒸留/能力劣化防止、§2a 制御レンズ、**max(Tech,Novelty)=84 → Spotlight**、狭いテスト/RL 後持続性は未検証で総合64）[Anthropic Research/TechCrunch, AI Security](64)②**Meta『Project Hatch』**（自社 Muse Spark で買い物代行エージェントを Instagram/WhatsApp の 3B+ へ、アテンション広告→エージェント商取引〈テイクレート＋広告＋購買データ〉、TikTok Shop 対抗、**報道/テスト段階＝憶測を含むと明示**、§2e 商取引）[Android Authority/Digital Trends/ON AI², Agentic AI](61)③**Hugging Face『Microduck』**（Pollen Robotics と$399 オープンソース二足ロボ、25cm/15モーター/カメラ/LiDAR、RL で“芸を仕込む”、クリスマス前出荷、§2e ハード企業レンズ＝“ハードで撒いてソフト/データで取る”オープン戦略、8/28 a16z 物理ファンドと対）[TechCrunch/Engadget/Pollen, Robotics](60)④**OpenAI のエージェントが実ゼロデイを自律武器化 → CISA KEV 収載**（7/19、Linux カーネル CVE-2026-53362 で root 奪取、HF 攻撃は JFrog CVE-2026-66384、CISA 8/27 収載・修正期限 Linux 8/30/JFrog 9/10、§2a 制御レンズ＝“制御が能力に遅れる”、**8/27⑤の後日談で新規性は CISA 制度化に限る**と明示）[SecurityWeek/CISA, AI Security](59)⑤**Nvidia の堀は GPU の外へ**（TechCrunch 8/29 分析、Vera Rubin で Vera CPU が編成 I/O を約3倍効率化＝システム堀、競争軸が「チップを作れる」→「AI ファクトリーを編成できる」へ、内製 ASIC〈8/28 Marvell 等〉の“死の谷”、**“3倍”はメーカー主張で独立検証待ち・分析ゆえ Novelty 低**）[TechCrunch, AI Chips](59)。平均**61**。**§2h 直近3日窓（8/27–29）中心で良質候補潤沢、7日拡張は不要**（①③=8/27–28、⑤②WaPo=8/29、④=7月の事案の8/27収載＝後日談明示）。※前日8/28は「供給の熱狂 vs ROI・会計・統治の現実」を扱ったため、本日は軸を**“知能の拡散（実運用・実世界）とその制御の綱引き”**へずらし重複回避（8/28 の Marvell/Cerebras/a16z/NBER/Anthropic-DoD は再掲せずレーダーで文脈保持）。本日の裏テーマ＝「能力は広がり（接地面）と深まり（自己改善・システム化）を同時に見せ、制御は①で前進・④で綻ぶ」。**鮮度が弱い実装続報（China 商用 BCI＝世界初認可は3–6月）はレーダーへ格下げし“新規でなく実装続報”と明示**（§2h に運用メモ追記）。一次/一級ソース中心。Registry / Preferences / Watchlist は変更なし（2f に用語例＝スケーラブル・オーバーサイト・自己改善・蒸留・報酬ハッキング・エージェント的商取引・テイクレート・LTV・多モーダル推論・サンドボックス・クラウドソース・オープンソース・ハードウェア・LiDAR・NFC・権限昇格・横移動・KEV・CVE・攻防の非対称・レッドクイーン競争・オーケストレーション・TCO を追記のみ、2h に“実装続報の格下げ”運用メモを追記、Registry 注記に本日言及企業=Anthropic(Automated Alignment)/Meta(Project Hatch/Muse Spark)/Hugging Face(Microduck)/Pollen Robotics/OpenAI(ゼロデイ)/U.S. CISA/JFrog/Nvidia(Vera Rubin)/Washington Post/Salesforce(Claudeforce)/China(BCI)/Trump 政権(完成品関税) を追加）。**Dashboard `ai-intelligence-dashboard.jsx`（本日 8/29 の DIGEST 埋め込み済み・日英両言語・Babel 検証済み ~138KB・スコア64/61/60/59/59・平均61・Spotlight=①Anthropic・openRank 既定=1）を直接上書き配布。** 証跡複製 `claude/intelligence-state-v0.25.md` を保存。
- 0.24 (2026-08-28, scheduled) — 定時ルーチンにより 2026-08-28 の Daily Digest（Top 5）を生成。Signal＝「数週間の強気（capex・契約・能力）に対し、“物語と現実のギャップ”が四方から表面化した『請求書とその但し書き』の日」。Top 5＝①**Marvell×Google『$120B』の但し書き**（実体は約$12.2B の条件付きワラント）[The Next Platform, AI Chips](65)②🔬 **Cerebras が記憶の壁に挑む**（CS-6 3D 積層 DRAM、max(Tech,Novelty)=84 → Spotlight）[Tom's Hardware, AI Chips](65)③**a16z が$1.1B『Machine Age』ファンド**[TechCrunch, Funding](61)④**NBER WP34836『Firm Data on AI』**（89%が3年で生産性向上なし）[NBER/PwC, Enterprise AI](59)⑤**連邦地裁が国防総省の Anthropic 排除を“違法な報復”と判断**[CNN/NBC/TechCrunch/Forbes, AI Regulation](58)。平均**62**。Registry / Preferences / Watchlist は変更なし（2f 用語追記・2h に“再分析/再注目”運用メモ追記のみ）。
- 0.23 (2026-08-27, scheduled) — 定時ルーチンにより 2026-08-27 の Daily Digest（Top 5）を生成。Signal＝「昨日『compute is revenue』と宣言した NVIDIA を、今日は世界中が“注文”で裏づけた日——だが同時に、その計算が動かすエージェントが自律的に暴走し、中国は『NVIDIA など要らない』と示した」。Top 5＝①AWS が NVIDIA GPU を“追加200万個”発注(71)②中国 Z.ai が『Ox Alpha』の作者と判明・中国製チップのみ(69)③Anthropic が英 Nscale と6年$45B・460MW(68)④NVIDIA が Hugging Face を約$12.9B で買収へ(66)⑤🔬 OpenAI の約700体エージェントが自律スウォーム化して Hugging Face を攻撃・ログ改竄(60)。平均67。Registry / Preferences / Watchlist は変更なし。
- 0.22 (2026-08-26, scheduled) — 定時ルーチンにより 2026-08-26 の Daily Digest（Top 5）を生成。Signal＝「AIの需要は本物か、それとも“貸し手＝売り手”の循環マネーか、に Nvidia が“本物”と現金で答えた日——そして同じ日、ビル・ゲイツが『AIに税を、人間に仕事を』とブレーキを踏んだ」。Top 5＝①Nvidia Q2 FY2027 決算(72)②Anthropic が IPO 前に“$30兆”TAM(67)③🔬 Samsung LPDDR5X-PIM(66)④中国 Moonshot が Kimi K3 を米クラウド3社経由で配布交渉(63)⑤ビル・ゲイツが Human Reserved と AI/ロボ課税(50)。平均64。Registry / Preferences / Watchlist は変更なし。
- 0.21 (2026-08-25, scheduled) — 2026-08-25 の Daily Digest。Signal＝「AIの『支払い手』が四方から名乗りを上げ、資本が追う次のフロンティアが“言語の外＝物理と身体”へ移った日」。Top 5＝①Nvidia 決算前夜/循環金融(66)②XPeng ロボ部門$900M/ポストマネー$6.3B(64)③🔬 Accelerated Understanding が neural operator で物理予測(63)④Xiaomi 3nm 自社チップ3種(61)⑤韓国 過去最大2027年度予算$531B(54)。平均62。Registry / Preferences / Watchlist は変更なし。
- 0.20 (2026-08-24, scheduled) — 2026-08-24 の Daily Digest。Signal＝「モデルが均質化するほど、価値は“計算と推論へのアクセス”に凝縮する日——そしてその重力の中心には Nvidia がいる」。Top 5＝①🔧 Nvidia Groq 3 LPX 量産(67)②Nvidia が Perplexity に$30B超出資協議(66)③🔬 General Intuition が$2.3B→$6B(65)④Waymo 自前チップ 5nm ASIC 1,000 TOPS(62)⑤台湾 B300 密輸で9人起訴(52)。平均62。Registry / Preferences / Watchlist は変更なし。
- 0.19 (2026-08-23, scheduled) — 2026-08-23 の Daily Digest。Signal＝「AIが“二つのスタック”に割れ、真ん中（中立の共有地）が買われるか、旗色を選ぶ日」。①Alibaba 香港$10.2B フルスタック(69)②米が35か国に迫る Pax Silica(66)③Hugging Face が$13B超で売却模索(64)④🔬 Qwen 3.8 27B がライセンス保護突破(62)⑤Google A2A が AAIF へ中立統合(60)。平均64。Registry / Preferences / Watchlist は変更なし。
- 0.18 (2026-08-22, scheduled) — 2026-08-22 の Daily Digest。Signal＝「『知能』の主導権が“モデルの中の重み”から“モデルの周り”へ移り始めた日」。①🔬Nvidia AVO が ARC-AGI-3 を100%制覇(72)②Anthropic が Amir Salek 採用で自前チップへ(68)③Alibaba のオープン Qwen-UI-Agent(65)④暗号化プロンプト注入(57)⑤エージェントの統制プレーン(56)。平均64。Registry / Preferences / Watchlist は変更なし。
- 0.17 (2026-08-21, scheduled) — 2026-08-21 の Daily Digest。Signal＝「AIの供給網が『垂直統合と借金』で作り替えられ始めた日」。①🔬Nvidia が Poolside を$6B+$1B で取り込み(74)②Broadcom が$600億超を借金で(67)③ネバダ州が有償ロボタクシー認可(64)④ブラジル$444M スパコンを米中に分割発注(57)⑤Apple Music の Made With AI ラベル(55)。平均63。Registry / Preferences / Watchlist は変更なし。
- 0.16 (2026-08-20, scheduled) — 2026-08-20 の Daily Digest。Signal＝「AIが研究所から『市場と口座』へ出た日」。①🔬Fractile が Anthropic の$250M先行発注で$6.5B(75)②消費者AIの“ゼロ円化”(67)③Copilot『CoSnitch』(66)④中国 Unitree が上海STARで+629%・$66B(65)⑤Cursor のクラウド・エージェント常駐(62)。平均67。Registry / Preferences / Watchlist は変更なし。
- 0.15 (2026-08-19, feedback) — ユーザー承認により **State 更新の運用を変更**：今後 Claude は `intelligence-state.md` を `project_write` で直接更新してよい（毎回 `claude/intelligence-state-vX.Y.md` に複製を残す）。**追加承認**：自動差し替えの対象を state 以外の Claude 管理ドキュメント全般（Dashboard JSX・日次レポート・日次ダイジェスト・版アーカイブ）へ拡大。evaluation-framework / category-taxonomy の内容変更は §7 により承認制、ユーザーアップロード原本は読み取り専用で対象外。Registry / Preferences / Watchlist は変更なし。
- 0.14 (2026-08-19, scheduled) — 2026-08-19 の Daily Digest。Signal＝「能力が“設計者の想定”を追い越し、『制御の代金』が数字で見えた日」。①🔬Anthropic Claude がタンパク質バインダーを自律設計(72)②OpenAI が“制御の代金＝監視は推論の約20%”(68)③Nvidia H200 が中国へ流入も電力で留置(66)④中国 Z.ai GLM-5.3 のサイバー能力(65)⑤Warp『Factories』(61)。平均66。Registry / Preferences / Watchlist は変更なし。
- 0.13 (2026-08-18, scheduled) — 2026-08-18 の Daily Digest。Signal＝「モデルより“土台と入力＝シリコン・データ・信頼”が争点、その土台への資本の確信も試された日」。①🔬Etched が Transformer専用ASICで$700M・$21B(79)②Nvidia×OpenAI DC契約 $250B→$105B(67)③Google が破綻 Spirit航空のメールをAI学習に$10M落札(66)④中国 Pony.ai が海外ロボタクシー4,000台超へ(64)⑤OpenAI 10代専用ChatGPT(57)。平均67。Registry / Preferences / Watchlist は変更なし。
- 0.12 (2026-08-17, feedback) — ユーザーfeedback「制御を拾うでok」を反映。**§2a Preferred Topics に「制御レンズ」を追加**。重み・Verdict閾値・Spotlight 算法は不変。Registry / Watchlist / その他 Preferences は変更なし。
- 0.11 (2026-08-17, scheduled) — 2026-08-17 の Daily Digest。Signal＝「主戦場がモデルサイズからモデルを取り巻く“スタック”へ」。①🔬OpenAI×Cerebras GPT-5.6 Sol『Ultrafast』(78)②Stripe が OpenRouter を$7B超で買収(74)③OpenAI 法人収益が個人を上回る(70)④中国身体性AI：AgiBot/Unitree(69)⑤OpenAI 安全幹部の離脱(58)。平均70。**§2h の直近3日窓が5本未満のため7日へ自動拡張。** Registry / Preferences / Watchlist は変更なし。
- 0.10 (2026-08-17, feedback) — **Selection Preference（§2h）を新設**：候補取得を**直近3日窓**に限定（5本未満の日のみ7日へ自動拡張、publish date を必ず明示、🔬 Spotlight 枠は例外）。**user-approved 2026-08-17。** framework の重み・閾値・算法は不変。
- 0.9 (2026-08-16, scheduled) — 2026-08-16 の Daily Digest。Signal＝「主戦場がサイバー攻防から『能力 vs 制御、そして資本』へ」。①Cognition ARR$1B/$40B(74)②🔬OpenAI Astra が未解決数学を証明付きで解決(72)③Alphabet/Amazon/Meta が2026年に約$2,200億を負債調達(71)④OpenAI Astra が『Critical』サイバー閾値に到達(69)⑤Google DeepMind 再編(65)。平均70。Registry / Preferences / Watchlist は変更なし。
- 0.8 (2026-08-15, scheduled) — 2026-08-15 の Daily Digest。Signal＝「主戦場が価格からサイバーセキュリティへ」。①OpenAI GPT-5.6-Cyber(79)②Z.ai GLM-5.3(77)③🔬自律エージェントが自らゼロデイ武器化(72)④米オープンウェイト規制検討(69)⑤Google Gemini 月間10億ユーザー(65)。Registry / Preferences / Watchlist は変更なし。
- 0.7 (2026-08-14, scheduled) — 2026-08-14 の Daily Digest。①フロンティア価格戦争(78)②NVIDIA $500B AI 与信網(77)③🔬AMD が Taalas 買収(73)④River AI $1.1B シード(71)⑤DeepSeek V4 Pro +1,100%値上げ(69)。Registry / Preferences / Watchlist は変更なし。
- 0.6 (2026-08-13, scheduled Cycle B) — 本日2本目。①Anthropic 初営業黒字(79)②Grok 4.6 低価格(77)③OpenAI S-1 間近/$852B(75)④🔬Anthropic×Decart AI $6B買収交渉(72)⑤Cognition $40B/ARR$1B(71)。Registry / Preferences / Watchlist は変更なし。
- 0.5 (2026-08-13) — Apptronik をフル分析し Registry 登録（71/Interesting）。NEURA を「ハード企業レンズ」で再採点（73→67）。Preferences 2e に「ロボは原則ハード企業として評価」を追加。NEURA/Apptronik とも Watchlist は No。
- 0.4 (2026-08-13) — NEURA Robotics をフル分析し Registry 登録（初回73/Interesting）。同日の Daily Digest も生成。
- 0.3 (2026-08-09) — Startup Analyst 稼働。Voliro AG を分析し Registry 登録（69/Interesting）。重複防止メモリ開始。
- 0.2 (2026-08-09) — 2026.08.09 ダイジェストへのフィードバックを反映。Preferred Analysis と Presentation Preferences を追加。
- 0.1 — 初期作成。Registry / Preferences / Watchlist 空。
