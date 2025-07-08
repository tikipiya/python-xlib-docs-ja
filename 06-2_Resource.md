# 6.2 Resource

`Resource` は、すべてのXリソースオブジェクトの基底クラスです。このクラスから他のすべてのXオブジェクト（Window、Pixmap、Fontなど）が派生します。

## 基本的な機能

すべてのリソースオブジェクトは以下の特徴を持ちます：

### 比較とハッシュ化
- **比較可能**: リソースオブジェクト同士を比較できます
- **ハッシュ化可能**: マッピングのインデックスとして使用できます
- **辞書のキー**: Pythonの辞書のキーとして使用可能

```python
# リソースオブジェクトを辞書のキーとして使用
window_info = {}
window_info[window_object] = "メインウィンドウ"

# リソースオブジェクトの比較
if window1 == window2:
    print("同じウィンドウです")
```

## メソッド

### `kill_client(onerror=None)`
このリソースを所有するクライアントを強制終了します。

**パラメータ:**
- `onerror` - エラーハンドラー（オプション）

**説明:**
このメソッドは、リソースを作成したクライアントプロセス全体を終了させます。使用には注意が必要です。

```python
# クライアントを強制終了
resource.kill_client()
```

## リソースIDについて

すべてのXリソースには一意のリソースIDが割り当てられます：

- **一意性**: 各リソースは一意のIDを持つ
- **サーバー管理**: XサーバーがIDを管理
- **クライアント識別**: どのクライアントがリソースを所有しているかを特定可能

## リソースの種類

Resourceクラスから派生する主なオブジェクト：

- **Window**: ウィンドウオブジェクト
- **Pixmap**: ピックスマップオブジェクト  
- **Font**: フォントオブジェクト
- **GC**: グラフィックコンテキスト
- **Colormap**: カラーマップ
- **Cursor**: カーソル

## 実用的な使用例

```python
# リソースオブジェクトの管理
resource_map = {}

def register_resource(resource, description):
    """リソースを登録"""
    resource_map[resource] = description

def cleanup_resource(resource):
    """リソースをクリーンアップ"""
    if resource in resource_map:
        del resource_map[resource]
        # 必要に応じてクライアントを終了
        # resource.kill_client()

# ウィンドウを登録
register_resource(window, "メインアプリケーションウィンドウ")

# リソースの確認
if window in resource_map:
    print(f"リソース情報: {resource_map[window]}")
```

## 注意事項

### kill_client()の使用について
- **慎重な使用**: クライアント全体を終了させるため、慎重に使用する
- **データ損失**: 保存されていないデータが失われる可能性
- **システム影響**: 他のアプリケーションに影響を与える可能性

### リソース管理
- **適切な解放**: 使用後はリソースを適切に解放する
- **メモリリーク回避**: 不要なリソース参照を保持しない
- **エラーハンドリング**: リソース操作時は適切なエラー処理を行う

## 継承関係

```
Resource
├── Drawable
│   ├── Window
│   └── Pixmap
├── Fontable
│   └── Font
├── GC
├── Colormap
└── Cursor
```

すべてのXオブジェクトは最終的にResourceクラスから派生し、共通の基本機能を継承します。