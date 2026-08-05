Immich Home Server
<img width="1013" height="840" alt="image" src="https://github.com/user-attachments/assets/5d2ff3fa-3713-49bf-a91c-a3649702e810" />

Docker Compose 기반 Immich 홈 사진 서버의 설정을 관리하는 저장소입니다.

Immich 서버: Ubuntu VM (Docker Compose)

사진 저장소: ipTIME NAS 

외부 HTTPS: Caddy 적용 예정

실제 저장 위치

휴대폰 업로드 원본

/mnt/immich-upload → /data

NAS Root/9999.ImmichUpload

외부 라이브러리

/mnt/iptime-photos

NAS Root/나리

썸네일

/home/zpzg333/immich-app/library/thumbs

VM 로컬 SSD

PostgreSQL

/home/zpzg333/immich-app/postgres

VM 로컬 SSD

외부 라이브러리는 읽기·쓰기로 마운트되어 있으므로 Immich에서 영구 삭제하면 NAS 원본도 삭제될 수 있습니다.

저장소에서 관리할 파일

.
├── docker-compose.yml
├── .env.example
├── fstab.immich
├── iptime-username.example
├── Caddyfile
├── .gitignore
└── README.md

