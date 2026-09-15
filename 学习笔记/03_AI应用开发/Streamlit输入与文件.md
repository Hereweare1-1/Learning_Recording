# Streamlit输入与文件

这篇笔记整理Streamlit中的用户输入、交互以及文件上传和下载组件。

## 1. 用户输入与交互

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

## 2. 文件

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
