# AWS Well-Architectedフレームワーク

## 背景・概念

- AWSは**2006年**にクラウドサービスを開始。最初のサービスは2つ：
  - **EC2**（Amazon Elastic Compute Cloud）— 仮想サーバー
  - **S3**（Simple Storage Service）— オブジェクトストレージ
- 現在は**200以上のサービス**があり、今も増え続けている

> **AWS Well-Architectedフレームワーク**とは、AWSが長年のクラウド運用から蓄積したベストプラクティスをまとめたガイドラインです。設計・運用の「チェックリスト」として活用します。

---

## 6つの柱

### 第1の柱：運用上の優秀性（Operational Excellence）

**目的：** ビジネス価値を継続的に提供し、運用プロセスを改善し続ける。

主なトピック：
- IaC（CloudFormation、CDK）による変更の自動化
- CloudWatch・X-Rayによる能動的な監視
- インシデント対応のRunbook/Playbookの整備
- 定期的なレビューと継続的改善（CI/CD）

**試験キーワード：** `運用プロセス`、`継続的改善`、`監視`、`自動化`

---

### 第2の柱：セキュリティ（Security）

**目的：** データ・システム・資産を保護する。アクセス権限の管理、暗号化、インシデント検出を徹底する。

主なトピック：
- 最小権限の原則（IAM Roles、Policies）
- 保存データと転送データの暗号化（TLS/HTTPS、S3/RDS）
- GuardDuty・Security Hub・CloudTrailによる検出と対応
- Secrets Managerによる認証情報の安全な管理

**試験キーワード：** `データの整合性`、`権限管理`、`監査ログ`、`暗号化`

---

### 第3の柱：信頼性（Reliability）

**目的：** 障害に耐え、**迅速に復旧**できるシステムを構築する。

> ⚠️ **第1の柱との違い：** Reliability は「障害発生後の復旧」に焦点を当てる。Operational Excellence は「運用プロセスの改善」に焦点を当てる。

主なトピック：
- マルチAZデプロイ（Single Point of Failureの排除）
- Auto Scaling（トラフィックに応じた動的なリソース確保）
- Backup & Disaster Recovery（RTO・RPOの定義）
- Circuit Breakerパターン（障害の局所化）

**試験キーワード：** `障害からの復旧`、`高可用性`、`フォルトトレランス`、`自己回復`

---

### 第4の柱：パフォーマンス効率（Performance Efficiency）

**目的：** 計算リソースを効率的に使用し、要件の変化に柔軟に対応する。

主なトピック：
- 適切なインスタンスタイプの選択（コンピュートCシリーズ、メモリRシリーズ等）
- サーバーレス活用（Lambda、Fargate）によるオーバープロビジョニングの回避
- キャッシュ戦略（CloudFront CDN、ElastiCache Redis/Memcached）
- 定期的なベンチマークによる実測値の確認

**試験キーワード：** `適切なリソースタイプ`、`要件の変化への対応`、`低レイテンシ`

---

### 第5の柱：コスト最適化（Cost Optimization）

**目的：** 不要なコストを排除し、適切な料金モデルと需要に応じたリソース管理を行う。

主なトピック：
- Right-sizing（Cost Explorerを使った最適なインスタンスサイズの選定）
- Reserved Instances / Savings Plans（1〜3年のコミットメントで30〜70%削減）
- Spot Instances（断続的なワークロードで最大90%削減）
- 未使用リソースの削除（古いスナップショット、未割当EIP等）

**試験キーワード：** `コスト削減`、`不要なコストの排除`、`適切な料金体系`

---

### 第6の柱：持続可能性（Sustainability）

**目的：** クラウドワークロードが環境に与える影響を最小化する。エネルギー消費とカーボン排出量を削減する。

> 📌 2021年に追加された**最新の柱**。

主なトピック：
- アイドルリソースの最適化（未使用リソースも電力を消費する）
- マネージドサービスの活用（AWSによるインフラ最適化）
- 再生可能エネルギーを使用するリージョンの選択（Carbon Footprint Tool）
- 不要なデータ転送の削減

**試験キーワード：** `エネルギー消費`、`環境への影響`、`カーボンフットプリント`

---

## 混同しやすい柱の見分け方

| 設問のキーワード | 対応する柱 |
|---|---|
| 障害からの復旧・高可用性・フォルトトレランス | **Reliability（信頼性）** |
| 運用プロセス改善・監視・CI/CD | **Operational Excellence** |
| 権限管理・暗号化・データ整合性 | **Security（セキュリティ）** |
| インスタンスタイプ選定・レイテンシ・スケーリング | **Performance Efficiency** |
| 節約・コスト削減・料金体系 | **Cost Optimization** |
| エネルギー・環境・カーボン | **Sustainability** |

