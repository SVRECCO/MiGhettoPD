<template>
  <transition name="fade" appear>
    <div v-if="show" class="hud-container">
      <div class="card-container">
        <div class="card tall-card">
          <div class="card-header">
            <img src="../assets/mgrp.png" alt="Picture" class="card-image" />
          </div>
          <div class="card-buttons">
            <div
              @click="changeMenu('userData', 'User Data')"
              @mouseenter="playSound"
              @mouseleave="stopSound"
              class="card-button mb-4"
              :class="{ selected: menu === 'userData' }"
            >
              <button>User Data</button>
            </div>
            <div
              @click="changeMenu('settings', 'Settings')"
              @mouseenter="playSound"
              @mouseleave="stopSound"
              class="card-button mb-4"
              :class="{ selected: menu === 'settings' }"
            >
              <button>Settings</button>
            </div>
            <div
              @click="changeMenu('notifications', 'Notifications')"
              @mouseenter="playSound"
              @mouseleave="stopSound"
              class="card-button mb-4"
              :class="{ selected: menu === 'notifications' }"
            >
              <button>Notifications</button>
            </div>
            <div
              @click="changeMenu('analytics', 'Analytics')"
              @mouseenter="playSound"
              @mouseleave="stopSound"
              class="card-button mb-4"
              :class="{ selected: menu === 'analytics' }"
            >
              <button>Analytics</button>
            </div>
            <div
              @click="changeMenu('help', 'Help')"
              @mouseenter="playSound"
              @mouseleave="stopSound"
              class="card-button"
              :class="{ selected: menu === 'help' }"
            >
              <button>Help</button>
            </div>
          </div>
        </div>
        <div class="card wide-card">
          <div class="card-header">
            <div class="header-content">
              <img
                src="../assets/mglogo.gif"
                alt="Picture"
                class="card-image2"
              />
              <div class="header-text">
                <div class="card-title">MiGhettoPD</div>
                <div class="card-subtitle">Subtitle Text</div>
              </div>
            </div>
          </div>
          <div class="card-content">
            <div v-if="menu === 'userData'" class="content-wrapper">
              <table class="user-data-table rounded-lg overflow-hidden">
                <thead>
                  <tr class="bg-gray-700">
                    <th class="rounded-tl-lg">Rank</th>
                    <th>Discord Name</th>
                    <th>Robberies</th>
                    <th>Arrests</th>
                    <th>Raids</th>
                    <th class="rounded-tr-lg">Time as PD</th>
                  </tr>
                </thead>
                <tbody>
                  <tr class="bg-gray-800 even:bg-gray-700">
                    <td class="rounded-bl-lg">PD DEV</td>
                    <td>SethV</td>
                    <td>12</td>
                    <td>5</td>
                    <td>0</td>
                    <td class="rounded-br-lg">03:46</td>
                  </tr>
                </tbody>
              </table>
            </div>
            <div v-else-if="menu === 'settings'" class="content-wrapper">
              <h2 class="menu-title">{{ menuTitle }}</h2>
              <!-- settings here -->
            </div>
            <div v-else-if="menu === 'notifications'" class="content-wrapper">
              <h2 class="menu-title">{{ menuTitle }}</h2>
              <!-- notifications here -->
            </div>
            <div v-else-if="menu === 'analytics'" class="content-wrapper">
              <h2 class="menu-title">{{ menuTitle }}</h2>
              <!-- analytics here -->
            </div>
            <div v-else-if="menu === 'help'" class="content-wrapper">
              <h2 class="menu-title">{{ menuTitle }}</h2>
            </div>
          </div>
        </div>
      </div>
    </div>
  </transition>
</template>

<script>
import customMenuSound from "@/assets/custom_menu_sound.wav";
import clickSound from "@/assets/click.wav";

export default {
  props: {
    show: {
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      menu: "userData",
      menuTitle: "User Data",
      audioPlayer: null,
      userData: {
        "211748804746149888": {
          policeRank: "PD DEV",
          discordName: "SethV",
          robberies: 12,
          arrests: 5,
          raids: 0,
          timeAsPD: "03:46",
          tickets: 0,
          strikes: 0,
          events: 0,
        },
      },
      showSupportModal: false,
      supportTicket: {
        topic: "",
        message: "",
      },
    };
  },
  methods: {
    changeMenu(menu, title) {
      this.menu = menu;
      this.menuTitle = title;
      this.playClickSound();
    },
    playSound() {
      this.audioPlayer = new Audio(customMenuSound);
      this.audioPlayer.volume = 0.25;
      this.audioPlayer.play();
    },
    stopSound() {
      if (this.audioPlayer) {
        this.audioPlayer.pause();
        this.audioPlayer = null;
      }
    },
    playClickSound() {
      this.clickAudioPlayer = new Audio(clickSound);
      this.clickAudioPlayer.volume = 0.004;
      this.clickAudioPlayer.play();
    },
    openSupportModal() {
      this.showSupportModal = true;
    },
    closeSupportModal() {
      this.showSupportModal = false;
      this.resetSupportTicket();
    },
    resetSupportTicket() {
      this.supportTicket.topic = "";
      this.supportTicket.message = "";
    },
    async submitSupportTicket() {
      const webhookUrl = "YOUR_DISCORD_WEBHOOK_URL";
      const embed = {
        title: "New Support Ticket",
        fields: [
          {
            name: "Topic",
            value: this.supportTicket.topic,
          },
          {
            name: "Message",
            value: this.supportTicket.message,
          },
        ],
        color: 16711680,
        timestamp: new Date().toISOString(),
        footer: {
          text: "Support Ticket System",
        },
      };

      try {
        const response = await fetch(webhookUrl, {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify({
            embeds: [embed],
          }),
        });

        if (response.ok) {
          console.log("Support ticket sent successfully");
          this.closeSupportModal();
        } else {
          console.error("Failed to send support ticket");
        }
      } catch (error) {
        console.error("Error sending support ticket:", error);
      }
    },
  },
};
</script>

