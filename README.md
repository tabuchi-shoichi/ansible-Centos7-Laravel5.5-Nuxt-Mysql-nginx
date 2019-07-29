# インストールされるコンポーネントとバージョン
このplaybookはCentOS7に以下の環境を構築します。

- Laravel 5.5.x
- PHP 7.2.x
- Composer latest
- nginx latest
- Mysql 5.6.44
- Node.js 10.16.0
- Nuxt.js 2.8.1
- Java 8.0.x (オプション)
- Elasticsearch 6.x (オプション)
- その他ツール(git, wget, zip, unzip, gcc-c++, make)

# 動作環境
- ホスト
  - ansible 2.8

- 対象サーバー
  - CentOS7
  - 必須空きメモリ領域：1GByte
    - Elasticsearchをインストールする場合は4GByte
  - 推奨空きメモリ領域：1.5GByte

メモリを最低でも1GB以上確保してください。  
512MB程度だとnpm insatall、Nuxtのbuild等が実行できない可能性があります。  
Elasticsearchをインストールする場合は最低でも4GB以上確保してください。

# 使用例
## 設定
`hosts.example`を`hosts`にリネームしてください。  
`group_vars/all/config.yml.example`を`group_vars/all/config.yml`にリネームしてください。

使用の前に以下2つのファイルを編集してください。

- hosts
  - プロビジョニングを行う対象サーバーのhostを指定します。
- group_vars/all/config.yml
  - ファイル内のコメントに沿って編集してください。


## Nuxtの起動
Nuxtをdevelopment modeで実行する場合は

```
ssh user@host "cd {{ nuxt_dir }}/{{ nuxt_project_project_name }} && sudo npm run dev"

例：ssh user@123.456.78.9 "cd /var/www/sample_project/nuxt_project_skeleton_for_ansible && sudo npm run dev"
```

Nuxtをproduction modeで実行する場合は

```
ssh user@host "cd {{ nuxt_dir }}/{{ nuxt_project_project_name }} && sudo npm run build && sudo npm run start"
```

pm2を使用してNuxtをデーモン化してproduction modeで実行する場合は
```
ssh user@host "sudo npm i -g pm2"
ssh user@host "cd {{ nuxt_dir }}/{{ nuxt_project_project_name }} && sudo npm run build && sudo pm2 start npm --name "{{ nuxt_project_project_name }}" -- start"
```

## NuxtとLaravelのエンドポイント
- Nuxt
  - /
- Laravel
  - /api/v1

エンドポイントを変更したい場合は`roles/nginx/templates/laravel.cnf.j2`を編集してください。