# 6.3 Colormap

追加のメソッドについては、[Resource](06-2_Resource.md) セクションを参照してください。

## メソッド

### `free(onerror=None)`
カラーマップを解放します。

### `copy_colormap_and_free(scr_cmap)`
カラーマップをコピーして解放します。

**戻り値:** Colormap

### `install_colormap(onerror=None)`
カラーマップをインストールします。

### `uninstall_colormap(onerror=None)`
カラーマップをアンインストールします。

### `alloc_color(red, green, blue)`
指定されたRGB値の色を割り当てます。

### `alloc_named_color(name)`
名前付きの色を割り当てます。

**戻り値:**
- `None` または以下の値を含むオブジェクト:
  - `Card32('pixel')` - ピクセル値
  - `Card16('exact_red')` - 正確な赤成分
  - `Card16('exact_green')` - 正確な緑成分
  - `Card16('exact_blue')` - 正確な青成分
  - `Card16('screen_red')` - 画面上の赤成分
  - `Card16('screen_green')` - 画面上の緑成分
  - `Card16('screen_blue')` - 画面上の青成分

### `alloc_color_cells(contiguous, colors, planes)`
読み書き可能な色セルを割り当てます。

**戻り値:**
- `List('pixels', Card32Obj)` - ピクセル値のリスト
- `List('masks', Card32Obj)` - マスク値のリスト

### `alloc_color_planes(contiguous, colors, red, green, blue)`
カラープレーンを割り当てます。

**戻り値:**
- `Card32('red_mask')` - 赤マスク
- `Card32('green_mask')` - 緑マスク
- `Card32('blue_mask')` - 青マスク
- `List('pixels', Card32Obj)` - ピクセル値のリスト

### `free_colors(pixels, plane_mask, onerror=None)`
割り当てられた色を解放します。

### `store_colors(items, onerror=None)`
色を保存します。

### `store_named_color(name, pixel, flags, onerror=None)`
名前付きの色を指定されたピクセルに保存します。

### `query_colors(pixels)`
指定されたピクセル値の色情報を取得します。

**戻り値:**
- `List('colors', structs.RGB)` - RGB構造体のリスト

### `lookup_color(name)`
名前付きの色を検索します。

**戻り値:**
- `Card16('exact_red')` - 正確な赤成分
- `Card16('exact_green')` - 正確な緑成分
- `Card16('exact_blue')` - 正確な青成分
- `Card16('screen_red')` - 画面上の赤成分
- `Card16('screen_green')` - 画面上の緑成分
- `Card16('screen_blue')` - 画面上の青成分