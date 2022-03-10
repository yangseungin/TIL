# 마리아디비 백업 정리
- [Mariabackup 레퍼런스](https://mariadb.com/kb/en/mariabackup-overview/#difference-in-versioning-schemes)를 참조하여 정리한 내용


# 테스트 환경
- OS: Amazon Linux 2 AMI
- Mariadb: 10.3.31-MariaDB (최신은 10.6.7인데 기존에 세팅해둔 db를 사용하였습니다.)
- 백업도구: Mariabackup

## 백업도구로 Mariabackup을 사용한 이유
- 간단히 백업할 때는 mysqldump를 사용할 수도 있지만 mysqldump는 논리적 백업으로 sql형태로 백업과 복원을 진행하여 데이터가 클 경우 속도면에서 큰 차이가 난다.
- Mariabackup을 사용할때는 데이터 업데이트가 발생하지 않도록 주의해야 하는데 해당 db가 백업을 진행할 시간에 데이터 업데이트가 발생하지 않을 것이라는 확신이 있기 때문에 Mariadbbackup을 사용하였다.

## 설치
설치는 yum을 사용하여 설치 하였다.
```bash
$sudo yum install MariaDB-backup
```

