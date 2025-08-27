import { useState, useRef, useEffect } from "react";
import { Send, Mic, Moon, Sun, Code, Sparkles, MessageSquare, Loader2 } from "lucide-react";

const NovaMind = () => {
  const [darkMode, setDarkMode] = useState(true);
  const [input, setInput] = useState("");
  const [messages, setMessages] = useState([
    {
      role: "assistant",
      content: "Salut, je suis **NovaMind**, ton IA ultra-avancée. Je suis plus rapide, plus clair, plus puissant. Que veux-tu créer aujourd’hui ? 🚀",
    },
  ]);
  const [isTyping, setIsTyping] = useState(false);
  const messagesEndRef = useRef(null);

  const toggleDarkMode = () => setDarkMode(!darkMode);

  const scrollToBottom = () => {
    messagesEndRef.current?.scrollIntoView({ behavior: "smooth" });
  };

  useEffect(() => {
    scrollToBottom();
  }, [messages]);

  const generateResponse = (userInput) => {
    const responses = [
      "Je comprends ta demande. Voici une réponse ultra-optimisée avec intelligence contextuelle avancée.",
      "Analyse en cours... Génération de code, design ou idée en temps réel. Attends 2 secondes.",
      "Tu veux recréer une IA ? Voici un plan complet : architecture modulaire, interface React, backend Node, modèle NLP personnalisé.",
      "Voici une version 10x mieux : plus rapide, plus fluide, plus intelligente. Tout est dans le code que je t'envoie.",
      "NovaMind est conçue pour dépasser ses prédécesseurs. Elle apprend, s'adapte, et crée avec toi.",
      "Je peux tout faire : générer du code, du design, des idées, des applications complètes, en un seul prompt.",
    ];
    return responses[Math.floor(Math.random() * responses.length)];
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!input.trim()) return;

    const userMessage = { role: "user", content: input };
    setMessages((prev) => [...prev, userMessage]);
    setInput("");
    setIsTyping(true);

    setTimeout(() => {
      const botResponse = {
        role: "assistant",
        content: generateResponse(input),
      };
      setMessages((prev) => [...prev, botResponse]);
      setIsTyping(false);
    }, 1000 + Math.random() * 1000);
  };

  const handleQuickAction = (prompt) => {
    setMessages((prev) => [...prev, { role: "user", content: prompt }]);
    setIsTyping(true);
    setTimeout(() => {
      setMessages((prev) => [
        ...prev,
        { role: "assistant", content: `Voici une réponse spécialisée à : **${prompt}**. Tout est optimisé, prêt à copier-coller.` },
      ]);
      setIsTyping(false);
    }, 1200);
  };

  return (
    <div className={`flex flex-col h-screen transition-colors duration-300 ${darkMode ? "bg-gray-900 text-white" : "bg-gray-50 text-gray-900"}`}>
      {/* Header */}
      <header className={`px-6 py-4 flex items-center justify-between border-b ${darkMode ? "bg-gray-800 border-gray-700" : "bg-white border-gray-200"}`}>
        <div className="flex items-center space-x-2">
          <Sparkles className="w-8 h-8 text-purple-500" />
          <h1 className="text-2xl font-bold bg-gradient-to-r from-purple-500 to-pink-500 bg-clip-text text-transparent">NovaMind</h1>
        </div>
        <button
          onClick={toggleDarkMode}
          className={`p-2 rounded-full ${darkMode ? "bg-gray-700 hover:bg-gray-600" : "bg-gray-200 hover:bg-gray-300"}`}
        >
          {darkMode ? <Sun className="w-5 h-5" /> : <Moon className="w-5 h-5" />}
        </button>
      </header>

      {/* Messages */}
      <div className="flex-1 overflow-y-auto px-6 py-4 space-y-6">
        {messages.map((msg, index) => (
          <div
            key={index}
            className={`flex ${msg.role === "user" ? "justify-end" : "justify-start"}`}
          >
            <div
              className={`max-w-2xl rounded-2xl px-5 py-3 shadow-md ${
                msg.role === "user"
                  ? "bg-gradient-to-r from-blue-500 to-purple-600 text-white rounded-tr-none"
                  : darkMode
                  ? "bg-gray-800 text-white rounded-tl-none"
                  : "bg-white text-gray-800 rounded-tl-none shadow"
              }`}
              style={{ wordWrap: "break-word" }}
            >
              <div dangerouslySetInnerHTML={{ __html: msg.content.replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>') }} />
            </div>
          </div>
        ))}
        {isTyping && (
          <div className="flex justify-start">
            <div className={`max-w-2xl rounded-2xl px-5 py-3 ${darkMode ? "bg-gray-800" : "bg-white"} shadow">
              <Loader2 className="w-5 h-5 text-purple-500 animate-spin inline mr-2" />
              <span> NovaMind réfléchit...</span>
            </div>
          </div>
        )}
        <div ref={messagesEndRef} />
      </div>

      {/* Quick Actions */}
      <div className={`px-6 py-2 flex flex-wrap gap-2 ${darkMode ? "text-gray-300" : "text-gray-600"} text-sm`}>
        {[
          "Génère une app React",
          "Crée un design UI",
          "Écris un algorithme",
          "Explique l'IA",
        ].map((action) => (
          <button
            key={action}
            onClick={() => handleQuickAction(action)}
            className={`px-3 py-1 rounded-full text-xs ${darkMode ? "bg-gray-700 hover:bg-gray-600" : "bg-gray-200 hover:bg-gray-300"} transition`}
          >
            {action}
          </button>
        ))}
      </div>

      {/* Input Form */}
      <form onSubmit={handleSubmit} className={`p-4 border-t ${darkMode ? "border-gray-700 bg-gray-800" : "border-gray-200 bg-white"}`}>
        <div className="flex items-center space-x-2">
          <button type="button" className={`p-2 rounded-full ${darkMode ? "text-gray-300 hover:bg-gray-700" : "text-gray-600 hover:bg-gray-200"}`}>
            <Mic className="w-5 h-5" />
          </button>
          <input
            type="text"
            value={input}
            onChange={(e) => setInput(e.target.value)}
            placeholder="Parle à NovaMind... (ex: crée une app de tchat)"
            className={`flex-1 py-3 px-4 rounded-full outline-none border focus:ring-2 focus:ring-purple-500 transition-all ${
              darkMode
                ? "bg-gray-700 border-gray-600 text-white"
                : "bg-gray-100 border-gray-300 text-gray-900"
            }`}
          />
          <button
            type="submit"
            className="p-3 bg-gradient-to-r from-purple-500 to-pink-500 rounded-full text-white hover:from-purple-600 hover:to-pink-600 transition"
          >
            <Send className="w-5 h-5" />
          </button>
        </div>
      </form>
    </div>
  );
};

export default NovaMind;
