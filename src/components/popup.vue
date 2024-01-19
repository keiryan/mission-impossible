<template>
  <div class="popup-container" @click="togglePopup">
    <div class="popup" @click.stop>
      <div class="popup-header">
        <div class="popup-heading">Participants</div>
        <div @click="togglePopup" class="popup-close">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="#fff"
            class="w-6 h-6"
            width="48"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M6 18 18 6M6 6l12 12"
            />
          </svg>
        </div>
      </div>
      <div class="participant-container">
        <div v-for="person in data.participants" class="participant">
          <div class="avatar-container">
            <img
              src="https://www.iconpacks.net/icons/1/free-user-icon-295-thumb.png"
              alt="avatar"
              class="avatar"
            />
          </div>
          <p>{{ person.displayName }}</p>
        </div>
      </div>
      <div class="call-button">Join Call</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "popup",
  props: {
    data: {
      type: Object,
      required: true,
      default: () => ({}),
    },
    togglePopup: {
      type: Function,
      required: true,
      default: () => {},
    },
  },

  mounted() {
    console.log("data for popup", this.data);
  },
};
</script>

<style scoped>
.popup-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  backdrop-filter: blur(18px) saturate(100%) contrast(100%) brightness(100%);
  -webkit-backdrop-filter: blur(18px) saturate(100%) contrast(100%)
    brightness(70%);
}
.popup {
  width: 600px;
  height: 400px;
  border-radius: 10px;
  padding: 20px;
  background-color: #202020;
  pointer-events: all;
  color: #fff;
  display: flex;
  flex-direction: column;
  font-weight: light;
}

.popup-close {
  cursor: pointer;
}

.popup-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.participant-container {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.participant {
  display: flex;
  align-items: center;
  gap: 4px;
  text-transform: capitalize;
}

.avatar-container {
  width: 50px;
  height: 50px;
  border-radius: 50px;
  margin-right: 20px;
  background-color: #fff;
  overflow: hidden;
}

.avatar {
  width: 50px;
  transform: translateY(10px);
}

.popup-heading {
  font-size: 2rem;
}

.call-button {
    width: 100%;
    height: 50px;
    background-color: #00b500;
    border-radius: 10px;
    margin-top: auto;
    cursor: pointer;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 1.5rem;
    text-transform: uppercase;
    letter-spacing: 1px;
    transition: all 0.2s ease-in-out;
}
</style>
