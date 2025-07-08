# 6.8 Fontable

Fontableは GC と Font オブジェクトの基底クラスです。詳細については、[GC](06-9_GC.md) と [Font](06-10_Font.md) のセクションを参照してください。追加のメソッドについては、[Resource](06-2_Resource.md) セクションを参照してください。

## メソッド

### `query()`
フォントの詳細情報を取得します。

**戻り値:**
- `Object('min_bounds', structs.CharInfo)` - 最小文字境界
- `Object('max_bounds', structs.CharInfo)` - 最大文字境界
- `Card16('min_char_or_byte2')` - 最小文字または第2バイト
- `Card16('max_char_or_byte2')` - 最大文字または第2バイト
- `Card16('default_char')` - デフォルト文字
- `Card8('draw_direction')` - 描画方向
- `Card8('min_byte1')` - 最小第1バイト
- `Card8('max_byte1')` - 最大第1バイト
- `Card8('all_chars_exist')` - すべての文字が存在するか
- `Int16('font_ascent')` - フォントアセント
- `Int16('font_descent')` - フォントディセント
- `List('properties', structs.FontProp)` - フォントプロパティのリスト
- `List('char_infos', structs.CharInfo)` - 文字情報のリスト

### `query_text_extents(string)`
文字列のテキスト寸法を取得します。

**戻り値:**
- `Card8('draw_direction')` - 描画方向
- `Int16('font_ascent')` - フォントアセント
- `Int16('font_descent')` - フォントディセント
- `Int16('overall_ascent')` - 全体アセント
- `Int16('overall_descent')` - 全体ディセント
- `Int32('overall_width')` - 全体幅
- `Int32('overall_left')` - 全体左端
- `Int32('overall_right')` - 全体右端