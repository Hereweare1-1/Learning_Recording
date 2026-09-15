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

## 4. 学习原则

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
