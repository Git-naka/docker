# dockerの環境構築の手順を記載します
## 1_Dockerデスクトップのインストール  

<Step1>以下からDockerデスクトップのインストーラを取得する  

　　Docker Desktop Installer.exe  
　　　https://docs.docker.com/desktop/install/windows-install/  

<Step2>インストーラを実行してセットアップする  
　　※起動時に無償、有償の説明が表示される場合あり。個人使用は無償のため、無償を選択する。（2022年8月時点）  

<Step3>Windows(ホストOS)の設定を実施する  
  　　・WSLと「仮想マシンプラットフォーム」を有効化する
![image](https://user-images.githubusercontent.com/115159924/194710488-96ddfb7c-27d7-4bcf-8c11-ae8a58a8fd1c.png)

  　　・Wls2を使用できる設定をする
    　　　以下のサイトからインストーラを取得する  
       
       wsl_update_x64.msi
       https://docs.microsoft.com/ja-jp/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package  
       
<Step4>

## 2_Docker環境でPostgres(PostGIS)をセットアップ  
