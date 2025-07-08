# 6.6 Window

`Window` オブジェクトは、X Window Systemにおけるウィンドウを表現します。これはDrawableから派生し、描画機能に加えてウィンドウ固有の管理機能を提供します。

## 基本概念

ウィンドウは以下の特徴を持ちます：

- **階層構造**: 親子関係を持つ階層構造
- **可視性**: 表示/非表示の状態管理
- **イベント**: ユーザー操作やシステムイベントの受信
- **属性**: 背景、境界線、カーソルなどの外観属性

## ウィンドウの作成

**`create_window(x, y, width, height, border_width, depth, window_class, visual, **attributes)`**
子ウィンドウを作成します。

```python
# 基本的なウィンドウを作成
child_window = parent_window.create_window(
    50, 50,           # x, y位置
    200, 150,         # 幅, 高さ
    2,               # 境界線の幅
    screen.root_depth, # 深度
    X.InputOutput,    # ウィンドウクラス
    X.CopyFromParent, # ビジュアル
    background_pixel=white_pixel,
    border_pixel=black_pixel,
    event_mask=X.ExposureMask | X.KeyPressMask
)
```

## ウィンドウの表示制御

### マッピング（表示）

**`map()`**
ウィンドウを表示します。

```python
# ウィンドウを表示
window.map()
```

**`unmap()`**
ウィンドウを非表示にします。

```python
# ウィンドウを非表示
window.unmap()
```

**`map_subwindows()`**
すべての子ウィンドウを表示します。

**`unmap_subwindows()`**
すべての子ウィンドウを非表示にします。

### ウィンドウの破棄

**`destroy()`**
ウィンドウを破棄します。

```python
# ウィンドウを破棄
window.destroy()
```

**`destroy_subwindows()`**
すべての子ウィンドウを破棄します。

## ウィンドウの属性管理

**`change_attributes(**attributes)`**
ウィンドウの属性を変更します。

```python
# 背景色を変更
window.change_attributes(background_pixel=red_pixel)

# イベントマスクを変更
window.change_attributes(event_mask=X.KeyPressMask | X.ButtonPressMask)

# カーソルを変更
window.change_attributes(cursor=hand_cursor)
```

**`get_attributes()`**
ウィンドウの現在の属性を取得します。

```python
# 属性を取得
attrs = window.get_attributes()
print(f"クラス: {attrs.class}")
print(f"背景ピクセル: {attrs.background_pixel}")
print(f"イベントマスク: {attrs.your_event_mask}")
```

## ウィンドウの配置とサイズ

**`configure(x=None, y=None, width=None, height=None, border_width=None, sibling=None, stack_mode=None)`**
ウィンドウの配置やサイズを変更します。

```python
# ウィンドウを移動
window.configure(x=100, y=50)

# ウィンドウをリサイズ
window.configure(width=300, height=200)

# 位置とサイズを同時に変更
window.configure(x=0, y=0, width=400, height=300)
```

**`move(x, y)`**
ウィンドウを移動します。

**`resize(width, height)`**
ウィンドウをリサイズします。

**`move_resize(x, y, width, height)`**
移動とリサイズを同時に行います。

## スタッキング順序

**`raise_window()`**
ウィンドウを最前面に移動します。

```python
# ウィンドウを最前面に
window.raise_window()
```

**`lower_window()`**
ウィンドウを最背面に移動します。

**`circulate(direction)`**
子ウィンドウの順序を循環させます。

## フォーカス管理

**`set_input_focus(revert_to, time)`**
このウィンドウに入力フォーカスを設定します。

```python
# フォーカスを設定
window.set_input_focus(X.RevertToParent, X.CurrentTime)
```

## ウィンドウマネージャーとの連携

### ウィンドウマネージャーヒント

**`set_wm_name(name)`**
ウィンドウのタイトルを設定します。

```python
# ウィンドウタイトルを設定
window.set_wm_name("My Application")
```

**`set_wm_icon_name(name)`**
アイコン名を設定します。

**`set_wm_class(instance, cls)`**
ウィンドウクラスを設定します。

```python
# ウィンドウクラスを設定
window.set_wm_class("myapp", "MyApplication")
```

**`set_wm_hints(**hints)`**
ウィンドウマネージャーヒントを設定します。

```python
# ヒントを設定
window.set_wm_hints(
    flags=Xutil.InputHint | Xutil.StateHint,
    input=True,
    initial_state=Xutil.NormalState
)
```

**`set_wm_normal_hints(**hints)`**
サイズに関するヒントを設定します。

```python
# サイズヒントを設定
window.set_wm_normal_hints(
    flags=Xutil.PMinSize | Xutil.PMaxSize,
    min_width=200, min_height=150,
    max_width=800, max_height=600
)
```

## プロパティ管理

**`change_property(property, type, format, data, mode=X.PropModeReplace)`**
ウィンドウプロパティを設定します。

```python
# カスタムプロパティを設定
window.change_property(
    display.intern_atom("MY_PROPERTY"),
    display.intern_atom("STRING"),
    8,  # format
    b"Hello, World!",
    X.PropModeReplace
)
```

**`get_property(property, type, offset, length, delete=False)`**
ウィンドウプロパティを取得します。

**`delete_property(property)`**
ウィンドウプロパティを削除します。

**`list_properties()`**
すべてのプロパティを一覧表示します。

## イベント送信

