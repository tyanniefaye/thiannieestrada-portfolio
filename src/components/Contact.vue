<template>
	<section id="contact" class="contact-section">
	    <div class="container">
	      <div class="text-center mb-5">
	        <h2 class="pt-4 text-white section-title">Contact</h2>
	        <p class="text-white mt-3">Let's work together on your next project</p>
	      </div>

	      <div class="row align-items-center">
	        <!-- Map Column -->
	        <div class="col-md-6 mb-4 mb-md-0">
	          <div class="map-wrapper shadow rounded overflow-hidden">
	            <iframe
	              src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3859.043878694711!2d120.98914821471804!3d14.594891089812636!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x3397cbe1b45e24d5%3A0x5a0f09c3465d26f1!2sCentro%20Escolar%20University!5e0!3m2!1sen!2sph!4v1697444405584!5m2!1sen!2sph"
	              class="map-iframe"
	              allowfullscreen=""
	              loading="lazy"
	              referrerpolicy="no-referrer-when-downgrade"
	            ></iframe>
	          </div>
	        </div>

	        <!-- Form Column -->
	        <div class="col-md-6">
	          <form @submit.prevent="submitForm">
	            <div class="mb-3">
	              <input
	                type="text"
	                class="form-control form-control-custom"
	                placeholder="First Name M.I Last Name" v-model="name"
	                required/>
	            </div>
	            <div class="mb-3">
	              <input
	                type="email"
	                class="form-control form-control-custom"
	                placeholder="Email" v-model="email"
	                required/>
	            </div>
	            <div class="mb-3">
	              <textarea
	                class="form-control form-control-custom"
	                rows="4"
	                placeholder="Message" v-model="message"
	                required></textarea>
	            </div>
	            <div class="d-flex justify-content-end mt-2">
	            		<div ref="recaptchaContainer"></div>
	            </div>

	            <!-- Icons + Submit -->
	            <div class="d-flex justify-content-between align-items-center">
	              <div class="social-icons">
	                <a href="#" class="social-icon"><i class="bi bi-linkedin"></i></a>
	                <a href="#" class="social-icon"><i class="bi bi-github"></i></a>
	                <a href="#" class="social-icon"><i class="bi bi-globe"></i></a>
	              </div>
	              <button type="submit" class="btn btn-primary px-4 rounded-pill" :disabled="isLoading">{{isLoading ? "Sending..." : "Submit"}} </button>
	            </div>
	          </form>
	        </div>
	      </div>
	    </div>
	  </section>
</template>

<script setup>
    
    import { ref, onMounted, onBeforeUnmount } from 'vue';
    import { Notyf } from 'notyf';
    import 'notyf/notyf.min.css';

    const notyf = new Notyf();

    const WEB3FORMS_ACCESS_KEY = "84c4e636-7fd4-4cc3-87a8-89fd056380df";

    const subject = "New message from Portfolio Contact Form";

    const name = ref("");
    const email = ref("");
    const message = ref("");

    const isLoading = ref(false);


    const submitForm = async() => {

        if(!recaptchaToken.value){
            notyf.error("Please verify that you are not a robot.");
            return;
        }

        isLoading.value = true;

        try {

            const response = await fetch("https://api.web3forms.com/submit", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    Accept: "application/json",
                },
                body: JSON.stringify({
                    access_key: WEB3FORMS_ACCESS_KEY,
                    subject: subject,
                    name: name.value,
                    email: email.value,
                    message: message.value
                })
            })
            const result = await response.json();

            if (result.success){
                console.log(result);
                isLoading.value = false;
                notyf.success("Message sent");
            }
        } catch (error){
            console.log(error);
            isLoading.value = false;
            notyf.error("Failed to send message");
        } finally {
            resetRecaptcha();
        }
    }


    const SITE_KEY = '6Lds7gksAAAAAFQh0WxWmwWH3Dypkf50IbWr2hvS'

    const recaptchaContainer = ref(null);
    const recaptchaWidgetId = ref(null);
    const recaptchaToken = ref('');

    function onRecaptchaSuccess(token){
        recaptchaToken.value = token;
    }

    function onRecaptchaExpired(){
        recaptchaToken.value = '';
    }

    function renderRecaptcha(){
        if(!window.grecaptcha){
            console.error('recaptcha not loaded');
            return;
        }
        recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
            sitekey: SITE_KEY,
            size: 'normal',
            callback: onRecaptchaSuccess,
            'expired-callback': onRecaptchaExpired
        });
    }

    function resetRecaptcha(){
        if(recaptchaWidgetId.value !==null){
            window.grecaptcha.reset(recaptchaWidgetId.value);
            recaptchaToken.value = '';
        }
    }

    onMounted(() => {
        const interval = setInterval(() => {
            if(window.grecaptcha && window.grecaptcha.render){
                renderRecaptcha();
                clearInterval(interval)
            }
        }, 100);

        onBeforeUnmount(() => {
            clearInterval(interval)
        })
    });

</script>

<style scoped>
  section {
    background-color: #0f172a !important;
  }
</style>