# Opentrons OT-2 / Flex: 大腸菌タンパク質自動発現・精製・分析 完全ガイド & 文献調査レポート

本リポジトリは、オープンソース自動分注機 **Opentrons OT-2** および **Opentrons Flex** を活用した、大腸菌（*E. coli*）による組換えタンパク質の発現・精製・品質分析の自動化技術、プロトコル、システムアーキテクチャ、および最新学術文献の調査レポートを体系化した総合技術リファレンスです。

エジンバラ大学開発の完全自動化プラットフォーム **「APEX (Automated Protein EXpression in Escherichia coli)」**（Kasprzyk et al., 2025）の原著論文・補足データ（SI1, S2）に掲載された全図版、および **Gemini Deep Research** による最新動向レポート（宿主工学、相分離精製、磁気ビーズ、PhyTip、自動分析等）を完全収録しています。

🌐 **GitHub Pages オンライン公開サイト:**  
[https://btx11.github.io/opentrons-apex-protein-guide/](https://btx11.github.io/opentrons-apex-protein-guide/)

---

## 📑 公開ドキュメント一覧 (HTML)

| ドキュメント | 概要・特徴 | リンク (GitHub Pages) |
| :--- | :--- | :--- |
| **① 全7プロトコル統合マトリクス & アーキテクチャ詳細ガイド (完全図解版)**<br>`Opentrons_7Protocols_Automation_Architecture_Matrix.html` | 原著論文の全Figure（ワークフロー、寒天モデリング幾何学式、コロニーサンプリング物理軌道、全7プロトコルの公式OT-2デッキ配置図、時間削減比較グラフ、電気泳動・SDS-PAGEゲル写真）を網羅。各プロトコルの検体数・所要時間・動作内容・必要機材・Opentrons純正モジュール一覧を一目で把握できます。 | [▶ ドキュメントを開く](https://btx11.github.io/opentrons-apex-protein-guide/Opentrons_7Protocols_Automation_Architecture_Matrix.html) |
| **② APEX総合技術ガイド & 下流磁気ビーズ精製展開**<br>`Opentrons_APEX_Protein_Expression_Purification_Guide.html` | APEXプラットフォームの設計思想、ノーコード設定（JSON/CSV）、Docker + Nextflowによるシミュレーション環境、タブ切り替え式ステップ解説、およびOpentrons Magnetic Module + Ni-NTA磁気ビーズを用いた下流アフィニティ自動精製への展開とトラブルシューティングTips。 | [▶ ドキュメントを開く](https://btx11.github.io/opentrons-apex-protein-guide/Opentrons_APEX_Protein_Expression_Purification_Guide.html) |
| **③ 【最新調査】タンパク質自動生産・精製・分析 文献調査レポート**<br>`Opentrons_Protein_DeepResearch_Literature_Report.html` | **Gemini Deep Research** による大腸菌タンパク質生産・精製・分析統合自動化の最新技術動向。機械的破砕を不要にする自己融菌株や相分離合成オルガネラ技術（PandaPure）、OT-2/Flexによる磁気ビーズ・PhyTip固相分離、LabChip等のハイスループット分析QCを網羅。**本文中の各記述に対応する引用文献（Ref 1〜7）を完全明記**。 | [▶ ドキュメントを開く](https://btx11.github.io/opentrons-apex-protein-guide/Opentrons_Protein_DeepResearch_Literature_Report.html) |

---

## 📊 APEX 実装全7自動化プロトコル対照表

| プロトコル | 対象実験 | 検体数 | 所要時間 (96検体) | 主な自動化動作 | 使用機器・モジュール |
| :--- | :--- | :---: | :---: | :--- | :--- |
| **Protocol 1** | ヒートショック形質転換 | 96 | 拘束 **0.5分** (手動比98%減) / 計109.9分 | 予冷セル分注、DNA添加、オンデッキ42℃熱ショック、SOC復帰培養 | OT-2 + Thermocycler GEN1, P20/P300 8ch |
| **Protocol 2** | 寒天選択培地への播種 | 96 | 拘束 **2.0分** (手動比90%減) / 計10.3分 | 寒天厚みの幾何学的算出、表面+0.5mmからのソフト着地、気泡防止吐出 | OT-2, P20 8ch, Nunc OmniTray |
| **Protocol 3** | コロニー採取 & 植菌 | 96 | 拘束 **2.0分** (手動比89%減) / 計12.9分 | カメラ不要の物理採取 (Single Pierce法 / Spiral法)、前培養液への植菌 | OT-2, P20/P300 8ch, 96ディープウェル |
| **Protocol 4** | 本培養植菌 & 発現誘導 | 96 | 拘束 **4.0分** (手動比87%減) / 計109.9分 | 新鮮LB培地分注、前培養液植菌、対数増殖期待機、誘導剤(IPTG等)自動添加 | OT-2, P20/P300 8ch, 96ディープウェル |
| **Protocol 5** | コロニーPCR調製 (QC) | 24〜96 | 拘束 約5〜10分 / 自動 約75〜90分 | 滅菌水分配、コロニー懸濁、PCRマスターミックス添加、30サイクルPCR | OT-2 + Thermocycler GEN1, P20/P300 8ch |
| **Protocol 6** | 菌体破砕・可溶化・熱変性 | 96 | 拘束 約5〜8分 / 自動 約45〜55分 | 遠心集菌、クリアランス3mm低速上清除去、BugBuster溶菌、可溶性画分回収、95℃変性 | OT-2 + Thermocycler GEN1, プレート遠心機 |
| **Protocol 7** | OD600測定サンプル調製 | 96 | 拘束 約1〜2分 / 自動 約6〜8分 | 希釈液分注、菌液均一化、10倍希釈分注、プレートリーダー吸光度測定 | OT-2, P20/P300 8ch, Corning 360µL平底 |

---

## 🔬 最新動向文献調査（Gemini Deep Research）の引用文献一覧

第3のドキュメント（`Opentrons_Protein_DeepResearch_Literature_Report.html`）では、以下の7報の主要論文・公式技術文書から各技術の知見を引用し、本文中の各論点にリファレンスバッジ `[Ref 1]` 〜 `[Ref 7]` を明記しています。

| 引用番号 | 対象技術分野 | 文献・著者・発表年 | 主要な技術内容・自動化ポイント |
| :---: | :--- | :--- | :--- |
| **[Ref 1]** | **上流プロセス自動化** | **APEX: Automated Protein EXpression in Escherichia coli**<br>Kasprzyk, Herrera, Stracquadanio (2025)<br>*bioRxiv* / *ACS Synthetic Biology* | Opentrons OT-2による形質転換・寒天播種・非視覚的コロニー採取・培養誘導・化学破砕のEnd-to-End自動化パイプライン。Nextflow基盤。 |
| **[Ref 2]** | **固相精製 (磁気ビーズ) & ハードウェア自動化** | **Automated Affinity Purification Workflow on Opentrons Flex**<br>MilliporeSigma & Opentrons Labworks Inc. (2024–2025)<br>Application Note / White Paper | 次世代Opentrons FlexとFlex Gripper（ロボットアーム）、PureProteome磁気ビーズによる完全無人プレートハンドリング＆ハイスループット精製。 |
| **[Ref 3]** | **固相精製 (PhyTip微量クロマトグラフィー)** | **Automated Protein Purification Using Biotage PhyTip Columns on the Opentrons OT-2**<br>Biotage & Opentrons (2022)<br>Technical Note | レジン充填チップ（PhyTip）を用いた双方向流動（Dual-flow）クロマトグラフィー。遠心・磁気モジュール不要、チップ内での平衡化・結合・洗浄・溶出。 |
| **[Ref 4]** | **磁気ビーズ標準プロトコル** | **Automated High-Throughput Protein Purification with Ni-NTA Magnetic Beads**<br>Lin & Watson, Opentrons Protocol Development Team (2022)<br>Application Protocol | OT-2 + Magnetic Module GEN2を用いたHis-tagタンパク質磁気精製標準プロトコル。CV < 5%の高再現性、96サンプルを約40分で処理。 |
| **[Ref 5]** | **宿主工学 (自己融菌系)** | **Self-Lytic *Escherichia coli* Platform for Cost-Effective Downstream Bioprocessing**<br>Hao et al. (2022)<br>*ACS Sustainable Chemistry & Engineering*, 10(14), 4567–4576 | 凍結融解や化学試薬（Triton/Lysozyme）に依存せず、温度シフトやシグナルペプチド制御バクテリオファージ・エンドリシンで自律崩壊する大腸菌株。 |
| **[Ref 6]** | **宿主工学 (相分離・合成オルガネラ)** | **PandaPure: Single-Step, Tag-Free Protein Purification via Synthetic Phase-Separated Organelles**<br>Guo, Chen, et al. / Ailurus Bio (2024–2026)<br>*bioRxiv* / *Nature Chemical Biology* | 液-液相分離（LLPS）と自己切断スプリットインテインを融合。細胞内で標的タンパク質を無膜凝集体（TEARS）に凝縮し、沈降分離とpHシフト切断によりTag-freeで単一操作回収。 |
| **[Ref 7]** | **自動分析・ハイスループットQC** | **Automated Microplate Assays and Capillary Electrophoresis for High-Throughput Bioprocess Development**<br>Revvity (PerkinElmer) & Bio-Rad Technical Compendium (2023–2025)<br>Application Compendium | OT-2で前処理した96サンプルのLabChip GXII（微小流体キャピラリー電気泳動）、BCA/Bradford比色定量、96ウェル蛍光プレートリーダー活性測定の統合。 |

---

## 🛠️ APEX論文で使用されたOpentrons純正ハードウェア一覧

1. **Opentrons OT-2 本体**（オープンソース液体ハンドリングロボット、デュアルマウント）
2. **Opentrons Thermocycler Module GEN1**（オンデッキ96ウェルサーマルサイクラー、4℃〜99℃温度制御、自動電動リッド）
3. **Opentrons P20 8-Channel Pipette GEN2**（1〜20 µL 8連ピペット）
4. **Opentrons P300 8-Channel Pipette GEN2**（20〜300 µL 8連ピペット）
5. **Opentrons 20 µL Filter Tips**（DNase/RNase-free 滅菌フィルターチップ）
6. **Opentrons 300 µL Filter Tips**（DNase/RNase-free 滅菌フィルターチップ）
7. **Opentrons Aluminum Block for 2.0 mL Screwcap Tubes**（冷水浴・試薬保持用）
8. **Opentrons 4-in-1 Tube Rack Set**（各種チューブの安定保持用）
9. *(下流拡張モジュール)* **Opentrons Magnetic Module GEN2**（高磁力ネオジム磁石アレイ、磁気ビーズ固相分離用）

---

## 💻 動作・閲覧環境

* 全ドキュメントはスタンドアロンHTML（HTML5 + CSS3 + Vanilla JavaScript）として構築されており、ブラウザ（Chrome, Edge, Safari, Firefox）で直接閲覧可能です。
* 外部CDN（Google Fonts, Font Awesome）の利用を除き、完全ローカル環境でもレイアウトが崩れないよう堅牢に設計されています。
* 各ページ上部には **「リポジトリ内ドキュメント切替バー」** が設置されており、3つの資料間をシームレスに行き来できます。

---

## 📜 ライセンス・引用クレジット

* **原著論文:** Martyna Kasprzyk, Michael A. Herrera, Giovanni Stracquadanio. "APEX: Automated Protein EXpression in Escherichia coli." *ACS Synthetic Biology* / *bioRxiv* (2025). DOI: [10.1101/2024.08.13.607171](https://doi.org/10.1101/2024.08.13.607171)
* **公式ソフトウェアパイプライン:** [https://github.com/stracquadaniolab/apex-nf](https://github.com/stracquadaniolab/apex-nf) (AGPL-3.0 License)
* **ドキュメント監修:** Gemini Deep Research & Antigravity Pair-Programming Assistant
