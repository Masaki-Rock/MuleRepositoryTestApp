# MuleRepositoryTestApp
Muleアプリケーションのバージョン管理をテストするアプリケーション

# バージョンを指定する。
$ mvn versions:set -DnewVersion=1.0.2

# アーカイブファイルを作成する。
$ mvn clean package -DattachMuleSources

# GithubのPackagesにアーカイブファイルをアップロードする。
$ mvn -s settings.xml -f pom_package.xml deploy

'''
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-deploy-plugin:2.8.2:deploy (default-deploy) on project account-v1: Failed to deploy artifacts: Could not transfer artifact sample.account:account-v1:pom:1.0.0 from/to github (https://maven.pkg.github.com/Masaki-Rock/MuleRepositoryTestApp): transfer failed for https://maven.pkg.github.com/Masaki-Rock/MuleRepositoryTestApp/sample/account/account-v1/1.0.0/account-v1-1.0.0.pom, status: 422 Unprocessable Entity -> [Help 1]
> groupIdを一意にする必要がある。
'''

# GithubのPackagesからアーカイブファイルを取得する
$ mvn dependency:get -DgroupId=test.account -DartifactId=account-v1 -Dversion=1.0.0  -Dclassifier=mule-application -Ddest=./

# ExchangのAssetにアーカイブファイルをアップロードする。
$ mvn -s settings.xml -f pom_exchange.xml deploy

# GithubのPackagesからアーカイブファイルを取得する
$ mvn -s settings.xml -f pom_exchange.xml dependency:get -DgroupId=b547aeec-a4c6-4c3a-a789-155149b7a023 -DartifactId=account-v1 -Dversion=1.0.3  -Dclassifier=mule-application -Ddest=./

# リリースフォルダにアーカイブファイルをアップロードする。
$ mvn -s settings.xml -f pom_release.xml github-release:github-release

# 指定ファイルをCloudHubにデプロイする
$ mvn -s settings.xml mule:deploy -Dmule.artifact="account-v1-1.0.3-mule-application.jar"