**`send_event(event, event_mask=0, propagate=0)`**
このウィンドウにイベントを送信します。

```python
# カスタムイベントを送信
event = Xlib.protocol.event.ClientMessage(
    window=window,
    client_type=display.intern_atom("MY_MESSAGE"),
    data=(8, b"Hello")
)
window.send_event(event)
```

## 選択（クリップボード）

**`set_selection_owner(selection, time)`**
選択の所有者になります。

**`convert_selection(selection, target, property, time)`**
選択を変換します。

## ウィンドウツリー操作

**`query_tree()`**
ウィンドウツリー情報を取得します。

```python
# ウィンドウツリーを取得
tree = window.query_tree()
print(f"親: {tree.parent}")
print(f"ルート: {tree.root}")
print(f"子の数: {len(tree.children)}")
```

**`reparent(parent, x, y)`**
ウィンドウを他の親に移動します。

## 実用例

### 基本的なウィンドウアプリケーション

```python
class SimpleWindow:
    def __init__(self, display):
        self.display = display
        self.screen = display.screen()
        
        # メインウィンドウを作成
        self.window = self.screen.root.create_window(
            100, 100, 400, 300,  # x, y, width, height
            2,                    # border_width
            self.screen.root_depth,
            X.InputOutput,
            X.CopyFromParent,
            background_pixel=self.screen.white_pixel,
            border_pixel=self.screen.black_pixel,
            event_mask=(X.ExposureMask | X.KeyPressMask | 
                       X.ButtonPressMask | X.StructureNotifyMask)
        )
        
        # ウィンドウプロパティを設定
        self.setup_window()
        
    def setup_window(self):
        """ウィンドウのプロパティを設定"""
        # タイトルを設定
        self.window.set_wm_name("Simple Window Application")
        self.window.set_wm_icon_name("SimpleApp")
        
        # ウィンドウクラスを設定
        self.window.set_wm_class("simpleapp", "SimpleApp")
        
        # サイズヒントを設定
        self.window.set_wm_normal_hints(
            flags=Xutil.PMinSize | Xutil.PMaxSize,
            min_width=200, min_height=150,
            max_width=800, max_height=600
        )
        
        # WM_DELETE_WINDOW プロトコルを設定
        wm_delete_window = self.display.intern_atom("WM_DELETE_WINDOW")
        wm_protocols = self.display.intern_atom("WM_PROTOCOLS")
        self.window.change_property(wm_protocols, X.ATOM, 32, [wm_delete_window])
    
    def show(self):
        """ウィンドウを表示"""
        self.window.map()
        
    def hide(self):
        """ウィンドウを非表示"""
        self.window.unmap()
        
    def close(self):
        """ウィンドウを閉じる"""
        self.window.destroy()

# 使用例
app = SimpleWindow(display)
app.show()
```

### 複数ウィンドウの管理

```python
class WindowManager:
    def __init__(self, display):
        self.display = display
        self.screen = display.screen()
        self.windows = []
        
    def create_window(self, title, x, y, width, height):
        """新しいウィンドウを作成"""
        window = self.screen.root.create_window(
            x, y, width, height, 2,
            self.screen.root_depth,
            X.InputOutput,
            X.CopyFromParent,
            background_pixel=self.screen.white_pixel,
            border_pixel=self.screen.black_pixel,
            event_mask=X.ExposureMask | X.StructureNotifyMask
        )
        
        window.set_wm_name(title)
        window.map()
        
        self.windows.append(window)
        return window
    
    def tile_windows(self):
        """ウィンドウをタイル状に配置"""
        if not self.windows:
            return
            
        cols = int(len(self.windows) ** 0.5) + 1
        rows = (len(self.windows) + cols - 1) // cols
        
        screen_width = self.screen.width_in_pixels
        screen_height = self.screen.height_in_pixels
        
        win_width = screen_width // cols
        win_height = screen_height // rows
        
        for i, window in enumerate(self.windows):
            col = i % cols
            row = i // cols
            
            x = col * win_width
            y = row * win_height
            
            window.configure(x=x, y=y, width=win_width-10, height=win_height-10)
    
    def close_all(self):
        """すべてのウィンドウを閉じる"""
        for window in self.windows:
            window.destroy()
        self.windows.clear()

# 使用例
wm = WindowManager(display)
wm.create_window("Window 1", 0, 0, 200, 150)
wm.create_window("Window 2", 220, 0, 200, 150)
wm.create_window("Window 3", 0, 170, 200, 150)
wm.tile_windows()
```

## 注意事項

### ウィンドウ階層
- **親子関係**: 親ウィンドウが破棄されると子も破棄される
- **座標系**: 子ウィンドウの座標は親相対
- **可視性**: 親が非表示の場合、子も非表示

### イベント処理
- **イベントマスク**: 必要なイベントのみを選択
- **イベント伝播**: 子から親へのイベント伝播
- **フォーカス**: 適切なフォーカス管理

### ウィンドウマネージャー
- **協調**: ウィンドウマネージャーとの協調が重要
- **ヒント**: 適切なヒントでユーザビリティ向上
- **プロトコル**: 標準プロトコルの遵守

## 関連するXオブジェクト

- **Display**: ウィンドウの作成と管理
- **Screen**: 画面情報とルートウィンドウ
- **GC**: ウィンドウへの描画
- **Colormap**: ウィンドウの色管理
- **Cursor**: ウィンドウのカーソル