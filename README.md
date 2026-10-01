# Opentrons OT-2: APEX タンパク質自動発現・精製ガイド (全7プロトコル統合)

本リポジトリは、エジンバラ大学（Kasprzyk et al., 2025）が開発した、オープンソース自動分注機 **Opentrons OT-2** による大腸菌組換えタンパク質の発現・精製完全自動化プラットフォーム **「APEX (Automated Protein EXpression in Escherichia coli)」** の技術詳細、全7つの自動化プロトコル、システムアーキテクチャ、および実証実験データをわかりやすく図解・体系化した技術リファレンスです。

---

## 📑 公開ドキュメント一覧 (HTML)

1. [**Opentrons_7Protocols_Automation_Architecture_Matrix.html**](./Opentrons_7Protocols_Automation_Architecture_Matrix.html)
   - **【推奨】全7プロトコル統合対照マトリクス & アーキテクチャ詳細ガイド (完全図解版)**
   - 論文中の全Figure（ワークフロー、寒天モデリング式、サンプリング軌道、全プロトコルの公式OT-2デッキ配置図、時間比較グラフ、電気泳動・SDS-PAGEゲル写真）を網羅。
   - 各プロトコルで「何検体を、どれだけの時間で、具体的に何をしたのか」が一目でわかる統合マスターテーブルを収録。

2. [**Opentrons_APEX_Protein_Expression_Purification_Guide.html**](./Opentrons_APEX_Protein_Expression_Purification_Guide.html)
   - **総合技術ガイド & 下流アフィニティ磁気ビーズ精製（Magnetic Module）連携解説**
   - APEXの背景、ノーコード設計（JSON/CSV）、タブ切り替えによるステップ別解説、現場向けトラブルシューティングTips。

---

## 📊 実装されている全7自動化プロトコルの概要

| プロトコル | 対象実験 | 検体数 | 所要時間 (96検体) | 主な自動化動作 | 使用機器・モジュール |
| :--- | :--- | :---: | :---: | :--- | :--- |
| **Protocol 1** | ヒートショック形質転換 | 96 | 拘束 **0.5分** (手動比98%減) / 計109.9分 | 予冷セル分注、DNA添加、オンデッキ42℃熱ショック、SOC復帰培養 | OT-2 + Thermocycler GEN1, P20/P300 8ch |
| **Protocol 2** | 寒天選択培地への播種 | 96 | 拘束 **2.0分** (手動比90%減) / 計10.3分 | 寒天厚みの幾何学的算出、表面+0.5mmからのソフト着地、気泡防止 | OT-2, P20 8ch, Nunc OmniTray |
| **Protocol 3** | コロニー採取 & 植菌 | 96 | 拘束 **2.0分** (手動比89%減) / 計12.9分 | カメラ不要のチップ物理採取 (Single Pierce法 / Spiral法)、前培養植菌 | OT-2, P20/P300 8ch, 96ディープウェル |
| **Protocol 4** | 本培養植菌 & 発現誘導 | 96 | 拘束 **4.0分** (手動比87%減) / 計109.9分 | 新鮮LB培地分注、前培養液植菌、対数増殖期待機、誘導剤(IPTG等)自動添加 | OT-2, P20/P300 8ch, 96ディープウェル |
| **Protocol 5** | コロニーPCR調製 (QC) | 24〜96 | 拘束 約5〜10分 / 自動 約75〜90分 | 滅菌水分配、コロニー懸濁、PCRマスターミックス添加、30サイクルPCR | OT-2 + Thermocycler GEN1, P20/P300 8ch |
| **Protocol 6** | 菌体破砕・可溶化・熱変性 | 96 | 拘束 約5〜8分 / 自動 約45〜55分 | 遠心集菌、クリアランス3mm低速上清除去、BugBuster溶菌、可溶性画分回収、95℃変性 | OT-2 + Thermocycler GEN1, プレート遠心機 |
| **Protocol 7** | OD600測定サンプル調製 | 96 | 拘束 約1〜2分 / 自動 約6〜8分 | 希釈液分注、菌液均一化、10倍希釈分注、プレートリーダー吸光度測定 | OT-2, P20/P300 8ch, Corning 360µL平底 |

---

## ⚙️ システムアーキテクチャ

* **入力層:** プログラミング不要。JSON（機器・デッキ定義）とCSV（分注マッピング）の2ファイルのみで制御。
* **パイプライン層:** Dockerコンテナ化されたNextflowにより、シミュレーション検証、Rによる配置図（PNG）、PDF手順書を自動生成。
* **物理層:** Opentrons OT-2 + Thermocycler Module GEN1 + 8チャンネルピペット。
* **精製への拡張:** Protocol 6で抽出された可溶性ライセート（Cell-free extract）は、Opentrons Magnetic ModuleとNi-NTA磁気ビーズを用いて完全自動アフィニティ精製（Binding → Washing → Elution）へとシームレスに直結可能。

---

## 📚 参考文献情報

* **論文題名:** APEX: Automated Protein EXpression in Escherichia coli
* **著者:** Martyna Kasprzyk, Michael A. Herrera, Giovanni Stracquadanio
* **所属:** School of Biological Sciences / School of Chemistry, The University of Edinburgh
* **DOI / Preprint:** [https://doi.org/10.1101/2024.08.13.607171](https://doi.org/10.1101/2024.08.13.607171) (bioRxiv / ACS Synthetic Biology 2025)
* **公式パイプライン:** [https://github.com/stracquadaniolab/apex-nf](https://github.com/stracquadaniolab/apex-nf) (AGPL-3.0 License)
