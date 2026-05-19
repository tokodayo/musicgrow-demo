# AI_READINESS

**プロダクト**: MusicGrow v5
**ステータス**: 設計フェーズ
**バージョン**: v1
**最終更新**: 2026-05-18

このドキュメントは、MusicGrow を AI 時代に対応させるための設計指針と実装ロードマップを定義します。

---

## 1. なぜ AI レディネスが必要か

### 1.1 MusicGrow の核と AI の相性

MusicGrow の核は「**練習した自分と、あとで再会するアプリ**」です。AI 時代において、この「再会」は新しい次元を獲得します。

ユーザーが自分の AI アシスタント（Claude、ChatGPT、Gemini など）に「最近の練習どうだった？」と聞いた時、AI が MusicGrow のデータを参照し、過去のひとことを引用しながら答える。これは「再会」の進化形です。

> 自分の机の引き出しに、自分の信頼する AI だけが手を伸ばせる。

### 1.2 ASI 時代への地ならし

2026年現在、MCP（Model Context Protocol）が AI と外部アプリの接続標準として確立しつつあります。OpenAI、Google、Anthropic すべてが採用しました。

つまり、これから作るアプリは「AI が使いやすい構造」であることが、競争力の前提条件になります。MusicGrow は最初からその構造で設計します。

---

## 2. 「AI に選ばれる」の定義

「AI に選ばれるアプリ」と一口に言っても、3 つのシナリオがあります。MusicGrow はどのシナリオも視野に入れます。

### シナリオ A: AI エージェントがユーザーの代理で使う

将来、ユーザーが「練習記録をつけて」と自分の AI に頼むと、AI が代わりに各種アプリを評価して最適なものを使う。

このシナリオで選ばれるためには、AI が理解しやすい操作仕様（MCP ツール定義）が必要です。

### シナリオ B: AI がユーザーのデータハブとして使う

ユーザーのデータが AI に渡り、AI が過去のひとことを参照しながら会話する、励ます、提案する。MusicGrow が「物語の源泉」として機能する。

このシナリオで選ばれるためには、データが構造化されていて、AI が文脈を組み立てやすい形であることが必要です。

### シナリオ C: AI がアプリを推薦する

「練習記録アプリ何がいい？」と AI に聞いた時、MusicGrow が推薦される。

このシナリオで選ばれるためには、AI が学習データとして読み込める形（公開ドキュメント、明確な独自性）であることが必要です。

---

## 3. MusicGrow の AI 連携思想

### 3.1 「ユーザー本人の AI にだけ開く」原則

MusicGrow のデータは個人的な日記です。これを開く相手は厳密に限定します。

| 相手 | 開くか |
|---|---|
| ユーザー本人 | ✓ |
| ユーザー本人の AI アシスタント（明示的に接続したもの） | ✓ |
| 他のユーザー | ✗ |
| 他のユーザーの AI | ✗ |
| MusicGrow 運営側の AI（解析・学習用） | ✗ |
| 第三者サービスの AI（学習データとして） | ✗ |

### 3.2 「アプリの中に AI を組み込む」のではなく「アプリの外の AI に開く」

これが MusicGrow の独自路線です。多くの音楽アプリは「AI 採点」「AI 譜面解析」など、アプリ内に AI を組み込みます。

MusicGrow は逆方向。**ユーザー自身が選んだ AI に、自分のデータを開いていく**。

> MusicGrow は道具、AI とユーザーが主役。

---

## 4. 競合との差別化

| 観点 | 一般的なアプリの AI 連携 | MusicGrow の AI 連携 |
|---|---|---|
| AI の位置 | アプリ内蔵 | ユーザー側 |
| データの主導権 | アプリ提供者 | ユーザー本人 |
| 何を提供するか | AI による採点・指導 | 自分の物語を語る素材 |
| 収益モデル | AI 機能を有料化 | AI 連携は無料（コアと整合） |
| プライバシー | データはサーバーに集約 | データは手元に残る |

この差別化は、MusicGrow の世界観と完全に一貫しています。

---

## 5. 現在の方針：AI 連携は無料で提供

### 5.1 方針

現時点では、AI 連携（JSON エクスポート、MCP サーバー）は**無料機能として提供します**。

理由は 3 つ：

1. **思想との整合**: 「再会するアプリ」のコア体験の延長であり、ここで人質を取らない
2. **競合への差別化**: 他社が AI 機能を有料目玉にする中、無料で打ち出すことで普及速度を上げる
3. **コスト構造の余裕**: 個人〜小規模利用のうちは、ホスティングコストがほぼゼロで済む

