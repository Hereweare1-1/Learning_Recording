# Streamlit常用方法

> 目标：记住“这个方法存在 + 它是干什么的”。  
> 不需要死记所有参数，具体参数用到时再查。

---
## 1. 使用前准备

Streamlit是一个Python库，可以只使用Python快速制作网页界面，不需要先编写HTML、CSS和JavaScript。

安装：

```bash
pip install streamlit
```

在程序文件所在目录运行：

```bash
streamlit run 文件名.py
```

AI聊天项目的整体结构可以查看[[Streamlit AI聊天项目]]。

---

## 2. 主题入口

1. [[Streamlit页面显示与布局]]：显示文本、组织页面布局以及展示提示和进度。
2. [[Streamlit输入与文件]]：接收用户输入，上传或下载文件。
3. [[Streamlit聊天与状态]]：制作AI聊天界面并保存、更新页面状态。

## 3. 最应该记住的15个方法

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

## 4. AI聊天页面中的常用方法

| 页面需求 | Streamlit方法 |
| --- | --- |
| 接收聊天输入 | `st.chat_input()` |
| 显示聊天消息 | `st.chat_message()` |
| 保存聊天记录 | `st.session_state` |
| 动态更新页面区域 | `st.empty()` |
| 显示加载提示 | `st.spinner()` |
