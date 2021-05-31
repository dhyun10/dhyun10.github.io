---
title: "[AWS] Elastic IP 할당하기"
date: 2021-05-31 11:00:00 +0900
categories: [Programming, AWS]
tags: [AWS]
---

## Elastic IP(EIP, 탄력적 IP) 할당하기

---

- 인스턴스도 IP가 존재하는데 같은 인스턴스를 중지하고 다시 시작할 때도 새 IP가 할당된다.

- 매번 변경되지 않고 고정 IP를 가지게 하기 위해 고정 IP를 할당해야한다.

  

1. 탄력적 IP - 탄력적 IP 주소 할당

   ![8](https://user-images.githubusercontent.com/70506979/120152178-2333a680-c228-11eb-8280-d6379419e4ad.PNG)

2. 탄력적 IP 주소 설정

   ![9](https://user-images.githubusercontent.com/70506979/120152181-2333a680-c228-11eb-8b2c-f9fc7865ad64.PNG)

3. 탄력적 IP 주소 할당 완료

   ![10](https://user-images.githubusercontent.com/70506979/120152182-23cc3d00-c228-11eb-9bb9-33045ef61cbc.PNG)

4. 탄력적 IP 주소 연결

   ![11](https://user-images.githubusercontent.com/70506979/120152175-22027980-c228-11eb-88f5-8a281c4f0153.PNG)

5. 확인

   ![12](https://user-images.githubusercontent.com/70506979/120152424-6e4db980-c228-11eb-8d44-8d78c5516990.PNG)

   - 생성한 탄력적 IP는 무조건 EC2에 바로 연결해야 하며 만약 더는 사용할 인스턴스가 없을 때도 탄력적 IP를 삭제해야합니다.