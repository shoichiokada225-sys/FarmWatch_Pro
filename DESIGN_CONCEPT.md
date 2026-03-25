# FarmWatch Pro - デザインコンセプト v1.0

## 概要

「AIっぽいダサさ」からの完全脱却。Splunkライクなプロフェッショナルダッシュボード。
データが主役で、UIは脇役に徹する。深夜の監視オペレーターが長時間使っても疲れない、
情報密度が高くてかっこいい画面を目指す。

---

## 1. カラーパレット（Tailwind CSS変数定義）

### CSS Custom Properties

```css
:root {
  /* === 背景色（3段階） === */
  --bg-base:    #0a0d14;   /* 最深部: ページ全体の背景 */
  --bg-surface: #111827;   /* 中間: カード・パネル背景 */
  --bg-raised:  #1a2235;   /* 浮き上がり: モーダル・ドロップダウン */
  --bg-overlay: #1e2d42;   /* ホバー時・選択状態 */

  /* === アクセントカラー（4色） === */
  --accent-primary:   #00d4ff;   /* エレクトリックシアン: メインCTA、アクティブ状態 */
  --accent-secondary: #3b82f6;   /* ロイヤルブルー: リンク、セカンダリアクション */
  --accent-success:   #10d98a;   /* エレクトリックグリーン: 正常、増加、OK */
  --accent-danger:    #f43f5e;   /* ビビッドローズ: アラート、減少、エラー */
  --accent-warning:   #f59e0b;   /* アンバー: 警告、注意 */

  /* === グロー効果（box-shadow用） === */
  --glow-primary:   0 0 20px rgba(0, 212, 255, 0.25);
  --glow-success:   0 0 20px rgba(16, 217, 138, 0.25);
  --glow-danger:    0 0 20px rgba(244, 63, 94, 0.25);
  --glow-warning:   0 0 20px rgba(245, 158, 11, 0.25);

  /* === テキスト色 === */
  --text-primary:   #f1f5f9;   /* メインテキスト */
  --text-secondary: #94a3b8;   /* サブテキスト、ラベル */
  --text-muted:     #475569;   /* プレースホルダー、無効状態 */
  --text-accent:    #00d4ff;   /* 強調テキスト、数値ハイライト */

  /* === ボーダー === */
  --border-subtle:  rgba(148, 163, 184, 0.08);  /* 区切り線 */
  --border-default: rgba(148, 163, 184, 0.15);  /* カード枠 */
  --border-accent:  rgba(0, 212, 255, 0.40);    /* フォーカス、アクティブ */

  /* === チャート用カラーパレット（8色、ダーク背景で映える） === */
  --chart-1: #00d4ff;   /* シアン */
  --chart-2: #3b82f6;   /* ブルー */
  --chart-3: #10d98a;   /* グリーン */
  --chart-4: #f59e0b;   /* アンバー */
  --chart-5: #a78bfa;   /* パープル */
  --chart-6: #f43f5e;   /* ローズ */
  --chart-7: #fb923c;   /* オレンジ */
  --chart-8: #34d399;   /* エメラルド（控えめ） */
}
```

### Tailwind設定への組み込み（tailwind.config.js）

```js
module.exports = {
  darkMode: 'class',
  theme: {
    extend: {
      colors: {
        bg: {
          base:    '#0a0d14',
          surface: '#111827',
          raised:  '#1a2235',
          overlay: '#1e2d42',
        },
        accent: {
          primary:   '#00d4ff',
          secondary: '#3b82f6',
          success:   '#10d98a',
          danger:    '#f43f5e',
          warning:   '#f59e0b',
        },
        ink: {
          primary:   '#f1f5f9',
          secondary: '#94a3b8',
          muted:     '#475569',
        },
      },
    },
  },
}
```

---

## 2. タイポグラフィ指針

