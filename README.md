# ActiveStorage + Cloudflare R2 Demo

## Configurations

```
$ bin/rails credentials:edit
cloudflare:
  r2_access_key_id: 00000000000000000000000000000000
  r2_secret_access_key: 0000000000000000000000000000000000000000000000000000000000000000
  r2_account_id: 00000000000000000000000000000000
  r2_bucket: yourbucketname
```

### config/storage.yml

```yaml
cloudflare:
  service: S3
  access_key_id: <%= Rails.application.credentials.dig(:cloudflare, :r2_access_key_id) %>
  secret_access_key: <%= Rails.application.credentials.dig(:cloudflare, :r2_secret_access_key) %>
  endpoint: https://<%= Rails.application.credentials.dig(:cloudflare, :r2_account_id) %>.r2.cloudflarestorage.com
  bucket: <%= Rails.application.credentials.dig(:cloudflare, :r2_bucket) %>
  region: auto
  force_path_style: true
```

### config/environments/development.rb

```yaml
  # Cloudflare R2 を使う
  config.active_storage.service = :cloudflare
  # R2のオブジェクトに署名付き URLでInternetから直接アクセスすることはできないので今回はプロキシモードを使う
  # 本運用では webp 変換などを行う Worker を介してアクセスするように設定する必要がありそう
  config.active_storage.resolve_model_to_route = :rails_storage_proxy
```
