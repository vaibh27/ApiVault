<template>
  <div
    class="modal fade"
    id="submitApiModal"
    tabindex="-1"
    aria-labelledby="submitApiModalLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog modal-dialog-centered">
      <div class="custom-border-radius glass-card modal-content">
        <div class="py-0 custom-border modal-header">
          <h1
            v-if="!successSubmit"
            class="ps-2 text-wrapper modal-title fs-5"
            id="submitApiModalLabel"
          >
            {{ title }}
          </h1>
          <h1
            v-else
            class="ps-2 text-wrapper modal-title fs-5"
            id="submitApiModalLabel"
          >
            API Submitted
          </h1>
          <GenericsButton
            @click.prevent="restoreSuccessState"
            class="mobile-wrapper-btn m-0 mt-3 me-2"
            data-bs-dismiss="modal"
            aria-label="Close"
          >
            <font-awesome-icon :icon="['fas', 'xmark']" />
          </GenericsButton>
        </div>
        <div v-if="!notAuth" class="modal-body p-4 pt-2">
          <div v-if="successSubmit">
            <AnimationSuccessCheckmark />
          </div>
          <form v-else>
            <div
              class="input-group mb-3"
              style="justify-content: space-between"
            >
              <div class="mobile-wrapper-section align-items-center">
                <label for="recipient-name" class="text-wrapper col-form-label"
                  >Name:</label
                >
                <input
                  v-model="name"
                  type="text"
                  placeholder="Your API name"
                  class="inverted-input-box text-wrapper form-control"
                  id="recipient-name"
                  style="margin-right: 6rem"
                />
              </div>
              <div class="mobile-wrapper-section align-items-center">
                <label for="message-text" class="text-wrapper col-form-label"
                  >Auth:</label
                >
                <select
                  v-model="auth"
                  placeholder="Choose Auth type"
                  class="px-3 inverted-input-box text-wrapper form-select"
                  aria-label="Select corse state"
                >
                  <option value="" selected disabled>Choose an option</option>
                  <option value="apiKey">API Key</option>
                  <option value="OAuth">OAuth</option>
                </select>
              </div>
            </div>
            <div class="mb-3">
              <label for="recipient-name" class="text-wrapper col-form-label"
                >Category:</label
              >
              <select
                v-model="category"
                placeholder="Select API category"
                class="inverted-input-box text-wrapper form-select"
                aria-label="Default select example"
              >
                <option value="" selected disabled>Select category</option>
                <option
                  v-for="category in categories"
                  :key="category.id"
                  :value="category.id"
                >
                  {{ category.name }}
                </option>
              </select>
            </div>
            <div class="mb-3">
              <label
                v-if="isValidUrl"
                for="message-text"
                class="text-wrapper col-form-label"
                >Url:</label
              >
              <label
                v-else
                for="message-text"
                class="col-form-label"
                style="color: red"
                >Url is not valid!</label
              >
              <input
                v-model="url"
                placeholder="Documentation URL"
                type="text"
                class="text-wrapper inverted-input-box form-control"
                id="message-text"
              />
            </div>
            <div class="mb-3">
              <label for="message-text" class="text-wrapper col-form-label"
                >Description:</label
              >
              <input
                v-model="description"
                placeholder="Describe your API"
                type="text"
                class="text-wrapper inverted-input-box form-control"
                id="message-text"
              />
            </div>
            <div id="space-between-form" class="input-group mb-0">
              <div class="mobile-wrapper-section align-items-center">
                <label
                  for="message-text"
                  class="me-2 text-wrapper col-form-label primary"
                  >Does your API use CORS?:</label
                >
                <select
                  v-model="cors"
                  placeholder="Choose an option"
                  class="inverted-input-box text-wrapper form-select"
                  aria-label="Select corse state"
                >
                  <option value="" selected disabled>Choose an option</option>
                  <option value="true">Yes</option>
                  <option value="false">No</option>
                </select>
              </div>
              <div class="text-wrapper align-items-center align-dividers">
                •
              </div>
              <div class="mobile-wrapper-section align-items-center">
                <label
                  for="message-text"
                  class="me-2 text-wrapper col-form-label"
                  >Is your API HTTPS?:</label
                >
                <select
                  v-model="https"
                  placeholder="Choose an option"
                  class="inverted-input-box text-wrapper form-select"
                  aria-label="Select corse state"
                >
                  <option value="" selected disabled>Choose an option</option>
                  <option value="true">Yes</option>
                  <option value="false">No</option>
                </select>
              </div>
            </div>
          </form>
          <p v-if="!isValidInput" class="m-0 mt-3 error-validation">
            Some fields are empty!!
          </p>
        </div>
        <div v-else class="modal-body p-4 pt-2">
          <h3 class="text-wrapper">Please login to submit a new Api</h3>
        </div>
        <div v-if="!notAuth" class="px-4 custom-border modal-footer">
          <p
            v-if="!successSubmit"
            class="text-wrapper"
            style="margin-right: auto; opacity: 70%"
          >
            All fields are mandatory!
          </p>
          <p
            v-else
            class="text-wrapper"
            style="margin-right: auto; opacity: 70%"
          >
            Your API will be reviewed soon!
          </p>
          <NuxtLink to="/user/apis">
            <GenericsButton
              v-if="successSubmit"
              @click="restoreSuccessState"
              class="mobile-wrapper-footer-btn m-1"
              data-bs-dismiss="modal"
              >Check APIs</GenericsButton
            >
          </NuxtLink>
          <GenericsButton
            @click.prevent="restoreSuccessState"
            class="mobile-wrapper-footer-btn m-1"
            data-bs-dismiss="modal"
            >Close</GenericsButton
          >
          <GenericsButton
            class="mobile-werapper-footer-btn2"
            v-if="!successSubmit"
            style="position: relative"
            @click.prevent="validateInput"
            :isInverted="true"
            :class="['text-wrapper-inverted dm-1', { tremor: !isValidInput }]"
            >Submit your API</GenericsButton
          >
        </div>
        <div v-else class="px-4 custom-border modal-footer">
          <GenericsButton
            @click.prevent="restoreSuccessState"
            class="mobile-wrapper-footer-btn m-1"
            data-bs-dismiss="modal"
            >Close</GenericsButton
          >
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import ApivaultServices from "~/services/ApivaultServices";
import { Category } from "~/models/types";
defineProps({
  title: {
    type: String,
    required: true,
  },
});

