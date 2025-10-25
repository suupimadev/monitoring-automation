# 監視アラート自動起票・集約通知フロー

本リポジトリは、監視ツールからのアラートを AWS へ連携し、
ServiceNow への自動起票と、まとめ通知を行う運用フローの構成をまとめたものです。

---

## 全体アーキテクチャ

```
flowchart LR
    ZB[監視ツール\n(Zabbixなど)] --> APG[API Gateway]
    APG --> LBD[Lambda\n(データ正規化)]
    LBD --> SF[Step Functions\n(フロー制御)]

    SF --> DDB[DynamoDB\n(イベント集約/状態管理)]
    SF --> SNOW[ServiceNow\n(インシデント起票)]
    SF --> SNS[(SNS)\n通知]
```
アラートはホスト単位、発生回復、Log、Trap単位で集約し、通知にはメッセージのみを一覧形式で含めます。

