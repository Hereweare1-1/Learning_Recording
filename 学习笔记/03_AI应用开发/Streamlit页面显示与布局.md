# Streamlit页面显示与布局

这篇笔记整理Streamlit中的页面文本、布局、提示信息和加载进度组件。

## 1. 页面与文本

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

## 2. 布局

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

## 3. 提示信息

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

## 4. 加载与进度

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
