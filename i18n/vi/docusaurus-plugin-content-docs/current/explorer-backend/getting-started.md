---
sidebar_position: 10
title: Bắt đầu
sidebar_label: Bắt đầu
---

## Yêu cầu

- Java (11 hoặc 17 được khuyến nghị)
- [PostgreSQL](https://www.postgresql.org)
- [A running full-node](full-node/getting-started.md)

## Tải về file cần thiết

Tải file `explorer-backend-x.x.x.jar` từ [Github release](https://github.com/alephium/explorer-backend/releases/latest).

## Tạo database:

1. Bắt đầu dịch vụ `postgresql`.
2. Đăng nhập vào PostgreSQL shell với user mặc định `postgres`:
   ```shell
   psql postgres # or `psql -U postgres` depending on your OS
   ```
3. Chắc chắn rằng `postgres` role đang khả dụng, nếu chưa, hãy tạo nó.  
   Liệt kê tất cả các roles:
   ```shell
   postgres=# \du
   ```
   Tạo `postgres` role:
   ```shell
   postgres=# CREATE ROLE postgres WITH LOGIN;
   ```
4. Sau đó, tạo database:
   ```shell
   postgres=# CREATE DATABASE explorer;
   ```

## Khởi chạy explorer-backend của bạn

```shell
java -jar explorer-backend-x.x.x.jar
```

Explorer-backend của bạn sẽ bắt đầu đồng bộ với full node. Nó có thể sẽ mất chút thời gian ở lần chạy đầu tiên

## Bắt đầu từ một snapshot

Để giảm thời gian cho lần đầu đồng bộ hoá, bạn có thể khôi phục một trong những snapshot của chúng tôi.

Các snapshot khả dụng tại https://archives.alephium.org/#mainnet/explorer-db/

Tải về phiên bản mới nhất, giải nén và khởi chạy:

```shell
psql explorer < explorer-db-xxx.pg_dump
```

Xin hãy lưu ý database của `explorer` phải được tạo từ trước và phải trống.
