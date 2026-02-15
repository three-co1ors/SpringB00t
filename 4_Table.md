### 데이터베이스 구성 요소 살펴보기

---

- 테이블(Table): 데이터의 저장 공간
    - 행(Row)과 열(Column)으로 구성
- 기본 키(Primary Key): 테이블의 데이터가 중복되어 저장되지 않게 함
    - 어떤 열을 기본 키로 설정하면 해당 열에는 동일한 값을 저장할 수 없음
        
        <img width="569" height="170" alt="image" src="https://github.com/user-attachments/assets/02ac674c-69b0-47fd-8d5b-143bbb590eac" />

        

### 엔티티 속성 구성하기

---

- 엔티티: DB 테이블과 매핑되는 자바 클래스
    
    > 엔티티를 모델 또는 도메인 모델이라고도 함
    > 
- 프로젝트의 질문과 답변 데이터를 저장할 DB 테이블과 매핑되는 질문, 답변 엔티티가 있어야 함
- 질문과 답변 엔티티에는 어떤 속성이 필요할까?
    - 사용자가 질문을 남기고 답변을 받을 수 있는 웹 서비스라면?
        - 사용자가 입력한 질문 저장
        - 질문의 제목/내용을 담을 수 있는 항목이 필요
            
            ⇒ ‘제목’, ‘내용’ 등을 엔티티의 속성으로 추가
            
            - 질문 엔티티
            
            | **속성 이름** | **설명** |
            | --- | --- |
            | id | 질문 데이터의 고유 번호 |
            | subject | 질문 데이터의 제목 |
            | content | 질문 데이터의 내용 |
            | createDate | 질문 데이터를 작성한 일시 |
            - 답변 엔티티
            
            | **속성 이름** | **설명** |
            | --- | --- |
            | id | 답변 데이터의 고유 번호 |
            | question | 질문 데이터 (어떤 질문의 답변인지 확인해야 하므로) |
            | content | 답변 데이터의 내용 |
            | createDate | 답변 데이터를 작성한 일시 |

### 질문 엔티티 만들기

---

- src/main/java 디렉토리의 com.mysite.sbb 패키지에 ‘Qusetion.java’ 파일 작성
    
    ```sql
    package com.mysite.sbb;
    
    import java.time.LocalDateTime;
    
    import jakarta.persistence.Column;
    import jakarta.persistence.Entity;
    import jakarta.persistence.GeneratedValue;
    import jakarta.persistence.GenerationType;
    import jakarta.persistence.Id;
    
    import lombok.Getter;
    import lombok.Setter;
    
    @Getter
    @Setter
    @Entity // JPA가 이 클래스를 관리하도록 지정
    public class Question {
        @Id // 고유 번호 (기본 키로 지정! 중복되면 안 됨)
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Integer id;
    
        @Column(length = 200) // 테이블의 열로 인식하고 싶지 않다면 @Transient
        private String subject; // 제목
    
        @Column(columnDefinition = "TEXT")
        private String content; // 내용
    
        private LocalDateTime createDate; // 작성 일시
    }
    
    ```
    
    **추가 설명**
    
    1. @Getter와 @Setter (Lombok 기능)
        - 자바에서는 데이터 보호를 위해 변수(필드)를 private로 감추고, 메서드를 통해서만 접근하는 것이 원칙 (캡슐화)
            - Getter의 기능: 읽기
                - 외부에서 변수의 값을 꺼내올 수 있게 (Read)
                - 컴파일러가 알아서 getSubject(), getContent() 등과 같은 메서드를 자동으로 생성
            - Setter의 기능: 쓰기
                - 외부에서 변수의 값을 저장/수정 (Write)
                - 자동으로 setSubject(…) 같은 메서드를 만들어 줌
    
    1. @Entity
        - 자바 클래스에게 ‘이제 너는 DB 테이블이야’라고 알려 주는 것
        - 이 표시를 보고 데이터베이스의 클래스 이름과 같은 테이블을 자동으로 생성
    
    1. @GeneratedValue
        - 번호를 직접 쓰지 않고, DB가 자동으로 매기게 한다는 것
        - 이 예시에서 id 값을 비워 두면, DB가 알아서 1번, 2번, 3번… 순서대로 번호 붙여 줌
    
    1. Column
        - 컬럼 생성 시 기본이 아닌 특별한 규칙 지정
            - length = 200: 제목 칸은 최대 200글자로 제한
            - columnDefinition = “TEXT”: 내용 칸은 글자 수 제한 없이 TEXT 타입으로 생성
    
    1. 엔티티의 속성 이름과 테이블의 열 이름 차이
        - createDate 속성의 이름은 DB 테이블에서 create_data라는 열 이름으로 설정됨
        - 모두 소문자로 변경되고 단어가 _로 구분됨
            
            (카멜 케이스: 맨 첫 글자를 제외한 나머지 단어의 첫 글자를 대문자로 쓰는 것)
            
    
    1. 엔티티를 만들 때 Setter 메서드는 사용하지 않음
        - 엔티티는 DB와 바로 연결되므로, 데이터를 자유롭게 변경할 수 있는 Setter 메서드르 허용하는 것이 안전하지 않다고 판단됨
        - 엔티티는 **생성자에 의해서만** 엔티티의 값을 저장할 수 있게 하고, 데이터 변경 시 메서드를 추가로 작성
            
            (복잡도 이슈로 이번에는 Setter 메서드로 추가하여 진행)
            