const regex = new RegExp(
  "/^(https?):\/\/([^\s\/$.?#].[^\s]*)\.[^\s]{2,}|www\.([^\s\/$.?#].[^\s]*)\.[^\s]{2,}$"
);
const accessToken = useCookie("accessToken");
const name = ref<string>("");
const description = ref<string>("");
const auth = ref<string>("");
const url = ref<string>("");
const category = ref<number>(0);
const cors = ref<boolean>(false);
const https = ref<boolean>(false);
const successSubmit = ref<boolean>(false);
const isValidUrl = ref<boolean>(true);
const notAuth = ref<boolean>(false);

const isValidInput = ref<boolean>(true);
const shakeAnimationState = ref<boolean>(false);
const validateInput = () => {
  if (regex.exec(url.value) === null) {
    isValidUrl.value = false;
    setTimeout(() => {
      isValidUrl.value = true;
    }, 4000);
  }
  if (
    name.value === "" ||
    description.value === "" ||
    url.value === "" ||
    category.value === null ||
    cors.value === null ||
    https.value === null
  ) {
    isValidInput.value = false;
    setTimeout(() => {
      isValidInput.value = true;
      shakeAnimationState.value = true;
    }, 4000);
    return;
  }
  submitApi();
};

const restoreSuccessState = () => {
  setTimeout(() => {
    successSubmit.value = false;
  }, 1000);
};

const submitApi = async () => {
  const resp = await ApivaultServices.submitApi(
    accessToken.value!,
    name.value,
    auth.value,
    category.value,
    cors.value,
    description.value,
    https.value,
    url.value
  );

  switch (resp) {
    case 201:
      successSubmit.value = true;
      break;
    case 400:
      console.error("Something gone wrong.");
      break;
    default:
      break;
  }
};

const categories = ref<Category[]>();
onMounted(async () => {
  if (accessToken.value === "" || accessToken.value === null)
    notAuth.value = true;
  categories.value = await ApivaultServices.categories();
});
</script>

<style scoped>
.inverted-input-box {
  background: var(--bg-trending-category);
  border: hidden;
}

.custom-border {
  border: hidden;
}

.custom-border-radius {
  border-radius: 8px;
}

#space-between-form {
  justify-content: space-between;
}

.align-dividers {
  align-self: end;
  margin-bottom: 9px;
}

.error-validation {
  color: red;
}

.tremor {
  animation: shake-animation 4.2s linear;
}

@keyframes shake-animation {
  0% {
    transform: translate(0, 0);
  }
  1.78571% {
    transform: translate(5px, 0);
  }
  3.57143% {
    transform: translate(0, 0);
  }
  5.35714% {
    transform: translate(5px, 0);
  }
  7.14286% {
    transform: translate(0, 0);
  }
  8.92857% {
    transform: translate(5px, 0);
  }
  10.71429% {
    transform: translate(0, 0);
  }
  100% {
    transform: translate(0, 0);
  }
}

@media (max-width: 480px) {
  .mobile-wrapper-btn {
    width: 13%;
  }
  .mobile-wrapper-section {
    width: 100%;
  }
  .mobile-wrapper-footer-btn {
    font-size: small;
    width: 30%;
  }
  .mobile-werapper-footer-btn2 {
    font-size: small;
  }
}
</style>
