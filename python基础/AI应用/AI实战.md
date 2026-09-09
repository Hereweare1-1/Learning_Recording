# 智能伴侣
## streamlit
- Python 库，**不用写 HTML/CSS/JS，只用 Python 通过它提供的功能就可以做出网页界面**
- 通过 **pip install streamlit** 安装
- 在对应文件所在目录下的终端通过 **streamlit run 文件名.py** 运行程序

### 常用方法

#### 1. 页面显示

```
st.title("标题")              # 页面大标题
st.header("标题")             # 小标题
st.write("内容")              # 显示内容，最常用
st.markdown("**加粗**")       # 显示 Markdown
```

#### 2. 用户输入

```
st.text_input("请输入")       # 单行文本输入
st.text_area("请输入")        # 多行文本输入
st.button("按钮")             # 按钮
st.chat_input("请输入")       # 聊天输入框
```

例如：

```
prompt = st.chat_input("请输入您的问题")
```

`st.chat_input()` 会创建聊天输入框，并返回用户输入的内容。

#### 3. 聊天界面

```
st.chat_message("user")       # 用户消息
st.chat_message("assistant")   # AI消息
```

常见写法：

```
st.chat_message("user").write(prompt)
```

也可以写成：

```
with st.chat_message("user"):
    st.write(prompt)
```

#### 4. 状态保存

```
st.session_state
```

用于保存网页运行过程中的数据，例如：

```
st.session_state["messages"] = []
```

常用于保存**聊天记录、会话状态**等。

#### 5. 页面布局

```
st.sidebar        # 侧边栏
st.columns()      # 多列布局
st.tabs()         # 标签页
```

#### 6. 其他常用功能

```
st.file_uploader()    # 文件上传
st.success()          # 成功提示
st.error()            # 错误提示
st.warning()          # 警告提示
st.spinner()          # 加载提示
st.rerun()            # 重新运行页面
```

### AI聊天基本结构

```
prompt = st.chat_input("请输入您的问题")

if prompt:
    st.chat_message("user").write(prompt)

    # 调用大模型 API
    response = ...

    st.chat_message("assistant").write(response)
```

整体流程：

**用户输入 → 获取 prompt → 调用大模型 → 获取 response → 显示 AI 回复**