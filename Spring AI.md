Spring AI是spring官方新推出的一个模块，地位跟Spring Batch, Spring Security等相同，属于独立的一个模块。

其官方的介绍如下

<font style="color:rgb(0, 0, 0);">Spring AI is an application framework for AI engineering. Its goal is to apply to the AI domain Spring ecosystem design principles such as portability and modular design and promote using POJOs as the building blocks of an application to the AI domain.</font>

<font style="color:rgb(0, 0, 0);"></font>

<font style="color:rgb(0, 0, 0);">翻译下大概就是这个模块的主要作用就是利用Spring生态系统的设计原则跟优点来快速使用AI功能，并集成到自己的应用。</font>

<font style="color:rgb(0, 0, 0);"></font>

<font style="color:rgb(0, 0, 0);">Spring AI支持主流的聊天，图片，语音AI模型，比如</font>

+ OpenAI
+ Amazon Bedrock
+ Google Gemini

.....

Spring AI提供了starter来快速使用对应的AI模型，我们以接入OpenAI为例来说明。



### 使用spring initializr初始化一个spring boot应用
注意spring ai只支持jdk17及以上，dependency选择spring web及OpenAI两个dependency,

![](https://cdn.nlark.com/yuque/0/2024/png/26411187/1712837555693-afb298e4-ab81-4a4c-8ce6-598b071a40ab.png)



### 修改配置文件中的OpenAI credential
```java
spring.ai.openai.api-key=YOUR_API_KEY
spring.ai.openai.chat.options.model=gpt-3.5-turbo
spring.ai.openai.chat.options.temperature=0.7
```





### 创建Controller
```java
@RestController
public class ChatController {

    private final OpenAiChatClient chatClient;

    @Autowired
    public ChatController(OpenAiChatClient chatClient) {
        this.chatClient = chatClient;
    }

    @GetMapping("/ai/generate")
    public Map generate(@RequestParam(value = "message", defaultValue = "Tell me a joke") String message) {
        return Map.of("generation", chatClient.call(message));
    }

    @GetMapping("/ai/generateStream")
	public Flux<ChatResponse> generateStream(@RequestParam(value = "message", defaultValue = "Tell me a joke") String message) {
        Prompt prompt = new Prompt(new UserMessage(message));
        return chatClient.stream(prompt);
    }
}
```



接一下就可以通过/ai/generate这个api来进行ai的调用了，是不是很简单！

curl "localhost:8080/ai/generate?message=test" 

