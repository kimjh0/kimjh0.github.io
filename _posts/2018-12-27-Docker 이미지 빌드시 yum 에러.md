---
title: "Docker 이미지 빌드시 yum 에러"
tags:
  - 가상화
---

Docker 이미지 빌드 중 yum 명령에서 간헐적으로 에러 발생
아래를 Dockerfile 상단에 입력하면 된다.

```docker
# for prevent "Rpmdb checksum is invalid : dCDPT (pkg checksums)"  
RUN rpm --rebuilddb && yum install -y yum-plugin-ovl \  
&& yum clean all \  
&& rm -rf /var/cache/yum/*  
```

한번만 해주면 이후에 yum 사용에 문제 없음
