# mongoDBを実際に動かしてみる

## 目次

- [mongoDBの基本コマンド]


## mongoDBの基本コマンド

- mongoDBのバージョン確認
    ```shell
    $ mongod --version
    db version v5.0.27
    Build Info: {
        "version": "5.0.27",
        "gitVersion": "49571988f1fea870e803f71a3ef8417173f3fbb1",
        "openSSLVersion": "OpenSSL 1.1.1f  31 Mar 2020",
        "modules": [],
        "allocator": "tcmalloc",
        "environment": {
            "distmod": "ubuntu2004",
            "distarch": "x86_64",
            "target_arch": "x86_64"
        }
    }
    ```

    mongoDBを操作するために、mongoDBを動かすためのシェアがある。  
    mongoshというが、このシェルのバージョン確認するコマンド  
    ```shell
    $ mongosh --veriosn
    2.2.12
    ```

- mongoDBを使うために、mongoDBを起動
    ```shell
    sudo systemctl start mongod
    ```

- mongoDBを使うために、mongoshを起動
    ```shell
    $ mongosh
    ・・・・・
    ・・・・・
    test> 
    ```
    「test」というのはデフォルトのデータベースのこと  

- Databaseの一覧を表示  
    ```shell 
    test> show dbs
    admin       40.00 KiB
    config      12.00 KiB
    local       40.00 KiB
    ```
    ※testというDBが表示されないのは、show dbsで表示されるデータベースはコレクションが存在するもののみが表示されるようになっているため  




https://qiita.com/airkoda/items/a88554644f040a627769