<script setup>
import { ref, reactive, computed } from 'vue'

const isListVisible = ref(true)

const hoveredUserId = ref(null)

const users = reactive([
  {
    id: 1,
    name: 'Александр Петров',
    age: 28,
    email: 'alex@example.com',
    position: 'Frontend-разработчик',
    city: 'Москва',
    hoverColor: '#667eea',
    showDetails: false,
    bio: '<p>Опыт: 5 лет в веб-разработке. Специализация — Vue.js и React.</p>'
  },
  {
    id: 2,
    name: 'Мария Иванова',
    age: 32,
    email: 'maria@example.com',
    position: 'UI/UX Дизайнер',
    city: 'Санкт-Петербург',
    hoverColor: '#f093fb',
    showDetails: false,
    bio: '<p>Опыт: 8 лет в дизайне интерфейсов. Работала с Figma и Sketch.</p>'
  },
  {
    id: 3,
    name: 'Дмитрий Михайлов',
    age: 25,
    email: 'dmitry@example.com',
    position: 'Backend-разработчик',
    city: 'Новосибирск',
    hoverColor: '#4facfe',
    showDetails: false,
    bio: '<p>Опыт: 3 года. Работает с Node.js, Python и PostgreSQL.</p>'
  },
  {
    id: 4,
    name: 'Елена Сидорова',
    age: 29,
    email: 'elena@example.com',
    position: 'Project Manager',
    city: 'Екатеринбург',
    hoverColor: '#43e97b',
    showDetails: false,
    bio: '<p>Опыт: 6 лет управления IT-проектами. Сертифицированный Scrum Master.</p>'
  },
  {
    id: 5,
    name: 'Игорь Волков',
    age: 35,
    email: 'igor@example.com',
    position: 'DevOps инженер',
    city: 'Казань',
    hoverColor: '#fa709a',
    showDetails: false,
    bio: '<p>Опыт: 10 лет. Эксперт по Docker, Kubernetes и CI/CD.</p>'
  }
])

// проверяет у всех ли пользователей раскрыты карточки
const allDetailsOpen = computed(() => {
  let allOpen = true

  users.forEach(user => {
    if (!user.showDetails) {
      allOpen = false
    }
  })

  return allOpen
})

// Переключатель скрыть/показать весь список
function toggleList() {
  isListVisible.value = !isListVisible.value
}

// Раскрытие карточки конкретного пользователя
function toggleDetails(userId) {
  const user = users.find(u => u.id === userId)
  if (user) {
    user.showDetails = !user.showDetails
  }
}

// Раскрытие карточек всех пользователей
function toggleAllDetails() {
  const newState = !allDetailsOpen.value
  users.forEach(user => {
    user.showDetails = newState
  })
}
</script>

<template>
  <div class="user-list-container">

    <div class="controls">
      <button
        v-on:click="toggleList"
        class="btn btn-primary"
      >
        {{ isListVisible ? 'Скрыть список' : 'Показать список' }}
      </button>

      <button
        @click="toggleAllDetails"
        class="btn btn-secondary"
        v-show="isListVisible"
      >
        {{ allDetailsOpen ? '🔼 Свернуть всё' : '🔽 Развернуть всё' }}
      </button>
    </div>

    <div v-if="isListVisible" class="list-wrapper">

      <p class="counter">
        Всего пользователей: <strong>{{ users.length }}</strong>
      </p>

      <div class="user-list">
        <li
          v-for="user in users"
          :key="user.id"
          class="user-card"
          v-bind:class="{ 'user-card--active': user.showDetails }"
        >
          <div
            class="user-header"
            v-bind:style="{
              color: hoveredUserId === user.id ? user.hoverColor : '#333',
            }"
            v-on:mouseenter="hoveredUserId = user.id"
            v-on:mouseleave="hoveredUserId = null"
            @click="toggleDetails(user.id)"
          >
            <div
              class="avatar"
              v-bind:style="{ backgroundColor: user.hoverColor }"
            >
              {{ user.name.charAt(0) }}
            </div>

            <div class="user-name">
              {{ user.name }}
            </div>

            <span class="toggle-icon">
              {{ user.showDetails ? '▲' : '▼' }}
            </span>
          </div>

          <div v-show="user.showDetails" class="user-details">
            <div class="detail-row">
              <span class="label">📅 Возраст:</span>
              <span class="value">{{ user.age }} лет</span>
            </div>
            <div class="detail-row">
              <span class="label">✉️ Email:</span>
              <span class="value">{{ user.email }}</span>
            </div>
            <div class="detail-row">
              <span class="label">💼 Должность:</span>
              <span class="value">{{ user.position }}</span>
            </div>
            <div class="detail-row">
              <span class="label">📍 Город:</span>
              <span class="value">{{ user.city }}</span>
            </div>

            <div class="user-bio" v-html="user.bio"></div>
          </div>
        </li>
    </div>
    </div>

    <div v-else class="hidden-message">
      <p>Список пользователей скрыт</p>
      <p class="hint">Нажмите кнопку выше, чтобы показать</p>
    </div>

  </div>
</template>

<style scoped>

.user-list-container {
  background: #ffffff;
  border-radius: 16px;
  padding: 30px;
  box-shadow: 0 4px 24px rgba(0, 0, 0, 0.08);
  min-width: 650px;
}

.controls {
  display: flex;
  gap: 12px;
  margin-bottom: 25px;
  flex-wrap: wrap;
}

.btn {
  padding: 12px 24px;
  border: none;
  border-radius: 10px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.1);
}

.btn:active {
  transform: translateY(0);
}

.btn-primary {
  background: #667eea;
  color: white;
}

.btn-secondary {
  background: #f0f2f5;
  color: #737373;
  border: 1px solid #e8e8e8;
}

.btn-secondary:hover {
  color: #333;
}

.counter {
  color: #888;
  margin-bottom: 20px;
  font-size: 0.9rem;
}

.counter strong {
  color: #667eea;
}

.user-list {
  list-style: none;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.user-card {
  background: #f8f9fb;
  border-radius: 12px;
  overflow: hidden;
  border: 1px solid #e8e8e8;
  transition: border-color 0.3s ease;
}

.user-card--active {
  border-color: #667eea;
}

.user-header {
  display: flex;
  align-items: center;
  padding: 16px 20px;
  cursor: pointer;
  user-select: none;
  gap: 15px;
}

.avatar {
  width: 45px;
  height: 45px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.2rem;
  font-weight: bold;
  color: white;
  flex-shrink: 0;
}

.user-name {
  flex: 1;
  font-size: 1.1rem;
  font-weight: 600;
}

.toggle-icon {
  font-size: 0.8rem;
  opacity: 0.5;
}

.user-details {
  padding: 0 20px 20px 80px;
  animation: slideDown 0.3s ease;
}

@keyframes slideDown {
  from {
    opacity: 0;
    transform: translateY(-10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.detail-row {
  display: flex;
  gap: 10px;
  padding: 6px 0;
  border-bottom: 1px solid #e8e8e8;
}

.label {
  color: #888;
  min-width: 120px;
  font-size: 0.9rem;
}

.value {
  color: #333;
  font-size: 0.9rem;
}

.user-bio {
  margin-top: 12px;
  padding: 12px;
  background: #f0f2f5;
  border-radius: 8px;
  font-size: 0.9rem;
  color: #737373;
  line-height: 1.5;
}

.hidden-message {
  text-align: center;
  padding: 60px 20px;
  color: #888;
}

.hidden-message p:first-child {
  font-size: 1.3rem;
  margin-bottom: 10px;
}

.hint {
  font-size: 0.85rem;
  color: #888;
}

</style>