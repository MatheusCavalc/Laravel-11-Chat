<script setup>
import AuthUserOptions from '@/Components/AuthUserOptions.vue'
import ChatBar from '@/Components/ChatBar.vue'
import { router, usePage } from '@inertiajs/vue3';
import { computed, ref } from 'vue';
import axios from 'axios';

const props = defineProps(['chats'])

const messagesChat = ref([])
const idchat = ref('')
const infosuser = ref('')
const content = ref('')

const showBar = ref(false)
const page = usePage()
const user = computed(() => page.props.auth.user)

const messages = (chatId, user) => {
    axios.post('/messages', {
        chatId: chatId,
    }).then((response) => {
        messagesChat.value = response.data
        idchat.value = chatId
        infosuser.value = user
    }).catch(error => {
        console.log(error)
    });
}

function sendMessage() {
    router.post('/new-message', {
        conversation_id: idchat.value,
        content: content.value
    })
}
</script>

<template>
    <div class="lg:grid lg:grid-cols-4 lg:gap-2 bg-gray-50">

        <div>
            <!--
                <ChatCard :chats="props.chats" @send-chat-id="chatId" @send-messages="addMessages"
                @send-user-infos="userInfos" />
            -->

            <section class="flex flex-col justify-center antialiased bg-gray-50 text-gray-600 lg:h-screen p-4">
                <div class="h-full">
                    <div class="relative max-w-[340px] mx-auto bg-white shadow-lg rounded-lg">

                        <div class="mb-2 py-3 px-4">
                            <AuthUserOptions />
                        </div>

                        <Transition>
                            <div class="mb-2 py-3 px-4" v-show="showBar">
                                <ChatBar />
                            </div>
                        </Transition>


                        <div class="py-3 px-5">
                            <h3 class="text-xs font-semibold uppercase text-gray-400 mb-1">Chats</h3>
                            <div class="divide-y divide-gray-200">
                                <div v-for="chat in chats" :key="chat.id">
                                    <button v-if="chat.user_1.id == user.id"
                                        class="w-full text-left py-2 focus:outline-none focus-visible:bg-indigo-50">
                                        <div @click="messages(chat.id, chat.user_2)" class="flex items-center">
                                            <img class="rounded-full items-start flex-shrink-0 mr-3"
                                                src="https://res.cloudinary.com/dc6deairt/image/upload/v1638102932/user-32-01_pfck4u.jpg"
                                                width="32" height="32" alt="Marie Zulfikar" />
                                            <div>
                                                <h4 class="text-sm font-semibold text-gray-900">{{ chat.user_2.name }}
                                                </h4>
                                                <div class="text-[13px]">The video chat ended · 2hrs</div>
                                            </div>
                                        </div>
                                    </button>
                                    <button v-else
                                        class="w-full text-left py-2 focus:outline-none focus-visible:bg-indigo-50">
                                        <div @click="messages(chat.id, chat.user_1)" class="flex items-center">
                                            <img class="rounded-full items-start flex-shrink-0 mr-3"
                                                src="https://res.cloudinary.com/dc6deairt/image/upload/v1638102932/user-32-01_pfck4u.jpg"
                                                width="32" height="32" alt="Marie Zulfikar" />
                                            <div>
                                                <h4 class="text-sm font-semibold text-gray-900">{{ chat.user_1.name }}
                                                </h4>
                                                <div class="text-[13px]">The video chat ended · 2hrs</div>
                                            </div>
                                        </div>
                                    </button>
                                </div>
                            </div>
                        </div>


                        <!-- Bottom right button -->
                        <button @click="showBar = !showBar"
                            class="absolute bottom-5 right-5 inline-flex items-center text-sm font-medium text-white bg-indigo-500 hover:bg-indigo-600 rounded-full text-center px-3 py-2 shadow-lg focus:outline-none focus-visible:ring-2">
                            <svg class="w-3 h-3 fill-current text-indigo-300 flex-shrink-0 mr-2" viewBox="0 0 12 12">
                                <path
                                    d="M11.866.146a.5.5 0 0 0-.437-.139c-.26.044-6.393 1.1-8.2 2.913a4.145 4.145 0 0 0-.617 5.062L.305 10.293a1 1 0 1 0 1.414 1.414L7.426 6l-2 3.923c.242.048.487.074.733.077a4.122 4.122 0 0 0 2.933-1.215c1.81-1.809 2.87-7.94 2.913-8.2a.5.5 0 0 0-.139-.439Z" />
                            </svg>
                            <span>New Chat</span>
                        </button>


                    </div>
                </div>
            </section>
        </div>

        <div class="col-span-3">
            <div class="mr-2 bg-white">
                <!--
                    <ChatField :chatId="idchat" :messages="messagesChat" :infosuser="infosuser" />
                -->

                <Transition>
                    <div v-show="idchat" class="flex-1 p:2 sm:p-6 justify-between flex flex-col h-screen">

                        <div class="flex sm:items-center justify-between py-3 border-b-2 border-gray-200">
                            <div class="relative flex items-center space-x-4">
                                <div class="relative">
                                    <span class="absolute text-green-500 right-0 bottom-0">
                                        <svg width="20" height="20">
                                            <circle cx="8" cy="8" r="8" fill="currentColor"></circle>
                                        </svg>
                                    </span>
                                    <img src="https://images.unsplash.com/photo-1549078642-b2ba4bda0cdb?ixlib=rb-1.2.1&amp;ixid=eyJhcHBfaWQiOjEyMDd9&amp;auto=format&amp;fit=facearea&amp;facepad=3&amp;w=144&amp;h=144"
                                        alt="" class="w-10 sm:w-16 h-10 sm:h-16 rounded-full">
                                </div>
                                <div class="flex flex-col leading-tight">
                                    <div class="text-2xl mt-1 flex items-center">
                                        <span class="text-gray-700 mr-3">{{ infosuser.name }}</span>
                                    </div>
                                    <span class="text-lg text-gray-600">{{ infosuser.email }}</span>
                                </div>
                            </div>
                            <!--
            <div class="flex items-center space-x-2">
                <button type="button"
                    class="inline-flex items-center justify-center rounded-lg border h-10 w-10 transition duration-500 ease-in-out text-gray-500 hover:bg-gray-300 focus:outline-none">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor"
                        class="h-6 w-6">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                            d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path>
                    </svg>
                </button>
                <button type="button"
                    class="inline-flex items-center justify-center rounded-lg border h-10 w-10 transition duration-500 ease-in-out text-gray-500 hover:bg-gray-300 focus:outline-none">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor"
                        class="h-6 w-6">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                            d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z">
                        </path>
                    </svg>
                </button>
                <button type="button"
                    class="inline-flex items-center justify-center rounded-lg border h-10 w-10 transition duration-500 ease-in-out text-gray-500 hover:bg-gray-300 focus:outline-none">
                    <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor"
                        class="h-6 w-6">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                            d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9">
                        </path>
                    </svg>
                </button>
            </div>
            -->
                        </div>

                        <div id="messages overflow-y-scroll"
                            class="flex flex-col space-y-4 p-3 overflow-y-auto scrollbar-thumb-blue scrollbar-thumb-rounded scrollbar-track-blue-lighter scrollbar-w-2 scrolling-touch">
                            <div v-for="message in messagesChat" :key="message.id" class="chat-message">
                                <div v-if="message.user_id == user.id" class="flex items-end justify-end">
                                    <div class="flex flex-col space-y-2 text-xs max-w-xs mx-2 order-1 items-end">
                                        <div><span
                                                class="px-4 py-2 rounded-lg inline-block rounded-br-none bg-blue-600 text-white ">{{
                                message.content }}</span></div>
                                    </div>
                                    <img src="https://images.unsplash.com/photo-1590031905470-a1a1feacbb0b?ixlib=rb-1.2.1&amp;ixid=eyJhcHBfaWQiOjEyMDd9&amp;auto=format&amp;fit=facearea&amp;facepad=3&amp;w=144&amp;h=144"
                                        alt="My profile" class="w-6 h-6 rounded-full order-2">
                                </div>

                                <div v-else class="flex items-end">
                                    <div class="flex flex-col space-y-2 text-xs max-w-xs mx-2 order-2 items-start">
                                        <div><span
                                                class="px-4 py-2 rounded-lg inline-block rounded-bl-none bg-gray-300 text-gray-600">{{
                                message.content }}</span></div>
                                    </div>
                                    <img src="https://images.unsplash.com/photo-1549078642-b2ba4bda0cdb?ixlib=rb-1.2.1&amp;ixid=eyJhcHBfaWQiOjEyMDd9&amp;auto=format&amp;fit=facearea&amp;facepad=3&amp;w=144&amp;h=144"
                                        alt="My profile" class="w-6 h-6 rounded-full order-1">
                                </div>
                            </div>
                        </div>


                        <div class="border-t-2 border-gray-200 px-4 pt-4 mb-2 sm:mb-0">
                            <div class="relative flex">
                                <span class="absolute inset-y-0 flex items-center">
                                    <button type="button"
                                        class="inline-flex items-center justify-center rounded-full h-12 w-12 transition duration-500 ease-in-out text-gray-500 hover:bg-gray-300 focus:outline-none">
                                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"
                                            stroke="currentColor" class="h-6 w-6 text-gray-600">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                                d="M19 11a7 7 0 01-7 7m0 0a7 7 0 01-7-7m7 7v4m0 0H8m4 0h4m-4-8a3 3 0 01-3-3V5a3 3 0 116 0v6a3 3 0 01-3 3z">
                                            </path>
                                        </svg>
                                    </button>
                                </span>

                                <form @submit.prevent="sendMessage" class="w-full">
                                    <input v-model="content" type="text" placeholder="Write your message!"
                                        class="w-full focus:outline-none focus:placeholder-gray-400 text-gray-600 placeholder-gray-600 pl-12 bg-gray-200 rounded-full py-3">

                                    <div class="absolute right-0 items-center inset-y-0 hidden sm:flex">
                                        <!--
                    <button type="button"
                        class="inline-flex items-center justify-center rounded-full h-10 w-10 transition duration-500 ease-in-out text-gray-500 hover:bg-gray-300 focus:outline-none">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor"
                            class="h-6 w-6 text-gray-600">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M15.172 7l-6.586 6.586a2 2 0 102.828 2.828l6.414-6.586a4 4 0 00-5.656-5.656l-6.415 6.585a6 6 0 108.486 8.486L20.5 13">
                            </path>
                        </svg>
                    </button>
                    <button type="button"
                        class="inline-flex items-center justify-center rounded-full h-10 w-10 transition duration-500 ease-in-out text-gray-500 hover:bg-gray-300 focus:outline-none">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor"
                            class="h-6 w-6 text-gray-600">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M3 9a2 2 0 012-2h.93a2 2 0 001.664-.89l.812-1.22A2 2 0 0110.07 4h3.86a2 2 0 011.664.89l.812 1.22A2 2 0 0018.07 7H19a2 2 0 012 2v9a2 2 0 01-2 2H5a2 2 0 01-2-2V9z">
                            </path>
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M15 13a3 3 0 11-6 0 3 3 0 016 0z"></path>
                        </svg>
                    </button>
                    <button type="button"
                        class="inline-flex items-center justify-center rounded-full h-10 w-10 transition duration-500 ease-in-out text-gray-500 hover:bg-gray-300 focus:outline-none">
                        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor"
                            class="h-6 w-6 text-gray-600">
                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                                d="M14.828 14.828a4 4 0 01-5.656 0M9 10h.01M15 10h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z">
                            </path>
                        </svg>
                    </button>
                    -->
                                        <button type="submit"
                                            class="inline-flex items-center justify-center rounded-r-full px-4 py-3 transition duration-500 ease-in-out text-white bg-blue-500 hover:bg-blue-400 focus:outline-none">
                                            <span class="font-bold">Send</span>
                                            <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"
                                                fill="currentColor" class="h-6 w-6 ml-2 transform rotate-90">
                                                <path
                                                    d="M10.894 2.553a1 1 0 00-1.788 0l-7 14a1 1 0 001.169 1.409l5-1.429A1 1 0 009 15.571V11a1 1 0 112 0v4.571a1 1 0 00.725.962l5 1.428a1 1 0 001.17-1.408l-7-14z">
                                                </path>
                                            </svg>
                                        </button>
                                    </div>
                                </form>
                            </div>
                        </div>


                    </div>
                </Transition>
            </div>
        </div>
    </div>
</template>
