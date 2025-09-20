# OneDriveExcelBatchWriter

## プロジェクト概要
このプロジェクトは、Microsoft Graph APIを使用してOneDriveにExcelファイルをアップロードし、そのExcelファイルに九九表（9x9）のデータを入力するC#コンソールアプリケーションです。Microsoft Entra ID（旧Azure AD）を使用した認証を実装しています。

## 必要なセットアップ

### 1. Microsoft Entra IDでのアプリ登録
1. [Microsoft Entra IDポータル](https://entra.microsoft.com/)にアクセスします。
2. **アプリの登録**を選択し、新しいアプリケーションを登録します。
3. アプリケーションの**クライアントID**と**テナントID**を取得します。
4. **APIのアクセス許可**で以下の権限を追加します（すべて委任済みの権限）：
   - `Files.ReadWrite`
   - `User.Read`
5. **認証**タブでリダイレクトURIを設定します（例: `http://localhost`）。

### 2. `appsettings.json`の設定
プロジェクトルートにある`appsettings.sample.json`をコピーし、`appsettings.json`にリネームしてください。その後、以下の情報を記載してください：
```json
{
  "clientId": "<Your-Client-ID>",
  "tenantId": "<Your-Tenant-ID>"
}
```

### 3. 必要な依存関係のインストール
以下のコマンドを実行して、必要なNuGetパッケージをインストールします：
```bash
dotnet restore
```

### 4. 認証設定の追加
1. **認証タブ**で、**プラットフォームの追加**を選択します。
2. **モバイル アプリケーションとデスクトップ アプリケーション**を選択します。
3. リダイレクトURIとして以下を追加します：
   ```
   https://login.microsoftonline.com/common/oauth2/nativeclient
   ```
4. **パブリック クライアント フローを許可する**をONにし、**次のモバイルとデスクトップのフローを有効にする**を有効化します。

## 使用方法
1. プロジェクトをビルドします：
   ```bash
   dotnet build
   ```
2. プログラムを実行します：
   ```bash
   dotnet run
   ```
3. 実行後、OneDriveにExcelファイルがアップロードされ、九九表が入力されます。

## 使用例

以下は、このプログラムを実行した際の出力例です：

```plaintext
PS C:\work\Dev01\OneDriveExcelBatchWriter> dotnet run
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code ABCD-EFGH to authenticate.
GraphAPI認証成功
アップロード完了: https://example-my.sharepoint.com/personal/user_example_com/_layouts/15/Doc.aspx?sourcedoc=%7BEXAMPLE-DOC-ID%7D&file=20250920153452.xlsx&action=default&mobileredirect=true
セルへの書き込み完了
一時ファイル削除完了
```

### 認証について
プログラムを実行すると、以下のようなメッセージが表示されます：

```plaintext
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code ABCD-EFGH to authenticate.
```

この指示に従い、ブラウザで`https://microsoft.com/devicelogin`にアクセスし、表示されたコード（例: `ABCD-EFGH`）を入力して認証を完了してください。

## 注意点
- `appsettings.json`には機密情報が含まれるため、GitHubなどの公開リポジトリにアップロードしないでください。
- 必要に応じて`.gitignore`に`appsettings.json`を追加してください。
- Microsoft Entra IDの設定が正しくない場合、認証エラーが発生します。
- **アップロード先はOneDriveのルートディレクトリ直下になります。必要に応じてプログラムを修正してください。**

## ライセンス
このプロジェクトはMITライセンスの下で提供されています。