| 用途 | フォント | サイズ | ウェイト | カラー |
|------|---------|--------|---------|--------|
| 見出し（H1） | `Inter` または `JetBrains Mono` | 24px | 700 | `--text-primary` |
| KPI数値 | `JetBrains Mono` | 36–48px | 700 | `--text-accent` |
| KPIラベル | `Inter` | 11px | 500 | `--text-muted` |
| ボディ | `Inter` | 14px | 400 | `--text-secondary` |
| ナビゲーション | `Inter` | 13px | 500 | `--text-secondary` |
| コード/データ | `JetBrains Mono` | 13px | 400 | `--text-primary` |

**ポイント:** 数値にはモノスペースフォントを使い、桁ズレを防ぐ。ラベルは小さめでも視認性を確保。

---

## 3. UIコンポーネントスタイルガイド

### 3-1. KPIカード

```
┌──────────────────────────────────┐  ← border: 1px solid var(--border-default)
│  ╔══╗                            │     background: var(--bg-surface)
│  ║  ║  総頭数                    │     border-radius: 12px
│  ╚══╝  TOTAL ANIMALS             │     左端に3pxの縦ボーダー（アクセントカラー）
│                                  │
│  1,247                           │  ← JetBrains Mono 48px, var(--text-accent)
│                                  │
│  ↑ 3.2% 前月比                   │  ← success color, 12px
└──────────────────────────────────┘
```

**CSS指針:**
```css
.kpi-card {
  background: var(--bg-surface);
  border: 1px solid var(--border-default);
  border-left: 3px solid var(--accent-primary);  /* アクセントライン */
  border-radius: 12px;
  box-shadow: var(--glow-primary);               /* グロー効果（控えめ） */
  padding: 20px 24px;
  transition: border-color 0.2s, box-shadow 0.2s;
}

.kpi-card:hover {
  border-left-color: var(--accent-primary);
  box-shadow: 0 0 30px rgba(0, 212, 255, 0.35);  /* ホバーでグロー強化 */
}

.kpi-value {
  font-family: 'JetBrains Mono', monospace;
  font-size: 2.5rem;
  color: var(--text-accent);
  letter-spacing: -0.02em;
}
```

**カードごとのアクセントカラーバリエーション:**
- 頭数・売上: `--accent-primary`（シアン）
- 正常指標: `--accent-success`（グリーン）
- アラート系: `--accent-danger`（ローズ）
- 注意指標: `--accent-warning`（アンバー）

---

### 3-2. ヘッダー

```
┌────────────────────────────────────────────────────────────────┐
│  [FW]  FarmWatch Pro          [検索]  [通知●]  [ユーザー]      │
│        多古農場                                                │
└────────────────────────────────────────────────────────────────┘
```

**CSS指針:**
```css
.header {
  background: var(--bg-base);
  border-bottom: 1px solid var(--border-subtle);
  height: 56px;
  padding: 0 24px;
  /* ガラスモーフィズムはNG: 農業ツールに不要な洗練さ */
  backdrop-filter: none;
}

.logo-mark {
  color: var(--accent-primary);
  font-family: 'JetBrains Mono', monospace;
  font-weight: 700;
}
```

---

### 3-3. タブナビゲーション

```
[ ダッシュボード ] [ 個体管理 ] [ 繁殖記録 ] [ レポート ] [ アラート● ]
     ↑アクティブ      ↑非アクティブ
```

**CSS指針:**
```css
.tab-nav {
  background: var(--bg-base);
  border-bottom: 1px solid var(--border-subtle);
  padding: 0 24px;
}

.tab-item {
  color: var(--text-muted);
  font-size: 13px;
  font-weight: 500;
  padding: 12px 16px;
  border-bottom: 2px solid transparent;
  transition: color 0.15s, border-color 0.15s;
}

.tab-item:hover {
  color: var(--text-secondary);
  border-bottom-color: var(--border-default);
}

.tab-item.active {
  color: var(--accent-primary);
  border-bottom-color: var(--accent-primary);
  /* background: なし（フラットに） */
}
```

---

### 3-4. ボタン

