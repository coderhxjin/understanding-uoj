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

已知配置项：
- `n_tests` 测试点个数
- `input_pre/output_pre` 输入/输出数据文件名前缀（一般相同）
- `input_suf/output_suf` 输入/输出数据文件名后缀
- `time_limit/memory_limit/output_limit` 相关限制（）
- `n_ex_tests` 附加数据（包含样例在内）组数
- `n_sample_tests` 样例组数
- `subtask_type_<subtask-id>` 子任务类型，其中：
- - `packed` 表示若有非 AC 数据点即记作 0 分
- - `min` 表示取所有测试点分数最小值