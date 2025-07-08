# 6.5 Drawable

`Drawable` は `Window` と `Pixmap` オブジェクトの基底クラスです。詳細については、[Window](06-6_Window.md) と [Pixmap](06-7_Pixmap.md) のセクションを参照してください。追加のメソッドについては、[Resource](06-2_Resource.md) セクションを参照してください。

## メソッド

### `get_geometry()`
Drawableのジオメトリ情報を取得します。

**戻り値:**
- `Window('root')` - ルートウィンドウ
- `Int16('x')` - X座標
- `Int16('y')` - Y座標
- `Card16('width')` - 幅
- `Card16('height')` - 高さ
- `Card16('border_width')` - 境界線の幅

### `create_pixmap(width, height, depth)`
Pixmapを作成します。

**戻り値:** Pixmap

### `create_gc(**keys)`
グラフィックコンテキストを作成します。

**戻り値:** GC

### `copy_area(gc, src_drawable, src_x, src_y, width, height, dst_x, dst_y, onerror=None)`
領域をコピーします。

### `copy_plane(gc, src_drawable, src_x, src_y, width, height, dst_x, dst_y, bit_plane, onerror=None)`
ビットプレーンをコピーします。

### `poly_point(gc, coord_mode, points, onerror=None)`
複数の点を描画します。

**内部実装例:**
```python
request.PolyPoint(display = self.display, onerror = onerror, coord_mode = coord_mode, 
                  drawable = self.id, gc = gc, points = points)
```

### `point(gc, x, y, onerror=None)`
単一の点を描画します。

### `poly_line(gc, coord_mode, points, onerror=None)`
複数の線を描画します。

### `line(gc, x1, y1, x2, y2, onerror=None)`
単一の線を描画します。

### `poly_segment(gc, segments, onerror=None)`
複数の線分を描画します。

### `poly_rectangle(gc, rectangles, onerror=None)`
複数の矩形を描画します。

### `rectangle(gc, x, y, width, height, onerror=None)`
単一の矩形を描画します。

### `poly_arc(gc, arcs, onerror=None)`
複数の円弧を描画します。

### `arc(gc, x, y, width, height, angle1, angle2, onerror=None)`
単一の円弧を描画します。

### `fill_poly(gc, shape, coord_mode, points, onerror=None)`
多角形を塗りつぶします。

### `poly_fill_rectangle(gc, rectangles, onerror=None)`
複数の矩形を塗りつぶします。

### `fill_rectangle(gc, x, y, width, height, onerror=None)`
単一の矩形を塗りつぶします。

### `poly_fill_arc(gc, arcs, onerror=None)`
複数の円弧を塗りつぶします。

### `fill_arc(gc, x, y, width, height, angle1, angle2, onerror=None)`
単一の円弧を塗りつぶします。

### `put_image()`
画像を配置します。

**注意:** まだ実装されていません。

### `get_image()`
画像を取得します。

**注意:** まだ実装されていません。

### `draw_text(gc, x, y, text, onerror=None)`
テキストを描画します。

### `poly_text(gc, x, y, items, onerror=None)`
複数のテキスト項目を描画します。

### `poly_text_16(gc, x, y, items, onerror=None)`
16ビット文字の複数のテキスト項目を描画します。

### `image_text(gc, x, y, string, onerror=None)`
背景付きテキストを描画します。

### `image_text_16(gc, x, y, string, onerror=None)`
16ビット文字の背景付きテキストを描画します。

### `query_best_size(item_class, width, height)`
最適なサイズを問い合わせます。

**戻り値:**
- `Card16('width')` - 最適な幅
- `Card16('height')` - 最適な高さ