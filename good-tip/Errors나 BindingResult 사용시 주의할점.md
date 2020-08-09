# 스프링의 유효성 검사
스프링에서는 import org.springframework.validation.Validator 인터페이스를 통해 객체 검증이나 에러 메세지등을 지원한다.

컨트롤러에서 객체의 값을 검증할 때 Validator 인터페이스를 구현하여 유효성 검사를 하는데 주의할 점이 있다.

BindingResult나 Errors는 바인딩 받는 객체의 바로 다음에 선언해야 된다. 이를 인지하지 않은 상태로 구현을 작성하여 BindException이 발생하였던 내용을 정리하였다.

~~~
org.springframework.validation.BindException: org.springframework.validation.BeanPropertyBindingResult: 1 errors
~~~

# 현상
내가 예상했던 동작은 validator를 통해 제목이나 내용의 길이가 맞지 않는 경우 errors.rejectValue()로 필드에 에러코드를 추가하였으니 memo 뷰를 다시 보여주고 작성해둔 메세지가 보이는 것 이였는데 예상과는 다르게 WhiteLabel 에러 페이지와 함께 BindExceptionn이 발생하였다.  
<img src="https://github.com/yangseungin/TIL/blob/master/spring/%EC%82%AC%EC%A7%84/validator/bindexception.png?raw=true" width="70%">

디버그를 해봐도 Validator에서 에러 코드를 추가하는 것 까지는 정상적으로 동작하였으나
PostMapping에 break point를 줘도 잡히지 않았다.

발생하였던 이슈를 예제코드로 작성해 보았다. (springboot, jpa, 타임리프를 사용하였습니다.)  
# 예제코드
~~~ java
@Entity
@Getter @Setter
@EqualsAndHashCode
@NoArgsConstructor
public class Memo {
    @Id @GeneratedValue
    private Long id;

    private String title;

    private String content;

    public Memo(MemoForm memoForm) {
        this.title = memoForm.getTitle();
        this.content = memoForm.getContent();
    }
}

~~~
~~~ java
@Data
public class MemoForm {
    @NotBlank
    @Length(max = 10)
    private String title;

    @NotBlank
    @Length(min = 10, max = 200)
    private String content;
}
~~~
Controller와 Repository (예시가 간단하여 service는 따로 구현하지 않았습니다.)
~~~ java
@Controller
@RequiredArgsConstructor
public class MemoController {

    private final MemoRepository memoRepository;

    //리스트 view
    @GetMapping("/memoList")
    public String viewMemoList(Model model){
        List<Memo> memoList = memoRepository.findAll(); //모든메모 가져와서
        model.addAttribute(memoList);   //모델에 추가
        return "memoList";
    }

    //메모작성 view
    @GetMapping("/memo")
    public String writeMemoForm(Model model){
        model.addAttribute(new MemoForm());
        return "memo";
    }
    @PostMapping("/memo")
    public String writeMemo(@Valid MemoForm  memoForm, Errors errors, Model model){
        new MemoFormValidator().validate(memoForm,errors);  //form 검증

        if(errors.hasErrors()) {    //에러 존재시 다시 작성view
            return "memo";
        }

        Memo memo = new Memo(memoForm); //저장하기위해 간단하게 mapping
        memoRepository.save(memo);      //저장

        return "redirect:/memoList";    //등록이후 list화면으로 redirect
    }
}
~~~
~~~ java
public interface MemoRepository extends JpaRepository<Memo, Long> {
}
~~~

메모 폼 Validator 구현 간단하게 길이 체크만 하였습니다.
~~~ java
@Component
public class MemoFormValidator implements Validator {
    @Override
    public boolean supports(Class<?> clazz) {
        return MemoForm.class.isAssignableFrom(clazz);
    }

