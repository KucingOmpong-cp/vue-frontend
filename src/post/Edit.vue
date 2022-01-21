<template>
  <div class="mt-10 sm:mt-0 container mx-auto py-4">
    <div class="md:grid md:grid-cols-3 md:gap-6">
      <div class="md:col-span-1">
        <div class="px-4 sm:px-0">
          <h3 class="text-lg font-medium leading-6 text-gray-900">
            Personal Information
          </h3>
          <p class="mt-1 text-sm text-gray-600">
            Use a permanent address where you can receive mail.
          </p>
        </div>
      </div>
      <div class="mt-5 md:mt-0 md:col-span-2">
        <form @submit.prevent="update">
          <div class="shadow overflow-hidden sm:rounded-md">
            <div class="px-4 py-5 bg-white sm:p-6">
              <div class="grid grid-cols-6 gap-6">
                <div class="col-span-6">
                  <label
                    for="street-address"
                    class="block text-sm font-medium text-gray-700"
                    >Title</label
                  >
                  <input
                    type="text"
                    v-model="post.title"
                    class="mt-1 focus:ring-indigo-500 focus:border-indigo-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md px-2 py-1"
                    placeholder="Masukkan Judul Post"
                  />
                  <!-- validation -->
                  <div v-if="validation.title">
                    {{ validation.title[0] }}
                  </div>
                </div>
                <div class="col-span-6">
                  <label
                    for="street-address"
                    class="block text-sm font-medium text-gray-700"
                    >Content</label
                  >
                  <textarea
                    class="mt-1 focus:ring-indigo-500 focus:border-indigo-500 block w-full shadow-sm sm:text-sm border-gray-300 rounded-md px-2 py-1"
                    rows="4"
                    v-model="post.content"
                    placeholder="Masukkan Konten Post"
                  ></textarea>
                  <!-- validation -->
                  <div v-if="validation.content">
                    {{ validation.content[0] }}
                  </div>
                </div>
              </div>
            </div>
            <div class="px-4 py-3 bg-gray-50 text-right sm:px-6">
              <button
                type="submit"
                class="inline-flex justify-center py-2 px-4 border border-transparent shadow-sm text-sm font-medium rounded-md text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
              >
                Save
              </button>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive, ref, onMounted } from "vue";
import { useRouter, useRoute } from "vue-router";
import axios from "axios";

export default {
  setup() {
    const post = reactive({
      title: "",
      content: "",
    });

    const validation = ref([]);

    const router = useRouter();

    const route = useRoute();

    onMounted(() => {
      axios
        .get(`http://localhost:8000/api/post/${route.params.id}`)
        .then((response) => {
          post.title = response.data.data.title;
          post.content = response.data.data.content;
        })
        .catch((error) => {
          console.log(error.response.data);
        });
    });

    function update() {
      let title = post.title;
      let content = post.content;

      axios
        .put(`http://localhost:8000/api/post/${route.params.id}`, {
          title: title,
          content: content,
        })
        .then(() => {
          router.push({
            name: "post.index",
          });
        })
        .catch((error) => {
          validation.value = error.response.data;
        });
    }

    return {
      post,
      validation,
      router,
      update,
    };
  },
};
</script>