### 5.2 「永久」は約束しない

将来の状況変化に備え、「永久無料」とは約束しません。約束しすぎないことも、ユーザーへの誠実さです。

ただし、以下を守る前提でのみ、有料化の可能性を残します。

- 十分な事前告知（最低 3 ヶ月前）
- 既存ユーザーへの配慮（既存利用分は無料継続など）
- 「コア機能」（タイマー、ひとこと、カレンダー、JSON エクスポート）は無料を維持
- 有料化するならアドオン的な高機能のみ（自動要約、AI による道のまとめ生成など）

詳細は MONETIZATION.md を参照。

---

## 6. データ設計の原則

### 6.1 すべて JSON で表現できる

MusicGrow のすべてのデータは JSON 構造で表現できます。これは AI が読みやすい前提です。

### 6.2 メタデータを丁寧に持つ

各「ひとこと」には、AI が文脈を組み立てやすいよう、以下のメタデータを持たせます（DESIGN_today_one_word.md 第5章のデータ構造を参照）。

- `pieceId`, `instrumentId`: 何の練習か
- `practiceMinutes`, `sessionType`: どんな時間か
- `mood`, `tone`, `promptId`: どんな空気で書かれたか
- `createdAt`, `tags`: いつ、何について
- `isFavorite`, `reappearedCount`: ユーザーとの関係

### 6.3 エクスポートの一貫性

エクスポートされる JSON は、内部データ構造とほぼ一致させる。AI が「内部仕様」と「エクスポート仕様」を別々に学習する必要がないように。

---

## 7. ロードマップ 3 段階

| 段階 | 内容 | 想定コスト | 実装時期 |
|---|---|---|---|
| 第1段階 | JSON エクスポート | 0円 | MVP と同時 |
| 第2段階 | MCP サーバー（Vercel上） | 0〜500円/月 | MVP 後、ユーザー数十人〜 |
| 第3段階 | スケール対応 | 規模次第 | ユーザー数千人〜 |

---

## 8. 第1段階：JSON エクスポート

### 8.1 概要

MusicGrow アプリ内に「データをダウンロード」ボタンを置きます。ユーザーはファイルを取得して、自分の AI ツールにドラッグ&ドロップで渡せます。

### 8.2 エクスポート形式の仕様

ファイル形式: JSON
エンコーディング: UTF-8
1ファイルにまとめる（複数ファイルにしない）

```json
{
  "format": "musicgrow-export-v1",
  "exportedAt": "2026-05-18T20:34:12+09:00",
  "user": {
    "instruments": [
      {
        "id": "piano",
        "label": "ピアノ",
        "clefs": ["treble", "bass"]
      }
    ],
    "tonePreference": "gentle",
    "startedAt": "2025-11-03"
  },
  "pieces": [
    {
      "id": "chopin-op10-3",
      "title": "ショパン エチュード Op.10-3",
      "instrumentId": "piano",
      "addedAt": "2026-01-15"
    }
  ],
  "entries": [
    {
      "id": "uuid-1",
      "date": "2026-05-18",
      "createdAt": "2026-05-18T20:34:12+09:00",
      "pieceId": "chopin-op10-3",
      "instrumentId": "piano",
      "practiceMinutes": 27,
      "mood": "😊",
      "text": "今日は左手が歌えた気がする",
      "promptId": "p_g_12",
      "tone": "gentle",
      "sessionType": "practice",
      "tags": ["left_hand", "tone"],
      "isFavorite": false,
      "reappearedCount": 0,
      "lastReappearedAt": null
    }
  ],
  "milestones": [
    {
      "type": "return_count",
      "value": 30,
      "reachedAt": "2026-04-20",
      "displayText": "これは、続いていると言っていい"
    }
  ]
}
```

### 8.3 エクスポート単位

3 種類提供：

| 単位 | 用途 |
|---|---|
| 全体 | すべてのデータ。バックアップや AI への一括渡し |
| 曲別 | 特定の曲との会話だけ。「この曲との関係を AI に語らせる」用途 |
| 期間指定 | 過去N日、または日付範囲。「最近の自分を AI に振り返ってもらう」用途 |

### 8.4 ファイル名規則

```
musicgrow_export_<scope>_<YYYYMMDD>.json
```

