PostgreSQL 18.4 (Windows x64) 向けビルド成果物
==============================================

対象: PostgreSQL 18.4 / Windows x64
ビルド環境: MSYS2 MINGW64, gcc 15.2.0
configure: --prefix=/c/pgsql18 --without-icu --with-openssl
ビルド日: 2026/09/04
作成者: ツー

収録物
------
lib/orafce.dll        orafce 4.15
lib/pgtt.dll          pgtt 1.2.0 (pgtt-rsl v1.2 系)
lib/pgtt_bgw.dll      pgtt バックグラウンドワーカー
                      ※ postgres15.14 版には含まれていなかったが、
                        セッション終了後の不要行を削除するのに必須。
share/extension/      orafce / pgtt の SQL 定義と control ファイル

導入手順
--------
1. lib/*.dll   を <PostgreSQL>\lib\ へコピー
2. share/extension/* を <PostgreSQL>\share\extension\ へコピー
3. postgresql.conf に追記して PostgreSQL を再起動
     shared_preload_libraries = 'pgtt_bgw,pgtt'
4. 対象 DB で実行
     CREATE EXTENSION orafce;
     CREATE EXTENSION pgtt;

注意
----
* PostgreSQL のメジャーバージョンが違うと DLL は動かない。18.x 専用。
* postgres.exe が MSYS2 の DLL に依存するため、Windows サービスとして
  起動する場合は libwinpthread-1.dll / zlib1.dll / libssl-3-x64.dll /
  libcrypto-3-x64.dll を <PostgreSQL>\bin\ に置く必要がある。
  （置かないと「pg_ctl: could not find postgres program executable」で
    サービスが起動しない）
* ソースコードは postgres15.14 版から変更していない。DLL のみ 18.4 で
  再ビルドしたもの。

詳細な構築手順:
  S3OP\document\99.Doc_Install_Postgres\scripts\README.md
