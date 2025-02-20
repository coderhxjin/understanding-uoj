# 评测端

## 提交程序评测流程（以传统题为例）

### 后端轮询

程序位置：`judger/judge_client` (Python)

流程：
1. `judger_loop` 函数每 2s 获取一次待评测提交
2. `send_and_fetch` 函数获取网页端提交评测信息
3. 调用 `judge` 函数，清空 `uoj_judger/result`, `uoj_judger/work` 目录
4. 将评测题号及提交程序语言写入 `work/submission.conf`；使用 HTTP POST 向网页端获取 `all.zip`，（不确定）其中包含选手代码（固定命名 `answer.code`）及可执行文件
5. 调用 `main_judger` 可执行文件（由 `main_judger` 调用 `builtin/judger/judger`）

### 评测流程

1. 调用 `judger_init` 函数，读取 `submission.conf` 和 `problem.conf` 中的配置存入 `config` 中
2. 调用 `ordinary_test` 函数进行评测

## 文件格式

### conf 文件

格式：
```plain
<key1> <value1>
<key2> <value2>
...
```
其中每行两个字符串（不包含空格），中间用一个空格分隔，表示一对键值对。