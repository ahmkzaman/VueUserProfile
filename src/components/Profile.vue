<script setup>
import { ref, reactive, computed, watch } from "vue";

const user = reactive({
  name: "K Zaman",
  profileImage: ref("/public/photo.jpg"),
  birthdate: "1980-01-01",
  email: "kzmn@gmail.com",
  description:
    "A pro web developer. Expert in both Frontend and Backend. 15 years of experience in versatile roles.",
});

const birthDate = ref(user.birthdate);
//computed property for the user's birth year.
const birthYear = computed(() => {
  const birthDateValue = new Date(birthDate.value);
  return birthDateValue.getFullYear();
});

// computed property for voter eligibility

const voterInfo = computed(() => {
  const birthDateValue = new Date(birthDate.value);
  const currentDate = new Date();
  const age = currentDate.getFullYear() - birthDateValue.getFullYear();
  const isVoter = age >= 18;
  const voterMessage = isVoter
    ? "Eligible for voting"
    : "Not eligible for voting";

  return { age, isVoter, voterMessage };
});

watch(
  () => user.birthdate,
  (newBirthdate) => {
    birthDate.value = newBirthdate;
  }
);

function onFileChange(event) {
  const file = event.target.files[0];
  if (file) {
    user.profileImage = URL.createObjectURL(file);
  }
}
const isEditing = ref(false);

function edit() {
  isEditing.value = true;
}

function save() {
  localStorage.setItem("user", JSON.stringify(user.value));

  isEditing.value = false;
}
function cancel() {
  if (isEditing.value) {
    user.name = "K Zaman";
    user.profileImage = ref("/public/photo.jpg");
    user.birthdate = "1980-01-01";
    user.email = "kzmn@gmail.com";
    user.description =
      "A pro web developer. Expert in both Frontend and Backend. 15 years of experience in versatile roles.";
    isEditing.value = false;
  }
}
</script>

<template>
  <div class="">
    <h1 class="text-3xl font-bold underline text-center text-teal-600 mb-6">
      USER DETAILS
    </h1>

    <div v-if="!isEditing" class="text-start font-bold text-slate-800 mt-6">
      <div class="flex justify-start items-center mt-4">
        <span>
          <img
            class="rounded-lg h-32 w-32 object-cover shadow-lg mb-4"
            :src="user.profileImage"
            alt="Profile image"
          />
        </span>
        <span class="ml-4 text-xl font-bold text-teal-700">{{
          voterInfo.voterMessage
        }}</span>
      </div>
      <p class="mb-4">Name: {{ user.name }}</p>
      <p class="mb-4">Birth Year: {{ birthYear }}</p>
      <p class="mb-4">Email: {{ user.email }}</p>
      <p class="mb-4">Description: {{ user.description }}</p>
      <button
        class="text-white bg-gradient-to-r from-teal-400 via-teal-500 to-teal-600 hover:bg-gradient-to-br focus:ring-teal-300 dark:focus:ring-teal-800 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2"
        @click="edit"
      >
        Edit
      </button>
    </div>

    <div v-else class="flex flex-col items-center">
      <img
        :src="user.profileImage"
        alt="Profile image"
        class="rounded-lg h-32 w-32 object-cover shadow-lg mb-4"
      />
      <input type="file" @change="onFileChange" accept="image/*" class="mb-4" />

      <input
        type="text"
        v-model="user.name"
        placeholder="Name"
        class="w-full mb-4"
      />
      <input type="date" v-model="user.birthdate" class="mb-4" />
      <input
        type="email"
        v-model="user.email"
        placeholder="Email"
        class="w-full mb-4"
      />
      <textarea
        v-model="user.description"
        placeholder="Description"
        class="w-full h-32 mb-4"
      ></textarea>

      <div class="flex justify-center">
        <button
          class="text-white bg-gradient-to-r from-green-400 via-green-500 to-green-600 hover:bg-gradient-to-br focus:ring-4 focus:outline-none focus:ring-green-300 dark:focus:ring-green-800 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2 ml-2"
          @click="save"
        >
          Save
        </button>
        <button
          class="text-white bg-gradient-to-r from-red-400 via-red-500 to-red-600 hover:bg-gradient-to-br focus:ring-4 focus:outline-none focus:ring-red-300 dark:focus:ring-red-800 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2 ml-2"
          @click="cancel"
        >
          Cancel
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
