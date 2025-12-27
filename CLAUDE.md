# CLAUDE.md

このファイルは Claude Code (claude.ai/code) がこのリポジトリで作業する際のガイダンスを提供します。

## プロジェクト概要

EcAuth Identity Provider で使用される共通ユーティリティライブラリです。NuGet パッケージとして GitHub Packages で配布されます。

## 関連リポジトリ

| リポジトリ                                                               | 説明                                    |
|--------------------------------------------------------------------------|-----------------------------------------|
| [EcAuth](https://github.com/EcAuth/EcAuth)                               | IdentityProvider メインアプリケーション |
| [EcAuth.MockIdP](https://github.com/EcAuth/EcAuth.MockIdP)               | E2E テスト用モック IdP                  |
| [ecauth-infrastructure](https://github.com/EcAuth/ecauth-infrastructure) | IaC（Terraform + Ansible）              |
| [EcAuthDocs](https://github.com/EcAuth/EcAuthDocs)                       | 設計ドキュメント                        |

## プロジェクト構造

```
EcAuth.IdpUtilities/
├── src/EcAuth.IdpUtilities/        # メインライブラリプロジェクト
│   ├── EmailHashUtil.cs            # メールアドレスハッシュ化
│   ├── Iron.cs                     # State パラメータ暗号化（Iron 仕様実装）
│   ├── RandomUtil.cs               # セキュアランダム文字列生成
│   └── Migrations/                 # EF Core マイグレーション用ヘルパー
├── tests/EcAuth.IdpUtilities.Test/ # xUnit ユニットテスト
└── .github/workflows/
    ├── ci.yml                      # CI (ビルド・テスト)
    └── publish-nuget.yml           # NuGet パッケージ公開
```

## 開発コマンド

### ビルド

```bash
# ソリューション全体のビルド
dotnet build EcAuth.IdpUtilities.sln

# Release ビルド
dotnet build --configuration Release
```

### テスト

```bash
# すべてのテスト実行
dotnet test

# 詳細出力
dotnet test --verbosity normal

# 特定のテストクラス実行
dotnet test --filter ClassName=IronTests
```

### NuGet パッケージング

```bash
# パッケージ作成（ビルド時に自動生成）
dotnet build --configuration Release

# 手動でパッケージ作成
dotnet pack src/EcAuth.IdpUtilities/EcAuth.IdpUtilities.csproj --configuration Release --output ./artifacts
```

## NuGet パッケージ公開

### GitHub Packages への公開

1. タグを作成してプッシュ：
   ```bash
   git tag v1.0.0
   git push origin v1.0.0
   ```

2. GitHub Actions ワークフロー（`publish-nuget.yml`）が自動的に実行され、GitHub Packages に公開されます。

### バージョニング

- セマンティックバージョニング（SemVer）を使用
- `src/EcAuth.IdpUtilities/EcAuth.IdpUtilities.csproj` の `<Version>` タグを更新
- リリース時は対応する Git タグを作成（例: `v1.0.0`）

## 依存関係

- .NET 9.0
- DotNetEnv (3.1.1)
- Microsoft.EntityFrameworkCore.SqlServer (9.0.x)
- System.Text.Json (9.0.x)
- System.IdentityModel.Tokens.Jwt (8.x)
- Microsoft.IdentityModel.Tokens (8.x)

## コーディング規約

- 行末の空白は削除
- 改行コードは LF
- Null 許容参照型を有効化（`<Nullable>enable</Nullable>`）
- 暗黙的 usings を有効化（`<ImplicitUsings>enable</ImplicitUsings>`）

## CI/CD

### GitHub Actions ワークフロー

- **CI (`.github/workflows/ci.yml`)**: PR 作成時に自動実行、ビルド・テストの成功を確認
- **NuGet Publish (`.github/workflows/publish-nuget.yml`)**: タグプッシュ時（`v*`）に自動実行、GitHub Packages にパッケージを公開

## トラブルシューティング

### ビルド失敗

```bash
# キャッシュクリア
dotnet clean
dotnet restore

# 再ビルド
dotnet build
```

### テスト失敗

```bash
# 特定のテストを実行して詳細確認
dotnet test --filter FullyQualifiedName~EmailHashUtilTests --verbosity detailed
```
