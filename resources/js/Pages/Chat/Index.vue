<script setup>
import AuthUserOptions from '@/Components/AuthUserOptions.vue'
import ChatBar from '@/Components/ChatBar.vue'
import MicrophoneIcon from '@/Components/Icons/MicrophoneIcon.vue'
import SendIcon from '@/Components/Icons/SendIcon.vue'
import LeafIcon from '@/Components/Icons/LeafIcon.vue'
import { router, usePage } from '@inertiajs/vue3';
import { computed, ref } from 'vue';
import axios from 'axios';

const props = defineProps(['chats'])

const messagesChat = ref([])
const idChat = ref('')
const userInfos = ref('')
const content = ref('')

const showSearchBar = ref(false)
const page = usePage()
const user = computed(() => page.props.auth.user)

const messages = (chatId, user) => {
    axios.post('/messages', {
        chatId: chatId,
    }).then((response) => {
        messagesChat.value = response.data
        idChat.value = chatId
        userInfos.value = user
    }).catch(error => {
        console.log(error)
    });
}

function sendMessage() {
    router.post('/new-message', {
        conversation_id: idChat.value,
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
                            <div class="mb-2 py-3 px-4" v-show="showSearchBar">
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
                        <button @click="showSearchBar = !showSearchBar"
                            class="absolute bottom-5 right-5 inline-flex items-center text-sm font-medium text-white bg-indigo-500 hover:bg-indigo-600 rounded-full text-center px-3 py-2 shadow-lg focus:outline-none focus-visible:ring-2">
                            <LeafIcon />
                            <span>New Chat</span>
                        </button>
                    </div>
                </div>
            </section>
        </div>

        <div class="col-span-3">
            <div class="mr-2 bg-white">
                <!--
                    <ChatField :chatId="idChat" :messages="messagesChat" :userInfos="userInfos" />
                -->

                <Transition>
                    <div v-show="idChat" class="flex-1 p:2 sm:p-6 justify-between flex flex-col h-screen">

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
                                        <span class="text-gray-700 mr-3">{{ userInfos.name }}</span>
                                    </div>
                                    <span class="text-lg text-gray-600">{{ userInfos.email }}</span>
                                </div>
                            </div>
                        </div>

                        <div class="flex flex-col space-y-4 p-3 overflow-y-auto">
                            <div v-for="message in messagesChat" :key="message.id">
                                <div v-if="message.user_id == user.id" class="flex items-end justify-end">
                                    <div class="flex flex-col space-y-2 text-xs max-w-xs mx-2 order-1 items-end">
                                        <div>
                                            <span
                                                class="px-4 py-2 rounded-lg inline-block rounded-br-none bg-blue-600 text-white">
                                                {{ message.content }}
                                            </span>
                                        </div>
                                    </div>
                                    <img src="https://images.unsplash.com/photo-1590031905470-a1a1feacbb0b?ixlib=rb-1.2.1&amp;ixid=eyJhcHBfaWQiOjEyMDd9&amp;auto=format&amp;fit=facearea&amp;facepad=3&amp;w=144&amp;h=144"
                                        alt="My profile" class="w-6 h-6 rounded-full order-2">
                                </div>

                                <div v-else class="flex items-end">
                                    <div class="flex flex-col space-y-2 text-xs max-w-xs mx-2 order-2 items-start">
                                        <div>
                                            <span
                                                class="px-4 py-2 rounded-lg inline-block rounded-bl-none bg-gray-300 text-gray-600">
                                                {{ message.content }}
                                            </span>
                                        </div>
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
                                        <MicrophoneIcon />
                                    </button>
                                </span>

                                <form @submit.prevent="sendMessage" class="w-full">
                                    <input v-model="content" type="text" placeholder="Write your message!"
                                        class="w-full focus:outline-none focus:placeholder-gray-400 text-gray-600 placeholder-gray-600 pl-12 bg-gray-200 rounded-full py-3">

                                    <div class="absolute right-0 items-center inset-y-0 hidden sm:flex">
                                        <button type="submit"
                                            class="inline-flex items-center justify-center rounded-r-full px-4 py-3 transition duration-500 ease-in-out text-white bg-blue-500 hover:bg-blue-400 focus:outline-none">
                                            <span class="font-bold">Send</span>
                                            <SendIcon />
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
