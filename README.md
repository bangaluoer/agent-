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
-------------------------------------------------------------
## 2026.5.10
```bash
messages = [
    {"role": "system",
     "content": "你是金融专家，将文本分类为['新闻报道', '财务报道', '公司公告', '分析师报告']，不清楚的分类为'不清楚类别' 下面有示例："},
]
for key,value in examples_data.items():
    messages.append({"role": "user","content": value})
    messages.append({"role": "assistant","content": key})#动态添加message

for q in questions:
    response=client.chat.completions.create(
        model="deepseek-v4-pro",
        messages=messages+[{
            "role":"user",
            "content": q#把要问的问题添加进来
        }]
    )
```
这是今天学的动态创建提示词
然后今天还学了list转json和json转list
```bash
json.dumps(list,ensure_ascii=flase)#将字典或者列表转换成json
json.loads(json)#将json转换成list或者字典
```
之前Python的数据结构学的迷迷糊糊，再总结一下字典和列表取值的时候用的方法：
```bash
字典：键(key) -> 值(value)
person = {
    "name": "张三",
    "age": 18
}
person["name"]=“张三”
列表：类似于数组，通过下标来取对应的数据
```
<img width="833" height="209" alt="image" src="https://github.com/user-attachments/assets/4260c225-be4f-4618-8537-b1a8cb66fc53" />
--------------------------------------------------------------------------------------------------------------------------------------

## 2026.5.11
今天学习langchain，我自己的理解就是langchain集成了很多功能，可以直接调用
还学了文字转向量，使用的是huggingface的embedding模型
```bash
from langchain_community.embeddings import HuggingFaceEmbeddings

embed = HuggingFaceEmbeddings(
    model_name="BAAI/bge-small-zh-v1.5"
)

result = embed.embed_query("我喜欢你")#embed_query单次转换，一次转换一句话
result1=embed.embed_documents(["我喜欢你","我稀饭你","晚上吃什么"])#embed_documents一次转换多句话，传入的变量是一个列表[]
print(result1[:5])
```
<img width="1231" height="448" alt="image" src="https://github.com/user-attachments/assets/2508d910-3c81-4b31-a1a7-b103a5628511" />

今天还学了关于langchain里面的fewshot
```bash
from langchain_core.prompts import PromptTemplate, FewShotPromptTemplate
from langchain_openai import ChatOpenAI

example_prompt = PromptTemplate.from_template(
    "输入:{input}, 输出:{output}"
)

examples = [
    {"input": "示例1", "output": "答案1"},
    {"input": "示例2", "output": "答案2"},
]

prompt = FewShotPromptTemplate(
    examples=examples,
    example_prompt=example_prompt,
    prefix="下面是一些示例：",
    suffix="请回答：{question}",
    input_variables=["question"]
)

prompt_text = prompt.invoke({"question": "真正的问题"}).to_string()

llm = ChatOpenAI(
    model="deepseek-chat",
    base_url="https://api.deepseek.com",
    temperature=0
)

res = llm.invoke(prompt_text).content
print(res)
```
