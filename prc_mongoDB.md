# mongoDBを実際に動かしてみる

## 目次

- [mongoDBの基本コマンド]
- [indexとは]
- [Replica Setとは]

## mongoDBの基本コマンド

[参考情報](https://qiita.com/airkoda/items/a88554644f040a627769)

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

- Databaseの作成
    ```shell
    test> use myDatabase
    switched to db myDatabase
    myDatabase> 
    ```
    作成と同時に操作対象のDBが切り替わる  

- Collectionの作成
    ```shell
    myDatabase> db.myCollection.insert({name: "John", age: 30, city: "New York"})

    DeprecationWarning: Collection.insert() is deprecated. Use insertOne, insertMany, or bulkWrite.
    {
      acknowledged: true,
      insertedIds: { '0': ObjectId('6690be112a97086d4e482f8b') }
    }
    ```
    これでmyCollectionという名前のコレクションが作成され、その中に一つのドキュメントが挿入される  

- Documentの検索
    ```shell
    myDatabase> db.myCollection.find({name: "John"})
    
    [
      {
        _id: ObjectId('6690be112a97086d4e482f8b'),
        name: 'John',
        age: 30,
        city: 'New York'
      }
    ]
    ```
    ObjectIdというのはコレクションの中で一意に必ず割り振られるID  
    ObjectIdが一意であるため、ドキュメントが被っても登録される  
    ```shell
    myDatabase> db.myCollection.find({name: "John"})
    [
        {
            _id: ObjectId('6690cf4138fd7a7beb482f8b'),
            name: 'John',
            age: 30,
            city: 'New York'
        },
        {
            _id: ObjectId('6690cf4638fd7a7beb482f8c'),
            name: 'John',
            age: 31,
            city: 'New York'
        },
        {
            _id: ObjectId('6690cf6238fd7a7beb482f8d'),
            name: 'John',
            age: 31,
            city: 'New York'
        }
    ]
    ```

- Documentの更新
    ```shell
    myDatabase> db.myCollection.update({name: "John"}, {$set: {age: 31}})

    DeprecationWarning: Collection.update() is deprecated. Use updateOne, updateMany, or bulkWrite.
    {
        acknowledged: true,
        insertedId: null,
        matchedCount: 1,
        modifiedCount: 1,
        upsertedCount: 0
    }
    ```

- Documentの削除
    ```shell
    myDatabase> db.myCollection.remove({name: "John"})

    DeprecationWarning: Collection.remove() is deprecated. Use deleteOne, deleteMany, findOneAndDelete, or bulkWrite.
    { acknowledged: true, deletedCount: 1 }
    ```

## RDBMS用語との比較

|RDBMS用語|MongoDB用語|
|-|-|
|database|database|
|table|collection|
|record(row)|document|
|column|field|

## indexとは

クエリの実行を効率化できるもの  

indexの作成方法
```
db.collection.createIndex( {<インデックスに指定したいキー名>: <昇降順>} )
```
<昇降>

birth yearをindexとした場合
```
db.friends.find({birth year: '1999'})
```
これで検索は早くなる

## 