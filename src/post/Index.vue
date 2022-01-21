<template>
  <div class="flex flex-col container mx-auto py-4">
    <div class="-my-2 overflow-x-auto sm:-mx-6 lg:-mx-8">
      <div class="py-2 align-middle inline-block min-w-full sm:px-6 lg:px-8">
        <router-link
          :to="{ name: 'post.create' }"
          class="text-white bg-gradient-to-r from-blue-500 via-blue-600 to-blue-700 hover:bg-gradient-to-br focus:ring-4 focus:ring-blue-300 dark:focus:ring-blue-800 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2"
          >ADD POST</router-link
        >
        <div
          class="shadow overflow-hidden border-b border-gray-200 sm:rounded-lg mt-4"
        >
          <table class="min-w-full divide-y divide-gray-200">
            <thead class="bg-gray-50">
              <tr>
                <th
                  scope="col"
                  class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  Title
                </th>
                <th
                  scope="col"
                  class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider"
                >
                  Content
                </th>
                <th scope="col" class="relative px-6 py-3">
                  <span class="sr-only">Edit</span>
                </th>
                <th scope="col" class="relative px-6 py-3">
                  <span class="sr-only">Delete</span>
                </th>
              </tr>
            </thead>
            <tbody class="bg-white divide-y divide-gray-200">
              <tr v-for="(post, index) in posts" :key="index">
                <td class="px-6 py-4 whitespace-nowrap">
                  <div class="text-sm text-gray-900">{{ post.title }}</div>
                </td>
                <td class="px-6 py-4 whitespace-nowrap">
                  <div class="text-sm text-gray-900">{{ post.content }}</div>
                </td>
                <td
                  class="px-6 py-4 whitespace-nowrap text-right text-sm font-medium"
                >
                  <router-link
                    :to="{ name: 'post.edit', params: { id: post.id } }"
                    class="text-indigo-600 hover:text-indigo-900"
                    >Edit</router-link
                  >
                </td>
                <td
                  class="px-6 py-4 whitespace-nowrap text-right text-sm font-medium"
                >
                  <button
                    @click.prevent="postDelete(post.id)"
                    class="text-indigo-600 hover:text-indigo-900"
                  >
                    Delete
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { onMounted, ref } from "vue";

export default {
  setup() {
    let posts = ref([]);

    onMounted(() => {
      axios
        .get("http://localhost:8000/api/post")
        .then((response) => {
          posts.value = response.data.data;
        })
        .catch((error) => {
          console.log(error.response.data);
        });
    });

    function postDelete(id) {
      axios
        .delete(`http://localhost:8000/api/post/${id}`)
        .then(() => {
          const index = this.posts.findIndex((post) => post.id === id); // find the post index
          if (~index) {
            // if the post exists in array
            this.posts.splice(index, 1);
          }
        })
        .catch((error) => {
          console.log(error.response.data);
        });
    }

    return {
      posts,
      postDelete,
    };
  },
};
</script>
