这个项目主要关注构建一个与聊天机器人进行互动的界面。

它使用了 Vue.js 和 Vuex 来管理组件和状态。用户可以从列表中选择一个机器人进行聊天，选择的机器人及其信息将在Header组件中显示，而所有可用的机器人则会在SideBar组件中列出。这些都通过Vuex进行状态管理，以确保前端的状态和数据保持同步。具体来说，项目有如下几个主要方面：

### 数据结构（Vuex Store）

1. State
   1. `currentBotIndex`：用于追踪当前选中的机器人的索引。
   2. `bots`：包含所有聊天机器人信息的数组。
2. Actions
   1. `fetchBots`：用于从一个名为 `bots.json` 的文件中获取机器人的信息。
3. Mutations
   1. `SEND_MESSAGE`：用于向某个机器人或用户发送消息。
   2. `SET_CURRENT_BOT`：设置当前与用户进行互动的机器人。
   3. `INIT_BOTS`：初始化机器人列表。

### Vue组件

1. Header组件
   1. 显示当前活动机器人的名字和头像。
2. SideBar组件
   1. 显示所有机器人的列表。
   2. 允许用户通过点击来选择一个机器人。
   3. 显示每个机器人最后一条消息和时间。

### 方法

1. `fetchBots`（在Vuex的Actions中）：异步加载机器人的数据。
2. `selectBot`（在SideBar组件中）：允许用户选择一个机器人。

### 数据字段

- 每个`bot`对象有：
  - `name`
  - `avatar`：头像
  - `init_prompt`：初始对话。
  - `messages`：与机器人的聊天记录。
- 每条`message`对象有：
  - `content`：消息内容。
  - `sender`：发送者。
  - `receiver`：接收者。
  - `time`：发送时间。

### 数据绑定和页面呈现

1. Header组件
   1. 在页面顶部呈现当前选中的机器人的名字和头像。
2. SideBar组件
   1. 在页面侧边栏呈现所有机器人列表。

用户可以从列表中选择一个机器人进行聊天，选择的机器人及其信息将在Header组件中显示，而所有可用的机器人则会在SideBar组件中列出。这些都通过Vuex进行状态管理，以确保前端的状态和数据保持同步。

#### 与API接口交互的部分位于Vuex的`mutations`里的`INIT_BOTS`函数中。这里使用了axios库来发送POST请求到服务器。

代码如下

```JavaScript
const path = `http://${window.location.hostname}:5000/get_answer`
            axios.post(path, param)
                .then((res) => {
                    this.commit('SEND_MESSAGE', {
                        content: res.data['answer'],
                        sender: bot.name,
                        receiver: 'user',
                        time: util.formatDate.format(new Date(), 'yyyy-MM-dd hh:mm:ss')
                    })
                })
                .catch((error) => {
                    this.commit('SEND_MESSAGE', {
                        content: `${bot.name} happened error: ${error}`,
                        sender: bot.name,
                        receiver: 'user',
                        time: util.formatDate.format(new Date(), 'yyyy-MM-dd hh:mm:ss')
                    })
                })
```

它发送一个POST请求，带有`bot`和`prompt`作为参数，然后等待一个响应，该响应应包含机器人的回答。
