<template>
    <div class="w-full flex justify-center">
        <input placeholder="Enter Book Title here" type="text" class="mt-10 p-2 border-blue-500 border-2"
            v-model="text" />
    </div>
    <div class="mt-10 p-4 flex flex-wrap justify-center">
        <div class="ml-4 text-2xl text-blue-400" v-for="(book, idx) in filteredBooks" :key="idx">
            <router-link :to="`/about/${titleIDLookup[book.title]}`">
                {{ book.title }}
            </router-link>
        </div>
    </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import { reactive, toRefs, computed } from "vue";


export default {
    name: 'HomeView',
    //   components: {
    //     HelloWorld
    //   }
    setup() {

        const state = reactive({
            books: [],
            filteredBooks: computed(() => updateBook()),
            text: "",
            titleIDLookup: {}
        })

        fetch("/api/v1/book")
            .then((res) => res.json())
            .then((data) => {
                console.log(data)
                state.books = data
                state.titleIDLookup = data.reduce((acc, curr) =>
                    acc = { ...acc, [curr.title]: curr.id }
                    , {})
            })

        function updateBook() {
            if (!state.text) {
                return []
            }
            return state.books.filter((book) =>
                book.title.includes(state.text)
            )
        }

        return { ...toRefs(state) };
    }
}
</script>
