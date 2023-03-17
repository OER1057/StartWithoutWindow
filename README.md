# StartWithoutWindow

CUI/GUIアプリケーションをウィンドウを表示せずに起動するアプリケーションです。バックグラウンドで動作させるアプリケーションを邪魔にならないようにできます。なお、同様のことは少々面倒ですがPowerShellでも可能です。これは少々の面倒を回避するためのものです。

## 使用法

### ランタイムのインストール

[Microsoft .NET Runtime 7.0](https://dotnet.microsoft.com/ja-jp/download/dotnet/7.0)がインストールされていない場合はインストールしてください。

`winget list --id Microsoft.DotNet`を実行し、`Microsoft.DotNet.Runtime.7`または`Microsoft.DotNet.DesktopRuntime.7`があればインストール済みです。

`winget install Microsoft.DotNet.Runtime.7`でインストールできます。

### コマンド

```
StartWithoutWindow.exe "パス" 引数
```

- `パス` : 非表示で実行したい実行ファイルへのパスです。絶対パス、または**`StartWithoutWindow.exe`に対する**相対パスで指定できます。つまり、`StartWithoutWindow.exe`を目的の実行ファイルと同じ場所に置くことで、実行ファイル名のみの指定で済みます。空白が含まれる場合は`"`でくくるなどして一つの文字列としてください。
- `引数` : 元のアプリケーションに渡すときと全く同じ形で渡すことができます。つまり、全体を`"`で囲ったりする必要はありません。

### スタートアップ登録

バックグラウンドで動作させるアプリケーションは、ログインと同時に起動してほしいことが多いと思われます。以下はその設定をする手順です。

0. 正しく動作するコマンドを確認しておく
1. `ファイル名を指定して実行`かエクスプローラのアドレスバーに`shell:startup`と入力し、スタートアップフォルダを開く。
2. 右クリック→新規作成→ショートカット
3. 項目の場所に`"path\to\StartWithoutWindow.exe" "パス" 引数`を入力し進める。

## 注意点

アプリケーションを起動できたかどうかを通知する手段がないので、タスクマネージャなどで確認し、コマンドが正しいことを確認してください。また、この方法で起動したアプリケーションには終了するためのUIがないので、タスクマネージャで探し出して強制終了するなどしてください。

## 使用例・PowerShellとの比較

ショートカット登録する際、PowerShellを使って

```
C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe -Command "Start-Process -WindowStyle Hidden 'D:\Program Files\OBSAutoReplayBuffer\OBSAutoReplayBuffer.exe' 'aces stormworks64'"
```

と書いていたものは、目的の実行ファイルと同じディレクトリに`StartWithoutWindow.exe`を配置すると、

```
"D:\Program Files\OBSAutoReplayBuffer\StartWithoutWindow.exe" OBSAutoReplayBuffer.exe aces stormworks64
```

と書けるようになります。

また、PowerShellでは一瞬PowerShell自体のウィンドウが表示されてしまいますが、`StartWithoutWindow.exe`は自身のウィンドウも表示しません。