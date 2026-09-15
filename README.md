# hmd34

Ruby on Rails と Kubernetes を軸に、プロダクト開発からプラットフォーム移行、開発者体験の改善までを手がけるソフトウェアエンジニアです。
「動くものを作る」だけでなく、**チームがプロダクトの変更を速く安全に届けられる状態を作る**ことに関心があります。

```text
Ruby 9年  /  Ruby on Rails 7年  /  AWS 7年  /  Kubernetes 4年
```

- 🧭 個人開発: 旅行計画アプリ **[旅ペンシル](https://tabipencil.com/)** を企画から運用まで一人で
- 🏠 自宅に物理サーバを置いて Kubernetes を運用していた時期があります

---

## 🧭 旅ペンシル — 個人開発 / 企画・設計・実装・インフラ・運用すべて

### **[tabipencil.com](https://tabipencil.com/)**

> その旅、いちばん楽しい "計画" から。
> 計画から、当日、帰り道の精算まで。旅のぜんぶに、ついていく。

旅行のしおりをグループで作って共有するアプリです。自分自身が旅行好きで、
「航空券もホテルも、行きたい場所も割り勘も、ぜんぶ 1 冊のしおりに」まとめたくて作りました。
**無料・登録不要・1 分で作成**でき、リンクを送るだけで仲間がその場で計画に参加できます。

想定しているのは、卒業旅行や初海外の大人数グループ、弾丸トリップ、サークル・ゼミの合宿、
推し活の遠征メンバー、そして一人旅の計画者です。

<br>

### ✨ 作った機能

| 機能 | 内容 |
| --- | --- |
| **リアルタイム共同編集** | 招待リンクを送るだけで参加。複数人が同時に旅程を編集できる |
| **日程・プラン管理** | スポット / グルメ / 宿泊 / 移動を時系列で 1 本に統合。複数のスケジュール案を作って比較し、採用する流れにも対応 |
| **タイムライン表示** | 1 日の流れを俯瞰。表示のカスタマイズと、日ごとの費用確認 |
| **支出管理・割り勘** | 支払い記録から「誰が誰にいくら渡せばいいか」を自動計算。**多通貨対応**で海外旅行でも破綻しない |
| **持ち物・やることリスト** | 全員分と個人用を分けて共有。テンプレートとして保存・再利用できる |
| **ファイル / ギャラリー** | 予約確認書などの添付と、旅の写真管理 |
| **メール転送で予定を自動作成** | ホテルや航空券の**予約確認メールを旅専用アドレスに転送するだけでプランを自動生成** |
| **AI チャット** | LLM にツールを持たせて内製。会話で旅の相談をしたり、予定の作成・変更をそのまま依頼できる |
| **モバイルアプリ** | iOS / Android ネイティブアプリを配信。**オフラインでも旅程を閲覧可能**、プッシュ通知・生体認証にも対応。海外で電波がない状況を想定 |
| **多言語対応** | 日本語 / English / 繁體中文 |

<br>

### 🏗 技術的にこだわったところ

**構成**

- **pnpm workspace + Turborepo のモノレポ**で Web・モバイル・共通パッケージを 1 リポジトリに集約
- Web は **Next.js (App Router) / React 19 / TypeScript**、PWA としても配信
- モバイルは **Expo (React Native)**。WebView をベースにしつつ、プッシュ通知・生体認証・SQLite によるオフライン閲覧・OTA 更新はネイティブ側で実装し、EAS でビルド/配信
- DB は **PostgreSQL + Prisma**。Lint / Format は **Biome** に統一

**コストをかけずに本番運用する**

- **Google Cloud / Cloud Run** でホスティングし、アクセスのない時間帯は**ゼロスケール**。常時起動のコストを持たない構成にしています
- 監視は **Grafana Cloud**（**Grafana Alloy** でメトリクス / ログを収集）。その他も可能な限り各クラウドの**無料枠に収まるよう**サービスを選定・設計しています
- 配信は **Cloudflare** 経由

**インフラを全部コードにする**

- インフラ設定は**すべて Terraform で管理**。Cloud Run / Cloudflare DNS / Supabase / Redis Cloud / Mailgun / Grafana Cloud / BigQuery(GA4) / KMS / Vertex AI など、**26 のモジュール**に分割して手作業の設定を残していません
- GitHub Actions から **Workload Identity 連携**で Terraform の plan / apply とデプロイを実行。シークレットは **SOPS** で暗号化してリポジトリ管理
- **PR ごとのビジュアルリグレッションテスト**（ベースラインとの差分スクリーンショット）、Release Drafter、Renovate を整備
- DB のバックアップジョブ、コスト監視、cron スケジューラもモジュール化し、運用まわりの仕組みをコードに寄せています
- **GA4 と Web ビーコン**で利用状況を計測し、**BigQuery** に流して分析。この連携も Terraform で構成
- その上で、**Claude / Codex からある程度のインフラ操作ができる**ように整備しました。個人開発で運用に割ける時間が限られるぶん、AI エージェントに任せられる領域を広げています。AI の実行コストとログを監視する Terraform モジュールも用意しています

<br>

**使っている技術**

`TypeScript` `Next.js (App Router)` `React 19` `Expo / React Native` `Prisma` `PostgreSQL` `Turborepo` `Biome` `Google Cloud Run` `Vertex AI` `Cloudflare` `Terraform` `SOPS` `GitHub Actions` `Grafana Cloud / Alloy` `BigQuery` `Google Maps API` `LLM / Tool Use`

---

## 💼 仕事

> 社名は伏せ、事業領域で記載しています。

| 領域 | プロジェクト | 役割 | 主な成果 |
| --- | --- | --- | --- |
| 後払い（BNPL）決済 | 決済システムの横断的な課題解決 | メンバー | PR マージまでの平均時間 1/3、単体テスト 60min → 36min |
| 後払い（BNPL）決済 | 11 の Rails アプリのモノレポ移行 | リーダー | 年間 約800回のタグ反映作業を撤廃 |
| 後払い（BNPL）決済 | ログの構造化と通知基盤の刷新 | メンバー | 通知条件を YAML 定義化し 8 倍に。ログ設定を Ansible で IaC 化 |
| 建築業界向け SaaS | 引合粗利管理機能の開発・運用保守 | メンバー | 締め処理を Model 検証に設計変更、モブプロ導入で消化 SP 1.2 倍 |
| 建築業界向け SaaS | Devise/Doorkeeper から Auth0 への認証基盤移行 | 2023年5月〜 リーダー | SAML ログインの設計、移行手順を横展開して他チームに委譲 |
| 建築業界向け SaaS | 本体システムの EC2 → EKS 移行 | 2021年6月〜 リーダー | リリース作業 5時間以上 → 40分、レスポンス p99 で約1.3倍 |
| 独立系 SIer | クラウド利用のセキュリティ研究と適用 | メンバー | Kubernetes/OpenShift で DevSecOps な CI/CD を検証し顧客環境へ適用 |
| 独立系 SIer | 就活支援サイトのサービスイン支援 | メンバー | CloudWatch の項目設計と負荷計測ツールを翌年も使える形で整備 |
| 独立系 SIer | 証券系システム構築自動化システムの構築・運用 | プロジェクトリーダー | 期待値を YAML 比較する機能を作成、PoC 前提のオフショア依頼で手戻り削減 |
| 独立系 SIer | 証券系システムの構築・標準化支援 | サブリーダー / PL | ベトナムオフショアの立ち上げ、本番確認作業 30分 → 5分以内 |

**書いたもの / 公開しているもの**

- [11個のRailsアプリケーションをモノレポに統合した話](https://np-techblog.hatenablog.com/entry/hsatoshi0001) — 2026年9月時点で社内で最も PV 数・はてブ数の多い記事になっています
- [本体サービスの EKS 移行について](https://tech.andpad.co.jp/entry/2022/04/06/100000)
- [初めて PR を作成する前に知りたいこと](https://tech.andpad.co.jp/entry/2021/07/16/120000) — 社内ドキュメントを一部公開したもの。PR テンプレートからリンクされる運用になりました
- [auth0/auth0-deploy-cli#683](https://github.com/auth0/auth0-deploy-cli/pull/683) — デプロイ CLI でエクスポートできるのにインポートできない項目があり設定が壊れる問題を修正

---

## 🛠 技術スタック

| 分類 | 内容 |
| --- | --- |
| 言語 | `Ruby` (9年) `TypeScript` `Python` (2年) `Java` (1年) `Go` (6ヶ月) |
| フレームワーク | `Ruby on Rails` (7年) `Next.js (App Router) / React` `Expo / React Native` `Spring` `Django` |
| クラウド | `AWS` (7年 / VPC, EC2, RDS, SQS, SNS, S3, ECS, EKS, Lambda, CloudWatch, DynamoDB) `Google Cloud` (Cloud Run, Vertex AI, BigQuery) `Cloudflare` |
| コンテナ / オーケストレーション | `Kubernetes` (kubeadm / EKS / OpenShift) `Docker` `Helm` `ArgoCD` |
| IaC / CI・CD | `Terraform` `Ansible` `Chef` `GitHub Actions` `CodeBuild / CodePipeline` `Turborepo` `SOPS` |
| 監視 | `Datadog` `Grafana Cloud / Alloy` `CloudWatch` |
| データベース | `PostgreSQL / Prisma` `Oracle` `Redis` `DynamoDB` |
| 認証 / セキュリティ | `Auth0` `Devise` `Doorkeeper` `SAML` `Workload Identity` `Aqua` `BurpSuite` |
| OS | `RHEL / CentOS` `Ubuntu` `Windows Server` |

---

## 📜 資格

`基本情報技術者` (2014年5月) ｜ `LPIC Level 1` (2014年10月) ｜ `応用情報技術者` (2014年12月) ｜ `情報セキュリティスペシャリスト` (2016年12月)
