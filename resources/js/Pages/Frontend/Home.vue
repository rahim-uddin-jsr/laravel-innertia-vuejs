<template>
    <div>
        <h2 class="text-center text-4xl mb-3">{{ title }}</h2>

        <!-- Button to request Location Permission -->
        <div class="flex justify-center flex-col w-2/5 mx-auto">
            <button @click="requestLocationPermission">Request Location Permission</button>

            <!-- Button to request Notification Permission -->
            <button @click="requestNotificationPermission">Request Notification Permission</button>

            <!-- Cookie Consent Button -->
            <button @click="requestCookieConsent">Request Cookie Consent</button>

        </div>
        <!-- Show confirmation status -->
        <p v-if="locationGranted !== null">Location Permission: {{ locationGranted ? 'Granted' : 'Denied' }}</p>
        <p v-if="notificationGranted !== null">Notification Permission: {{ notificationGranted }}</p>
        <p v-if="cookieConsent !== null">Cookie Consent: {{ cookieConsent ? 'Accepted' : 'Rejected' }}</p>
    </div>
</template>

<script setup>
import { ref, defineProps } from 'vue';

// State variables to track permission statuses
const locationGranted = ref(null);
const notificationGranted = ref(null);
const cookieConsent = ref(null);

// Request Location Permission
const requestLocationPermission = () => {
    if ('geolocation' in navigator) {
        navigator.geolocation.getCurrentPosition(
            () => {
                locationGranted.value = true;
                alert('Location permission granted.');
            },
            () => {
                locationGranted.value = false;
                alert('Location permission denied.');
            }
        );
    } else {
        alert('Geolocation is not supported by your browser.');
    }
};

// Request Notification Permission
const requestNotificationPermission = () => {
    if ('Notification' in window) {
        Notification.requestPermission().then((result) => {
            notificationGranted.value = result;
            alert(`Notification permission: ${result}`);
        });
    } else {
        alert('Notifications are not supported by your browser.');
    }
};

// Request Cookie Consent
const requestCookieConsent = () => {
    const consent = confirm('Do you accept cookies?');
    cookieConsent.value = consent;
    if (consent) {
        document.cookie = "userConsent=true; path=/";
        alert('Cookies accepted.');
    } else {
        alert('Cookies rejected.');
    }
};
defineProps({
    title: String
})
</script>

<style scoped>
button {
    margin: 10px;
    padding: 8px 16px;
    background-color: #42b983;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background-color: #349e76;
}
</style>
