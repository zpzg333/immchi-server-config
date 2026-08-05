Immich Home Server

Docker Compose 기반 Immich 홈 사진 서버의 설정을 관리하는 저장소입니다.

Immich 서버: Ubuntu VM (192.168.100.116)

사진 저장소: ipTIME NAS (192.168.100.4)

컨테이너 실행: Docker Compose

외부 HTTPS: Caddy 적용 예정

구성도

flowchart TB
    Client["휴대폰 · PC"]
    Router["공유기 / 포트포워딩"]

    subgraph VM["Immich VM · 192.168.100.116"]
        Caddy["Caddy · HTTPS 적용 예정"]
        Immich["Immich Server · Docker · 2283"]
        DB["PostgreSQL · 로컬 SSD"]
        Thumbs["썸네일 · 로컬 SSD"]
    end

    subgraph NAS["ipTIME NAS · 192.168.100.4"]
        Upload["9999.ImmichUpload\n휴대폰 업로드 원본"]
        External["나리\n외부 라이브러리 원본"]
    end

    Client --> Router
    Router --> Caddy
    Caddy --> Immich
    Immich --> DB
    Immich --> Thumbs
    Immich --> Upload
    Immich --> External

데이터 저장 위치

데이터

VM 경로

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

다음 파일과 디렉터리는 비밀번호 또는 실제 데이터를 포함하므로 Git에 올리지 않습니다.

.env
iptime-username
library/
postgres/
*.key
*.pem

NAS 접속 정보는 저장소 밖의 root 전용 파일로 관리합니다.

/etc/samba/credentials/immich-nas

권장 권한은 root:root, 600입니다. NAS의 관리자 계정보다는 필요한 공유 폴더만 접근 가능한 Immich 전용 계정을 사용합니다.

새 VM에 적용하는 순서

Ubuntu VM에 Docker와 Docker Compose를 설치합니다.

이 저장소를 복제합니다.

.env.example을 .env로 복사하고 새 비밀번호를 설정합니다.

NAS 자격 증명 파일을 /etc/samba/credentials/immich-nas에 생성합니다.

fstab.immich 내용을 /etc/fstab에 반영하고 NAS 마운트를 확인합니다.

docker compose config --quiet으로 설정을 검사합니다.

docker compose up -d로 Immich를 실행합니다.

PostgreSQL 데이터는 이 저장소에서 백업하지 않습니다. 새 VM으로 이전할 때 기존 DB를 복원하지 않는다면 Immich 계정, 앨범, 얼굴 인식 결과와 외부 라이브러리 등록 정보는 새로 구성해야 합니다. NAS에 저장된 사진 원본은 유지됩니다.

상태 확인

docker compose ps
findmnt -T /mnt/immich-upload
findmnt -T /mnt/iptime-photos
docker inspect immich_server \
  --format '{{range .Mounts}}{{println .Destination "RW=" .RW "Source=" .Source}}{{end}}'
