# 6.1 Display

`Display` オブジェクトは、Python X Libraryの中核となるクラスです。Xサーバーへの接続を表し、すべてのX操作の起点となります。

## 基本メソッド

### `get_display_name()`
サーバーへの接続に使用された名前を返します。これはDisplayオブジェクトの作成時に提供された名前、または環境変数 `$DISPLAY` から取得された名前です。

### `fileno()`
基となるソケットのファイル記述子番号を返します。このメソッドは、Displayオブジェクトを `select.select()` に渡すために提供されています。

### `close()`
ディスプレイを閉じ、それが保持するリソースを解放します。

### `flush()`
リクエストキューをフラッシュし、キューに入っているリクエストを構築して送信します。これは、イベントを待機しないアプリケーションやマルチスレッドアプリケーションで必要になる場合があります。

### `sync()`
キューをフラッシュし、サーバーがキューに入っているすべてのリクエストを処理するまで待機します。特定のリクエストが原因で発生するエラーをトラップすることが重要な場合などに使用します。

## エラー処理

### `set_error_handler(handler)`
すべての未処理エラーに対して呼び出されるデフォルトエラーハンドラーを設定します。ハンドラーは通常のリクエストエラーハンドラーと同様に2つの引数を受け取りますが、2番目の引数（リクエスト）は None になります。

## イベント処理

### `next_event()`
次のイベントを返します。キューにイベントがない場合、サーバーから次のイベントを取得するまでブロックします。

### `pending_events()`
キューに入っているイベントの数を返します。つまり、`Display.next_event()` をブロックせずに呼び出せる回数です。

## 拡張機能

### `has_extension(extension)`
サーバーとクライアントライブラリの両方が `extension` という名前のX拡張をサポートしているかをチェックします。

### `query_extension(name)`
サーバーが拡張機能 `name` をサポートしているかを問い合わせます。

### `list_extensions()`
サーバーが提供するすべての拡張機能のリストを返します。

## リソース管理

### `create_resource_object(type, id)`
整数 `id` に対して `type` のリソースオブジェクトを作成します。`type` は以下の文字列のいずれかである必要があります：
- `resource`
- `drawable`
- `window`
- `pixmap`
- `fontable`
- `font`
- `gc`
- `colormap`
- `cursor`

## スクリーン情報

### `screen(sno=None)`
スクリーン番号 `sno` についての情報を返します。`sno` が None の場合、デフォルトスクリーンの情報を返します。

戻り値オブジェクトの属性：
- `root` - ルートウィンドウ
- `default_colormap` - デフォルトカラーマップ
- `white_pixel`, `black_pixel` - 白と黒のピクセル値
- `current_input_mask` - 現在の入力マスク
- `width_in_pixels`, `height_in_pixels` - ピクセル単位の幅と高さ
- `width_in_mms`, `height_in_mms` - ミリメートル単位の幅と高さ
- `min_installed_maps`, `max_installed_maps` - インストール済みマップの最小・最大数
- `root_visual` - ルートビジュアル
- `backing_store` - バッキングストア
- `save_unders` - アンダーセーブ
- `root_depth` - ルートの深度
- `allowed_depths` - 許可される深度

### `screen_count()`
ディスプレイの総スクリーン数を返します。

### `get_default_screen()`
ディスプレイ名から抽出されたデフォルトスクリーンの番号を返します。

## キーボード関連

### `keycode_to_keysym(keycode, index)`
キーコードを `index` エントリを参照してキーシンボルに変換します。通常、index 0は非シフト、1はシフト、2はaltグリッド、3はshift+altグリッドです。そのキーエントリがバインドされていない場合、`X.NoSymbol` が返されます。

### `keysym_to_keycode(keysym)`
`keysym` にバインドされている主要なキーコードを検索します。複数のキーコードが見つかった場合、最低のインデックスと最低のコードを持つものが返されます。`keysym` がどのキーにもバインドされていない場合、0が返されます。

### `keysym_to_keycodes(keysym)`
`keysym` にバインドされているすべてのキーコードを検索します。タプル `(keycode, index)` のリストが返され、主に最低のインデックス、次に最低のキーコードでソートされています。

### `refresh_keyboard_mapping(evt)`
`MappingNotify` イベントを受信したときに一度呼び出して、キーマップキャッシュを更新します。`evt` はイベントオブジェクトである必要があります。

### `lookup_string(keysym)`
`keysym` を単一の文字または文字列に変換しようとします。変換が見つからない場合、None が返されます。

### `rebind_string(keysym, newstring)`
`keysym` の文字列表現を `newstring` に設定し、`Display.lookup_string()` によって返されるようにします。

## アトム関連

### `intern_atom(name, only_if_exists=False)`
文字列 `name` をインターンし、そのアトム番号を返します。`only_if_exists` が true でアトムが存在しない場合、作成されず `X.NONE` が返されます。

### `get_atom_name(atom)`
`atom` の名前を文字列として検索します。アトムが存在しない場合、`BadAtom` を発生させます。

## 選択関連

### `get_selection_owner(selection)`
選択（アトム）を所有するウィンドウを返します。選択の所有者がない場合、`X.NONE` を返します。`BadAtom` を発生させる可能性があります。

### `send_event(destination, event, event_mask, propagate, onerror=None)`
合成イベントを宛先ウィンドウに送信します。`destination` はウィンドウオブジェクト、`X.PointerWindow`、または `X.InputFocus` です。`event` は `protocol.events` のクラスの1つからインスタンス化されたイベントオブジェクトです。

## ポインタ関連

