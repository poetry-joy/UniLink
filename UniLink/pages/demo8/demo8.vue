<template>
  <view class="chat-app">
    <!-- <!-- 右侧聊天窗口 -->
    <view class="chat-window">
      <view class="chat-header">
        <img :src="currentContact.avatar" alt="头像" class="avatar" @click="nav(currentContact)"/>
        <h4>{{ currentContact.name }}</h4>
      </view>
      <view class="chat-content" ref="chatContent">
        <view
          v-for="message in currentContact.messages"
          :key="message.id"
          :class="['chat-message', message.sender === 'me' ? 'sent' : 'received']"
        >
          <p>{{ message.text }}</p>
          <span class="message-time">{{ message.time }}</span>
        </view>
      </view> 

      <view class="chat-input">
        <input
          type="text"
          v-model="newMessage"
          placeholder="输入消息..."
          @keyup.enter="sendMessage"
        />
        <button @click="sendMessage"><span class="send">发送</span></button>
      </view>
    </view>
  </view>
</template>
<script>
import {
	ref,
	onMounted,
	getCurrentInstance,
	computed
} from 'vue'; // 引入 onMounted 和 getCurrentInstance
export default {
  data() {
    return {
      contacts1: [
		{
		  id: 0,
		  name: '共谋大事组',
		  avatar: '/static/toux1.jpg',
		  lastMessage: 'Hey, how are you?',
		  messages: [],
		},
        {
          id: 1,
          name: '张栋',
          avatar: '/static/toux13.jpg',
          lastMessage: 'Hey, how are you?',
          messages: [
            { id: 1, text: '你们的项目完成的怎么样了?', sender: '张栋', time: '10:01 AM' },
            { id: 12, text: '哦!my dear tutor~实在是太抱歉了！完成进度堪忧啊我们', sender: 'me', time: '10:02 AM' }
          ]
        },
        {
          id: 2,
          name: '张诗雨',
          avatar: '/static/toux2.jpg',
          lastMessage: 'What’s up?',
          messages: [
			{ id: 12, text: '你能不能好好按时完成项目?求求你咯！', sender: 'me', time: '11:30 AM' },
			{ id: 3, text: '还没睡醒', sender: '张诗雨', time: '11:32 AM' },
          ]
        },
		{
		  id: 3,
		  name: '詹镇號',
		  avatar: '/static/toux10.jpg',
		  lastMessage: 'Hey, how are you?',
		  messages: [
		    { id: 12, text: '你能不能好好按时完成项目?求求你咯！', sender: 'me', time: '10:01 AM' },
		    { id: 4, text: '还在睡', sender: '詹镇號', time: '10:02 AM' }
		  ]
		},
		{
		  id: 4,
		  name: '刘哲锐',
		  avatar: '/static/toux12.jpg',
		  lastMessage: 'Hey, how are you?',
		  messages: [
		    { id: 12, text: '你能不能好好按时完成项目?求求你咯！', sender: 'me', time: '10:02 AM' },
		    { id: 5, text: 'wa', sender: '刘哲锐', time: '10:15 AM' }
		  ]
		},
		{
		  id: 5,
		  name: '黄悦迦',
		  avatar: '/static/toux7.jpg',
		  lastMessage: 'Hey, how are you?',
		  messages: [
		    { id: 12, text: '你能不能好好按时完成项目?求求你咯！', sender: 'me', time: '10:03 AM' },
		    { id: 6, text: '瓦', sender: '黄悦迦', time: '10:15 AM' }
		  ]
		},
		{
		  id: 6,
		  name: '朱佳捷',
		  avatar: '/static/toux14.jpg',
		  lastMessage: 'Hey, how are you?',
		  messages: [
		    { id: 12, text: '你能不能好好按时完成项目?求求你咯！', sender: 'me', time: '10:04 AM' },
		    { id: 7, text: '瓦？', sender: '朱佳捷', time: '10:15 AM' }
		  ]
		},
		{
		  id: 7,
		  name: '吴三丰',
		  avatar: '/static/toux8.jpg',
		  lastMessage: 'Hey, how are you?',
		  messages: [
		    { id: 12, text: '你能不能好好按时完成项目?求求你咯！', sender: 'me', time: '10:05 AM' },
		    { id: 8, text: '瓦罗兰特', sender: '吴三丰', time: '10:15 AM' }
		  ]
		},
		{
		  id: 8,
		  name: '严加一',
		  avatar: '/static/toux6.jpg',
		  lastMessage: 'Hey, how are you?',
		  messages: [
		    { id: 12, text: '你能不能好好按时完成项目?求求你咯！', sender: 'me', time: '10:06 AM' },
		    { id: 9, text: '启动！！', sender: '严加一', time: '10:15 AM' }
		  ]
		},
		{
		  id: 9,
		  name: '黄馨贻',
		  avatar: '/static/toux15.jpg',
		  lastMessage: 'Hey, how are you?',
		  messages: [
		    { id: 12, text: '你能不能好好按时完成项目?求求你咯！', sender: 'me', time: '10:07 AM' },
		    { id: 10, text: '好好好，马上马上', sender: '黄馨贻', time: '10:12 AM' }
		  ]
		},
		{
		  id: 10,
		  name: '韦❤茹',
		  avatar: '/static/toux4.jpg',
		  lastMessage: 'Hey, how are you?',
		  messages: [
		    { id: 12, text: '你能不能好好按时完成项目?求求你咯！', sender: 'me', time: '10:08 AM' },
		    { id: 9, text: '我是一枚小可爱', sender: '韦❤茹', time: '10:10 AM' }
		  ]
		},
		{
		  id: 11,
		  name: '郑❤艺',
		  avatar: '/static/toux5.jpg',
		  lastMessage: 'Hey, how are you?',
		  messages: [
		    { id: 12, text: '你能不能好好按时完成项目?求求你咯！', sender: 'me', time: '10:09 AM' },
		    { id: 9, text: '小韦帮我做，你找她', sender: '郑❤艺', time: '10:10 AM' }
		  ]
		},
      ],
      currentContact: null, // 当前选中的联系人
      newMessage: '' ,// 新消息输入框
	  ID: null,
    };
  },
  methods: {
    // 选择联系人进行聊天
    selectContact(contact1) {
      this.currentContact = contact1;
      this.$nextTick(() => {
        // 自动滚动到最新消息
        this.scrollToBottom();
      });
    },
	nav(contact){
		this.currentContact = contact;
		console.log('Current contact:', this.currentContact.id);
		// 这里可以添加跳转到聊天页面的代码，例如使用vue-router  
		console.log('Current contact:', this.currentContact.id);  
		if (this.currentContact && (this.currentContact.id || this.currentContact.id===0)) {
		  console.log(this.currentContact.id);
				uni.navigateTo({
				  url: `/pages/demo9/demo9?id=${this.currentContact.id}`,
				  success: () => {
				    console.log(`跳转到项目 ID 为 ${this.currentContact.id} 的详情页面`);
				  },
				  fail: (err) => {
				    console.error('跳转失败：', err);
				  }
				});
		} else {
		  console.error('currentContact 未定义或没有 id');
		}
	},
    // 发送消息
    sendMessage() {
      if (this.newMessage.trim() === '') return; // 如果输入为空，直接返回

      const message = {
        id: Date.now(),
        text: this.newMessage,
        sender: 'me',
        time: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
      };

      // 将新消息加入当前联系人的消息列表
      this.currentContact.messages.push(message);
      this.newMessage = ''; // 清空输入框

      this.$nextTick(() => {
        // 发送完消息后自动滚动到底部
        this.scrollToBottom();
      });
    },
    // 滚动到最新消息
    scrollToBottom() {
      const chatContent = this.$refs.chatContent;
      chatContent.scrollTop = chatContent.scrollHeight;
    }
  },
  mounted() {
    // 默认选择第一个联系人
	const contactId = this.$route.query.id; // 获取 URL 中的 id 参数
	// console.log(contactId);
	this.ID=contactId;
	// console.log(this.ID);
    this.currentContact = this.contacts1[this.ID];
  }
};
</script>
<style scoped>
.chat-app {
  display: flex;
  height: 100vh;
  width: 90vh;
  background-color: #f0f0f0;
}

