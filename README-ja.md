# tiny11builder
*PowerShellでWindows 11の軽量化イメージを作成するスクリプトです。*

[English](README.md) | 日本語

## 概要
Tiny11 builderは、Windows 11の軽量化されたイメージを自動作成するスクリプトです。PowerShellで実装されており、特定のリリースだけでなく、さまざまなWindows 11のリリース、言語、アーキテクチャで利用できます。

Windows ADKの`oscdimg.exe`以外に外部ツールを必要とせず、DISMのRecovery圧縮を使用して最終ISOのサイズを小さくします。付属の無人セットアップファイルにより、OOBEでのMicrosoftアカウント要求を回避し、`/compact`フラグでイメージを展開します。

また、開発・検証用にtiny11 Core builderも提供しています。通常版より多くのコンポーネントを削除しますが、作成後に言語、更新プログラム、機能を追加できないため、通常利用には適していません。

## スクリプトの種類
- **tiny11maker.ps1**: サービス性を維持しながら、多くの不要なアプリや機能を削除します。作成後も言語、更新プログラム、機能を追加できます。通常利用にはこちらを推奨します。
- ⚠️ **tiny11coremaker.ps1**: さらに多くのコンポーネントを削除しますが、作成後のサービス性が失われます。言語、更新プログラム、機能を追加できないため、短時間のテストや開発向けです。

## 使用方法
1. [MicrosoftのWebサイト](https://www.microsoft.com/software-download/windows11)または[Rufus](https://github.com/pbatard/rufus)からWindows 11を入手します。
2. Windows ExplorerでダウンロードしたISOをマウントします。
3. PowerShell 5.1を管理者として開きます。
4. スクリプトの実行ポリシーを変更します。

```powershell
Set-ExecutionPolicy Bypass -Scope Process
```

`-Scope Process`を指定すると、現在のPowerShellセッション中だけ設定が変更され、元のポリシーは維持されます。

5. スクリプトを起動します。

```powershell
C:/path/to/your/tiny11/script.ps1 -ISO <letter> -SCRATCH <letter>
```

`get-help`コマンドでスクリプトのヘルプを確認できます。

6. マウントしたISOのドライブ文字を入力します。コロン（`:`）は不要です。
7. 作成するイメージのSKUを選択します。
8. 処理が完了するまで待ちます。
9. 完了したISOは、スクリプトを展開したフォルダー内に`tiny11.iso`として作成されます。

## 削除されるもの
<table>
  <tbody>
    <tr>
      <th>Tiny11maker</th>
      <th>Tiny11coremaker</th>
    </tr>
    <tr>
      <td>
        <ul>
          <li>Clipchamp</li>
          <li>News</li>
          <li>Weather</li>
          <li>Xbox</li>
          <li>Get Help</li>
          <li>Get Started</li>
          <li>Office Hub</li>
          <li>Microsoft Sticky Notes</li>
          <li>Microsoft 3D Viewer</li>
          <li>Mixed Reality Portal</li>
          <li>OneNote</li>
          <li>Paint</li>
          <li>Skype</li>
          <li>Wallet</li>
          <li>Camera</li>
          <li>Windows Terminal</li>
          <li>Microsoft Family</li>
          <li>Dolby Access</li>
          <li>Copilot</li>
          <li>New Outlook</li>
          <li>Microsoft Teams</li>
          <li>Dev Home</li>
          <li>Photos</li>
          <li>Windows Backup</li>
          <li>Widgets</li>
          <li>Windows Web Experienceパッケージ</li>
          <li>Solitaire</li>
          <li>People</li>
          <li>Power Automate</li>
          <li>To Do</li>
          <li>Alarms</li>
          <li>Mail and Calendar</li>
          <li>Feedback Hub</li>
          <li>Maps</li>
          <li>Sound Recorder</li>
          <li>Your Phone</li>
          <li>Media Player</li>
          <li>Quick Assist</li>
          <li>Internet Explorer</li>
          <li>Tablet PC Math</li>
          <li>Edge</li>
          <li>OneDrive</li>
          <li>Windows Spotlight機能</li>
          <li>検索のWeb・クラウド検索結果</li>
          <li>アプリの音声起動</li>
          <li>Fax and Scan、XPS Viewer、Internet Printing Client</li>
        </ul>
      </td>
      <td>
        <ul>
          <li>通常版の削除対象すべて</li>
          <li>Windows Component Store（WinSxS）</li>
          <li>Windows Defender（無効化のみ。必要に応じて再有効化可能）</li>
          <li>Windows Update（WinSxSがないため動作せず、有効化するとシステム障害につながります）</li>
          <li>WinRE</li>
        </ul>
      </td>
    </tr>
  </tbody>
</table>

## Tiny11makerで無効化されるもの
- すべてのユーザーの広告ID
- Windows Spotlight、スポンサーアプリ、その他のコンシューマー向けクラウドコンテンツ
- 検索のハイライト、Web検索、クラウド検索結果
- アプリの音声起動（ロック画面上での起動を含む）
- Widgets / News and Interests
- テレメトリ、Windows Error Reporting、ユーザーアクティビティ履歴、Delivery OptimizationのP2P共有
- Copilot、Recall、Click to Do、AIデータ分析、スナップショット保存、AIエージェントのコネクター、アプリからのAIモデルアクセス
- Edge、メモ帳、PaintのAI機能
- OneDriveフォルダーのバックアップ、Windows Chatのタスクバーアイコン、予約済み記憶域、BitLockerデバイス暗号化
- `DiagTrack`、`WerSvc`、`dmwappushservice`、`WSAIFabricSvc`、`RemoteRegistry`、`MapsBroker`サービス

Windows Update自体は無効化されません。無効化されるのはDelivery OptimizationによるP2P共有だけです。

## Tiny11makerで削除されるスケジュールタスク
- Windows AI / Recall関連タスク
- Application Compatibility Appraiser、Program Data Updater
- Customer Experience Improvement Programのタスク
- Windows Error Reportingのキュー、Chkdsk Proxyのタスク

⚠️ **tiny11 Coreでは、削除した機能を後から追加できません。**<br>
イメージ作成中に.NET Framework 3.5のサポートを有効にするか選択できます。

## 既知の問題
- Edgeを削除しても設定画面に一部の残骸が表示されることがありますが、アプリ本体は削除されています。
- アプリをインストールする前に、Microsoft Storeを使用してWingetを更新する必要がある場合があります。
- OutlookとDev Homeが時間の経過後に再表示されることがあります。これは継続的に対策中で、現在のスクリプトでは再インストールをより強く防止します。
- arm64で使用すると、実行中に一瞬エラーが表示されることがあります。arm64イメージのSystem32フォルダーにOneDriveSetup.exeが含まれていないことが原因です。

## 今後実装予定の機能
- ~~テレメトリの無効化~~（2024-04-29リリースで実装済み）
- ~~広告のさらなる抑制~~（2025-09-06リリースで実装済み）
- 言語とアーキテクチャの検出改善
- 保持・削除対象をより柔軟に指定する機能
- GUI

## プロジェクトの支援
このプロジェクトが役立った場合は、開発を継続するための支援を検討してください。

**[Patreon](http://patreon.com/ntdev) | [PayPal](http://paypal.me/ntdev2) | [Ko-fi](http://ko-fi.com/ntdev)**

ご利用いただきありがとうございます。フィードバックもお待ちしています。
