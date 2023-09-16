# プロジェクト名

- Docker + PHP + Laravel + Ngix + MySQL 環境
- 例）Google Mapの口コミ投稿を促すSMS送信ツール

# 初期起動手順

1. 空のプロジェクトフォルダ内で「git clone {docker-laravel-env}」を行う
1. 階層が1つ下のはずなので、全切り取り貼り付けで一階層上に持ってくる
1. 「.git」フォルダを削除
1. gitを初期化して、コミット、プッシュする
1. 「fixme」で検索して、プロジェクト毎に適宜修正
1. .env.devをコピーして、.envを作成する
1. SSL関連ファイル生成
    - http://friendspring.starfree.jp/https/
1. Docker起動
    - docker-compose up -d --build
1. appコンテナにはいる
    - docker-compose exec app bash
1. /work にいることを確認
1. composer create-project --prefer-dist laravel/laravel {プロジェクト名:myapp}
    - myappにしない場合は、各所を修正
1. https://localhost:8000 でLaravel初期画面が表示されればOK
1. project配下の所有者を変更（ファイル変更権限がない場合）
    - コンテナに入っている場合は、exitで抜け、wsl上に戻る。
    - git cloneした直下のファイル群の所有者を確認しておく（ls -lh）
    - cd project/myapp
    - sudo chown -R hoge:hoge .
        - ※wslのユーザーでOK（確認したユーザー:グループ）
        - （グループの確認コマンド「groups hoge」）
1. app/storage 配下の権限変更
    - chmod -R 777 app/storage

# ログイン機能付与手順
1. Breezeをインストール
    - composer require laravel/breeze --dev
1. php artisan breeze:install
1. php artisan migrate
1. npm install
1. npm run build
1. Log in, Sign upリンクを押して、画面が表示されればOK

# サーバー設置手順
1. SSL関連ファイル生成
    - http://friendspring.starfree.jp/https/
        - httpでアクセスしたい場合は、nginx/certs/配下に空ファイルを作成
1. Docker環境ファイルを作成
    - .env.dev -> .env
    - WEB_PORTを443に変更
        - httpでアクセスしたい場合は80に変更
    - DB_PASSを適宜変更
1. Docker起動
    - docker-compose up -d --build
1. コンテナに入る
    - docker-compose exec app bash
1. cd /work/myapp
1. Laravel環境ファイルを作成
    - .env.example -> .env
    - LOG_CHANNEL=daily
    - APP_NAME=アクセスログ解析
    - APP_ENV=prod
1. composer install
1. php artisan key:generate
1. npm install
1. npm run build
1. php artisan migrate
1. (php artisan db:seed)
1. (php artisan storage:link)
1. sudo chmod 777 -Rf storage
    - もっと早い段階の方が良いかも
1. logrotate.confのパス調整と配置


# リリース時注意事項

・jsファイルを修正した場合、呼出箇所の日時パラメータを更新してキャッシュクリアを強制する

