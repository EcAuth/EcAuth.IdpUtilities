# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## プロジェクト概要

EcAuth Identity Providerで使用される共通ユーティリティライブラリです。NuGetパッケージとしてGitHub Packagesで配布されます。

## プロジェクト構造

```
EcAuth.IdpUtilities/
├── src/
│   └── EcAuth.IdpUtilities/        # メインライブラリプロジェクト
│       ├── EmailHashUtil.cs        # メールアドレスハッシュ化
│       ├── Iron.cs                 # Stateパラメータ暗号化（Iron仕様実装）
│       ├── RandomUtil.cs           # セキュアランダム文字列生成
│       └── Migrations/             # EF Core マイグレーション用ヘルパー
├── tests/
│   └── EcAuth.IdpUtilities.Test/   # xUnit ユニットテスト
│       ├── EmailHashUtilTests.cs
│       ├── IronTests.cs
│       └── RandomUtil.cs
├── .github/
│   └── workflows/
│       ├── ci.yml                  # CI (ビルド・テスト)
│       └── publish-nuget.yml       # NuGet パッケージ公開
├── EcAuth.IdpUtilities.sln
├── README.md
├── CLAUDE.md
└── LICENSE
```

## 開発コマンド

### ビルド

```bash
# ソリューション全体のビルド
dotnet build EcAuth.IdpUtilities.sln

# Releaseビルド
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

2. GitHub Actionsワークフロー（`publish-nuget.yml`）が自動的に実行され、GitHub Packagesに公開されます。

### ローカルからの公開（手動）

```bash
# GitHub Packages に公開
dotnet nuget push ./artifacts/EcAuth.IdpUtilities.1.0.0.nupkg \
  --source https://nuget.pkg.github.com/EcAuth/index.json \
  --api-key YOUR_GITHUB_PAT
```

## バージョニング

- セマンティックバージョニング（SemVer）を使用
- `src/EcAuth.IdpUtilities/EcAuth.IdpUtilities.csproj` の `<Version>` タグを更新
- リリース時は対応するGitタグを作成（例: `v1.0.0`）

## 依存関係

- .NET 9.0
- DotNetEnv (3.1.1)
- Microsoft.EntityFrameworkCore.SqlServer (9.0.9)
- System.Text.Json (9.0.9)
- System.IdentityModel.Tokens.Jwt (8.14.0)
- Microsoft.IdentityModel.Tokens (8.14.0)

## コーディング規約

- 行末の空白は削除
- 改行コードはLF
- Null許容参照型を有効化（`<Nullable>enable</Nullable>`）
- 暗黙的usingsを有効化（`<ImplicitUsings>enable</ImplicitUsings>`）

## CI/CD

### GitHub Actions ワークフロー

- **CI (`.github/workflows/ci.yml`)**:
  - PR作成時に自動実行
  - ビルド・テストの成功を確認

- **NuGet Publish (`.github/workflows/publish-nuget.yml`)**:
  - タグプッシュ時（`v*`）に自動実行
  - GitHub Packagesにパッケージを公開

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

### NuGet パッケージが見つからない

GitHub PackagesからのNuGetパッケージ取得には認証が必要です。`nuget.config`を確認してください。

## 関連リポジトリ

- [EcAuth](https://github.com/EcAuth/EcAuth) - メインIdentity Providerアプリケーション
- [EcAuth.MockIdP](https://github.com/EcAuth/EcAuth.MockIdP) - E2Eテスト用モックIdP
- [ecauth-infrastructure](https://github.com/EcAuth/ecauth-infrastructure) - IaC（Terraform + Ansible）
