# 開発学習ログ

## Session 1: Git と Branch の基礎を学ぶ

### 学習した概念

#### 1. Git とは
- **定義**: コードの変更履歴を記録するバージョン管理システム
- **できること**: 
  - 過去のコードに遡れる
  - 誰が何をいつ変えたかを追跡できる
  - 間違えたときに前の状態に戻せる

#### 2. Commit（コミット）
- **定義**: その時点での状態をセーブすること
- **比喩**: ゲームのセーブポイント
- **重要**: 「何をしたか」を説明するメッセージを付ける

#### 3. Branch（ブランチ）
- **定義**: 別の作業線を作ること
- **比喩**: 線路の脇線。本線に影響を与えずに作業できる
- **利点**:
  - 複数機能を同時開発できる
  - 失敗してもメイン線路（main）は無傷
  - 完成してからマージ（合流）できる

#### 4. Branch の構成
```
main（メイン線路）
  ← feature/init-structure（脇線：初期構成）完成 → main に合流
  ← feature/preset-toggle（脇線：プリセット機能）完成 → main に合流
  ← ...（以降の機能）
```

#### 5. Git が管理するもの
- **物理的**: `.git/` フォルダ内にすべてのコミット履歴が保存
- **ブランチの実態**: ポインタ（参照）に過ぎず、別物理ファイルではない
- **切り替え時**: `.git/` から該当コミットの内容を読み込んで working directory を更新
- **不思議な感覚**: ファイルは1セットなのに、ブランチ切り替えで内容が変わる

#### 6. Git の3つの状態
1. **Untracked**: ファイルが存在するが Git が追跡していない
2. **Staged**: `git add` 後。次のコミットに含めると予約された状態
3. **Committed**: セーブ完了

#### 7. add と commit が分かれてる理由
- **add（ステージング）**: 「どのファイルをセーブに含めるか」を選別
- **柔軟性**: 修正した複数ファイルの中から、完成したものだけ commit できる
- **例**: fileA と fileB は完成、fileC はバグがある → fileA, B だけ commit

#### 8. .gitignore ファイル
- **役割**: Git で追跡しないファイルを指定
- **このプロジェクト**: `.claude/` を無視（個人設定フォルダ）
- **.gitignore 自体**: commit する（「何を無視するか」の指示書なので）

#### 9. 改行コード（Line Ending）
- **LF**: Unix/Linux/Mac スタイル（`\n`）
- **CRLF**: Windows スタイル（`\r\n`）
- **このプロジェクト**: 警告が出たが無視して OK（個人開発・Windows専用）

### 実行したコマンド

| コマンド | 説明 |
|---------|------|
| `git switch -c feature/init-structure` | 新規ブランチ作成＆移動 |
| `git status` | 現在の状態を確認 |
| `code .gitignore` | VS Code でファイル作成・編集 |
| `git add <file>` | ファイルをステージング |
| `git commit -m "メッセージ"` | 変更をセーブ |

### 作成した成果物

1. **`.gitignore`**: `.claude/` フォルダを Git で追跡しない
2. **`README.md`**: プロジェクト説明（GitHub に表示される）
3. **`index.html`**: アプリの HTML テンプレート（これから機能を追加）

### 重要な気付き

- **開発は段階的**: 一気に完成させるのではなく、feature ブランチ単位で実装 → テスト → merge
- **Git の強力さ**: 並行開発・巻き戻し・履歴追跡が可能になる
- **学習の流れ**: コマンド実行 → 状態確認 → 理解 → 次へ進む

### 次のステップ（予定）

- [x] GitHub にプッシュ（feature ブランチを remote に送る）
- [x] Pull Request の概念を学ぶ
- [x] feature ブランチを main にマージ
- [ ] 次の feature ブランチ（常備品プリセット機能）を切ってから実装開始

---

## Session 2: GitHub との連携と Pull Request ワークフロー

### 学習した概念

#### 1. Remote（リモート）とは
- **定義**: GitHub 上のリポジトリのこと
- **Origin**: デフォルトの遠隔リポジトリ（GitHub）
- **構造**:
  ```
  Local（あなたのパソコン）
      ↑↓
  GitHub（origin）
  ```
- **確認コマンド**: `git remote -v` で接続情報を表示

#### 2. Fetch と Push
- **Fetch**: `GitHub → Local`（データを引っ張ってくる）
- **Push**: `Local → GitHub`（データを送る）
- **origin の設定**: 両方の役割を持つ
  - Push 先：ローカルのコミットを送る
  - Fetch 元：他人の変更や新しい branch を取得

