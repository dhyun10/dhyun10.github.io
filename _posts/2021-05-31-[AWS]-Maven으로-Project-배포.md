---
title: "[AWS] Maven으로 Project 배포"
date: 2021-05-31 09:00:00 +0900
categories: [Programming, AWS]
tags: [AWS]
---

## Maven으로 war 파일 만들기

---

1. pom.xml에 plugin 추가

   ```java
   <build>
       	<plugins>
       		<plugin>
       		<groupId>org.springframework.boot</groupId>
       		<artifactId>spring-boot-maven-plugin</artifactId>
       	</plugin>
       </plugins>
   </build>
   ```

   ![pom](https://user-images.githubusercontent.com/70506979/120141072-bb289480-c216-11eb-82bb-c68fd195dd26.PNG)

   

2. Maven - Lifecycle - package 실행

   ![package](https://user-images.githubusercontent.com/70506979/120140936-8288bb00-c216-11eb-8b95-319129c4d9e0.PNG)

   

3. /target/axboot-1.0.0.war 생성

   ![war](https://user-images.githubusercontent.com/70506979/120141003-9e8c5c80-c216-11eb-84aa-f64588ba088f.PNG)



4. cmd  명령어 실행

   - C:\Project\deploy>java -jar axboot-1.0.0.war

   ![cmd](https://user-images.githubusercontent.com/70506979/120141498-9254cf00-c217-11eb-890a-be35d01afa06.PNG)

   - 정상 실행됨

