<template>
  <div class="app">
    <!-- The container for the Jitsi Meet iframe -->
    <!-- <div id="meet"></div>  -->
    <header>
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="48"
        height="48"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        stroke-width="2"
        stroke-linecap="round"
        stroke-linejoin="round"
        class="lucide lucide-menu"
      >
        <line x1="4" x2="20" y1="12" y2="12" />
        <line x1="4" x2="20" y1="6" y2="6" />
        <line x1="4" x2="20" y1="18" y2="18" />
      </svg>
      <span>Beast Console</span>
    </header>

    <div class="filter-and-heading">
      <div class="call-heading">{{ selectedFilter === 'active' ? 'Active Calls' : 'Rooms' }}</div>
      <div class="filters">
        <div class="filter" v-for="filter in filters" @click="setView(filter)">
          {{ filter.name }}
        </div>
      </div>
    </div>
    <div class="active-meetings" v-if="selectedFilter === 'active'">
      <div
        v-for="(room, index) in rooms"
        :key="room.id"
        class="meeting"
        @click="openRoom(room)"
      >
        {{ index + 1 }}
      </div>
    </div>
    <Rooms v-if="selectedFilter === 'rooms'" />
  </div>
  <PopUp
    v-if="showPopup"
    :data="selectedRoom"
    :show="true"
    :togglePopup="closePopup"
  />
</template>

<script>
import PopUp from "./components/popup.vue";
import Rooms from "./components/rooms.vue";
export default {
  components: {
    PopUp,
    Rooms,
  },
  // name: 'JitsiMeetComponent',
  // mounted() {
  //   // Load the Jitsi Meet API script
  //   let script = document.createElement('script');
  //   script.src = 'https://meet.jit.si/external_api.js';
  //   script.async = true;
  //   script.onload = this.initJitsi;
  //   document.head.appendChild(script);
  // },
  // methods: {
  //   initJitsi() {
  //     const options = {
  //       roomName: "YourConferenceRoomName",
  //       width: '100%',
  //       height: 700,
  //       parentNode: document.querySelector('#meet'),
  //       // other options...
  //     };
  //     // Make sure to check if JitsiMeetExternalAPI is available
  //     if (typeof JitsiMeetExternalAPI === 'function') {
  //       const api = new JitsiMeetExternalAPI("meet.jit.si", options);
  //       console.log("Jitsi Meet API initialized", api);
  //     } else {
  //       console.error('Jitsi Meet API script not loaded');
  //     }
  //   }
  // }

  data() {
    return {
      rooms: [],
      selectedRoom: {},
      showPopup: false,
      filters: [{ name: "Active" }, { name: "Rooms" }],
      selectedFilter: "active",
    };
  },

  methods: {
    generateRandomJitsiData() {
      function getRandomInt(min, max) {
        return Math.floor(Math.random() * (max - min + 1)) + min;
      }

      function generateRandomString(length) {
        const characters = "abcdefghijklmnopqrstuvwxyz0123456789";
        let result = "";
        for (let i = 0; i < length; i++) {
          result += characters.charAt(getRandomInt(0, characters.length - 1));
        }
        return result;
      }

      const generateParticipant = (roomJid) => ({
        jid: `${roomJid}/${generateRandomString(6)}`,
        role: "participant",
        displayName: `p${getRandomInt(1, 150)}`,
        id: generateRandomString(6),
      });

      const generateRoom = (isMainRoom, roomNumber) => {
        const roomJid = isMainRoom
          ? `room_${roomNumber}@conference.jitsi`
          : `${generateRandomString(24)}@breakout.jitsi`;

        return {
          isMainRoom: isMainRoom,
          id: roomJid,
          jid: roomJid,
          participants: [
            generateParticipant(roomJid),
            generateParticipant(roomJid),
          ],
        };
      };

      const roomCount = getRandomInt(1, 25);
      const rooms = Array.from({ length: roomCount }, (_, index) =>
        generateRoom(index === 0, index + 1)
      );
      return { rooms };
    },

    openRoom(room) {
      this.showPopup = !this.showPopup;
      this.selectedRoom = room;
      console.log("openRoom", room);
    },

    closePopup() {
      this.showPopup = false;
    },

    setView(filter) {
      this.selectedFilter = filter.name.toLowerCase();
      console.log("setView", filter.name.toLowerCase());
    },
  },
  mounted() {
    this.rooms = this.generateRandomJitsiData().rooms;
    console.log("mounted", this.generateRandomJitsiData());
  },
};
</script>

<style scoped>
#meet {
  width: 100%;
  height: 700px; /* or any other size */
}

.app {
  width: 100vw;
  height: 100vh;
  background-color: #121212;
  display: flex;
  flex-direction: column;
  overflow: auto;
}

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
  background-color: #00b500;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 4rem;
  cursor: pointer;
  border-radius: 4px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  color: #fff;
}

header {
  display: flex;
  align-items: center;
  color: #fff;
  gap: 1.5rem;
  font-size: 4rem;
  background-color: #202020;
  padding: 10px;
  padding-left: 10px;
}

.call-heading {
  color: #fff;
  font-size: 2rem;
  padding-left: 10px;
  font-weight: light !important;
}

.filter-and-heading {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20px;
}

.filters {
  display: flex;
  align-items: center;
  gap: 20px;
}

.filter {
  background-color: #202020;
  padding: 10px 20px;
  color: #fff;
  border-radius: 50px;
  cursor: pointer;
}

.filter:hover {
  background-color: #303030;
}
</style>
