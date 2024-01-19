<template>
  <div class="active-meetings">
    <div class="meeting" v-for="room in rooms" :key="room">
      {{ room }}
    </div>
  </div>
</template>

<script>
export default {
  name: "Rooms",
  data() {
    return {
      rooms: Array(100)
        .fill(0)
        .map((_, i) => i + 1),
      selectedRoom: {},
      showPopup: false,
    };
  },
};
</script>

<style scoped>
.active-meetings {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: 20px;
  padding: 20px;
}

.meeting {
  width: 120px;
  height: 120px;
  background-color: #989AA2;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 4rem;
  cursor: pointer;
  border-radius: 4px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  color: #000;
}
</style>
