# agent-学习路线记录
-----------------------
## 2026.5.9
今天观看了黑马程序员的课程
<img width="269" height="265" alt="image" src="https://github.com/user-attachments/assets/1467a1a6-41f0-4d94-bf89-57ac2cd96e03" />
看到了提示词工程
然后这是大致的代码块：
```bash
from openai import OpenAI

client=OpenAI(
    base_url="https://api.deepseek.com"
)

#调用模型
response=client.chat.completions.create(
    model="deepseek-v4-pro",
    messages=[
        {"role": "system", "content": "你是一个Python编程专家，并且说话很多"},
        {"role":"assistant","content":"好的，我是Python编程专家，你想问什么？"},
        {"role":"user","content":"输出1-10的数字，使用Python代码。"}
    ],
    #打开流式输出开关
    stream=True
)

#处理结果
for chunk in response:

    # 取出content
    content = chunk.choices[0].delta.content

    # 过滤None
    if content is not None:
        print(content, end="", flush=True)
```
觉得今天看的课程比较重要的就是这段代码，流式输出，然后过滤none
