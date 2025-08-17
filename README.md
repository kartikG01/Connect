# Connect
ort { useState, useRef, useEffect } from 'react';
import { Send } from 'lucide-react';
import { Button } from '/components/ui/button';
import { Input } from '/components/ui/input';

type Message = {
  id: string;
  text: string;
  sender: 'user' | 'other';
  timestamp: Date;
};

export default function ChatApp() {
  const [messages, setMessages] = useState<Message[]>([
    {
      id: '1',
      text: 'Hey there! How are you doing today?',
      sender: 'other',
      timestamp: new Date(Date.now() - 300000),
    },
    {
      id: '2',
      text: 'I\'m doing great! Just finished working on that project.',
      sender: 'user',
      timestamp: new Date(Date.now() - 240000),
    },
    {
      id: '3',
      text: 'That\'s awesome! What project were you working on?',
      sender: 'other',
      timestamp: new Date(Date.now() - 180000),
    },
  ]);
  const [inputValue, setInputValue] = useState('');
  const messagesEndRef = useRef<HTMLDivElement>(null);

  const handleSend = () => {
    if (inputValue.trim() === '') return;

    const newMessage: Message = {
      id: Date.now().toString(),
      text: inputValue,
      sender: 'user',
      timestamp: new Date(),
    };

    setMessages([...messages, newMessage]);
    setInputValue('');

    // Simulate reply after a delay
    setTimeout(() => {
      const replyMessage: Message = {
        id: (Date.now() + 1).toString(),
        text: `Thanks for your message: "${inputValue}"`,
        sender: 'other',
        timestamp: new Date(),
      };
      setMessages(prev => [...prev, replyMessage]);
    }, 1000);
  };

  const handleKeyPress = (e: React.KeyboardEvent) => {
    if (e.key === 'Enter' && !e.shiftKey) {
      e.preventDefault();
      handleSend();
    }
  };

  // Scroll to bottom when messages change
  useEffect(() => {
    messagesEndRef.current?.scrollIntoView({ behavior: 'smooth' });
  }, [messages]);

  return (
    <div className="flex flex-col h-screen max-w-md mx-auto bg-white border border-gray-200 rounded-lg shadow-lg overflow-hidden">
      {/* Chat Header */}
      <div className="bg-indigo-600 text-white p-4 flex items-center">
        <div className="bg-gray-200 border-2 border-dashed rounded-xl w-10 h-10" />
        <div className="ml-3">
          <h2 className="font-semibold">Alex Johnson</h2>
          <p className="text-xs text-indigo-200">Online</p>
        </div>
      </div>

      {/* Messages Container */}
      <div className="flex-1 overflow-y-auto p-4 bg-gray-50">
        <div className="space-y-4">
          {messages.map((message) => (
            <div
              key={message.id}
              className={`flex ${message.sender === 'user' ? 'justify-end' : 'justify-start'}`}
            >
              <div
                className={`max-w-xs lg:max-w-md px-4 py-2 rounded-lg ${
                  message.sender === 'user'
                    ? 'bg-indigo-500 text-white rounded-br-none'
                    : 'bg-white border border-gray-200 rounded-bl-none'
                }`}
              >
                <p className="text-sm">{message.text}</p>
                <p
                  className={`text-xs mt-1 ${
                    message.sender === 'user' ? 'text-indigo-200' : 'text-gray-500'
                  }`}
                >
                  {message.timestamp.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })}
                </p>
              </div>
            </div>
          ))}
          <div ref={messagesEndRef} />
        </div>
      </div>

      {/* Input Area */}
      <div className="border-t border-gray-200 p-3 bg-white">
        <div className="flex items-center">
          <Input
            type="text"
            value={inputValue}
            onChange={(e) => setInputValue(e.target.value)}
            onKeyPress={handleKeyPress}
            placeholder="Type a message..."
            className="flex-1 mr-2"
          />
          <Button 
            onClick={handleSend} 
            disabled={inputValue.trim() === ''}
            className="bg-indigo-600 hover:bg-indigo-700"
          >
            <Send className="h-4 w-4" />
          </Button>
        </div>
      </div>
    </div>
  );
}
Share
