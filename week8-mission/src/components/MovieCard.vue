<script setup>
import { ref } from 'vue';

// 부모로부터 수신할 데이터(Props)
const props = defineProps({
  movie: {
    type: Object,
    required: true
  }
});

// 편집 상태 관리
const isEditing = ref(false);
const editTitle = ref('');
const editRating = ref('');
const emit = defineEmits(['like-movie', 'delete-movie', 'update-movie']);

// [수정] 버튼 클릭 → 편집 모드 진입
const startEdit = () => {
  editTitle.value = props.movie.title;
  editRating.value = props.movie.rating;
  isEditing.value = true;
};

// [저장] 버튼 클릭
const saveEdit = () => {
  emit('update-movie', {
    id: props.movie.id,
    title: editTitle.value,
    rating: editRating.value
  });
  isEditing.value = false;
};

// [취소] 버튼 클릭 → 편집 모드 종료
const cancelEdit = () => {
  isEditing.value = false;
};
</script>

<template>
  <div class="card">
    <div class="poster-area">
      <img :src="movie.poster" :alt="movie.title" class="poster">
    </div>
    <div class="content-area">
      <!-- 뷰 모드 -->
      <template v-if="!isEditing">
        <h3>{{ movie.title }}</h3>
        <p class="rating">평점: ⭐ {{ movie.rating }} / 10</p>
        <p class="likes">❤️ 좋아요: {{ movie.likes }}</p>

        <div class="btn-group">
          <button class="btn like-btn" @click="$emit('like-movie', movie.id)">👍 추천</button>
          <button class="btn edit-btn" @click="startEdit">✏️ 수정</button>
          <button class="btn del-btn" @click="$emit('delete-movie', movie.id)">🗑️ 삭제</button>
        </div>
      </template>

      <!-- 편집 모드 -->
      <template v-else>
        <div class="edit-field">
          <label>제목</label>
          <input v-model="editTitle" type="text" class="edit-input" />
        </div>
        <div class="edit-field">
          <label>평점</label>
          <input v-model.number="editRating" type="number" min="0" max="10" step="0.1" class="edit-input" />
        </div>
        <div class="btn-group">
          <button class="btn save-btn" @click="saveEdit">💾 저장</button>
          <button class="btn cancel-btn" @click="cancelEdit">✖ 취소</button>
        </div>
      </template>
    </div>
  </div>
</template>

<style scoped>
.card { 
  background: #fff; 
  border: 1px solid #e0e0e0; 
  border-radius: 12px; 
  overflow: hidden; 
  box-shadow: 0 4px 6px rgba(0,0,0,0.05); 
  transition: transform 0.2s ease;
}
.card:hover { 
  transform: translateY(-5px); 
  box-shadow: 0 8px 15px rgba(0,0,0,0.1); 
}

.poster-area { 
  position: relative; 
  width: 100%; 
  height: 300px; 
}
.poster { 
  width: 100%; 
  height: 100%; 
  object-fit: cover; 
}

.content-area {
  padding: 20px;
  text-align: center;
}
h3 { 
  margin: 0 0 10px 0; 
  color: #2c3e50; 
  font-size: 1.4rem;
}
.rating { 
  font-weight: 600; 
  color: #f39c12; 
  margin: 5px 0;
}
.likes { 
  font-weight: 600; 
  color: #e74c3c; 
  margin: 5px 0 20px 0;
}

.edit-field {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  margin-bottom: 12px;
  gap: 4px;
}
.edit-field label {
  font-size: 0.8rem;
  font-weight: 700;
  color: #7f8c8d;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}
.edit-input {
  width: 100%;
  padding: 8px 10px;
  border: 2px solid #3498db;
  border-radius: 6px;
  font-size: 1rem;
  box-sizing: border-box;
  outline: none;
  transition: border-color 0.2s;
}
.edit-input:focus {
  border-color: #2980b9;
}

.btn-group {
  display: flex;
  gap: 8px;
  justify-content: center;
  margin-top: 16px;
}
.btn {
  padding: 9px 12px;
  cursor: pointer;
  border: none;
  border-radius: 6px;
  font-weight: bold;
  font-size: 0.85rem;
  transition: opacity 0.2s;
  flex: 1;
}
.btn:hover { opacity: 0.8; }

.like-btn   { background-color: #3498db; color: white; }
.edit-btn   { background-color: #f39c12; color: white; }
.del-btn    { background-color: #e74c3c; color: white; }
.save-btn   { background-color: #2ecc71; color: white; }
.cancel-btn { background-color: #95a5a6; color: white; }
</style>