```
[  プライマリCTA  ]   [ セカンダリ ]   [ キャンセル ]   [ 削除 ]
  ▶ 実線・シアン        outline            ghost         danger
```

**CSS指針:**
```css
/* プライマリ */
.btn-primary {
  background: var(--accent-primary);
  color: #0a0d14;  /* 暗い背景文字でコントラスト確保 */
  font-weight: 600;
  border: none;
  border-radius: 6px;
  padding: 8px 20px;
  box-shadow: 0 0 12px rgba(0, 212, 255, 0.3);
  transition: box-shadow 0.2s;
}
.btn-primary:hover { box-shadow: 0 0 20px rgba(0, 212, 255, 0.5); }

/* セカンダリ */
.btn-secondary {
  background: transparent;
  border: 1px solid var(--border-accent);
  color: var(--accent-primary);
  border-radius: 6px;
  padding: 8px 20px;
}

/* デンジャー */
.btn-danger {
  background: transparent;
  border: 1px solid rgba(244, 63, 94, 0.4);
  color: var(--accent-danger);
  border-radius: 6px;
  padding: 8px 20px;
}
.btn-danger:hover {
  background: rgba(244, 63, 94, 0.1);
  box-shadow: var(--glow-danger);
}
```

---

### 3-5. チャートスタイリング（recharts / Chart.js 対応）

**背景・グリッド設定:**
```js
// recharts の場合
<CartesianGrid
  strokeDasharray="3 3"
  stroke="rgba(148, 163, 184, 0.08)"   // 極めて控えめなグリッド
  vertical={false}                      // 横線のみ（Splunkスタイル）
/>
<XAxis
  tick={{ fill: '#475569', fontSize: 11 }}
  axisLine={{ stroke: 'rgba(148, 163, 184, 0.15)' }}
/>
<YAxis
  tick={{ fill: '#475569', fontSize: 11 }}
  axisLine={false}
  tickLine={false}
/>
```

**ツールチップ:**
```js
<Tooltip
  contentStyle={{
    background: '#1a2235',
    border: '1px solid rgba(0, 212, 255, 0.3)',
    borderRadius: '8px',
    color: '#f1f5f9',
    boxShadow: '0 0 20px rgba(0, 212, 255, 0.15)',
  }}
  labelStyle={{ color: '#94a3b8' }}
/>
```

**チャートエリア塗りつぶし（グラデーション）:**
```jsx
<defs>
  <linearGradient id="fillPrimary" x1="0" y1="0" x2="0" y2="1">
    <stop offset="0%" stopColor="#00d4ff" stopOpacity={0.3} />
    <stop offset="100%" stopColor="#00d4ff" stopOpacity={0.02} />
  </linearGradient>
</defs>
<Area fill="url(#fillPrimary)" stroke="#00d4ff" strokeWidth={2} />
```

---

## 4. ダッシュボードレイアウト（ワイヤーフレーム）

