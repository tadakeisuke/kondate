# データスキーマ定義

このドキュメントでは、kondate リポジトリで使用するデータ構造を定義します。

## 在庫管理 (Inventory)

### 冷蔵庫 (fridge.yaml)

```yaml
vegetables:        # 野菜類
  - name: string           # 食材名
    quantity: number       # 数量
    unit: string          # 単位（個、本、玉、g、ml など）
    purchase_date: date   # 購入日 (YYYY-MM-DD)
    expiry_date: date     # 賞味期限 (YYYY-MM-DD)
    location: string      # 保存場所

proteins:          # たんぱく質（肉、魚など）
  - name: string
    quantity: number
    unit: string
    purchase_date: date
    expiry_date: date
    location: string

dairy:             # 乳製品
  - name: string
    quantity: number
    unit: string
    purchase_date: date
    expiry_date: date
    location: string

condiments:        # 調味料
  - name: string
    quantity: number
    unit: string
    status: string        # 充分、普通、少ない
    location: string

frozen:            # 冷凍食品
  - name: string
    quantity: number
    unit: string
    purchase_date: date
    expiry_date: date
```

### パントリー (pantry.yaml)

```yaml
grains:            # 穀物類
  - name: string
    quantity: number
    unit: string
    status: string        # 充分、普通、少ない

canned_foods:      # 缶詰
  - name: string
    quantity: number
    unit: string
    expiry_date: date

seasonings:        # 調味料
  - name: string
    quantity: number
    unit: string
    status: string
```

## 家族情報 (Family)

### メンバー (members.yaml)

```yaml
members:
  - id: string              # 一意識別子
    name: string            # 呼称
    age_group: string       # 年齢層（幼稚園、小学生、中学生、高校生、20代、30代など）
    preferences:
      likes: string[]       # 好きな料理・食材
      dislikes: string[]    # 苦手な料理・食材
      allergies: string[]   # アレルギー
      dietary_restrictions: string[]  # 食事制限
    health_notes: string[]  # 健康上の注意事項

household_preferences:
  meal_style: string              # 食事スタイル
  spice_level: string            # 辛さレベル (mild, medium, hot)
  portion_preference: string      # 分量の好み
  cooking_time_preference: string # 調理時間の好み
  budget_per_meal: string        # 1食あたりの予算
```

## 献立履歴 (History)

### 月次履歴 (YYYY-MM.yaml)

```yaml
meals:
  - date: date                    # 日付 (YYYY-MM-DD)
    day_of_week: string          # 曜日
    breakfast:                    # 朝食（オプション）
      main: string
      side: string[]
    lunch:                        # 昼食（オプション）
      main: string
      side: string[]
    dinner:                       # 夕食
      main: string               # メイン料理
      side: string[]             # 副菜
      family_reaction:
        overall: string          # 全体評価（好評、普通、不評など）
        notes: string            # 備考・コメント

weekly_summary:                   # 週次サマリー（オプション）
  week: string                   # 週の範囲
  cuisine_variety:
    - 和食: number               # 回数
    - 洋食: number
    - 中華: number
    - その他: number
  protein_variety:
    - 豚肉: number
    - 鶏肉: number
    - 牛肉: number
    - 魚: number
  notes: string[]                # 気づき・改善点
```

## レシピ (Recipes)

### レシピ定義 (favorites.yaml など)

```yaml
recipes:
  - id: string                   # レシピID
    name: string                 # レシピ名
    category: string             # カテゴリ（和食、洋食、中華など）
    difficulty: string           # 難易度 (easy, medium, hard)
    cooking_time: string         # 調理時間
    servings: string             # 何人分
    ingredients:                 # 材料リスト
      - <材料名>: string         # 分量
    instructions: string[]       # 手順
    family_rating: number        # 家族の評価 (0-5)
    frequency: string            # 作る頻度
    tags: string[]               # タグ（時短、作り置き可など）
    notes: string                # 備考
```

## スーパー情報 (Shops)

### スーパーマーケット (supermarkets.yaml)

```yaml
shops:
  - id: string                   # 店舗ID
    name: string                 # 店舗名
    distance: string             # 距離・アクセス
    open_hours:
      weekday: string            # 平日営業時間
      weekend: string            # 土日営業時間
    characteristics: string[]    # 特徴
    good_deals:                  # お得情報
      - day: string              # 曜日
        items: string            # 対象商品
        discount: string         # 割引内容
    typical_prices:              # 主要商品の通常価格
      <商品名>: string
    notes: string                # 備考

shopping_strategy:               # 買い物戦略
  regular: object                # 定期的な買い物パターン
  tips: string[]                 # 買い物のコツ
```

## データ更新のベストプラクティス

1. **日次更新**: 在庫データ (inventory/) は使用後・購入後に更新
2. **食後更新**: 献立履歴 (history/) は食事後にその日の評価を記録
3. **週次レビュー**: 週末に週次サマリーを記録
4. **月次整理**: 月初に前月の履歴を確認し、傾向を分析
5. **随時更新**: 新しいレシピを試した場合は recipes/ に追加
6. **柔軟な拡張**: 必要に応じてカテゴリやフィールドを追加可能