例：
- `musicgrow_export_all_20260518.json`
- `musicgrow_export_piece-chopin-op10-3_20260518.json`
- `musicgrow_export_30days_20260518.json`

### 8.5 インポート機能

エクスポートと対になる「JSON インポート」も用意します。理由：

- 機種変更時のデータ移行
- 端末紛失時のリカバリ
- AI で加工したデータを書き戻したい場合（上級者向け）

インポート時の検証：
- `format` フィールドのバージョン確認
- 既存データとの ID 衝突回避（マージか上書きを選択）

### 8.6 ユーザーが自分の AI に渡す手順

想定フロー：

1. MusicGrow の設定画面で「データをダウンロード」をタップ
2. JSON ファイルが端末に保存される
3. 自分の AI ツール（Claude、ChatGPT 等）を開く
4. JSON ファイルをドラッグ&ドロップ
5. AI に質問する（「最近の私の練習を振り返って」「この曲との関係を語って」など）

この手順を MusicGrow のヘルプページに記載します。AI 別の手順例（Claude 用、ChatGPT 用）も載せると親切。

### 8.7 実装難度

| 項目 | 難度 |
|---|---|
| エクスポート機能 | 低（既存データを JSON 化して download 属性付きリンクで配るだけ） |
| インポート機能 | 中（ファイル読み込みとバリデーション） |
| ヘルプドキュメント | 低 |

総工数: 1〜2 日（既存実装に追加する場合）

### 8.8 コスト

0 円。クライアント側で完結。サーバー側の処理不要。

---

## 9. 第2段階：MCP サーバー

### 9.1 MCP とは何か

**Model Context Protocol（MCP）** は、AI と外部システムを繋ぐ標準プロトコルです。2024年11月に Anthropic が発表、OpenAI と Google も採用し、2026年現在で事実上の業界標準。

ざっくり言うと「AI が外部のアプリやデータベースと話すための共通言語」。USB-C みたいなもの。

MusicGrow が MCP サーバーを提供すると、ユーザーは自分の Claude や ChatGPT から MusicGrow に接続できる。JSON ファイルのダウンロード・ドラッグ&ドロップが不要になり、AI が直接データを参照できます。

### 9.2 MusicGrow MCP サーバーが提供するツール一覧

MCP では「ツール（tool）」という単位で機能を公開します。MusicGrow が提供すべきツールは以下。

すべて**読み取り専用**（AI に書き込みさせない）。これは「他人には見せない日記」を守るため。

```typescript
// MusicGrow MCP Tools の定義（TypeScript 風）

const tools = [
  {
    name: "list_recent_entries",
    description: "直近のひとこと（練習メモ）を取得する。デフォルト30件。",
    inputSchema: {
      type: "object",
      properties: {
        limit: { type: "number", default: 30, maximum: 100 },
        since: { type: "string", format: "date", description: "この日付以降。省略可" }
      }
    }
  },
  {
    name: "list_entries_for_piece",
    description: "特定の曲に紐づくひとことを取得する。「この曲との会話」を AI に渡したい時に使う。",
    inputSchema: {
      type: "object",
      properties: {
        pieceId: { type: "string", description: "曲ID" },
        limit: { type: "number", default: 50 }
      },
      required: ["pieceId"]
    }
  },
  {
    name: "get_favorites",
    description: "お気に入りマークがついたひとこと（道の宝物）を取得する。",
    inputSchema: {
      type: "object",
      properties: {
        limit: { type: "number", default: 50 }
      }
    }
  },
  {
    name: "get_summary_stats",
    description: "練習時間の累計、節目の達成状況、戻ってきた回数などの統計を取得する。",
    inputSchema: { type: "object", properties: {} }
  },
  {
    name: "list_pieces",
    description: "登録されている曲の一覧を取得する。",
    inputSchema: { type: "object", properties: {} }
  },
  {
    name: "search_entries",
    description: "ひとことを全文検索する。気分スタンプやタグでも絞れる。",
    inputSchema: {
      type: "object",
      properties: {
        query: { type: "string", description: "検索キーワード。省略可" },
        mood: { type: "string", description: "気分スタンプ。省略可" },
        tone: { type: "string", description: "トーン。省略可" },
        tags: { type: "array", items: { type: "string" }, description: "タグ。省略可" }
      }
    }
  },
  {
    name: "get_rest_days",
    description: "「休符の日」として記録された日のリストを取得する。",
    inputSchema: {
      type: "object",
      properties: {
        limit: { type: "number", default: 30 }
      }
    }
  }
];
```

