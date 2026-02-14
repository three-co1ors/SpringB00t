### 목표

---

- 방문자들이 질문과 답변을 남길 수 있는 게시판 서비스 제작
    - 게시판의 내용을 사용자가 질문 또는 답변 작성 ⇒ 데이터 생성
    - 이 데이터를 관리하려면 저장/조회/수정 기능 구현이 필요하고, DB로 관리
    
    (SQL을 사용해야 하지만, ORM을 이용해 자바 문법으로 DB를 다룰 수 있음)
    

### ORM과 JPA 이해하기

---

- ORM이란?
    - DB의 테이블을 자바 클래스로 만들어 관리할 수 있음
    - 아래와 같은 question 테이블에 데이터를 저장하려면?
        
        <img width="316" height="187" alt="image" src="https://github.com/user-attachments/assets/cd4b4cea-1c84-44e3-819b-7bf61acbaa18" />

        
        - SQL
            
            ```sql
            insert into question (id, subject, content) values (1, '안녕하세요', '가입 인사드립니다 ^^');
            insert into question (id, subject, content) values (2, '질문 있습니다', 'ORM이 궁금합니다');
            ```
            
        - ORM 사용
            
            ```sql
            Question q1 = new Question();
            q1.setId(1);
            q1.setSubject("안녕하세요");
            q1.setContent("가입 인사드립니다 ^^");
            this.questionRepository.save(q1);
            
            Question q2 = new Question();
            q2.setId(2); 
            q2.setSubject("질문 있습니다"); 
            q2.setContent("ORM이 궁금합니다"); 
            this.questionRepository.save(q2);
            // ORM의 자바 클래스를 '엔티티'라고 함
            // 엔티티는 DB의 테이블과 매핑되는 자바 클래스
            ```
            
        
- JPA(Java Persistence API)이란?
    - 스프링 부트는 JPA를 사용하여 DB를 관리 (JPA를 ORM 기술의 표준으로 사용)
    - JPA는 인터페이스의 모음이고, 이 인터페이스를 구현한 실제 클래스가 필요함
        - 대표적으로 하이버네이트
            
            ⇒ 하이버네이트는 JPA의 인터페이스를 구현한 실제 클래스, 자바의 ORM 프레임워크
            
        
        💡 인터페이스: 클래스가 구현해야 하는 메서드 목록
        

### H2 데이터베이스 설치

---

1. build.gradle 파일에 입력 (설치 후 Gradle → Refresh Gradle Project 필수)
    
    ```sql
    dependencies { 
      implementation 'org.springframework.boot:spring-boot-starter-web' 
      ...
      runtimeOnly 'com.h2database:h2' // 이 부분
    }
    ```
    
2. H2 DB를 사용하려면 src/mainresources 디렉터리의 [application.properties](http://application.properties) 파일에 새로운 설정 추가
    
    ```sql
    # DATABASE
    spring.h2.console.enabled=true # H2 콘솔에 접속할 건지? (H2 콘솔은 H2 DB를 웹 UI로 보여 줌)
    spring.h2.console.path=/h2-console # H2 콘솔로 접속하기 위한 URL 경로
    spring.datasource.url=jdbc:h2:~/local # DB에 접속하기 위한 경로
    spring.datasource.driverClassName=org.h2.Driver # DB에 접속할 때 사용하는 드라이버명
    spring.datasource.username=sa # DB의 사용자명 (기본 값 sa)
    spring.datasource.password= # DB의 비밀번호
    ```
    

1. spring.datasource.url에 설정한 경로에 해당하는 DB 파일 만들기
    - jdbc:h2:~/local로 설정했으므로 사용자의 홈 디렉토리 아래에 H2 DB 파일로 local.mv.db라는 파일 생성
        
        <img width="338" height="119" alt="image" src="https://github.com/user-attachments/assets/b66ea8e2-704a-4c27-971e-e31fc43484f1" />

        
        - `copy con local.mv.db`  입력 후 Ctrl+Z, Enter

1. 홈 디렉토리에서 확인 ⇒ 파일 사이즈는 반드시 0KB

1. H2 콘솔을 통해 DB에 접속하기
    
    ```sql
    http://localhost:8080/h2-console
    ```
    
    <img width="701" height="447" alt="image" src="https://github.com/user-attachments/assets/22ad7b91-519a-458f-b3c5-c30dde25b455" />

    
    (이런 오류가 떴지만 서버를 다시 껐다가 켜니 해결 ⇒ 요청한 페이지 주소를 못 찾았던 것)
    

1. 데이터베이스 연결 주소인 jdbc:h2:~/local로 변경 후 연결
    
    <img width="454" height="361" alt="image" src="https://github.com/user-attachments/assets/a6cb1bb3-caf0-46ec-abdf-e5e8d960e010" />

    
    <img width="698" height="564" alt="image" src="https://github.com/user-attachments/assets/76c63e40-0857-45b2-959b-d71b410b3b57" />

    

### JPA 환경 설정하기

---

- 프로그램에서 H2 DB 사용할 수 있게 설정하기

1. build.gradle 파일 수정
    
    ```sql
    dependencies { 
        ...
        implementation 'org.springframework.boot:spring-boot-starter-data-jpa' 
    }
    ```
    
💡 **implementation**

  - 라이브러리 설치를 위해 가장 일반적으로 사용하는 설정
  - 해당 라이브러리가 변경되더라도 이 라이브러리와 연관된 모든 모듈을 컴파일하지 않고 변경된 내용과 관련이 있는 모듈만 컴파일 ⇒ 리빌드 속도 빠름
    

2. [application.properties](http://application.properties) 파일 수정
    
    ```sql
    # JPA
    
    # 스프링 부트와 하이버네이트를 함께 사용할 때 필요한 설정 항목
    spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.H2Dialect
    
    # 엔티티를 기준으로 데이터의 테이블을 생성하는 규칙 설정
    spring.jpa.hibernate.ddl-auto=update
    ```
    
    **Spring.jpa.hibernate.ddl-auto의 규칙** 
    
    - none: 엔티티가 변경되더라도 데이터베이스 변경 X (운영 환경에서 주로 사용)
    - update: 엔티티의 변경된 부분만 데이터베이스에 적용 (개발 환경에서 주로 사용)
    - validate: 엔티티와 테이블 간에 차이점이 있는지 검사 (운영 환경에서 주로 사용)
    - create: 스프링 부트 서버를 시작할 때 테이블을 모두 삭제한 후 다시 생성
    - create-drop: create와 동일하지만 스프링 부트 서버를 종료할 때에도 테이블을 모두 삭제
