变量输出：一：<title><th:block th:text="${msg}" /> - 服务器错误。</title>
二：<title>[[${msg}]] - 服务器错误。</title>
`Hello, [[${user.name}]]!   //[[]]写法会html转义
Hello, [(${user.name})]!   //[()]写法不会html转义`
`三：<span th:text="${msg}" th:remove="tag"></span>`