### 9.3 認証方式

OAuth 2.1 を使います。フローは以下：

1. ユーザーが Claude（または ChatGPT 等）で「MusicGrow に接続」を選ぶ
2. ブラウザで MusicGrow の認証画面が開く
3. ユーザーが MusicGrow にログイン（端末内データ用なら端末認証で代替可）
4. 「Claude に読み取り権限を与える」を承認
5. Claude にトークンが発行される
6. 以後、Claude は MusicGrow からデータを取得できる

トークンの有効期限：30日（更新可）
スコープ：`read:entries`, `read:stats` のみ。`write:*` は提供しない。

### 9.4 読み取り専用設計

MCP サーバーは読み取り API のみを公開します。書き込み（ひとことを AI が代筆する等）はしません。

理由：
- 「他人には見せない日記」は本人の言葉だけで成り立つ
- AI が代筆すると、それは本人の道ではなくなる
- 万が一トークンが漏れても、データを汚されない

### 9.5 ホスティング選定：Vercel 推奨

MusicGrow は既に Vercel にデプロイされています。MCP サーバーも同じ Vercel に追加するのが最もシンプル。

Vercel のメリット：
- 既存の Next.js / 静的サイトと同居できる
- OAuth サポート組み込み済み
- 自動スケール
- 個人開発レベルなら無料枠で済む

ファイル構成（例）：

```
musicgrow-demo/
├── index.html          (既存のメインアプリ)
├── api/                (新規追加)
│   ├── mcp/
│   │   └── server.ts   (MCP サーバー本体)
│   ├── auth/
│   │   ├── authorize.ts
│   │   └── token.ts
│   └── data/
│       └── entries.ts  (データアクセス層)
└── vercel.json
```

### 9.6 MCP サーバー実装の最小例

Claude/ChatGPT に「これ実装して」と頼む時に貼る素材として、最小実装の骨格を示します。

```typescript
// api/mcp/server.ts
// MusicGrow MCP サーバーの最小実装

import { Server } from "@modelcontextprotocol/sdk/server";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio";

const server = new Server({
  name: "musicgrow-mcp",
  version: "1.0.0"
});

// ツール一覧を返す
server.setRequestHandler("tools/list", async () => {
  return {
    tools: [
      {
        name: "list_recent_entries",
        description: "直近のひとことを取得する",
        inputSchema: {
          type: "object",
          properties: {
            limit: { type: "number", default: 30 }
          }
        }
      },
      // ... 他のツール定義
    ]
  };
});

// ツール呼び出しを処理する
server.setRequestHandler("tools/call", async (request) => {
  const { name, arguments: args } = request.params;

  // 認証チェック
  const userId = await authenticateRequest(request);
  if (!userId) {
    throw new Error("Unauthorized");
  }

  switch (name) {
    case "list_recent_entries":
      const entries = await db.entries.findMany({
        where: { userId },
        orderBy: { createdAt: "desc" },
        take: args.limit ?? 30
      });
      return {
        content: [
          {
            type: "text",
            text: JSON.stringify(entries, null, 2)
          }
        ]
      };

    case "list_entries_for_piece":
      const pieceEntries = await db.entries.findMany({
        where: { userId, pieceId: args.pieceId },
        orderBy: { createdAt: "desc" },
        take: args.limit ?? 50
      });
      return {
        content: [
          {
            type: "text",
            text: JSON.stringify(pieceEntries, null, 2)
          }
        ]
      };

    // ... 他のツール

    default:
      throw new Error(`Unknown tool: ${name}`);
  }
});

const transport = new StdioServerTransport();
await server.connect(transport);
```

### 9.7 想定リクエスト数とコスト試算

| ユーザー数 | 1人あたり日次リクエスト | 月間総リクエスト | Vercel コスト |
|---|---|---|---|
| 10人 | 5回 | 1,500回 | 無料枠内 |
| 100人 | 5回 | 15,000回 | 無料枠内 |
| 1,000人 | 5回 | 150,000回 | 無料枠内 |
| 10,000人 | 5回 | 1,500,000回 | Pro プラン（$20/月） |

Vercel の無料枠（Hobby プラン）は月100GB帯域、100万関数呼び出し。MCP サーバーの呼び出しは1回あたりごく軽量なため、ユーザー数百〜1000人規模までは無料で運用可能。

### 9.8 実装難度と工数

