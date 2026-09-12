# Streamlit 常见方法速查

> 目标：记住“这个方法存在 + 它是干什么的”。  
> 不需要死记所有参数，具体参数用到时再查。

---

## 一、页面与文本

### `st.title()`
页面大标题。

```python
st.title("AI 学习助手")
```

### `st.header()`
一级标题。

```python
st.header("聊天记录")
```

### `st.subheader()`
二级标题。

```python
st.subheader("功能介绍")
```

### `st.write()`
最常用的通用输出方法，可以输出文字、变量等。

```python
st.write("你好")
st.write(result)
```

### `st.markdown()`
输出 Markdown 格式内容。

```python
st.markdown("**这是加粗文字**")
```

### `st.text()`
输出普通纯文本。

```python
st.text("这是一段文本")
```

---

## 二、用户输入与交互

### `st.button()`
创建按钮。

```python
if st.button("开始"):
    st.write("开始运行")
```

### `st.text_input()`
普通单行文本输入框。

```python
name = st.text_input("请输入姓名")
```

### `st.text_area()`
多行文本输入框。

```python
content = st.text_area("请输入内容")
```

### `st.number_input()`
数字输入框。

```python
age = st.number_input("请输入年龄")
```

### `st.checkbox()`
复选框。

```python
agree = st.checkbox("我同意")
```

### `st.radio()`
单选按钮。

```python
choice = st.radio("选择一个", ["A", "B", "C"])
```

### `st.selectbox()`
下拉选择框。

```python
choice = st.selectbox("选择模型", ["模型A", "模型B"])
```

### `st.multiselect()`
多选框。

```python
choices = st.multiselect("选择功能", ["聊天", "搜索", "翻译"])
```

### `st.slider()`
滑动条。

```python
temperature = st.slider("温度", 0.0, 1.0)
```

---

## 三、AI 对话系统重点

### `st.chat_input()`
聊天输入框。

```python
prompt = st.chat_input("请输入你的问题")
```

**记忆：聊天输入。**

### `st.chat_message()`
显示一条聊天消息。

```python
with st.chat_message("user"):
    st.write("你好")

with st.chat_message("assistant"):
    st.write("你好，有什么可以帮你？")
```

**记忆：显示聊天消息。**

### `st.session_state`
保存页面运行过程中的状态，例如聊天记录。

```python
if "messages" not in st.session_state:
    st.session_state.messages = []
```

**记忆：保存会话状态 / 聊天记录。**

---

## 四、布局

### `st.sidebar`
侧边栏。

```python
st.sidebar.title("设置")
```

也可以：

```python
with st.sidebar:
    st.write("这里是侧边栏")
```

**记忆：左侧设置区域。**

### `st.columns()`
把页面分成多列。

```python
col1, col2 = st.columns(2)

with col1:
    st.write("左边")

with col2:
    st.write("右边")
```

### `st.tabs()`
创建标签页。

```python
tab1, tab2 = st.tabs(["聊天", "设置"])

with tab1:
    st.write("聊天页面")

with tab2:
    st.write("设置页面")
```

### `st.expander()`
创建可以展开/收起的区域。

```python
with st.expander("查看详细信息"):
    st.write("详细内容")
```

---

## 五、文件

### `st.file_uploader()`
上传文件。

```python
file = st.file_uploader("上传文件")
```

**记忆：文件上传。**

### `st.download_button()`
提供文件下载按钮。

```python
st.download_button(
    "下载文件",
    data="Hello",
    file_name="test.txt"
)
```

---

## 六、状态、刷新与占位

### `st.empty()`
创建一个空的占位区域，之后可以动态更新。

```python
placeholder = st.empty()

placeholder.write("正在处理...")
placeholder.write("处理完成")
```

**AI 流式输出时比较常见。**

### `st.rerun()`
重新运行当前 Streamlit 页面。

```python
st.rerun()
```

---

## 七、提示信息

### `st.success()`
成功提示。

```python
st.success("操作成功")
```

### `st.info()`
普通信息提示。

```python
st.info("这是一条提示")
```

### `st.warning()`
警告提示。

```python
st.warning("请注意")
```

### `st.error()`
错误提示。

```python
st.error("操作失败")
```

### `st.exception()`
显示异常信息。

```python
try:
    ...
except Exception as e:
    st.exception(e)
```

---

## 八、加载与进度

### `st.spinner()`
显示“正在处理”的加载提示。

```python
with st.spinner("AI 正在思考..."):
    result = get_answer()
```

### `st.progress()`
显示进度条。

```python
progress = st.progress(0)

progress.progress(50)
progress.progress(100)
```

---

## 九、最应该记住的 15 个

如果觉得上面太多，先只记这一组：

| 方法 | 一句话记忆 |
|---|---|
| `st.title()` | 页面大标题 |
| `st.write()` | 输出内容 |
| `st.button()` | 按钮 |
| `st.text_input()` | 文本输入 |
| `st.selectbox()` | 下拉选择 |
| `st.checkbox()` | 复选框 |
| `st.sidebar` | 侧边栏 |
| `st.columns()` | 分列 |
| `st.file_uploader()` | 上传文件 |
| `st.chat_input()` | **聊天输入** |
| `st.chat_message()` | **聊天消息** |
| `st.session_state` | **保存会话状态** |
| `st.empty()` | 占位区域 / 动态更新 |
| `st.spinner()` | 加载提示 |
| `st.rerun()` | 重新运行页面 |

---

## 十、AI 对话系统最核心的一组

以后你做 Streamlit AI 项目，最容易反复看到的是：

```text
st.chat_input()
    ↓
获取用户问题

st.chat_message()
    ↓
显示用户 / AI 消息

st.session_state
    ↓
保存聊天历史

st.empty()
    ↓
动态更新内容 / 辅助流式显示

st.spinner()
    ↓
等待 AI 回复时显示加载状态
```

可以把它记成：

**输入 → 显示 → 保存 → 更新 → 等待**

---

## 十一、学习原则

### 要记

- 方法的名字
- 方法是干什么的
- 大概什么时候使用

### 不用死记

- 所有参数
- 参数的顺序
- 很少用的高级参数
- 完整 API 文档

### 最终目标

以后看到需求：

> “我要做一个 AI 聊天页面。”

脑子里能够想到：

```text
聊天输入 → st.chat_input()

显示消息 → st.chat_message()

保存聊天记录 → st.session_state

动态更新 → st.empty()

加载提示 → st.spinner()
```

这就达到了现在阶段需要的程度。
