### 목표

---

1. 컨트롤러를 이용해 URL과 매핑되는 메서드 관리
2. JPA를 이용해 데이터베이스를 제어
3. HTML, CSS 등을 활용하여 웹 프로그램 꾸미기


### 구조 이해하기

---

<img width="369" height="500" alt="image" src="https://github.com/user-attachments/assets/db08e472-b3b4-4413-b6dc-1f9a0188ca57" />

- src/main/java 디렉토리
    - src/main/java 디렉토리는 자바 파일을 저장하는 공간

- [com.example.](http://com.example.app)sbb
    - SBB의 자바 파일을 저장하는 공간
    - HelloController.java와 같은 스프링 부트의 컨트롤러, 폼과 DTO, 데이터베이스 처리를 위한 에티티, 서비스 등의 자바 파일이 이곳에 위치
        - 컨트롤러: URL 요청 처리
        - 폼: 사용자의 입력 검증
        - DTO, 엔티티, 서비스 파일: 데이터베이스를 처리하기 위해 필요한 파일

- [SbbApplication.java](http://SbbApplication.java) 파일
    - 모든 프로그램에는 프로그램의 시작을 담당하는 파일이 있음
    - 스트링 부트로 만든 프로그램에도 시작을 담당하는 파일이 있는데, 그 파일이 ‘프로젝트명 + [Application.java](http://Application.java) 파일’
    
    ```jsx
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    
    @SpringBootApplication
    public class SbbApplication {
    
        public static void main(String[] args) {
            SpringApplication.run(SbbApplication.class, args);
        }
    }
    
    ```
    
    - SbbApplication 클래스에는 반드시 @SpringBootApplication 애너테이션이 적용되어 있어야 함


### src/main/resources 디렉토리

---

- 자바 파일을 제외한 HTML, CSS, 자바스크립트, 환경 파일을 저장하는 공간

### templates 디렉토리

---

- 템플릿 파일을 저장함
- 템플릿 파일: 자바 코드를 삽입할 수 있는 HTML 형식의 파일로, 스프링 부트에서 생성한 자바 객체를 HTML 형태로 출력할 수 있음

### Static 디렉토리

---

- 프로젝트의 스타일시트(css), 자바스크립트(js), 이미지 파일 등을 저장

### [appllication.properties](http://appllication.properties) 파일

---

- 프로젝트의 환경을 설정함 (환경 변수, 데이터베이스 등의 설정을 이 파일에 저장)

### [src.test.java](http://src.test.java) 디렉토리

---

- 프로젝트에서 작성한 파일을 테스트하는 코드를 저장하는 공간
- JUnit과 스프링 부트의 테스트 도구를 사용하여 서버를 실행하지 않은 상태에서 src/main/java 디렉토리에 작성한 코드를 테스트할 수 있음

### build.gradle 파일

---

- Gradle이 사용하는 환경 파일

### 간단한 웹 프로그램 만들기

---

**기본:** 브라우저와 같은 클라이언트의 페이지 요청이 발생하면, 스프링 부트는 가장 먼저 컨트롤러에 등록된 URL 매핑을 찾고, 해당 URL 매핑을 발견하면 URL 매핑과 연결된 메서드를 실행함

- URL 매핑: URL과 컨트롤러의 메서드를 일대일로 연결하는 것
- 컨트롤러의 메서드에 @GetMapping 또는 @PostMapping과 같은 애너테이션을 적용하면 해당 URL과 메서드가 연결됨

**목표:** http://localhost:8080/sbb ****페이지를 요청했을 때 ‘안녕하세요 sbb에 오신 것을 환영합니다.’라는 문자열을 출력하도록 만들기

### URL 매핑과 컨트롤러 이해하기

---

- 로컬 서버 구동
    - 404: HTTP 오류 코드 중 하나로, 브라우저가 요청한 페이지를 찾을 수 없다는 의미
    - 브라우저와 같은 클라이언트의 페이지 요청이 발생하면 스프링 부트는 가장 먼저 컨트롤러에 등록된 URL 매핑을 찾고, 해당 매핑을 발견하면 URL 매핑과 연결된 메서드를 실행함

```jsx
// 컨트롤러 작성하여 URL 매핑을 추가하기 위해 scr/main/java
// 디렉터리의 com.mysite.sbb 패키지에 MainCotroller.java 파일 작성

package com.mysite.sbb;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class MainController {
    @GetMapping("/sbb")
    public void index() {
        System.out.println("index");
    }
}
```

- @@controller 애너테이션을 적용하면 MainController 클래스는 스프링 부트의 컨트롤러가 됨
- index 메서드의 @GetMapping 애너테이션은 요청된 URL(/sbb)과의 매핑을 담당
    - 브라우저가 URL을 요청하면 스프링 부트는 요청 페이지와 매핑되는 메서드를 찾아 실행

<img width="701" height="198" alt="image" src="https://github.com/user-attachments/assets/40ec3796-20b3-4b01-98f1-00e75074524d" />

- 500 오류 코드
    - 원래 URL과 매핑된 메서드는 결괏값을 리턴해야 하는데, 아무 값도 리턴하지 않아 발생한 오류
        
        ⇒ 오류를 해결하려면 클라이언트(브라우저)로 응답을 리턴해야 함
        

```jsx
// MainController.java 수정
package com.mysite.sbb;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class MainController {

    @GetMapping("/sbb")
    @ResponseBody // URL 요청에 대한 응답으로 문자열을 리턴하라는 의미로 사용
    public String index() {
    // index 메서드의 리턴 자료형을 String으로 변경
        return "index";
    }
}
```

```jsx
package com.mysite.sbb;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;

@Controller
public class MainController {

    @GetMapping("/sbb")
    @ResponseBody
    public String index() {
        return "안녕하세요 sbb에 오신 것을 환영합니다.";
    }
}

// 서버를 재시작하지 않고 브라우저만 새로 고침하여 문자열이 제대로 출력됨
```
