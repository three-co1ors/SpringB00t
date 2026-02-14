### 스프링 부트란?

---

- 웹 프로그램을 쉽고 빠르게 만들 수 있는 ‘자바’의 웹 프레임워크
- ‘톰캣’ 서버 내장, 여러 편의 기능 추가
    - 톰캣: 클라이언트 요청 해석 ⇒ 그에 맞는 자바 프로그램 실행 후 결과 응답

### 웹 프레임워크

---

```jsx
// 웹 브라우저에 'Hello World' 출력

@Controller
public class HelloController {
	@GetMapping("/")
	@ResponseBody
	public String hello() {
		return "Hello World";
	}
}
```

- 스프링 부트는 보안에 강함
    - SQL 인젝션: 악의적인 SQL 주입하여 공격
    - XSS: 자바스크립트를 삽입해 공격하는 방법
    - CSRF: 위조된 요청 보내는 공격 방법
    - 클릭 재킹: 사용자가 의도하지 않은 클릭 유도

### JDK & STS 설치

---

- JDK: 자바로 코드를 실행하는 도구와 코드를 번역하는 컴파일러 등으로 이루어짐
    
     https://www.oracle.com/java/technologies/downloads/
    
- STS: IDE
    
    https://spring.io/tools
    

### STS 실행

---
<img width="486" height="626" alt="image" src="https://github.com/user-attachments/assets/b65af21c-b5bb-4415-b603-a9d17d827f00" />

- Name: 프로젝트 이름
- Type: 프로젝트를 관리하는 도구를 선택하는 항목, 기본 값은 Gradle-Groovy
- Java Version: 자바 버전 선택 (21 선택)

<img width="703" height="823" alt="image" src="https://github.com/user-attachments/assets/9fd44171-531e-4653-b639-a0aad27604a5" />

최신 안정화 버전 선택 (현재 3.5.9)

- 오류 해결
    - gradle 오류: windows에서 preferences → gradle

 
### 💡 ‘http://localhost:8080/hello’ URL 입력했을 때 화면에 ‘Hello World’라는 문구를 출력하는 웹 프로그램 작성하기


- 컴퓨터(localhost)가 웹 서버가 되어 8080 포트에서 실행되어야 하고, 서버에 요청이 발생하면 ‘Hello World’ 문장이 브라우저 화면에 출력되어야 함
  

### 웹 서비스 동작 과정

---

- 클라이언트와 서버 구조
    
    클라이언트: 자주 사용하는 브라우저, 서버: 브라우저로 접속 가능한 원격 컴퓨터
    
    - 크롬 브라우저에서 서버에 요청을 보낼 때는 서버의 주소(IP 주소) 또는 서버의 주소를 대체할 수 있는 도메인명을 알아야 함
        
        (ex. naver.com을 입력하면 네이버에 운용하는 웹 서버가 호출되고, 서버는 요청에 대한 응답을 브라우저에 돌려줌 ⇒ 웹 서버는 요청에 대한 응답으로 HTML 문서나 다른 리소스들을 브라우저에 표시)
        

### IP 주소와 포트 이해하기

---

- 서버는 웹 서비스뿐만 아니라 FTP, 이메일 서비스 등도 운용할 수 있음 (보통 서비스별로 다른 IP 주소를 사용하지는 않음)
    - 포트로 서비스들 구분 가능
    - 포트: 네트워크 서비스를 구분하는 번호 (하나의 서버 주소에서 포트를 사용하여 많은 서비스 운용 가능)
        
        
        | 프로토콜 | 서비스 내용 | 포트 |
        | --- | --- | --- |
        | HTTP | 웹 서비스 | 80 |
        | HTTPS | SSL을 적용한 웹 서비스 | 443 |
        | FTP | 파일 전송 서비스 | 21 |
        | SSH, SFTP | 보안이 강화된 TELNET(텔넷), FTP 서비스 | 22 |
        | TELNET | 원격 서버 접속 서비스 | 23 |
        | SMTP | 메일 전송 서비스 | 25 |

      

### [localhost:8080](http://localhost:8080) 이해하기

---

- localhost라는 도메인명은 127.0.0.1이라는 IP 주소를 의미, 127.0.0.1 IP 주소는 내 컴퓨터를 의미
- 8080은 8080번 포트로 서비스를 운용한다는 의미
    
    ⇒ localhost:8080는 내 컴퓨터에 8080번 포트로 실행된 서비스를 의미하는 것
    

### 컨트롤러 만들기

---

- 브라우저의 요청을 처리하려면 컨트롤러가 필요함
- 컨트롤러: 서버에 전달된 클라이언트의 요청을 처리하는 자바 클래스

