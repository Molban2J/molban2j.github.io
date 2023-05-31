---
layout: single
title:  "스프링 부트 게시판 예제2"
categories:
  - springboot
---


<br>

**깃허브 repository** : [SpringBoot class practice](https://github.com/Molban2J/SpringBootPractice.git "github")

<br>

## 스프링 부트 예제

스프링 부트 프레임 워크로 넘어와서의 게시판 예제입니다.

---

<br>


```java
 @GetMapping("/main")
    public String main(Model model){
        model.addAttribute("list",service.boardList());
        return"board/main";
    }
```

<div style="text-align:center; font-size:0.8em;">main mapper 생성</div>

<br>

그리고 이에 해당하는 main.html을 board파일 안에 만들어 줍니다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css">
</head>
<body>
  <div class="container">
      <table class = "table table-hover">
          <thead>
            <tr>
                <th>글번호</th>
                <th>작성자</th>
                <th>제 목</th>
                <th>조회수</th>
            </tr>
          </thead>
          <tr th:each="list: ${list}">
            <td>[[${list.boardId}]]</td>
            <td>[[${list.name}]]</td>
              <td>[[${list.title}]]</td>
              <td>[[${list.read}]]</td>
          </tr>
      </table>
  </div>
</body>
</html>
```

<div style="text-align:center; font-size:0.8em;">이 예제에서는  부트 스트랩을 이용하기 때문에 부트스트랩 링크를 걸어줬다.</div>

<br>

![image]({{ site.baseurl }}/images/2023-05-31/20230531_111925.png)

<div style="text-align:center; font-size:0.8em;">실행 화면</div>

<br>

---

<br>

제목을 클릭했을 때 상세보기 페이지로 넘어가는 작업을 해줍니다.
<br>

```java
  @GetMapping("/view")
    public String view(Model model, int boardId){
        model.addAttribute("view",service.getBoard(boardId));
        return "board/view";
    }
```

<div style="text-align:center; font-size:0.8em;">view mapper 생성</div>

<br>

그리고 getBoard sql은 만들어 놨으니, 인터페이스와 서비스에 코드를 작성해줍니다.

```java
  Board getBoard(int boardId);
```

<div style="text-align:center; font-size:0.8em;">인터페이스(BoardMapper.java)</div>

<b>

```java
   public Board getBoard(int boardId) {
        return boardMapper.getBoard(boardId);
    }
```

<div style="text-align:center; font-size:0.8em;">BoardService</div>

<br>
view.html도 작성해줍니다.

```html
  <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css">
</head>
<body>
<div class="container">
    <p>글 번호: [[${view.boardId}]]</p>
    <p>제 목: [[${view.title}]]</p>
    <p>작성자: [[${view.name}]]</p>
    내용: <textarea class="form-control" th:text="${view.content}"></textarea>
    <p>조회수: [[${view.read}]]</p>
</div>
</body>
</html>
```

<div style="text-align:center; font-size:0.8em;">view.html</div>

<br>

제목을 클릭했을때 view.html로 넘어가야하니 main.html에 a태그를 넣어서 수정해줍니다.

```html
  <td>[[${list.title}]]</td>
```

<div style="text-align:center; font-size:0.8em;">기존코드</div>

<br>


```html
  <td>
    <a th:href="@{/board/view(boardId=${list.boardId})} ">
          [[${list.title}]]
    </a>
  </td>
```

<div style="text-align:center; font-size:0.8em;">수정</div>

<br>

<br>

![image]({{ site.baseurl }}/images/2023-05-31/20230531_120148.png)

<div style="text-align:center; font-size:0.8em;">수정 후main 화면</div>

<br>

![image]({{ site.baseurl }}/images/2023-05-31/20230531_120205.png)

<div style="text-align:center; font-size:0.8em;">view 화면</div>

<br>

---

<br>

이제 게시글 등록기능을 추가해 줍니다.
mapper, interface, service에 게시글 등록을 위한 코드를 작성해줍니다.

```xml
   <insert id="uploadBoard" parameterType="com.example.springbootpractice.domain.Board">
        insert into tbl_board (title, content, name) values (#{title}, #{content}, #{name});
    </insert>
```

<div style="text-align:center; font-size:0.8em;">BoardMapper.xml</div>

<br>

```java
    void uploadBoard(Board board);
```

<div style="text-align:center; font-size:0.8em;">BoardMapper.java</div>

<br>


```java
   public void uploadBoard(Board board){
        boardMapper.uploadBoard(board);
    }
```

<div style="text-align:center; font-size:0.8em;">BoardService.java</div>

<br>

업로드를 할때 form태그를 post방식으로 제출할 것이기 때문에 업로드 페이지 접속을 위한 get매퍼와 게시글 등록을 위한 post매퍼를 작성해 줍니다.

```java
   @GetMapping("/upload")
    public String upload(Model model){
        return "board/upload";
    }
    @PostMapping("/upload")
    public String uploadBoard(Board board){
        service.uploadBoard(board);
        return "redirect:/board/main";
    }
```

<div style="text-align:center; font-size:0.8em;">BoardController.java</div>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div class="container">
    <form th:action="@{upload}" method="post">
        이름: <input type="text" class="form-control" name="name">
        제목: <input type="text" class="form-control" name="title">
        내용: <textarea class="form-control" name="content"></textarea>
        <button type="submit">등록</button>
    </form>
</div>
</body>
</html>
```

<div style="text-align:center; font-size:0.8em;">upload.html</div>

<br>

아직 upload페이지에 접속하는 버튼을 안 만들었으니 직접 링크를 타고 들어가서 게시글이 잘 작성 되는지 테스트 해봅니다.

<br>

<br>

![image]({{ site.baseurl }}/images/2023-05-31/20230531_121020.png)

<div style="text-align:center; font-size:0.8em;">upload 화면</div>

<br>

등록 버튼을 누르니 에러가 뜬다.

![image]({{ site.baseurl }}/images/2023-05-31/20230531_120630.png)

<div style="text-align:center; font-size:0.8em;">error 발생</div>

<br>

Connection이 read-only로 되어있어서 그렇다는 것 같다. read-only를 적용한것은 Service에 **Transactional** 어노테이션 밖에 없는것 같아서 Transactional(readOnly = false)로 바꿨다.

<br>

![image]({{ site.baseurl }}/images/2023-05-31/20230531_121235.png)

<div style="text-align:center; font-size:0.8em;">수정</div>

<br>

바꿔 줬더니 잘 등록 된다. 근데 다른 사람들은 readOnly = true 였어도 잘 되는데 무슨 문제인지 모르겠다.

<br>

![image]({{ site.baseurl }}/images/2023-05-31/20230531_122134.png)

<div style="text-align:center; font-size:0.8em;">잘 등록된 모습</div>

<br>

---

<br>

남은 update와 delete기능을 구현한다. 위와 동일한 절차로 코드를 작성해준다.

<br>

```xml
   <update id="updateBoard" parameterType="com.example.springbootpractice.domain.Board">
        update tbl_board set title=#{title}, content=#{content} where boardId = #{boardId};
    </update>

    <delete id="deleteBoard" parameterType="int">
        delete from tbl_board where boardId = #{boardId};
    </delete>
```

<div style="text-align:center; font-size:0.8em;">BoardMapper.xml</div>

<br>


```java
    void updateBoard(Board board);

    void deleteBoard(int boardId);
```

<div style="text-align:center; font-size:0.8em;">BoardMapper.java</div>

<br>

```java
    public void updateBoard(Board board){
        boardMapper.updateBoard(board);
    }

    public void deleteBoard(int boardId){
        boardMapper.deleteBoard(boardId);
    }
```

<div style="text-align:center; font-size:0.8em;">BoardService.java</div>

<br>


```java
   @PutMapping("/update")
    public String updateBoard(Board board){
        service.updateBoard(board);
        return "redirect: /board/main";
    }

    @DeleteMapping("/delete")
    public String deleteBoard(int boardId){
        service.deleteBoard(boardId);
        return "redirect: /board/main";
    }
```

<div style="text-align:center; font-size:0.8em;">BoardController.java</div>

<br>
<br>

새로운 view단 페이지를 만들지 않고 view.html을 수정해서 만들어 줍니다.

<br>

```html
<body>
<div class="container">
    <p>글 번호: [[${view.boardId}]]</p>
    <p>제 목: [[${view.title}]]</p>
    <p>작성자: [[${view.name}]]</p>
    <div id="content">
        내용: <textarea class="form-control" th:text="${view.content}" readonly></textarea>
    </div>
    <p>조회수: [[${view.read}]]</p>

    <button id="deleteBtn" class="btn btn-danger btn-sm float-left">삭제</button>
    <button id="updateBtn" class="btn btn-info btn-sm float-right">수정</button>
    <form id="form" th:action="@{/}" method="post">

    </form>
</div>
```

<div style="text-align:center; font-size:0.8em;">view.html 수정</div>

<br>

추가로 스크립트문을 작성해주고 jquery스크립트를 임포트 해줍니다.

```html
 <script src="https://code.jquery.com/jquery-1.12.0.min.js"></script>
    <script src="js/vendor/modernizr-3.8.0.min.js"></script>
    <script src="https://code.jquery.com/jquery-1.12.0.min.js"></script>
    <script>window.jQuery || document.write('<script src="@{/js/vendor/jquery-3.4.1.min.js}"><\/script>')</script>
```

<div style="text-align:center; font-size:0.8em;">view.html head 태그 안에 삽입</div>

<br>

```html
 <script th:inline="javascript">
    $(document).on('ready', function (e) {
        var form = $("#form");
        var boardId = [[${view.boardId}]];
        $(document).on('click', '#deleteBtn', function (e) {
            $('#form').attr("action", "delete");
            form.append("<input type='hidden' name='boardId' value='"+boardId+"'>");
            form.append("<input type='hidden' name='_method' value='delete'>");
            form.submit();
        });
        $(document).on('click', '#updateBtn', function (e) {
            var str = "<input class='form-control' width='250' placeholder='제목 입력' id='updateTitle'>";
            $('#title').html(str);
            str = "<textarea class='form-control' placeholder='내용 입력' id='updateContent'></textarea>";
            $('#content').html(str);
            $('#updateBtn').attr("id", "updateConfirmBtn");
        });

        $(document).on('click', '#updateConfirmBtn', function (e) {
            $('#form').attr("action", "update");
            var updateTitle = $('#updateTitle').val();
            var updateContent = $('#updateContent').val();
            form.append("<input type='hidden' name='boardId' value='"+boardId+"'>");
            form.append("<input type='hidden' name='_method' value='put'>");
            form.append("<input type='hidden' name='title' value='"+updateTitle+"'>");
            form.append("<input type='hidden' name='content' value='"+updateContent+"'>");
            form.submit();
        });
    });
</script>
```

<div style="text-align:center; font-size:0.8em;">view.html script</div>

> 스크립트 문을 작성할 때 큰따옴표와 작은따옴표를 잘 혼용해서 쓰는데 서로 어디서 닫히는지 잘 확인해야겠다. 자동으로 닫히기 때문에 이를 인지를 잘 못해서 오류가 빈번하게 발생한다.

<br>
<br>

![image]({{ site.baseurl }}/images/2023-05-31/20230531_142918.png)

<div style="text-align:center; font-size:0.8em;">view.html</div>

<br>

![image]({{ site.baseurl }}/images/2023-05-31/20230531_142934.png)

<div style="text-align:center; font-size:0.8em;">view.html. 수정버튼 클릭시 창이 바뀜</div>


<br>

![image]({{ site.baseurl }}/images/2023-05-31/20230531_142955.png)

<div style="text-align:center; font-size:0.8em;">수정 잘 됨</div>

<br>

<br>

게시글 예제는 여기까지