### `ungrab_pointer(time, onerror=None)`
グラブされたポインタとキューに入っているイベントを解放します。

### `change_active_pointer_grab(event_mask, cursor, time, onerror=None)`
ポインタグラブの動的パラメータを変更します。

### `warp_pointer(x, y, src_window=X.NONE, src_x=0, src_y=0, src_width=0, src_height=0, onerror=None)`
ポインタを現在の位置からオフセット `(x, y)` だけ移動します。

## キーボード関連

### `ungrab_keyboard(time, onerror=None)`
グラブされたキーボードとキューに入っているイベントを解放します。

### `allow_events(mode, time, onerror=None)`
キューに入っているイベントの一部を解放します。`mode` は以下のいずれかである必要があります：
- `X.AsyncPointer`
- `X.SyncPointer`
- `X.AsyncKeyboard`
- `X.SyncKeyboard`
- `X.ReplayPointer`
- `X.ReplayKeyboard`
- `X.AsyncBoth`
- `X.SyncBoth`

## サーバー制御

### `grab_server(onerror=None)`
サーバーがアングラブされるまで、他のすべてのクライアント接続での要求処理を無効にします。サーバーグラブは可能な限り避けるべきです。

### `ungrab_server(onerror=None)`
このクライアントによって以前にグラブされていた場合、サーバーを解放します。

## 入力フォーカス

### `set_input_focus(focus, revert_to, time, onerror=None)`
入力フォーカスを `focus` に設定します。`focus` はウィンドウ、`X.PointerRoot`、または `X.NONE` である必要があります。

### `get_input_focus()`
以下の属性を持つオブジェクトを返します：
- `focus` - フォーカスウィンドウ
- `revert_to` - 復帰先

### `query_keymap()`
キーボードの論理状態のビットベクトルを返します。

## フォント関連

### `open_font(name)`
パターン `name` で識別されるフォントを開き、そのフォントオブジェクトを返します。

### `list_fonts(pattern, max_names)`
`pattern` にマッチするフォント名のリストを返します。`max_names` を超えることはありません。

### `list_fonts_with_info(pattern, max_names)`
`pattern` にマッチするフォントのリストを返します。各リスト項目は1つのフォントを表し、以下のプロパティを持ちます：
- `name` - フォント名
- `min_bounds`, `max_bounds` - 境界
- `min_char_or_byte2`, `max_char_or_byte2` - 文字範囲
- `default_char` - デフォルト文字
- `draw_direction` - 描画方向
- `min_byte1`, `max_byte1` - バイト範囲
- `all_chars_exist` - すべての文字が存在するか
- `font_ascent`, `font_descent` - フォントの上昇・下降
- `replies_hint` - 返答ヒント

### `set_font_path(path, onerror=None)`
フォントパスを `path`（文字列のリスト）に設定します。

### `get_font_path()`
現在のフォントパスを文字列のリストとして返します。

## その他のメソッド

### `change_keyboard_mapping(first_keycode, keysyms, onerror=None)`
`first_keycode` から始まるキーボードマッピングを変更します。

### `get_keyboard_mapping(first_keycode, keycode_count)`
現在のキーボードマッピングをタプルのリストとして返します。

### `change_keyboard_control(onerror=None, **keys)`
キーワード引数として提供されたパラメータを変更します。

### `get_keyboard_control()`
以下の属性を持つオブジェクトを返します：
- `global_auto_repeat` - グローバル自動リピート
- `auto_repeats` - 自動リピート
- `led_mask` - LEDマスク
- `key_click_percent` - キークリック音量
- `bell_percent` - ベル音量
- `bell_pitch` - ベル音程
- `bell_duration` - ベル継続時間

### `bell(percent=0, onerror=None)`
ベース音量に対して相対的な音量 `percent` でベルを鳴らします。

### `change_pointer_control(accel=None, threshold=None, onerror=None)`
ポインタ加速度を変更するには、`accel` をタプル `(num, denum)` に設定します。

### `get_pointer_control()`
以下の属性を持つオブジェクトを返します：
- `accel_num`, `accel_denom` - 加速度
- `threshold` - 閾値

### `set_screen_saver(timeout, interval, prefer_blanking, allow_exposures, onerror=None)`
スクリーンセーバーのパラメータを設定します。

### `get_screen_saver()`
以下の属性を持つオブジェクトを返します：
- `timeout` - タイムアウト
- `interval` - 間隔
- `prefer_blanking` - ブランキング設定
- `allow_exposures` - 露出許可

### `change_hosts(mode, host_family, host, onerror=None)`
ホストのアクセス制御リストを変更します。

### `list_hosts()`
以下の属性を持つオブジェクトを返します：
- `mode` - モード
- `hosts` - ホスト一覧

### `set_access_control(mode, onerror=None)`
`mode` が `X.EnableAccess` の場合、接続セットアップ時のアクセス制御リストの使用を有効にします。

### `set_close_down_mode(mode, onerror=None)`
接続終了時にクライアントのリソースに何が起こるかを制御します。

### `force_screen_saver(mode, onerror=None)`
スクリーンセーバーを有効化または無効化します。

### `set_pointer_mapping(map, onerror=None)`
ポインタボタンのマッピングを設定します。

### `get_pointer_mapping()`
ポインタボタンマッピングのリストを返します。

### `set_modifier_mapping(keycodes, onerror=None)`
8つのモディファイアのキーコードを設定します。

### `get_modifier_mapping()`
8つのモディファイアそれぞれに対応する8つのリストを返します。

### `no_operation(onerror=None)`
何もしませんが、サーバーにリクエストを送信します。