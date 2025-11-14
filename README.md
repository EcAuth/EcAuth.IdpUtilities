# EcAuth.IdpUtilities

Common utilities library for EcAuth Identity Provider.

## 概要

EcAuth.IdpUtilitiesは、EcAuth Identity Providerで使用される共通ユーティリティライブラリです。以下の機能を提供します：

- **Email hashing** (`EmailHashUtil`): メールアドレスのSHA-256ハッシュ化
- **Iron encryption** (`Iron`): Stateパラメータの署名付き暗号化（Iron仕様実装）
- **Random string generation** (`RandomUtil`): セキュアランダム文字列生成

## インストール

### NuGet Package Manager

```bash
dotnet add package EcAuth.IdpUtilities
```

### Package Manager Console

```powershell
Install-Package EcAuth.IdpUtilities
```

### GitHub Packages

このパッケージはGitHub Packagesで配布されています。`nuget.config`に以下の設定を追加してください：

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <add key="github" value="https://nuget.pkg.github.com/EcAuth/index.json" />
  </packageSources>
  <packageSourceCredentials>
    <github>
      <add key="Username" value="YOUR_GITHUB_USERNAME" />
      <add key="ClearTextPassword" value="YOUR_GITHUB_PAT" />
    </github>
  </packageSourceCredentials>
</configuration>
```

## 使用方法

### EmailHashUtil

メールアドレスをSHA-256でハッシュ化します。

```csharp
using EcAuth.IdpUtilities;

string email = "user@example.com";
string hash = EmailHashUtil.HashEmail(email);
// 結果: "b4c9a289323b21a01c3e940f150eb9b8c542587f1abfd8f0e1cc1ffc5e475514"
```

### Iron (Stateパラメータ暗号化)

OAuth2のStateパラメータなどを暗号化・復号化します。

```csharp
using EcAuth.IdpUtilities;

// 暗号化
var data = new { userId = "123", timestamp = DateTime.UtcNow };
string password = "your-secret-password";
string sealed = Iron.Seal(data, password);

// 復号化
var unsealed = Iron.Unseal<MyDataType>(sealed, password);
```

### RandomUtil

セキュアなランダム文字列を生成します。

```csharp
using EcAuth.IdpUtilities;

// 32バイト（256ビット）のランダム文字列
string randomString = RandomUtil.GenerateRandomString(32);
```

## 開発

### 必要な環境

- .NET 9.0 SDK

### ビルド

```bash
dotnet build EcAuth.IdpUtilities.sln
```

### テスト

```bash
dotnet test
```

## ライセンス

MIT License

## リンク

- [GitHub Repository](https://github.com/EcAuth/EcAuth.IdpUtilities)
- [EcAuth Main Repository](https://github.com/EcAuth/EcAuth)