### 답변 엔티티 만들기

---

- [Answer.java](http://Answer.java) 파일 작성
    
    ```sql
    package com.mysite.sbb;
    
    import java.time.LocalDateTime;
    import jakarta.persistence.Column;
    import jakarta.persistence.Entity;
    import jakarta.persistence.GeneratedValue;
    import jakarta.persistence.GenerationType;
    import jakarta.persistence.Id;
    import jakarta.persistence.ManyToOne;
    import lombok.Getter;
    import lombok.Setter;
    
    @Getter
    @Setter
    @Entity
    public class Answer {
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Integer id;
    
        @Column(columnDefinition = "TEXT")
        private String content;
    
        private LocalDateTime createDate; 
        
    	  @ManyToOne 
    		// 질문 엔티티를 참조하기 위해 question 속성 추가
        private Question question;  
    }
    ```
    

**추가 설명**

1. 답변을 통해 질문의 제목을 알고 싶다면 **answer.getQuestion().getSubject()**를 사용해 접근 가능
    - question 속성만 추가하면 안 되고, 질문 엔티티와 연결된 속성이라는 것을 답변 엔티티에 표시해야 함
        
        ⇒ import jakarta.persistence.ManyToOne;, @ManyToOne 추가
        

1. 게시판 서비스에서는 하나의 질문에 답변 여러 개가 달릴 수 있음
    - 답변: Many, 질문: One
    - @ManyToOne 애너테이션을 사용해 N:1 관계를 나타낼 수 있고, 답변 엔티티의 question 속성과 질문(Question) 엔티티가 서로 연결됨 ⇒ 외래 키 관계 생성
    - 부모-자식 관계: 부모는 Question, 자식은 Answer

1. 질문에서 답변을 참조하기
    - 답변과 질문은 N:1 관계, 질문과 답변은 1:N 관계
    - @oneToMany 애너테이션 사용
    - 질문 하나에 답변이 여러 개이므로 Question 엔티티에 추가할 Answer 속성은 List 형태
        
        ```sql
        package com.mysite.sbb; 
        
        import java.time.LocalDateTime; 
        import java.util.List; 
        
        import jakarta.persistence.CascadeType; 
        import jakarta.persistence.Column; 
        import jakarta.persistence.Entity; 
        import jakarta.persistence.GeneratedValue; 
        import jakarta.persistence.GenerationType; 
        import jakarta.persistence.Id; 
        import jakarta.persistence.OneToMany; 
        
        import lombok.Getter; 
        import lombok.Setter; 
        
        @Getter 
        @Setter 
        @Entity 
        public class Question { 
            @Id 
            @GeneratedValue(strategy = GenerationType.IDENTITY) 
            private Integer id; 
        
            @Column(length = 200) 
            private String subject; 
        
            @Column(columnDefinition = "TEXT") 
            private String content; 
        
            private LocalDateTime createDate; 
        
            @OneToMany(mappedBy = "question", cascade = CascadeType.REMOVE) 
            private List<Answer> answerList; 
        }
        ```
        
        - 답변을 참조하려면 question.getAnswerList()를 호출
        - @OneToMany 애너테이션에 사용된 mappedBy는 참조 엔티티의 속성명을 정의
            
            (Answer 엔티티에서 Question 엔티티를 참조한 속성인 question을 mappedBy에 전달)
            
        
        **추가 지식**
        
        - CascadeType.REMOVE
            - 질문 하나에 답변이 여러 개 작성된 경우, 질문을 삭제할 때 그에 달린 답변도 모두 삭제되도록 cascade = CascadeType.REMOVE 사용

### 테이블 확인

---

<img width="216" height="319" alt="image" src="https://github.com/user-attachments/assets/9c37de38-d9a1-42a8-b615-fe8b46273771" />