| 項目 | 難度 | 工数目安 |
|---|---|---|
| MCP サーバー本体（ツール7つ） | 中 | 3〜5日 |
| OAuth 認証 | 高 | 3〜5日 |
| データアクセス層 | 中 | 2〜3日 |
| ヘルプドキュメント | 低 | 1日 |

合計: 2〜3週間（Claude や ChatGPT に手伝ってもらう前提）

---

## 10. 第3段階：規模が出てから

### 10.1 移行のトリガー

第2段階から先に進むのは、以下のいずれかが該当した時：

- 月間アクティブユーザーが1,000人を超える
- MCP リクエスト数が月100万回を超える
- Vercel のレスポンスタイムが劣化する
- 収益が出始めて、本格的なインフラ投資が正当化される

### 10.2 移行先候補

| 移行先 | 特徴 | 月額コスト感 |
|---|---|---|
| **Cloudflare Workers** | エッジ実行、グローバル分散、リクエスト課金 | 数百円〜数千円 |
| **Composio** | フルマネージド MCP、認証含む | 数千円〜 |
| **自前 VPS（Hetzner等）** | 完全管理、運用コスト要 | 数百円〜 |

優先順位は規模と人手次第。1人で運用するなら Cloudflare、運用人員がいるならどれでも。

### 10.3 コスト構造の変化

第2段階までは「ほぼ固定費 0 円」ですが、第3段階以降は「リクエスト数 × 単価」のモデルに移行します。

この時点でマネタイズ（MONETIZATION.md 参照）が機能している前提で、収益とコストが並走する形になります。

---

## 11. プライバシーとセキュリティ

### 11.1 端末内保存の徹底

MVP 段階では、データは端末内（localStorage または IndexedDB）に保存。サーバーに自動送信しない。

MCP サーバー化する際も、データは「ユーザーが明示的に同期した分」のみクラウドに置く。デフォルトは端末内のまま。

### 11.2 ユーザー本人の AI に限定する仕組み

MCP サーバーへのアクセスは OAuth で認証。トークンはユーザー本人が明示的に発行・取り消しできる。

複数の AI に接続する場合は、それぞれにトークンを発行。AI ごとに権限を分離。

### 11.3 データ販売・解析しない明文

MusicGrow は以下を約束します：

- ユーザーのひとことを、学習データとして第三者に提供しない
- 広告ターゲティングに使わない
- 統計分析の対象としない（運営側も読まない）
- 緊急時のセキュリティ対応を除き、いかなる場面でも内容を参照しない

これはプライバシーポリシーに明記します。

### 11.4 GDPR / 個人情報保護法への対応指針

- データの完全削除機能（アカウント削除時、すべて消去）
- データのエクスポート権（JSON で全データ取得可能）
- 同意なき第三者提供の禁止
- 海外サーバーを使う場合は所在地の明示

---

## 12. 将来の変更可能性について

### 12.1 「永久無料」を約束しない理由

AI 業界のコスト構造は変動が激しく、ホスティング料金、認証サービス料金、AI モデル提供料金、すべてが数年単位で変わる可能性があります。

「永久無料」と約束すると、状況変化時にユーザーを裏切ることになります。代わりに、以下を約束します。

### 12.2 もし有料化する場合のルール

| 項目 | 内容 |
|---|---|
| 事前告知 | 最低 3ヶ月前 |
| コア機能の保護 | タイマー、ひとこと、カレンダー、JSON エクスポートは無料維持 |
| 既存ユーザーへの配慮 | 移行期間中の既存利用は無料継続 |
| 段階的導入 | いきなり全機能有料化はしない |
| 説明責任 | なぜ有料化が必要か、ユーザーに丁寧に説明する |

### 12.3 ユーザーへの誠実さ

「自分の机の引き出し」というメタファーは、ユーザーとの信頼関係そのものです。

機能を約束しすぎないこと、現実に応じて誠実に伝えること、これも信頼の一部です。MusicGrow はその誠実さを守ります。

---

## 関連ドキュメント

- [DESIGN_today_one_word.md](./DESIGN_today_one_word.md) - 「今日のひとこと」機能の設計書
- [PLACEHOLDER_DATA.md](./PLACEHOLDER_DATA.md) - プレースホルダー文の台帳
- [MONETIZATION.md](./MONETIZATION.md) - マネタイズ方針
- [CHANGELOG_v2.md](./CHANGELOG_v2.md) - 設計書 v2 への変更履歴
