Immich Home Server
<img width="1013" height="840" alt="image" src="https://github.com/user-attachments/assets/5d2ff3fa-3713-49bf-a91c-a3649702e810" />

Docker Compose 기반 Immich 홈 사진 서버의 설정을 관리하는 저장소입니다.

1. Immich 서버: Ubuntu VM (Docker Compose)
2. 사진 저장소: ipTIME NAS 
* 외부 HTTPS: Caddy 적용 예정

실제 저장 위치

#library(NAS) -> /mnt/immich-upload (NAS:Root/나리)
=> 새로운 사진 저장 library
=> backups
=> encoded-video
=> library (이미지 저장 공간)

#외부 library(NAS) -> /mnt/iptime-photos (NAS:Root\9999.ImmichUpload)
=> 기존 사진을 외부library로 적용

#썸네일(VM Local) -> /home/zpzg333/immich-app/library/thumbs
#PostgreSQL(VM Local) -> /home/zpzg333/immich-app/postgres

외부 라이브러리는 읽기·쓰기로 마운트되어 있으므로 Immich에서 영구 삭제하면 NAS 원본도 삭제