#### 3. Tracking（追跡設定）
- **定義**: ローカルの branch が GitHub のどの branch に対応するかを記憶
- **`-u` フラグの役割**: push 時に追跡設定を同時に作成
- **効果**:
  - `-u` でセット → `git push` だけで自動的に対応する branch に送信
  - なし → 毎回 `git push origin branch-name` と明示的に指定

#### 4. Branch 名の統一
- **ルール**: ローカルと GitHub は同じブランチ名で統一
- **理由**: 違う名前だと混乱のもと（同じコミットなのに別 branch）
- **慣例**: 自動的に同じ名前で push される

#### 5. Pull Request（PR）
- **定義**: 「この branch の変更を main にマージしていいですか？」という提案・確認プロセス
- **利点**:
  - 変更内容をレビュー（1人でも自分が確認）
  - コミット履歴が見やすい
  - チーム開発の習慣づけ
- **流れ**:
  ```
  feature branch → GitHub push
               → PR 作成
               → 内容確認
               → マージ
               → branch 削除
  ```

#### 6. Conflict（競合）
- **定義**: 複数人が同じファイルの同じ行を修正したとき発生
- **例**:
  ```
  main:      line 5 = "aaa"
  feature:   line 5 = "bbb"
             → どっちを採用する？
  ```
- **このプロジェクトでは**: Conflict なし（新規ファイルだけ追加）

#### 7. Branch の削除
- **安全性**: Branch を削除しても **commit 履歴は消えない**
- **理由**: Branch はポインタに過ぎず、実データは `.git/` に永遠に保存
- **確認方法**: `git log` で commit 履歴は残ってる
- **GitHub 履歴**: PR タブで merge 履歴は残る

#### 8. Remote-Tracking Branch（リモート追跡 branch）
- **定義**: ローカルキャッシュ内の、GitHub branch の状態
- **表示**: `origin/feature/init-structure` という形式
- **削除方法**: GitHub から削除しても、ローカルキャッシュには残ることがある
  ```
  git fetch --prune   ← GitHub から削除された branch をローカルからも削除
  ```

#### 9. Fetch vs Pull
- **Fetch**: GitHub からの取得のみ
  ```
  GitHub → origin/main（ローカルキャッシュ）
  自分の main には反映されない
  ```
- **Pull**: Fetch + 自動 Merge
  ```
  GitHub → origin/main → 自動的に main にマージ
  ```
- **使い分け**:
  - Fetch = 「GitHub に何があるか確認したい（merge は後で）」
  - Pull = 「GitHub の最新を今すぐ反映したい」

### 実行したコマンド

| コマンド | 説明 |
|---------|------|
| `git remote -v` | リモート接続情報を表示 |
| `git push -u origin feature/init-structure` | branch を GitHub に送信＆追跡設定 |
| `git switch main` | main ブランチに切り替え |
| `git pull origin main` | GitHub から最新を取得＆マージ |
| `git branch` | ローカルブランチ一覧 |
| `git branch -r` | リモートブランチ一覧 |
| `git branch -a` | 全ブランチ表示 |
| `git fetch --prune` | 削除された remote branch をローカルからも削除 |

### 完成させたサイクル

```
① feature branch 作成（git switch -c）
② ローカルで 3つの commit
③ GitHub に push（git push -u）
④ Pull Request 作成（GitHub UI）
⑤ マージ（GitHub UI）
⑥ ローカル main を更新（git pull）
⑦ リモート追跡 branch をクリーンアップ（git fetch --prune）
```

### 重要な気付き

- **Branch は単なるポインタ**: 削除しても commit 履歴は永遠に残る
- **Pull Request の価値**: 1人開発でも、コード確認 → merge という習慣は大事
- **Git + GitHub の協力体制**:
  - Git：ローカルの履歴管理
  - GitHub：リモートの履歴管理 + PR による確認プロセス

### 疑問から学ぶ（重要）

- 「add と commit に分かれてるのは面倒？」→ 実は細かい制御ができるから強力
- 「branch 名が同じになるのはなぜデフォルト継承されない？」→ Git は柔軟性を優先（違う名前で push したい人もいる）
- 「branch を削除しても大丈夫？」→ 実は commit 履歴は残るから安全
- 「fetch と pull は何が違う？」→ 自動 merge の有無（便利さ vs 確認機会）

### 次のセッション

- [ ] 常備品プリセット機能を feature ブランチで実装
- [ ] 同じワークフロー（branch → commit → push → PR → merge）を繰り返す
