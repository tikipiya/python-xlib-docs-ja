# 6.4 Cursor

追加のメソッドについては、[Resource](06-2_Resource.md) セクションを参照してください。

## メソッド

### `free(onerror=None)`
カーソルを解放します。

### `recolor((fore_red, fore_green, fore_blue), (back_red, back_green, back_blue), onerror=None)`
カーソルの色を変更します。

**パラメータ:**
- `(fore_red, fore_green, fore_blue)` - フォアグラウンド色のRGB値
- `(back_red, back_green, back_blue)` - バックグラウンド色のRGB値
- `onerror` - エラーハンドラー（オプション）