# Dokumentasi Deploy Kafka Topics dengan Docker Compose

## Overview

Dokumentasi ini menjelaskan cara menggunakan Docker Compose untuk secara otomatis membuat topik Kafka dalam cluster Kafka. Ini menggunakan container khusus `init-kafka` yang bergantung pada ketersediaan Kafka dan membuat topik sesuai dengan konfigurasi yang ditentukan.

## Requirements

- Docker dan Docker Compose terinstal pada sistem.
- Cluster Kafka yang sudah berjalan dan dapat diakses.
- Pastikan bahwa jumlah broker Kafka dalam cluster Anda cukup untuk mendukung `replication-factor` yang ditetapkan untuk setiap topik.

## Deployment Steps

### 1. Konfigurasi `docker-compose.yaml`

Edit file `docker-compose.yaml` untuk mengganti placeholder `<ip-kafka>` dan `<port-kafka>` dengan IP dan port dari Kafka broker yang dapat diakses.

Contoh penggantian:

- `<ip-kafka>` diganti dengan IP dari salah satu broker Kafka Anda, misalnya `192.168.1.100`.
- `<port-kafka>` diganti dengan port broker Kafka tersebut, misalnya `9092`.

### 2. Atur `replication-factor`

Pastikan bahwa nilai `replication-factor` untuk setiap topik yang akan dibuat tidak melebihi jumlah broker Kafka dalam cluster Anda. Hal ini penting untuk memastikan keandalan dan ketersediaan topik.

- Jika cluster Anda memiliki 3 broker Kafka, maka pengaturan `replication-factor 3` adalah maksimal yang bisa Anda gunakan.

### 3. Menjalankan Docker Compose

Setelah konfigurasi selesai, jalankan Docker Compose untuk memulai proses pembuatan topik:

```sh
docker-compose up -d
```

Container `init-kafka` akan mengeksekusi perintah yang didefinisikan dalam `command`, menunggu Kafka menjadi tersedia, dan kemudian membuat topik sesuai dengan spesifikasi yang diberikan.

### 4. Verifikasi Topik

Setelah proses pembuatan topik selesai, Anda dapat memverifikasi pembuatan topik dengan menggunakan perintah `kafka-topics` seperti yang ditunjukkan dalam konfigurasi container:

```sh
kafka-topics --bootstrap-server <ip-kafka>:<port-kafka> --list
```

Gantikan `<ip-kafka>` dan `<port-kafka>` dengan nilai yang sesuai untuk cluster Kafka Anda.

## Catatan

- Program ini bisa dijalankan di komputer local tidak perlu di VM