<style scoped>
.hud-container {
  z-index: 40;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.icon-container {
  display: flex;
  align-items: center;
  gap: 8px;
}

.small-icon {
  height: 20px;
  width: 20px;
}
.card-container {
  display: flex;
  gap: 16px;
  padding: 40px 0;
}

.card {
  background-color: #111111b4;
  padding: 20px;
  border-radius: 18px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  display: flex;
  flex-direction: column;
}

.tall-card {
  width: 300px;
  height: 600px;
  border-color: #ffffffb4;
  border-width: 2px;
}

.wide-card {
  width: 1000px;
  height: 600px;
  border-color: #ffffffb4;
  border-width: 2px;
  z-index: 0;
}

.card-header {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  margin-bottom: 16px;
  margin-top: 16px;
}

.header-content {
  display: flex;
  align-items: center;
}

.header-text {
  display: flex;
  flex-direction: column;
  margin-left: 16px;
}

.card-buttons {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  flex-grow: 1;
}

.card-title {
  color: #ffffff;
  font-weight: bold;
  font-size: 24px;
}

.card-subtitle {
  color: #ffffff;
  font-size: 16px;
  background-color: #9c9c9c;
  padding: 4px 8px;
  border-radius: 4px;
}

.card-image {
  height: max-content;
  width: max-content;
  border-radius: 12px;
  margin-bottom: 22px;
  margin-top: 28px;
}

.card-image2 {
  height: 120px;
  width: 120px;
  border-radius: 12px;
}

.card-button {
  font-weight: 400;
  padding: 14px;
  background-color: #881e20;
  border-radius: 16px;
  border: 1px solid transparent;
  color: #ffffff;
  cursor: pointer;
  flex-grow: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}

.card-button.selected {
  border-color: #ffffff;
}

.card-button:hover {
  border-color: #ffffff;
}

.card-content {
  padding: 20px;
  background-color: #9c9c9ca2;
  border-radius: 16px;
  color: #ffffff;
  flex-grow: 1;
  overflow-y: auto;
}

.mb-4 {
  margin-bottom: 16px;
}

.fade-enter-active,
.fade-leave-active {
  transition:
    opacity 0.5s,
    transform 0.5s;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
  transform: scale(0.9);
}
.content-wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100%;
}

.menu-title {
  margin: 0;
}

.user-data-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 16px;
  margin-bottom: 270px;
}

.user-data-table th,
.user-data-table td {
  padding: 10px;
  text-align: left;
  border-bottom: 1px solid #555;
}

.user-data-table th {
  background-color: #333;
}

.user-data-table tbody tr:nth-child(even) {
  background-color: #444;
}

.settings-container {
  width: 100%;
}

.settings-section {
  @apply mb-8;
}

.settings-title {
  @apply text-gray-400 font-medium mb-4;
}

.settings-item {
  @apply mb-4;
}

.settings-item label {
  @apply block text-gray-400 font-medium mb-2;
}

.settings-item input[type="text"],
.settings-item select {
  @apply bg-gray-700 text-gray-400 border-gray-600 rounded-md px-4 py-2 w-full;
}

.settings-item input[type="range"] {
  @apply w-full;
}

.settings-item input[type="checkbox"].form-checkbox {
  @apply h-5 w-5 text-red-500;
}

.settings-item label.ml-2 {
  @apply text-gray-400;
}
.user-data-table.rounded-lg {
  border-radius: 0.5rem;
}

.user-data-table thead th:first-child {
  @apply rounded-tl-lg;
}

.user-data-table thead th:last-child {
  @apply rounded-tr-lg;
}

.user-data-table tbody tr:first-child td:first-child {
  @apply rounded-bl-lg;
}

.user-data-table tbody tr:first-child td:last-child {
  @apply rounded-br-lg;
}
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

.modal-content {
  background-color: #fff;
  padding: 20px;
  border-radius: 4px;
  max-width: 500px;
  width: 100%;
}

.form-group {
  margin-bottom: 10px;
}

label {
  display: block;
  font-weight: bold;
  margin-bottom: 5px;
}

input,
textarea {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.send-button,
.close-button {
  padding: 8px 16px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  margin-top: 10px;
}

.close-button {
  background-color: #ccc;
  color: #000;
  margin-left: 10px;
}
</style>