.contact-list {
  width: 25%;
  background-color: #fff;
  border-right: 1px solid #ddd;
  overflow-y: auto;
}

.contact-item {
  padding: 20px;
  display: flex;
  align-items: center;
  cursor: pointer;
}

.contact-item.active {
  background-color: #f0f8ff;
}

.contact-item:hover {
  background-color: #e6f7ff;
}

.avatar {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  margin-right: 15px;
}

.contact-info h4 {
  margin: 0;
  font-size: 16px;
}

.contact-info p {
  margin: 5px 0 0;
  font-size: 12px;
  color: #888;
}

.chat-window {
  width: 100vw;
  height: 95vh;
  display: flex;
  flex-direction: column;
}

.chat-header {
  background-color: #fff;
  padding: 20px;
  display: flex;
  align-items: center;
  border-bottom: 1px solid #999;
}

.chat-header h4 {
  margin-left: 15px;
}

.chat-content {
  flex-grow: 1;
  display: flex;
  flex-direction: column;
  padding: 20px;
  overflow-y:auto;
  background-color: #f9f9f9;
}

.chat-message {
  margin-bottom: 25px;
  max-width: 70%;
  padding: 10px;
  border-radius: 8px;
  position: relative;
}

.sent {
  background-color: #007bff;
  color: white;
  align-self: flex-end;
  position: relative;
}

.received {
  background-color: #eee;
  align-self: flex-start;
}

.message-time {
  font-size: 10px;
  color: #000;
  margin-top: 5px;
  display: block;
  text-align: right;
}

.chat-input {
  display: flex;
  padding: 10px;
  background-color: #fff;
  border-top: 1px solid #ddd;
  width: 95vw;
  height: 7vh;
}

.chat-input input {
  flex-grow: 1;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 20px;
  margin-right: 10px;
}

.chat-input button {
  font-size: 15px;
  width: 50px;
  height: 40px;
  padding: 2px 10px;
  border: none;
  background-color: #007bff;
  color: white;
  border-radius: 20px;
  cursor: pointer;
  white-space: nowrap;
}

.chat-input button:hover {
  background-color: #0056b3;
}
</style>
