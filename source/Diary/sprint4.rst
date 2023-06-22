====================
Sprint4(～6/21)
====================

期間
====================
6/7(水）～6/21（水）


本スプリントの報告
====================

進捗状況
---------
.. list-table:: タスク一覧
    :widths: 5 10 40
    :header-rows: 1

    * - No
      - 実施内容
      - 説明
    * - 1
      - AWS仕様調査
      - App Mech、ALB、ECSの調査
    * - 2
      - CI/CD周りの調査
      - Github actionsの調査、CICDの考え方など、

1.AWS仕様調査
------------------
1. | ECSの仕様調査
   | :doc:`../AWSResearch/ecs`
2. | AWS App Meshの仕様調査
   | :doc:`../AWSResearch/awsAppMesh`
3. | aws ALBの仕様調査
   | :doc:`../AWSResearch/awsALB` 

2.CI/CD周りの調査
------------------
1. | Github actionsの調査
   | :doc:`../CICD/gitHubActions`
2. | CICDの考え方
   | :doc:`../CICD/cicd`


次スプリントの予定
====================
実施予定
------------------
.. list-table:: タスク一覧
    :widths: 5 10 40
    :header-rows: 1

    * - No
      - 実施内容
      - 説明
    * - 1
      - AWSの環境構築
      - ALB、ECS、ECRなどのコンテナ環境を利用したAWS環境の構築
    * - 2
      - CICDパイプラインの作成
      - github acionsなどを利用したパイプラインの作成（ymlファイルなどは次スプリントで）