# Langchain学习

--------

**LangChain**是一个框架，用于开发由大型语言模型（LLM）驱动的应用程序。

LangChain 简化了 LLM 应用程序生命周期的每个阶段：

- **开发**：使用LangChain的开源[构建块](https://python.langchain.com/v0.1/docs/expression_language/)和[组件](https://python.langchain.com/v0.1/docs/modules/)构建应用程序。使用[第三方集成](https://python.langchain.com/v0.1/docs/integrations/platforms/)和[模板](https://python.langchain.com/v0.1/docs/templates/)开始运行。
- **生产化**：使用 [LangSmith](https://python.langchain.com/v0.1/docs/langsmith/) 检查、监控和评估您的链条，以便您可以自信地持续优化和部署。
- **部署**：使用 [LangServe](https://python.langchain.com/v0.1/docs/langserve/) 将任何链转换为 API。

LLM 支持的最强大的应用程序之一是复杂的问答 （Q&A） 聊天机器人。这些应用程序可以回答有关特定源信息的问题。这些应用程序使用一种称为检索增强生成 （RAG） 的技术。

### 什么是RAG？

RAG 是一种使用额外数据增强 LLM 知识的技术。

LLM 可以对广泛的主题进行推理，但他们的知识仅限于他们接受培训的特定时间点之前的公共数据。如果要构建可以推理私有数据或模型截止日期后引入的数据的 AI 应用程序，则需要使用模型所需的特定信息来增强模型的知识。引入适当信息并将其插入模型提示符的过程称为检索增强生成 （RAG）。

LangChain有许多组件，旨在帮助构建Q&A应用程序，以及更普遍的RAG应用程序。

## RAG 架构

典型的 RAG 应用程序有两个主要组件：

**索引**：用于从源引入数据并对其进行索引的管道。*这通常发生在离线状态。*

**检索和生成**：实际的 RAG 链，它在运行时接受用户查询并从索引中检索相关数据，然后将其传递给模型。

从原始数据到答案最常见的完整序列如下所示：

#### 索引

1. **加载**：首先我们需要加载数据。这是使用 [DocumentLoaders](https://python.langchain.com/v0.1/docs/modules/data_connection/document_loaders/) 完成的。
2. **拆分**：[文本拆分器将](https://python.langchain.com/v0.1/docs/modules/data_connection/document_transformers/)大块拆分为更小的块。这对于索引数据和将数据传递到模型都很有用，因为大块更难搜索，并且不适合模型的有限上下文窗口。`Documents`
3. **存储**：我们需要某个地方来存储和索引我们的拆分，以便以后可以搜索它们。这通常是使用 [VectorStore](https://python.langchain.com/v0.1/docs/modules/data_connection/vectorstores/) 和 [Embeddings](https://python.langchain.com/v0.1/docs/modules/data_connection/text_embedding/) 模型完成的。

![image-20240610205424416](https://stupid-blog-img.oss-cn-beijing.aliyuncs.com/Blog/image-20240610205424416.png)

#### 检索和生成

1. **检索**：给定用户输入，使用 [Retriever](https://python.langchain.com/v0.1/docs/modules/data_connection/retrievers/) 从存储中检索相关拆分。
2. **生成**：[ChatModel](https://python.langchain.com/v0.1/docs/modules/model_io/chat/) / [LLM](https://python.langchain.com/v0.1/docs/modules/model_io/llms/) 使用包含问题和检索数据的提示生成答案

![image-20240610205525304](https://stupid-blog-img.oss-cn-beijing.aliyuncs.com/Blog/image-20240610205525304.png)

## Langchain

LangChain应用程序的核心构建块是LLMChain。这结合了三件事：

1.   1.LLM：语言模型是这里的核心推理引擎。为了使用 LangChain，您需要了解不同类型的语言模型以及如何使用它们。
2.   2.提示模板：这为语言模型提供说明。这控制语言模型输出的内容，因此了解如何构造提示和不同的提示策略至关重要。
3.   3.输出解析器：这些将来自LLM的原始响应转换为更可行的格式，从而可以轻松使用下游的输出。

我们将单独介绍这三个组件，然后介绍组合所有这些组件的LLMChain。

了解这些概念将为您能够使用和自定义 LangChain 应用程序做好准备。

大多数LangChain应用程序都允许您配置LLM和/或使用的提示，因此知道如何利用这一点将是一个很大的推动因素。

## LLMs

有两种类型的语言模型，在LangChain中称为：

- llms：这是一个语言模型，它将字符串作为输入并返回一个字符串
- chat_models：这是一种语言模型，它将消息列表作为输入并返回消息

LLM 的输入/输出简单易懂 - 一个字符串。

但是chat_models呢？

那里的输入是ChatMessage的列表 ，输出是单个ChatMessage .

一个ChatMessage有两个必需的组件：

- > content ：这是消息的内容。

- > role ：这是来自的 ChatMessage 实体的角色。

LangChain 提供了几个对象来轻松区分不同的角色：

- HumanMessage ： ChatMessage来自人类/用户。
- AIMessage ： ChatMessage来自AI/助手。
- SystemMessage ：来自系统的ChatMessage 。
- FunctionMessage ：来自函数调用的ChatMessage 。

LangChain 为两者公开了一个标准接口，但了解这种差异以便为给定语言模型构造提示很有用。LangChain 公开的标准接口有两种方法：

- predict ：接受字符串，返回字符串
- predict_messages ：接收消息列表，返回消息。

对于这两种方法，还可以将参数作为关键字参数传入。

例如，您可以传入 temperature=0 以根据对象的配置调整使用的温度。在运行时传入的任何值都将始终覆盖对象配置的内容。

###  提示模板

大多数LLM应用程序不会将用户输入直接传递到LLM。

通常，他们会将用户输入添加到称为提示模板的较大文本中，该文本为手头的特定任务提供额外的上下文。

```python
from langchain.prompts import PromptTemplate

prompt = PromptTemplate.from_template("What is a good name for a company that makes {product}?")
prompt.format(product="colorful socks")
```

​    提示模板还可用于生成消息列表。在这种情况下，提示不仅包含有关内容的信息，还包含每条消息（其角色，其在列表中的位置等）在这里，最常发生的是聊天提示模板是聊天消息模板的列表。每个聊天消息模板都包含有关如何格式化该聊天消息的说明         - 它的角色，然后是它的内容。      

### 输出解析器      

 输出解析器将LLM的原始输出转换为可以在下游使用的格式。输出分析器的主要类型很少，包括：      

 从LLM转换文本->结构化信息（例如JSON）      

 将聊天消息转换为字符串 

 将调用返回的额外信息转换为字符串，而不是消息（如 OpenAI 函数调用）。 

###   大语言模型链

我们现在可以将所有这些组合成一个链。此链将获取输入变量，将这些变量传递给提示模板以创建提示，将提示传递给 LLM，然后通过（可选）输出分析器传递输出。这是捆绑模块化逻辑的便捷方法。让我们看看它的实际效果！

# langchain 提示词 

--------

任何语言模型应用程序的核心元素都是...模型。
LangChain为您提供了与任何语言模型交互的构建块。
Prompts：模板化、动态选择和管理模型输入
语言模型：通过通用接口调用语言模型
输出解析器：从模型输出中提取信息

##  Prompts

语言模型提示是用户提供的一组指令或输入，用于指导模型的响应，帮助它理解上下文并生成相关且连贯的基于语言的输出，例如回答问题、完成句子或参与对话。
LangChain 提供了多个类和函数来帮助构造和使用提示。
提示模板：参数化模型输入
示例选择器：动态选择要包含在提示中的示例

###  PromptTemplate

提示模板是为语言模型生成提示的预定义配方。
模板可能包括说明、几个镜头示例以及适合给定任务的特定上下文和问题。
LangChain 提供了创建和使用提示模板的工具。
LangChain 致力于创建与模型无关的模板，以便轻松地跨不同语言模型重用现有模板。
通常，语言模型希望提示是字符串或聊天消息列表。

```python
from langchain import PromptTemplate #用于 PromptTemplate 为字符串提示创建模板。

#默认情况下， PromptTemplate 使用 Python 的 str.format 语法进行模板化;但是可以使用其他模板语法（例如， jinja2 ）
prompt_template = PromptTemplate.from_template(
    "Tell me a {adjective} joke about {content}."
)
prompt_template.format(adjective="funny", content="chickens")
```

```python
from langchain import PromptTemplate

#对于其他验证，请显式指定 input_variables 。
#在实例化期间，这些变量将与模板字符串中存在的变量进行比较，如果不匹配，则会引发异常;
invalid_prompt = PromptTemplate(
    input_variables=["adjective"],
    template="Tell me a {adjective} joke about {content}."
)
```

聊天模型的提示是聊天消息列表。
每条聊天消息都与内容相关联，以及一个名为 role 的附加参数。例如，在 OpenAI 聊天完成 API 中，聊天消息可以与 AI 助手、人员或系统角色相关联。

```python
from langchain.prompts import ChatPromptTemplate

#ChatPromptTemplate.from_messages 接受各种消息表示形式。
template = ChatPromptTemplate.from_messages([
    ("system", "You are a helpful AI bot. Your name is {name}."),
    ("human", "Hello, how are you doing?"),
    ("ai", "I'm doing well, thanks!"),
    ("human", "{user_input}"),
])

messages = template.format_messages(
    name="Bob",
    user_input="What is your name?"
)
```

除了使用上一个代码块中使用的（类型、内容）的 2 元组表示形式外，
还可以传入 or BaseMessage 的 MessagePromptTemplate 实例。

```python
from langchain.prompts import ChatPromptTemplate
from langchain.prompts.chat import SystemMessage, HumanMessagePromptTemplate

template = ChatPromptTemplate.from_messages(
    [
        SystemMessage(
            content=(
                "You are a helpful assistant that re-writes the user's text to "
                "sound more upbeat."
            )
        ),
        HumanMessagePromptTemplate.from_template("{text}"),
    ]
)

from langchain.chat_models import ChatOpenAI

#设置代理
import os
os.environ['http_proxy'] = 'http://127.0.0.1:10809'
os.environ['https_proxy'] = 'http://127.0.0.1:10809'

llm = ChatOpenAI()
llm(template.format_messages(text='i dont like eating tasty things.'))
```

### 自定义提示模板

LangChain 提供了一组默认的提示模板，可用于为各种任务生成提示。但是，在某些情况下，默认提示模板可能无法满足您的需求。例如，您可能希望创建一个提示模板，其中包含语言模型的特定动态说明。在这种情况下，您可以创建自定义提示模板。

我们在写提示词模板的时候，有些情况下，我们需要给出一些例子，这样可以让模型更好的理解我们的意图。

```python
from langchain.prompts.few_shot import FewShotPromptTemplate
from langchain.prompts.prompt import PromptTemplate

#这里我们用字典来表示一个例子，每个示例都应该是一个字典，其中键是输入变量，值是这些输入变量的值。
examples = [
  {
    "question": "Who lived longer, Muhammad Ali or Alan Turing?",
    "answer":
"""
Are follow up questions needed here: Yes.
Follow up: How old was Muhammad Ali when he died?
Intermediate answer: Muhammad Ali was 74 years old when he died.
Follow up: How old was Alan Turing when he died?
Intermediate answer: Alan Turing was 41 years old when he died.
So the final answer is: Muhammad Ali
"""
  },
  {
    "question": "When was the founder of craigslist born?",
    "answer":
"""
Are follow up questions needed here: Yes.
Follow up: Who was the founder of craigslist?
Intermediate answer: Craigslist was founded by Craig Newmark.
Follow up: When was Craig Newmark born?
Intermediate answer: Craig Newmark was born on December 6, 1952.
So the final answer is: December 6, 1952
"""
  },
  {
    "question": "Who was the maternal grandfather of George Washington?",
    "answer":
"""
Are follow up questions needed here: Yes.
Follow up: Who was the mother of George Washington?
Intermediate answer: The mother of George Washington was Mary Ball Washington.
Follow up: Who was the father of Mary Ball Washington?
Intermediate answer: The father of Mary Ball Washington was Joseph Ball.
So the final answer is: Joseph Ball
"""
  },
  {
    "question": "Are both the directors of Jaws and Casino Royale from the same country?",
    "answer":
"""
Are follow up questions needed here: Yes.
Follow up: Who is the director of Jaws?
Intermediate Answer: The director of Jaws is Steven Spielberg.
Follow up: Where is Steven Spielberg from?
Intermediate Answer: The United States.
Follow up: Who is the director of Casino Royale?
Intermediate Answer: The director of Casino Royale is Martin Campbell.
Follow up: Where is Martin Campbell from?
Intermediate Answer: New Zealand.
So the final answer is: No
"""
  }
]
```

####  使用例子选择器

在上面的例子中，我们使用了一个简单的例子选择器，它只是返回全部的例子。但是，我们可以使用自定义的例子选择器来选择例子。例如，我们可以使用一个例子选择器，该选择器将选择与输入最相似的例子。

###  MessagePromptTemplate**的类型*

LangChain提供不同类型的 MessagePromptTemplate .最常用的是 AIMessagePromptTemplate 和 SystemMessagePromptTemplate HumanMessagePromptTemplate ，它们分别创建 AI 消息、系统消息和人类消息。
但是，如果聊天模型支持使用任意角色接收聊天消息，则可以使用 ChatMessagePromptTemplate ，它允许用户指定角色名称。

LangChain 还提供了 MessagesPlaceholder ，它使您可以完全控制在格式化过程中要呈现的消息。
当您不确定应为邮件提示模板使用什么角色时，或者当您希望在格式化过程中插入邮件列表时，这可能很有用。

```python
from langchain.prompts import MessagesPlaceholder,HumanMessagePromptTemplate,ChatPromptTemplate

human_prompt = "Summarize our conversation so far in {word_count} words."
human_message_template = HumanMessagePromptTemplate.from_template(human_prompt)

chat_prompt = ChatPromptTemplate.from_messages([MessagesPlaceholder(variable_name="conversation"), human_message_template])
```

```
from langchain.schema import HumanMessage,AIMessage
human_message = HumanMessage(content="What is the best way to learn programming?")
ai_message = AIMessage(content="""\
1. Choose a programming language: Decide on a programming language that you want to learn.

2. Start with the basics: Familiarize yourself with the basic programming concepts such as variables, data types and control structures.

3. Practice, practice, practice: The best way to learn programming is through hands-on experience\
""")

chat_prompt.format_prompt(conversation=[human_message, ai_message], word_count="10").to_messages()
```

###   序列化储存提示词

通常建议将提示存储为文件，而不是 python 代码。
 这可以轻松共享、存储和版本提示。
本笔记本介绍了如何在 LangChain 中执行此操作，演练了所有不同类型的提示和不同的序列化选项。
所有提示都是通过`load_prompt`函数加载的。

```python
from langchain.prompts import load_prompt
prompt = load_prompt("promptstore/simple_prompt.json")
print(prompt.format(adjective="funny", content="chickens"))
```

```python
#这显示了从 JSON 加载几个镜头示例的示例。
prompt = load_prompt("promptstore/few_shot_prompt.json")
print(prompt.format(adjective="funny"))
```

# 大语言模型

*大型语言模型（LLM）是LangChain的核心组件。LangChain不提供自己的LLM，而是提供了一个标准接口，用于与许多不同的LLM进行交互。
https://python.langchain.com/docs/modules/model_io/models/llms/

## 大语言模型的跟踪令牌使用情况
LangChain提供了一个方便的方法，用于跟踪LLM的令牌使用情况。这对于调试或教育目的非常有用。
仅适用于OpenAI API。

```python
from langchain.llms import OpenAI
from langchain.callbacks import get_openai_callback
llm = OpenAI(model_name="text-davinci-002", n=2, best_of=2,cache = None)

with get_openai_callback() as cb:
    result = llm("讲个笑话")
    print(cb)
```



# 输出解析器

语言模型输出文本。但很多时候，您可能希望获得更结构化的信息，而不仅仅是文本回复。这就是输出解析器的用武之地。
输出分析器是帮助构建语言模型响应的类。输出分析器必须实现两种主要方法：

“获取格式指令”：返回一个字符串的方法，其中包含有关如何格式化语言模型输出的说明。

“Parse”：一种接收字符串（假设是来自语言模型的响应）并将其解析为某种结构的方法。

然后是一个可选的：

“Parse with prompt”：一种方法，它接受一个字符串（假设是来自语言模型的响应）和一个提示（假设是生成此类响应的提示）并将其解析为某种结构。提示主要在 OutputParser 想要以某种方式重试或修复输出时提供，并且需要来自提示的信息才能执行此操作。

## 列表解析器

当您想要返回逗号分隔项的列表时，可以使用此输出分析器。

```python
# 导入必要的模块和类
from langchain.output_parsers import CommaSeparatedListOutputParser
from langchain.prompts import PromptTemplate, ChatPromptTemplate, HumanMessagePromptTemplate
from langchain.llms import OpenAI
from langchain.chat_models import ChatOpenAI

# 初始化一个解析由逗号分隔的列表的解析器
output_parser = CommaSeparatedListOutputParser()

# 获取解析器的格式指令
format_instructions = output_parser.get_format_instructions()

# 定义提示模板，用于生成关于特定主题的由逗号分隔的列表
prompt = PromptTemplate(
    template="List five {subject}.\n{format_instructions}",
    input_variables=["subject"],
    partial_variables={"format_instructions": format_instructions}
)

# 初始化OpenAI模型，温度设置为0（生成的回答将更加确定，少有随机性）
model = OpenAI(temperature=0)

# 格式化提示，这里的主题是“ice cream flavors”（冰淇淋口味）
_input = prompt.format(subject="冰淇淋口味")

# 使用模型生成输出
output = model(_input)

# 使用解析器解析输出
output_parser.parse(output)
```

## 日期时间解析器

此输出解析器显示将LLM输出解析为日期时间格式。

```python
# 导入必要的模块和类
from langchain.prompts import PromptTemplate
from langchain.output_parsers import DatetimeOutputParser
from langchain.chains import LLMChain
from langchain.llms import OpenAI

\# 初始化一个日期时间输出解析器
output_parser = DatetimeOutputParser()

\# 定义提示模板，用于引导模型回答用户的问题
template = """回答用户的问题:

{question}

{format_instructions}"""

\# 使用模板创建一个提示实例
prompt = PromptTemplate.from_template(
    template,
    partial_variables={"format_instructions": output_parser.get_format_instructions()},
)

\# 初始化一个LLMChain，它结合了提示和OpenAI模型来生成输出
chain = LLMChain(prompt=prompt, llm=OpenAI())

\# 运行链来获取关于特定问题的答案，这里的问题是“bitcoin是什么时候成立的？”
output = chain.run("bitcoin是什么时候成立的？用英文格式输出时间")
```

##  枚举解析器

```python
from langchain.output_parsers.enum import EnumOutputParser
from enum import Enum


class Colors(Enum):
    RED = "red"
    GREEN = "green"
    BLUE = "blue"

parser = EnumOutputParser(enum=Colors)
```

##     

```python
parser.parse("red")

# Can handle spaces
parser.parse(" green")

# And new lines
parser.parse("blue\n")

# And raises errors when appropriate
parser.parse("yellow")
```



##     5.4 自动修复解析器      

​        此输出解析器包装另一个输出解析器，如果第一个输出解析器失败，它会调用另一个 LLM 以修复任何错误。      

​        但是除了抛出错误之外，我们还可以做其他事情。具体来说，我们可以将格式错误的输出以及格式化的指令传递给模型，并要求它修复它。      

```python
# 导入所需的库和模块
from langchain.prompts import PromptTemplate, ChatPromptTemplate, HumanMessagePromptTemplate
from langchain.llms import OpenAI
from langchain.chat_models import ChatOpenAI
from langchain.output_parsers import PydanticOutputParser
from pydantic import BaseModel, Field, validator
from typing import List

\# 定义一个表示演员的数据结构，包括他们的名字和他们出演的电影列表
class Actor(BaseModel):
    name: str = Field(description="name of an actor")                   # 演员的名字
    film_names: List[str] = Field(description="list of names of films they starred in")  # 他们出演的电影列表

\# 定义一个查询，用于提示生成随机演员的电影作品列表
actor_query = "Generate the filmography for a random actor."

\# 使用`Actor`模型初始化解析器
parser = PydanticOutputParser(pydantic_object=Actor)

\# 定义一个格式错误的字符串数据
misformatted = "{'name': 'Tom Hanks', 'film_names': ['Forrest Gump']}"
# 使用解析器尝试解析上述数据
parser.parse(misformatted)
```

```python
# 要使misformatted字符串正确格式化，您需要确保它是一个有效的JSON字符串。这意味着您应该使用双引号（"）而不是单引号（'）来包围键和值。以下是修改后的
parser.parse('{"name": "Tom Hanks", "film_names": ["Forrest Gump"]}')
```

```python
from langchain.output_parsers import OutputFixingParser

new_parser = OutputFixingParser.from_llm(parser=parser, llm=ChatOpenAI())
new_parser.parse(misformatted)
```


## 结构化输出解析器
当您想要返回多个字段时，可以使用此输出分析器。

虽然 Pydantic/JSON 解析器功能更强大，但我们最初尝试使用仅包含文本字段的数据结构。

```python
# 导入必要的模块和类
from langchain.output_parsers import StructuredOutputParser, ResponseSchema
from langchain.prompts import PromptTemplate, ChatPromptTemplate, HumanMessagePromptTemplate
from langchain.llms import OpenAI
from langchain.chat_models import ChatOpenAI

# 定义响应模式，用于指导模型提供特定类型的输出
response_schemas = [
    ResponseSchema(name="answer", description="回答用户的问题"),
    ResponseSchema(name="source", description="回答用户问题的来源，应为一个网站。")
]

# 使用响应模式初始化一个结构化输出解析器
output_parser = StructuredOutputParser.from_response_schemas(response_schemas)

# 获取解析器的格式指令
format_instructions = output_parser.get_format_instructions()

# 定义提示模板，用于引导模型根据用户的问题提供答案及答案的来源
prompt = PromptTemplate(
    template="尽可能回答用户的问题。\n{format_instructions}\n{question}",
    input_variables=["question"],
    partial_variables={"format_instructions": format_instructions}
)

# 初始化OpenAI模型，温度设置为0（生成的回答将更加确定，少有随机性）
model = OpenAI(temperature=0)

# 格式化提示，这里的问题是“法国的首都是什么？”
_input = prompt.format_prompt(question="法国的首都是什么？")

# 使用模型生成输出
output = model(_input.to_string())

# 使用解析器解析输出
output_parser.parse(output)
```

```python
# 导入必要的模块和类
from langchain.chat_models import ChatOpenAI
from langchain.prompts import ChatPromptTemplate, HumanMessagePromptTemplate

# 初始化聊天模型，温度设置为0（生成的回答将更加确定，少有随机性）
chat_model = ChatOpenAI(temperature=0)

# 定义聊天提示模板，该模板包含一系列的消息对象
# 这里我们只定义一个消息，该消息提示模型尽可能地回答用户的问题
prompt = ChatPromptTemplate(
    messages=[
        HumanMessagePromptTemplate.from_template("尽可能回答用户的问题。\n{format_instructions}\n{question}")
    ],
    input_variables=["question"],
    partial_variables={"format_instructions": format_instructions}
)

# 格式化提示，这里的问题是“法国的首都是什么？”
_input = prompt.format_prompt(question="法国的首都是什么？")

# 使用聊天模型生成输出
output = chat_model(_input.to_messages())

# 使用解析器解析输出的内容
output_parser.parse(output.content)
```



# 文档加载器      

许多LLM应用程序需要用户特定的数据，这些数据不属于模型的训练集。

实现此目的的主要方法是通过检索增强生成 （RAG）。在此过程中，检索外部数据，然后在执行生成步骤时传递给LLM。

LangChain为RAG应用程序提供了所有构建块 - 从简单到复杂。文档的这一部分涵盖了与检索步骤相关的所有内容 - 例如数据的获取。

虽然这听起来很简单，但它可能非常复杂。这包括几个关键模块。



从许多不同的来源加载文档。LangChain提供了100多种不同的文档加载器，以及与该领域其他主要提供商的集成，如AirByte和Unstructured。

我们提供集成，从所有类型的位置（私有 s3 存储桶、公共网站）加载所有类型的文档（html、PDF、代码）。

使用文档加载器以 的形式从 Document 源加载数据。



A Document 是一段文本和关联的元数据。

例如，有用于加载简单 .txt 文件、加载任何网页的文本内容甚至加载 YouTube 视频脚本的文档加载器。

文档加载程序公开一个“加载”方法，用于将数据作为文档从配置的源加载。它们还可以选择实现“延迟加载”，以便将数据延迟加载到内存中。

```python
from langchain.document_loaders import TextLoader

loader = TextLoader("documentstore/index.md")
```

加载 CSV 数据，每个文档一行。

```python
from langchain.document_loaders.csv_loader import CSVLoader



loader = CSVLoader(file_path='documentstore/index.csv')
data = loader.load()
```

自定义 csv 解析和加载

```python
loader = CSVLoader(file_path='documentstore/index.csv', csv_args={
    'delimiter': ',',
    'quotechar': '"',
    'fieldnames': ['title','context']
})

data = loader.load()
```

使用该 source_column 参数为从每一行创建的文档指定源。否则 file_path 将用作从 CSV 文件创建的所有文档的源。

```python
loader = CSVLoader(file_path='documentstore/index.csv', source_column="context")

data = loader.load()
```

更改加载程序类

```python
from langchain.document_loaders import PythonLoader
loader = DirectoryLoader('E:\\pycharmproject\\LangChainStudyProject\\LangChainStudyProject', glob="*.py", loader_cls=PythonLoader)
docs = loader.load()
```



# 文档转换器      

加载文档后，您通常需要转换它们以更好地适合您的应用程序。

最简单的示例是，您可能希望将长文档拆分为可以放入模型上下文窗口的较小块。

LangChain 有许多内置的文档转换器，可以轻松拆分、组合、过滤和以其他方式操作文档。
当您想要处理长文本时，有必要将该文本拆分为块。

听起来很简单，但这里有很多潜在的复杂性。

理想情况下，您希望将语义相关的文本片段放在一起。

“语义相关”的含义可能取决于文本的类型。本笔记本展示了实现此目的的几种方法。

默认推荐的文本拆分器是 RecursiveCharacterTextSplitter。此文本拆分器采用字符列表。它尝试基于第一个字符的拆分来创建块，但如果任何块太大，它就会移动到下一个字符，依此类推。默认情况下，它尝试拆分的字符是 ["\n\n", "\n", " ", ""]

### text_splitter

```python
# This is a long document we can split up.
with open('documentstore/state_of_the_union.txt') as f:
    state_of_the_union = f.read()

from langchain.text_splitter import RecursiveCharacterTextSplitter
text_splitter = RecursiveCharacterTextSplitter(
    # Set a really small chunk size, just to show.
    chunk_size = 100,
    chunk_overlap  = 20,
    length_function = len,
    add_start_index = True,
)
texts = text_splitter.create_documents([state_of_the_union])
print(texts[0])
print(texts[1])
```

下面是将元数据与文档一起传递的示例，请注意，元数据与文档一起拆分。

```python
metadatas = [{"document": 1}, {"document": 2}]
documents = text_splitter.create_documents([state_of_the_union, state_of_the_union], metadatas=metadatas)
print(documents[0])
```

### CodeTextSplitter 

CodeTextSplitter 允许您使用多种语言支持拆分代码。导入枚举 Language 并指定语言。

```python
from langchain.text_splitter import (
    RecursiveCharacterTextSplitter,
    Language,
)
# Full list of support languages
[e.value for e in Language]
```

###  PythonTextSplitter

下面是一个使用 PythonTextSplitter 的示例

```python
PYTHON_CODE = """
def hello_world():
    print("Hello, World!")

\# Call the function
hello_world()
"""
python_splitter = RecursiveCharacterTextSplitter.from_language(
    language=Language.PYTHON, chunk_size=50, chunk_overlap=0
)
python_docs = python_splitter.create_documents([PYTHON_CODE])
python_docs
```

### js_splitter 

下面是一个使用 JS 文本拆分器的示例

```
JS_CODE = """
function helloWorld() {
  console.log("Hello, World!");
}

// Call the function
helloWorld();
"""

js_splitter = RecursiveCharacterTextSplitter.from_language(
    language=Language.JS, chunk_size=60, chunk_overlap=0
)
js_docs = js_splitter.create_documents([JS_CODE])
```

### Markdown



~~~python
markdown_text = """
# 🦜️🔗 LangChain

⚡ Building applications with LLMs through composability ⚡

## Quick Install

```bash
# Hopefully this code block isn't split
pip install langchain
```

As an open source project in a rapidly developing field, we are extremely open to contributions.
"""
md_splitter = RecursiveCharacterTextSplitter.from_language(
    language=Language.MARKDOWN, chunk_size=60, chunk_overlap=0
)
md_docs = md_splitter.create_documents([markdown_text])
~~~

### 按字符递归拆分

```
from langchain.text_splitter import RecursiveCharacterTextSplitter
# This is a long document we can split up.
with open('documentstore/state_of_the_union.txt') as f:
    state_of_the_union = f.read()
text_splitter = RecursiveCharacterTextSplitter(
    # Set a really small chunk size, just to show.
    chunk_size = 100,
    chunk_overlap  = 20,
    length_function = len,
)
texts = text_splitter.create_documents([state_of_the_union])
print(texts[0])
print(texts[1])
```



##  文本嵌入模型
嵌入类是设计用于与文本嵌入模型接口的类。有很多嵌入模型提供程序（OpenAI，Cohere，Hugging Face等） - 这个类旨在为所有它们提供一个标准接口。

嵌入创建一段文本的矢量表示形式。这很有用，因为这意味着我们可以考虑向量空间中的文本，并执行诸如语义搜索之类的操作，在其中我们寻找向量空间中最相似的文本片段。

LangChain 中的基本嵌入类公开了两种方法：一种用于嵌入文档，另一种用于嵌入查询。前者将多个文本作为输入，而后者则采用单个文本。将它们作为两个独立方法的原因是，某些嵌入提供程序对文档（要搜索）和查询（搜索查询本身）具有不同的嵌入方法。

```python
from langchain.embeddings import OpenAIEmbeddings

embeddings_model = OpenAIEmbeddings()

embeddings = embeddings_model.embed_documents(
    [
        "Hi there!",
        "Oh, hello!",
        "What's your name?",
        "My friends call me World",
        "Hello World!"
    ]
)
len(embeddings), len(embeddings[0])
```

> 嵌入可以存储或临时缓存，以避免需要重新计算它们。

```python
from langchain.storage import InMemoryStore, LocalFileStore
from langchain.embeddings import OpenAIEmbeddings, CacheBackedEmbeddings
from langchain.document_loaders import TextLoader
from langchain.embeddings.openai import OpenAIEmbeddings
from langchain.text_splitter import CharacterTextSplitter
from langchain.vectorstores import FAISS
underlying_embeddings = OpenAIEmbeddings()
fs = LocalFileStore("./cache/")

cached_embedder = CacheBackedEmbeddings.from_bytes_store(
    underlying_embeddings, fs, namespace=underlying_embeddings.model
)
```

加载文档，将其拆分为块，嵌入每个块并将其加载到矢量存储中。

```python
raw_documents = TextLoader("documentstore/state_of_the_union.txt").load()
text_splitter = CharacterTextSplitter(chunk_size=1000, chunk_overlap=0)
documents = text_splitter.split_documents(raw_documents)
#create the vectorstore 创建矢量存储
db = FAISS.from_documents(documents, cached_embedder)
```

如果我们尝试再次创建 vectostore，它会快得多，因为它不需要重新计算任何嵌入。

```python
db2 = FAISS.from_documents(documents, cached_embedder)
```

```python
store = InMemoryStore()
underlying_embeddings = OpenAIEmbeddings()
embedder = CacheBackedEmbeddings.from_bytes_store(
    underlying_embeddings, store, namespace=underlying_embeddings.model
)
embeddings = embedder.embed_documents(["hello", "goodbye"])
```

# 向量数据库

存储和搜索非结构化数据的最常见方法之一是嵌入它并存储生成的嵌入向量，

然后在查询时嵌入非结构化查询并检索与嵌入查询“最相似”的嵌入向量。

矢量存储负责存储嵌入数据并为您执行矢量搜索。

```python
from langchain.embeddings.openai import OpenAIEmbeddings
from langchain.vectorstores import Chroma
\# 使用向量数据库chroma，利用openai的embadding对文本进行向量化
db = Chroma.from_texts(list_text, OpenAIEmbeddings())
```

> 相似性搜索

```python
query = "客户的身份证号是多少"
docs = db.similarity_search(query)
print(docs[0].page_content)
```

按向量搜索相似性

```python
embedding_vector = OpenAIEmbeddings().embed_query(query)
print(f'embedding_vector：{embedding_vector}')
docs = db.similarity_search_by_vector(embedding_vector)
print(docs[0].page_content)
```

保存和加载

```
db.save_local("faiss_index")
new_db = FAISS.load_local("faiss_index", OpenAIEmbeddings())
```