---

## 練習問題（Renshuu Mondai）

**問題1：** ある企業が、正確なクラウドアーキテクチャを設計し、リリース後も継続してアプリケーションを運用し、障害が発生した際に迅速に復旧できる体制を整えたいと考えています。これはAWS Well-Architectedフレームワークのどの柱に該当しますか？

- A. パフォーマンス効率
- B. コスト最適化
- C. 運用上の優秀性
- D. セキュリティ

**解説：** 「正確な設計」「継続的な運用」「障害からの迅速な復旧」は、すべて **Operational Excellence（運用上の優秀性）** のスコープです。単なる「障害復旧」だけであればReliabilityですが、運用ライフサイクル全体（設計→運用→改善）を含む場合はOperational Excellenceです。

---

## 日本語語彙・文法リスト

### 語彙（Vocabulary）

| 日本語 | 読み方（Furigana） | 意味（Nghĩa） | 品詞 |
|---|---|---|---|
| 運用 | うんよう | vận hành, vận dụng | 名詞 |
| 優秀性 | ゆうしゅうせい | tính ưu tú, xuất sắc | 名詞 |
| 障害 | しょうがい | sự cố, chướng ngại | 名詞 |
| 復旧 | ふっきゅう | phục hồi, khôi phục | 名詞 |
| 信頼性 | しんらいせい | độ tin cậy | 名詞 |
| 可用性 | かようせい | tính khả dụng | 名詞 |
| 暗号化 | あんごうか | mã hóa | 名詞 |
| 整合性 | せいごうせい | tính nhất quán, toàn vẹn | 名詞 |
| 権限 | けんげん | quyền hạn, phân quyền | 名詞 |
| 効率 | こうりつ | hiệu suất, hiệu quả | 名詞 |
| 最適化 | さいてきか | tối ưu hóa | 名詞 |
| 持続可能性 | じぞくかのうせい | tính bền vững | 名詞 |
| 排除 | はいじょ | loại bỏ, loại trừ | 名詞 |
| 監視 | かんし | giám sát | 名詞 |
| 改善 | かいぜん | cải thiện, cải tiến | 名詞 |
| 蓄積 | ちくせき | tích lũy, đúc kết | 名詞 |
| 継続的 | けいぞくてき | liên tục, tiếp diễn | な形容詞 |
| 動的 | どうてき | động, linh hoạt | な形容詞 |
| 適切な | てきせつな | phù hợp, thích hợp | な形容詞 |
| 排出量 | はいしゅつりょう | lượng khí thải | 名詞 |

### 文法（Grammar Patterns）

| 文法パターン | 使い方・例文 | 意味・用法 |
|---|---|---|
| 〜に焦点を当てる | 「Reliabilityは復旧**に焦点を当てる**」 | tập trung vào〜 |
| 〜に応じて | 「需要**に応じて**リソースを調整する」 | tùy theo〜, phụ thuộc vào〜 |
| 〜を徹底する | 「暗号化**を徹底する**」 | triệt để thực hiện〜 |
| 〜を排除する | 「不要なコスト**を排除する**」 | loại bỏ〜 |
| 〜に基づく | 「ベストプラクティス**に基づく**設計」 | dựa trên〜 |
| 〜を蓄積する | 「長年の経験**を蓄積する**」 | tích lũy〜 |
| 〜を最小化する | 「環境への影響**を最小化する**」 | tối thiểu hóa〜 |
| 〜に対応する | 「要件の変化**に対応する**」 | đáp ứng, ứng phó với〜 |
| 〜し続ける | 「サービスは増え**続けている**」 | tiếp tục〜 (diễn ra liên tục) |
| 〜として活用する | 「チェックリスト**として活用する**」 | sử dụng như là〜 |
| 〜に耐える | 「障害**に耐える**システム」 | chịu đựng, chống chịu〜 |
| 〜を定義する | 「RTO・RPO**を定義する**」 | định nghĩa, xác định〜 |

---

## 参考資料（Sankou Shiryou）

1. [AWS Well-Architectedフレームワーク（公式）](https://docs.aws.amazon.com/ja_jp/wellarchitected/latest/framework/welcome.html)
2. [AWS Well-Architectedツール](https://aws.amazon.com/jp/well-architected-tool/)
3. AWS ホワイトペーパー — Well-Architectedフレームワーク