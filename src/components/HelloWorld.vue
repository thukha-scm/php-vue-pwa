<template>
  <div>
    <h1>Javascript & PHP push notif demo</h1>
    <button @click="enableNotif">Enable Notif</button>

    <p>{{ consoleData }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      consoleData: '', // Initialized with an empty string
    }
  },
    mounted() {
    // Simulate getting data from the console
    this.enableNotif();
  },
  methods: {
    enableNotif() {
      if ('Notification' in window) {
        Notification.requestPermission().then((permission) => {
          if (permission === 'granted') {
            // Check if service worker is available
            if ('serviceWorker' in navigator) {
              navigator.serviceWorker.ready.then((swRegistration) => {
                // Subscribe to push notifications
                swRegistration.pushManager.subscribe({
                  userVisibleOnly: true,
                  applicationServerKey: urlBase64ToUint8Array('BE1tfgottBh58rAv4pXi3-omdZk5cde5Lfon2HVSkOtJewBF8sBdQdiuOBGo9UOg9C50H__lVvci3wYLIb4BcOQ'),
                }).then((subscription) => {
                  console.log(JSON.stringify(subscription));
                  this.consoleData = JSON.stringify(subscription);
                });
              });
            }
          }
        });
      }
    },
  },
};

function urlBase64ToUint8Array(base64String) {
  const padding = '='.repeat((4 - (base64String.length % 4)) % 4);
  const base64 = (base64String + padding)
    .replace(/-/g, '+')
    .replace(/_/g, '/');

  const rawData = window.atob(base64);
  const outputArray = new Uint8Array(rawData.length);

  for (let i = 0; i < rawData.length; i++) {
    outputArray[i] = rawData.charCodeAt(i);
  }
  return outputArray;
}
</script>
