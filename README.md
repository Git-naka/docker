# dockerの環境構築の手順を記載します
## 1_Dockerデスクトップのインストール  

***Step1*** 以下からDockerデスクトップのインストーラを取得する  

　　Docker Desktop Installer.exe  
　　　https://docs.docker.com/desktop/install/windows-install/  

***Step2*** インストーラを実行してセットアップする  
　　※起動時に無償、有償の説明が表示される場合あり。個人使用は無償のため、無償を選択する。（2022年8月時点）  

***Step3*** Windows(ホストOS)の設定を実施する  
  　　・WSLと「仮想マシンプラットフォーム」を有効化する
![image](https://user-images.githubusercontent.com/115159924/194710488-96ddfb7c-27d7-4bcf-8c11-ae8a58a8fd1c.png)

  　　・Wls2を使用できる設定をする  
    　　　以下のサイトからインストーラを取得する  
       
　　wsl_update_x64.msi  
　　　https://docs.microsoft.com/ja-jp/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package  

    　　　デフォルトでWls2を使用する設定をする

![image](https://user-images.githubusercontent.com/115159924/194710730-e2324647-adda-4f10-9b2d-9e7a80a34075.png)  


## 2_Docker環境でPostgres(PostGIS)をセットアップ  

docker環境の構築に必要なファイル  

　 docker-compose.yml  
　 create-tables.sql  
　 insert-data.sql  

以下のようなフォルダ構成を作成する  
______________________________________________________________________________________________________________  
├── docker  
│   └── db  
│       └── data（※自動作成のため作成不要）  
│       └── sql  
│           ├── create-tables.sql  
│           └── insert-data.sql  
├── docker-compose.yml  

______________________________________________________________________________________________________________  

以下のコマンドでコンテナの作成と起動  
　`docker-compose up -d`  

以下のコマンドでコンテナの中（バッシュ環境）に入る  
　`docker-compose exec db bash`  

以下のコマンドでテスト用仮テーブルと仮データを作成する  
　`psql -U docker -d postgre < "/docker-entrypoint-initdb.d/create-tables.sql"`  
　`psql -U docker -d postgre < "/docker-entrypoint-initdb.d/insert-data.sql"`  

以下のコマンドでpostgresに入り、postgisが適用されているか確認する  
　`psql -U docker -d postgre`  
　`\dx`  

以下のコマンドでテスト用仮テーブルを表示できるか確認する  
　`select * from test_table;`  

## 失敗例  
その1　create-tables.sqlを実行する際に、「No such file or directory」と表示されて実行できない  
　→　準備したフォルダ構成のファイル名と、ymlファイルに指定したvolumeのパスが異なる  
 　　その状態で一度コンテナを開始してしまうと間違ったフォルダ名を記憶してしまい、後からフォルダ名を正しい名前に変更してもそのフォルダを読みこまず実行できないようだった  
   　`docker-compose down`でコンテナを削除して、再度`docker-compose up -d`からやり直すと解決した  

その2　Docker環境でPostgres(PostGIS)をセットアップできたか確認し、postgisが適応されてテスト用仮テーブルが表示できることを確認したが、後日テスト用仮テーブルが表示できなくなった  
　→　dockerのフォルダ構成を全体的にコピーしてから表示されなくなったため、これが原因かと考えている  
 　　新しくフォルダを作成し、まっさらな状態でdocker環境を再構築したら解決した  