```jsx
// New > Class 생성
package com.example.sbb;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller // HelloController 클래스가 컨트롤러의 기능을 수행한다는 의미
// 애너테이션이라고 부르고, 이게 있어야 스프링 부트 프레임워크가 컨트롤러로 인식

public class HelloController {
// URL 요청이 발생하면 hello 메서드가 실행됨을 의미 (/hello URL과 hello 메서드를 매핑하는 역할)
    @GetMapping("/hello")
    @ResponseBody
    public String hello() {
        return "Hello World";
    }
```

- 애너테이션: 자바의 클래스, 메서드, 변수 등에 정보를 부여하여 부가 동작을 할 수 있게 하는 목적으로 사용
- 매핑: 특정 URL 경로를 서버의 특정 메서드와 연결하는 것
- Get 방식의 URL 요청을 위해 @GetMapping을 사용, Post 방식의 URL 요청을 위해서는 @PostMapping을 사용함
    - Get 방식: 데이터를 URL에 노출시켜 요청 (주로 서버에서 데이터를 조회, 읽기 위한 목적으로 사용)
    - Post 방식: 데이터를 숨겨서 요청 (로그인 정보와 같은 민감한 데이터를 서버에 제출 & 저장하는 목적)
- @ResponseBody 애너테이션은 hello 메서드의 출력 결과가 문자열 그 자체임을 나타냄
  

### 로컬 서버 실행 순서

---

<img width="639" height="486" alt="image" src="https://github.com/user-attachments/assets/3dddc701-0af4-4a98-9605-f131631c7faa" />


### Spring Boot Devtools 설치

---

```jsx
package com.example.sbb;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class HelloController {
    @GetMapping("/hello")
    @ResponseBody
    public String hello() {
        return "Hello SBB"; // 이렇게 바꿔도 localhost:8080/hello는 여전히 Hello World 출력
    }
}
```

- 별도의 과정 없이는 로컬 서버가 변경된 클래스를 즉시 반영하지 않음
    - 이러한 문제 해결을 위해 Devtools 설치
    - bulid.gradle 파일을 찾아 수정
        
        ```jsx
        // build.gradle에 해당 코드 추가 후 Refresh Gradle Project
        developmentOnly 'org.springframework.boot:spring-boot-devtools' 
        ```
        

### 롬복 설치하기

---

- 소스 코드를 작성할 때 자바 클래스에 애너테이션을 사용하여 자주 쓰는 Getter 메서드, Setter 메서드, 생성자 등을 자동으로 만들어 주는 도구
- 게시물과 관련된 데이터를 처리하기 위해 엔티티 클래스나 DTO 클래스 등을 사용해야 하는데, 먼저 이 클래스들의 속성 값을 읽고 저장하는 Getter, Setter 메서드를 만들어야 함
    - 엔티티 클래스: 데이터베이스에 데이터를 저장하고 조회하기 위한 클래스
    - DTO 클래스는 데이터베이스로 조회한 데이터들을 관리하기 위한 클래스
- 플러그인 설치: https://projectlombok.org/download
    
    ```jsx
    // [cmd] 파일이 설치된 경로에서 해당 명령어 실행
    java -jar lombok.jar
    ```
    
    ```jsx
    // build.gradle 파일 수정
    compileOnly 'org.projectlombok:lombok' 
    annotationProcessor 'org.projectlombok:lombok' 
    ```

### **롬복(Lombok)으로 Getter, Setter 메서드 만들기**

---

```jsx
// 롬복을 사용하면 Setter, Getter 메서드를 별도로 작성하지 않아도 됨

import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
public class HelloLombok {
    private String hello;
    private int lombok;

    public static void main(String[] args) {
        HelloLombok helloLombok = new HelloLombok();
        helloLombok.setHello("헬로");
        helloLombok.setLombok(5);

        System.out.println(helloLombok.getHello());
        System.out.println(helloLombok.getLombok());
    }
}
```

### 롬복으로 생성자 만들기

---

```jsx
import lombok.Getter; 
import lombok.RequiredArgsConstructor; 

@RequiredArgsConstructor 
@Getter 
public class HelloLombok { 
    private final String hello; 
    private final int lombok; 

    public static void main(String[] args) { 
        HelloLombok helloLombok = new HelloLombok("헬로", 5); 
        System.out.println(helloLombok.getHello()); 
        System.out.println(helloLombok.getLombok()); 
    } 
}
```

- hello, lombok 속성에 final을 추가하고 @RequiredArgsConstructor 애너테이션을 적용하면 해당 속성(hello, lombok)을 필요로 하는 생성자가 롬복에 의해 자동으로 생성됨
    - final: 뒤에 따라오는 자료형, 변수 등을 변경할 수 없게 만드는 키워드
        - 속성 값을 변경할 수 없기 때문에 @Setter는 의미가 없어짐
