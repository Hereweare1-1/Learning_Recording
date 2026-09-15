# Streamlit聊天与状态

这篇笔记整理开发AI聊天页面时常用的聊天、状态保存和页面更新组件。

## 1. AI对话组件

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

## 2. 状态、刷新与占位

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

## 3. AI对话系统最核心的一组

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
