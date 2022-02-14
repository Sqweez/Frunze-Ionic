<template>
    <ion-page>
        <div>
            <div class="intro-wrapper d-flex align-items-center justify-content-center text-center">
                <!-- Background Shape-->
                <div class="container"><img class="big-logo" src="./../img/logo__main.png" alt=""></div>
            </div>
            <div class="get-started-btn">
                <a class="btn btn-success btn-lg w-100 button" href="#" @click.prevent="openBrowser">Начать</a>
            </div>
        </div>
    </ion-page>
</template>

<script>
    import { IonPage } from '@ionic/vue';
    import { InAppBrowser } from '@ionic-native/in-app-browser';
    import { defineComponent } from 'vue';
    import { Plugins } from '@capacitor/core';
    const { PushNotifications, SplashScreen } = Plugins;

    export default defineComponent({
        name: 'Home',
        components: {
            IonPage,
        },
        data: () => ({
            url: 'https://кр.com/',
            browser: null,
            token: null,
        }),
        methods: {
            toggleBrowser() {
                this.openBrowser();
            },
            async openBrowser() {
                const browser = await InAppBrowser.create(`${this.url}?push_token=${this.token}&secret=VcMbPLzzFh`, '_blank', {
                    location: 'no',
                    hideurlbar: 'yes',
                    zoom: 'no',
                    fullscreen: 'no',
                    hidenavigationbuttons: 'yes',
                    toolbar: 'no'
                });
                await SplashScreen.hide({
                    fadeOutDuration: 200,
                }, () => {
                    browser.show();
                });
            },
            async init() {
                try {
                    await PushNotifications.register();
                    const pushNotification = localStorage.getItem('push_token') || null;
                    if (!pushNotification) {
                        PushNotifications.addListener('registration', async token => {
                            this.token = token.value;
                            localStorage.setItem('push_token', this.token);
                        })
                    } else {
                        this.token = pushNotification;
                    }
                } catch (e) {
                    console.log(e);
                } finally {
                    await this.openBrowser();
                }
            }
        },
        async created() {
            await this.init();
        }
    });
</script>

<style scoped>
    @import url('https://fonts.googleapis.com/css2?family=Rubik&display=swap');    /* Reboot CSS */
    .ion-page {
        background-color: #EDDFD4;
    }

    .big-logo {
        width: 40%;
        height: auto;
    }

    .btn:focus {
        box-shadow: none;
    }

    .container {
        width: 100%;
        padding-right: 15px;
        padding-left: 15px;
        margin-right: auto;
        margin-left: auto;
    }

    img {
        max-width: 100%;
        height: auto;
    }

    img {
        max-width: 100%;
        height: auto;
    }

    /* Hero CSS */
    .intro-wrapper {
        position: relative;
        width: 100%;
        height: 100vh;
        background: #fff;
        z-index: 1;
        display: flex;
        justify-content: center;
        align-items: center;
        text-align: center;
    }


    .get-started-btn {
        position: fixed;
        z-index: 100;
        bottom: 40px;
        width: 280px;
        left: 50%;
        margin-left: -140px;
    }

    .button {
        border-radius: 3.84px;
        color: #fff !important;
        font-size: 14px;
        letter-spacing: 0.1em;
        line-height: 18.66px;
        text-align: center;
        transition: all 100ms ease-in-out;
        padding: 10px;
        width: 100%;
        display: block;
        background: #0C9344;
        outline: none;
        border: none;
        font-family: 'Rubik', serif;
        font-style: normal;
        font-weight: normal;
        text-transform: uppercase;
        text-decoration: none;
    }

    .lds-ring {
        transform: scale(.5);
        display: inline-block;
        position: relative;
        width: 80px;
        height: 80px;
    }

    .lds-ring div {
        box-sizing: border-box;
        display: block;
        position: absolute;
        width: 64px;
        height: 64px;
        margin: 8px;
        border: 8px solid #DB8D38;
        border-radius: 50%;
        animation: lds-ring 1.2s cubic-bezier(0.5, 0, 0.5, 1) infinite;
        border-color: #DB8D38 transparent transparent transparent;
    }

    .lds-ring div:nth-child(1) {
        animation-delay: -0.45s;
    }

    .lds-ring div:nth-child(2) {
        animation-delay: -0.3s;
    }

    .lds-ring div:nth-child(3) {
        animation-delay: -0.15s;
    }

    @keyframes lds-ring {
        0% {
            transform: rotate(0deg);
        }
        100% {
            transform: rotate(360deg);
        }
    }


</style>