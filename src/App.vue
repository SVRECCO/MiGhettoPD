<template>
  <div id="app" class="app">
    <div class="app__background"></div>
    <div class="app__overlay">
      <transition-group name="toast" tag="div" class="app__toast-container">
        <Toast
          v-for="(toast, index) in toasts"
          :key="toast.id"
          :message="toast.message"
        />
      </transition-group>
      <Killfeed v-if="showKillfeed" :killfeedItems="killfeedItems" />
      <transition name="fade">
        <Hud v-if="showHud" :show="showHud" />
      </transition>
    </div>
    <div class="app__test-controls">
      <div class="app__input-container">
        <input
          v-model="toastMessage"
          placeholder="Enter toast message"
          class="app__toast-input"
          @keyup.enter="sendToast"
        />
        <button @click="sendToast" class="app__send-button">
          <i class="fas fa-paper-plane"></i>
        </button>
      </div>
      <div class="app__test-buttons">
        <button @click="toggleHud" class="app__test-button">Toggle HUD</button>
        <button
          @click="addKillfeedItem('killfeed-item')"
          class="app__test-button app__test-button--killfeed"
        >
          Normal Kill
        </button>
        <button
          @click="addKillfeedItem('killfeed-item-me')"
          class="app__test-button app__test-button--killfeed"
        >
          Me Kill
        </button>
        <button
          @click="addKillfeedItem('killfeed-item-me-die')"
          class="app__test-button app__test-button--killfeed"
        >
          Me Killed
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import e from"./components/Toast.vue";import t from"./components/Killfeed.vue";import s from"./components/Hud.vue";import o from"@/assets/openmenu.wav";import l from"@/assets/back.wav";import a from"@/assets/kill.wav";import i from"@/assets/toast.wav";let toastId=0;export default{name:"App",components:{Toast:e,Killfeed:t,Hud:s},data:()=>({toasts:[],toastMessage:"",showKillfeed:!0,showHud:!1,killfeedItems:[],weapons:["\n Assault Rifle \n","\n AP-Pistol \n","\n Heavy Sniper \n","\n Pump Shotgun \n","\n MG \n","\n Pistol \n",]}),methods:{sendToast(){""!==this.toastMessage.trim()&&(this.toasts.push({id:toastId++,message:this.toastMessage}),this.playSound3(i),setTimeout(()=>{this.toasts.shift()},3e3))},toggleKillfeed(){this.showKillfeed=!this.showKillfeed},toggleHud(){this.showHud=!this.showHud,this.showHud?this.playSound(o):this.playSound2(l)},playSound(e){let t=new Audio(e);t.volume=.04,t.play()},playSound2(e){let t=new Audio(e);t.volume=.2,t.play()},playSound3(e){let t=new Audio(e);t.volume=.01,t.play()},getRandomWeapon(){let e=Math.floor(Math.random()*this.weapons.length);return this.weapons[e]},addKillfeedItem(e){this.playKillSound();let t="Player 1",s="Player 2";"killfeed-item-me"===e?t="Me":"killfeed-item-me-die"===e&&(s="Me");let o={killer:t,weapon:this.getRandomWeapon(),victim:s,type:e};this.killfeedItems.unshift(o),setTimeout(()=>{this.killfeedItems.pop()},6e3)},playKillSound(){let e=new Audio(a);e.volume=.1,e.play()}}};
</script>

<style>
.app{text-align:center;position:relative;height:100vh;font-family:MartianMonoNerdFont,sans-serif;background-color:#1c1c1c;color:#fff}.app__background{z-index:0;position:absolute;top:0;left:0;width:100%;height:100%;background-image:url(./assets/image.webp);background-position:center;background-repeat:no-repeat;background-size:cover;filter:blur(2px)}.app__overlay{z-index:1;position:relative;height:100%}.app__test-controls,.app__toast-container{position:absolute;left:50%;transform:translateX(-50%);display:flex}.app__test-controls{z-index:2;bottom:20px;flex-direction:column;align-items:center;width:90%}.app__toast-input{padding:12px;width:100%;max-width:300px;border:none;border-radius:8px;background-color:#2c2c2c;color:#fff;font-size:16px}.app__test-button,.app__test-buttons button{background-color:#1c1c1c;border:none;transition:background-color .3s;color:#fff;cursor:pointer}.app__test-button{margin:0 5px;padding:12px 24px;border-radius:8px;font-size:16px}.app__test-buttons{display:flex;justify-content:center;flex-wrap:wrap}.app__test-buttons button{margin:5px;padding:10px 20px;border-radius:8px;font-size:14px}.app__test-button:hover,.app__test-buttons button:hover{background-color:#c1292e}@media (max-width:768px){.app__test-controls{width:100%}.app__toast-input{max-width:100%}.app__test-buttons button{font-size:12px;padding:8px 16px}}.app__toast-container{top:20px;flex-direction:column;align-items:center}.toast-enter-active,.toast-leave-active{transition:opacity .5s}.toast-enter,.toast-leave-to{opacity:0}.app__input-container{position:relative;margin-bottom:10px}.app__toast-input{margin-bottom:0;padding-right:40px}.app__send-button{position:absolute;top:50%;right:10px;transform:translateY(-50%);background:0 0;border:none;color:#fff;cursor:pointer}.app__test-button--killfeed{background-color:#2c2c2c}.app__test-button--killfeed:hover{background-color:#881e20}.fade-enter-active,.fade-leave-active{transition:opacity .5s,transform .5s}.fade-enter,.fade-leave-to{opacity:0;transform:scale(.9)}
</style>