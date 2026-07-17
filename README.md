// websocket-chat.js — WebSocket asosida oddiy chat mijozi
class ChatClient {
  constructor(url) {
    this.url = url;
    this.socket = null;
    this.reconnectAttempts = 0;
  }

  connect() {
    this.socket = new WebSocket(this.url);

    this.socket.onopen = () => {
      console.log('✅ Serverga ulanildi');
      this.reconnectAttempts = 0;
      this.send({ type: 'join', user: 'Aziz' });
    };

    this.socket.onmessage = (event) => {
      const data = JSON.parse(event.data);
      this.handleMessage(data);
    };

    this.socket.onerror = (error) => {
      console.error('❌ WebSocket xatosi:', error);
    };

    this.socket.onclose = (event) => {
      console.log(`🔌 Ulanish yopildi (kod: ${event.code})`);
      this.tryReconnect();
    };
  }

  handleMessage(data) {
    switch (data.type) {
      case 'message':
        console.log(`${data.user}: ${data.text}`);
        break;
      case 'user_joined':
        console.log(`👋 ${data.user} chatga qo'shildi`);
        break;
      case 'user_left':
        console.log(`👋 ${data.user} chatdan chiqdi`);
        break;
      default:
        console.log('Noma\'lum xabar turi:', data);
    }
  }

  send(data) {
    if (this.socket && this.socket.readyState === WebSocket.OPEN) {
      this.socket.send(JSON.stringify(data));
    } else {
      console.warn('Ulanish ochiq emas, xabar yuborilmadi');
    }
  }

  sendMessage(text) {
    this.send({ type: 'message', user: 'Aziz', text });
  }

  tryReconnect() {
    if (this.reconnectAttempts < 5) {
      this.reconnectAttempts++;
      const delay = this.reconnectAttempts * 1000;
      console.log(`Qayta ulanish ${delay}ms dan so'ng...`);
      setTimeout(() => this.connect(), delay);
    }
  }

  disconnect() {
    if (this.socket) {
      this.socket.close(1000, 'Foydalanuvchi chiqdi');
    }
  }
}

// Foydalanish
const chat = new ChatClient('wss://echo.example.com/chat');
chat.connect();

setTimeout(() => {
  chat.sendMessage("Salom hammaga!");
}, 2000);