```
┌─────────────────────────────────────────────────────────────────────┐
│  [FW] FarmWatch Pro  多古農場        [検索] [🔔2] [admin▾]          │  ← bg-base
├─────────────────────────────────────────────────────────────────────┤
│  ダッシュボード  個体管理  繁殖記録  レポート  アラート(2)           │  ← タブナビ
├────────────────────────────────────────────────────────────────────-┤
│  [期間選択: 今月▾]  [農場: 全体▾]              最終更新: 14:32      │  ← フィルターバー
├──────┬──────┬──────┬──────────────────────────────────────────────-┤
│ KPI  │ KPI  │ KPI  │  KPI                                           │  ← KPIカード行
│ 1247 │ 98.2%│ ¥8.4M│  23                                            │     bg-surface
│ 総頭数│ 分娩率│ 売上 │  アラート                                      │
├──────┴──────┴──────┴──────────────────────────────────────────────-┤
│                                                                     │
│   月別頭数推移（エリアチャート）          繁殖状況（ドーナツ）       │
│   ──────────────────────────           ┌──────────┐                │
│   ╭─────────────────────╮              │   78%    │ 妊娠中          │
│   │                     │              │          │ 乾乳期          │
│   ╰─────────────────────╯              └──────────┘ 分娩待ち       │
│                                                                     │
├─────────────────────────────────────────────────────────────────────┤
│   最近のアラート                        個体ランキング               │
│   ● ID:1042 発情検知  14:20            1. 優香 (ID:0891) 産乳+12%   │
│   ● ID:0987 体重減少  13:45            2. 桜花 (ID:0234) 高分娩     │
│   ○ ID:1108 定期検診  12:00            3. 春風 (ID:0567) ...        │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 5. デザイン原則 — 「AIっぽさ」脱却のための具体的指針

### やってはいけないこと（NGリスト）

| NG | 理由 |
|----|------|
| エメラルドグリーン/ティールを主役にする | AI生成UI・環境系アプリの定番色として手垢がついている |
| 全体的なグラデーション背景 | 「映える」ようで情報が読みにくくなる |
| カードを全部同じサイズで並べる | 情報の重要度が伝わらない、退屈に見える |
| グラスモーフィズム（blur背景） | 農業業務ツールとして不釣り合い、また視認性が下がる |
| アニメーションの多用 | プロフェッショナルツールはアニメを控えめにする |
| パステルカラー | コントラストが取れず、暗い背景で映えない |
| `rounded-2xl` 以上の角丸 | 柔らかすぎる。プロツールは `rounded` (4-8px) 程度 |

### やるべきこと（DOリスト）

| DO | 効果 |
|----|------|
| 数値にモノスペースフォントを使う | データダッシュボードらしい精密さが出る |
| グリッド線を最小限に（水平線のみ） | Splunkライクなクリーンさ |
| アクセントカラーを「点で」使う | 左ボーダー3px、アイコン、数値のみ |
| アラートの色に意味を持たせ一貫させる | 赤=危険、アンバー=警告、緑=正常を絶対に守る |
| 情報密度を高める（空白を減らす） | プロツール感が出る |
| ヘッダーは薄いborder-bottomのみ | 影やグラデーションなし。引き算の美学 |
| ホバー時のグロー効果は控えめに | `opacity: 0.3` 以下。主張しすぎない |

### Splunkライクなクール感の出し方

1. **左ボーダーアクセント**: カードの左端3-4pxだけにアクセントカラーを入れる
2. **数値の扱い**: 大きな数値は`JetBrains Mono`で表示、単位は小さく右下
3. **ステータスドット**: `●` や `○` を色付きで使う（シンプルだが情報密度高い）
4. **段階的な背景**: `bg-base < bg-surface < bg-raised` の3段階で奥行きを出す
5. **グリッドの使い方**: 12カラムグリッドで、チャートが大きく、KPIは小さく
6. **エラー/アラートの演出**: 赤いグローエフェクトで視線を自然に誘導

---

## 6. アクセシビリティ（WCAG AA準拠）

| 組み合わせ | コントラスト比 | 判定 |
|-----------|-------------|------|
| `#f1f5f9` on `#111827` | 14.5:1 | AA合格 |
| `#94a3b8` on `#111827` | 5.8:1 | AA合格 |
| `#00d4ff` on `#111827` | 8.2:1 | AA合格 |
| `#0a0d14` on `#00d4ff` | 9.1:1 | AA合格（ボタンテキスト） |
| `#f43f5e` on `#111827` | 5.2:1 | AA合格 |

---

## 7. 実装優先順位

1. **フェーズ1（即実施）**: カラーパレットをCSS変数に移行、背景色を刷新
2. **フェーズ2**: KPIカードの左ボーダーアクセント + グロー効果
3. **フェーズ3**: チャートの配色・グリッド線・ツールチップ刷新
4. **フェーズ4**: ヘッダー・タブナビの最終調整、フォント変更

---

*策定者: UXデザイナー（AI社員）| 2026-03-15*
