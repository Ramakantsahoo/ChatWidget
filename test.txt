import React, { useState, useEffect, useRef } from 'react';
import './ChatWidget.css';
import SendRoundedIcon from '@mui/icons-material/SendRounded';
import ChatBubbleOutlineRoundedIcon from '@mui/icons-material/ChatBubbleOutlineRounded';
import AndroidRoundedIcon from '@mui/icons-material/AndroidRounded';

const ChatWidget = () => {
  const [isChatOpen, setIsChatOpen] = useState(false);
  const [messages, setMessages] = useState([]);
  const [inputValue, setInputValue] = useState('');
  const [isTyping, setIsTyping] = useState(false);
  const messagesEndRef = useRef(null);

  const handleChatButtonClick = () => {
    setIsChatOpen(true);
  };

  const handleCloseChat = () => {
    setIsChatOpen(false);
  };

  const handleSendMessage = () => {
    if (inputValue.trim() !== '') {
      const userMessage = { text: inputValue, sender: 'user' };
      setMessages((prevMessages) => [...prevMessages, userMessage]);
      setInputValue('');
      
      // Show typing indicator
      setIsTyping(true);

      // Dummy bot response
      setTimeout(() => {
        const botMessage = { text: 'This is a dummy response.', sender: 'bot' };
        setMessages((prevMessages) => [...prevMessages, botMessage]);
        setIsTyping(false); // Hide typing indicator
      }, 1000);
    }
  };

  const handleInputChange = (e) => {
    setInputValue(e.target.value);
  };

  const handleKeyPress = (e) => {
    if (e.key === 'Enter') {
      handleSendMessage();
    }
  };

  useEffect(() => {
    if (messagesEndRef.current) {
      messagesEndRef.current.scrollIntoView({ behavior: 'smooth' });
    }
  }, [messages, isTyping]);

  return (
    <div>
      {!isChatOpen && (
        <div id="chat-widget" className="chat-widget">
          <button id="chat-button" className="chat-button" onClick={handleChatButtonClick}>
            <ChatBubbleOutlineRoundedIcon className="chat-icon" />
          </button>
        </div>
      )}

      {isChatOpen && (
        <div id="chat-page" className="chat-page open">
          <div className="chat-header">
            <AndroidRoundedIcon className="bot-icon" />
            <h2 className="chatbot-title">ChatBot</h2>
            <button id="close-chat" className="close-chat" onClick={handleCloseChat}>
              <span className="close-icon">&times;</span>
            </button>
          </div>
          <div className="chat-messages">
            {messages.map((message, index) => (
              <div key={index} className={`chat-message ${message.sender}`}>
                {message.text}
              </div>
            ))}
            {isTyping && (
              <div className="typing-indicator">
                <div className="dot"></div>
                <div className="dot"></div>
                <div className="dot"></div>
              </div>
            )}
            <div ref={messagesEndRef} />
          </div>
          <div className="chat-input">
            <input
              type="text"
              id="chat-input-field"
              value={inputValue}
              onChange={handleInputChange}
              onKeyPress={handleKeyPress}
              placeholder="Type your message..."
            />
            <button id="send-message" className="send-message" onClick={handleSendMessage}>
              <SendRoundedIcon />
            </button>
          </div>
        </div>
      )}
    </div>
  );
};

export default ChatWidget;
