# 6.7 Pixmap

`Pixmap` オブジェクトには追加のオブジェクトがあります。詳細については、[Resource](06-2_Resource.md) と [Drawable](06-5_Drawable.md) を参照してください。

## メソッド

### `free(onerror=None)`
Pixmapを解放します。

### `create_cursor(mask, (fore_red, fore_green, fore_blue), (back_red, back_green, back_blue), x, y)`
Pixmapからカーソルを作成します。

**パラメータ:**
- `mask` - マスクPixmap
- `(fore_red, fore_green, fore_blue)` - フォアグラウンド色のRGB値
- `(back_red, back_green, back_blue)` - バックグラウンド色のRGB値
- `x, y` - ホットスポットの座標

**戻り値:** Cursor