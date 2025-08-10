<template>
  <div class="rpg-container">
    <div class="story-window" ref="storyWindow">
      <div class="story-content">
        <p v-for="(line, index) in storyLines" :key="index" class="story-line">
          {{ line }}
        </p>
      </div>
    </div>
    
    <div class="command-buttons">
      <button v-for="command in commands" :key="command" @click="executeCommand(command)" class="command-btn">
        {{ command }}
      </button>
    </div>
    
    <div class="input-section">
      <input 
        v-model="currentInput" 
        @keyup.enter="sendInput" 
        placeholder="Enter your command..."
        class="text-input"
        ref="textInput"
      />
      <button @click="sendInput" class="send-btn">Send</button>
    </div>
  </div>
</template>

<script setup>
import { ref, nextTick, onMounted } from 'vue'

const storyLines = ref([
  'Welcome to the Ancient Library...',
  'You find yourself standing before towering shelves filled with leather-bound tomes.',
  'Dust motes dance in shafts of golden light filtering through tall windows.',
  'The air smells of aged parchment and mysterious secrets.',
  'What would you like to do?'
])

const commands = ref(['Look', 'Examine', 'Take', 'Use', 'Go', 'Inventory'])
const currentInput = ref('')
const storyWindow = ref(null)
const textInput = ref(null)

const sendInput = async () => {
  if (!currentInput.value.trim()) return
  
  storyLines.value.push(`> ${currentInput.value}`)
  
  // Simple response system
  const response = processInput(currentInput.value.toLowerCase())
  storyLines.value.push(response)
  
  currentInput.value = ''
  
  await nextTick()
  scrollToBottom()
}

const executeCommand = (command) => {
  currentInput.value = command.toLowerCase()
  textInput.value?.focus()
}

const processInput = (input) => {
  if (input.includes('look')) {
    return 'You see ancient books, weathered stone walls, and flickering candles casting dancing shadows.'
  } else if (input.includes('examine')) {
    return 'The details reveal intricate craftsmanship and the passage of countless years.'
  } else if (input.includes('inventory')) {
    return 'You carry: a worn leather satchel, a flickering candle, and an old brass key.'
  } else {
    return 'The ancient library holds many secrets. Try exploring with different commands.'
  }
}

const scrollToBottom = () => {
  if (storyWindow.value) {
    storyWindow.value.scrollTop = storyWindow.value.scrollHeight
  }
}

onMounted(() => {
  textInput.value?.focus()
})
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Playwrite+AU+SA:wght@100..400&display=swap');

.rpg-container {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  font-family: 'Playwrite AU SA', cursive;
  background: linear-gradient(135deg, #8B4513 0%, #A0522D 50%, #CD853F 100%);
  color: #2F1B14;
  padding: 20px;
  box-sizing: border-box;
}

.story-window {
  flex: 1;
  background: linear-gradient(45deg, #F5E6D3 0%, #E8D5B7 100%);
  border: 8px solid #8B4513;
  border-radius: 12px;
  padding: 20px;
  margin-bottom: 15px;
  overflow-y: auto;
  box-shadow: 
    inset 0 0 20px rgba(139, 69, 19, 0.3),
    0 8px 20px rgba(0, 0, 0, 0.4);
  background-image: 
    radial-gradient(circle at 25% 25%, rgba(139, 69, 19, 0.1) 0%, transparent 70%),
    radial-gradient(circle at 75% 75%, rgba(160, 82, 45, 0.1) 0%, transparent 70%);
  max-height: 60vh;
}

.story-content {
  line-height: 1.6;
  font-size: 16px;
  letter-spacing: 0.3px;
}

.story-line {
  margin: 10px 0;
  text-shadow: 1px 1px 2px rgba(139, 69, 19, 0.2);
}

.story-line:first-child {
  font-weight: 400;
  font-size: 18px;
  color: #8B4513;
  text-align: center;
  border-bottom: 2px solid #8B4513;
  padding-bottom: 10px;
  margin-bottom: 20px;
}

.command-buttons {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
  flex-wrap: wrap;
  justify-content: center;
}

.command-btn {
  background: linear-gradient(145deg, #D2B48C, #DEB887);
  border: 3px solid #8B4513;
  border-radius: 8px;
  padding: 8px 16px;
  font-family: 'Playwrite AU SA', cursive;
  font-size: 14px;
  color: #2F1B14;
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
  text-shadow: 1px 1px 1px rgba(255, 255, 255, 0.5);
}

.command-btn:hover {
  background: linear-gradient(145deg, #DEB887, #F0E68C);
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.4);
}

.command-btn:active {
  transform: translateY(0);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
}

.input-section {
  display: flex;
  gap: 10px;
  background: linear-gradient(145deg, #8B4513, #A0522D);
  padding: 15px;
  border-radius: 12px;
  box-shadow: 
    inset 0 2px 4px rgba(0, 0, 0, 0.3),
    0 4px 8px rgba(0, 0, 0, 0.2);
}

.text-input {
  flex: 1;
  padding: 12px 16px;
  border: 3px solid #654321;
  border-radius: 8px;
  background: linear-gradient(145deg, #F5E6D3, #E8D5B7);
  font-family: 'Playwrite AU SA', cursive;
  font-size: 16px;
  color: #2F1B14;
  box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.2);
}

.text-input:focus {
  outline: none;
  border-color: #DAA520;
  box-shadow: 
    inset 0 2px 4px rgba(0, 0, 0, 0.2),
    0 0 8px rgba(218, 165, 32, 0.5);
}

.text-input::placeholder {
  color: #8B4513;
  opacity: 0.7;
}

.send-btn {
  background: linear-gradient(145deg, #DAA520, #B8860B);
  border: 3px solid #8B4513;
  border-radius: 8px;
  padding: 12px 24px;
  font-family: 'Playwrite AU SA', cursive;
  font-size: 16px;
  color: #2F1B14;
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
  text-shadow: 1px 1px 1px rgba(255, 255, 255, 0.5);
  font-weight: 300;
}

.send-btn:hover {
  background: linear-gradient(145deg, #FFD700, #DAA520);
  transform: translateY(-2px);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.4);
}

.send-btn:active {
  transform: translateY(0);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
}

::-webkit-scrollbar {
  width: 12px;
}

::-webkit-scrollbar-track {
  background: #8B4513;
  border-radius: 6px;
}

::-webkit-scrollbar-thumb {
  background: linear-gradient(145deg, #D2B48C, #DEB887);
  border-radius: 6px;
  border: 2px solid #8B4513;
}

::-webkit-scrollbar-thumb:hover {
  background: linear-gradient(145deg, #DEB887, #F0E68C);
}
</style>