    @Override
    public void validate(Object target, Errors errors) {
        MemoForm memoForm = (MemoForm) target;
        if(memoForm.getTitle().length()>10){
            errors.rejectValue("title","title.error","타이틀이 초과하였습니다."); // 필드에 대한 에러정보(필드, 에러코드, 에러메세지) 추가
        }
        if(memoForm.getContent().length()<10 ||memoForm.getContent().length()>200 ){
            errors.rejectValue("content","content.error","내용의 길이가 맞지않습니다.");
        }

    }
}
~~~

메모 등록 뷰 : memo.html
~~~
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head th:fragment="head">
    <meta charset="UTF-8">
    <title>메모작성</title>
</head>
<body>
    <h2>메모작성</h2>
    <div>
        <form th:action="@{/memo}" th:object="${memoForm}" method="post" novalidate>
            <div>
                <label for="title">제목</label>
                <input id="title" type="text" th:field="*{title}" aria-describedby="titleDesc" required max="10">
                <small id="titleDesc">제목은 10자까지</small>
                <small class="form-text text-danger" th:if="${#fields.hasErrors('title')}" th:errors="*{title}">Title Error</small>
            </div>

            <div>
                <label for="content">내용</label>
                <textarea id="content" type="text" th:field="*{content}" aria-describedby="contentDesc" required minlength="10" maxlength="200"></textarea>
                <small id="contentDesc">10자이상 200자이내로 작성하세요</small>
                <small class="form-text text-danger" th:if="${#fields.hasErrors('content')}" th:errors="*{content}">Content Error</small>
            </div>
            <div>
                <button type="submit">저장</button>
           </div>
        </form>
    </div>
</body>
</html>
~~~
메모리스트 뷰 : memoList.html
~~~
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head th:fragment="head">
    <meta charset="UTF-8">
    <title>메모리스트</title>
</head>
<body>
<h2>메모리스트</h2>
<table>
    <thead>
    <tr>
        <th>제목</th>
        <th>내용</th>
    </tr>
    </thead>
    <tbody>
    <tr th:each="memo: ${memoList}">
        <td th:text="${memo.title}"></td>
        <td th:text="${memo.content}"></td>
    </tr>
    </tbody>
</table>
</body>
</html> 
~~~
처음 언급했던 문제가 발생한것은 Model 파라미터 이후 Errors를 선언하여서 발생하였다.
~~~ java
// 기존에 BindException이 발생하던 코드
@PostMapping("/memo")
    public String writeMemo(@Valid MemoForm  memoForm, Model model, Errors errors){
        //...
    }
~~~
수정 후 정상적으로 동작한다.  
## - 비정상적인 값 입력 시
에러 메세지를 보여준다.

<img src="https://github.com/yangseungin/TIL/blob/master/spring/%EC%82%AC%EC%A7%84/validator/error.png?raw=true" width="70%">  

값의 길이나 필수값과 같은정보는 JSR 303 애노테이션(@NotBlank, @Length)으로 검증하여 Errors에 에러가 담기게 되는데 예시로 든 코드가 애매한 것 같다...
메모가 아니라 줒ㅇ복이 허용되지 않는 title과 같은 제약사항을두고 Validator에서 해당 제목의 메모가 있는지 체크하는 식으로 수정하는게 좋을 것 같다.


## - 정상적인 값 입력시
저장 후 리스트를 보여준다.

<img src="https://github.com/yangseungin/TIL/blob/master/spring/%EC%82%AC%EC%A7%84/validator/memolist.png?raw=true" width="70%">



# 결론
BindingResult나 Errors는 객체 바로 다음에 선언하자.  
스프링 래퍼런스에서도 아래와 같이 설명하고 있다.
~~~
For access to errors from validation and data binding for a command object (that is, a @ModelAttribute argument) or errors from the validation of a @RequestBody or @RequestPart arguments. You must declare an Errors, or BindingResult argument immediately after the validated method argument.
~~~

# 참고 
https://docs.spring.io/spring/docs/current/spring-framework-reference/web.html#mvc-ann-arguments