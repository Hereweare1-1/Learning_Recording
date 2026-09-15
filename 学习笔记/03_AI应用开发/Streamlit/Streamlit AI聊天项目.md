# Streamlit AI聊天项目

## 1. 项目作用

这个项目使用Streamlit制作AI聊天网页。用户在页面中输入问题，程序调用大模型API，再把回答显示到页面中。

开始前可以先查看：

- Streamlit组件：[[Streamlit常用方法]]
- 大模型调用流程：[[大模型API调用]]

## 2. 需要用到的核心方法

| 方法 | 在聊天项目中的作用 |
| --- | --- |
| `st.chat_input()` | 获取用户输入的问题 |
| `st.chat_message()` | 显示用户消息和AI消息 |
| `st.session_state` | 保存聊天记录和会话状态 |

其他页面、布局和提示方法统一整理在[[Streamlit页面显示与布局]]中。

## 3. AI聊天的基本结构

```python
import streamlit as st

prompt = st.chat_input("请输入您的问题")

if prompt:
    st.chat_message("user").write(prompt)

    # 调用大模型API
    response = ...

    st.chat_message("assistant").write(response)
```

`st.chat_message()`也可以配合`with`使用：

```python
with st.chat_message("user"):
    st.write(prompt)
```

## 4. 整体流程

```text
用户输入问题
   ↓
st.chat_input()获得prompt
   ↓
调用大模型API
   ↓
获得response
   ↓
st.chat_message()显示AI回复
```

> [!summary] 一句话理解
> Streamlit负责网页输入和显示，大模型API负责生成回答。
