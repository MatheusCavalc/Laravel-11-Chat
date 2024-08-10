<script setup>
import AppLayout from '@/Layouts/AppLayout.vue'
import Dropdown from '@/Components/Dropdown.vue';
import DropdownLink from '@/Components/DropdownLink.vue';
import MicrophoneIcon from '@/Components/Icons/MicrophoneIcon.vue'
import SendIcon from '@/Components/Icons/SendIcon.vue'
import LeafIcon from '@/Components/Icons/LeafIcon.vue'
import BackIcon from '@/Components/Icons/BackIcon.vue'
import EllipsisIcon from '@/Components/Icons/EllipsisIcon.vue'
import { Link, router, usePage } from '@inertiajs/vue3';
import { computed, ref, onMounted } from 'vue';
import axios from 'axios';

const props = defineProps(['chats'])

const messagesChat = ref([])
const idChat = ref('')
const userInfos = ref('')
const content = ref('')

const showSearchBar = ref(false)
const page = usePage()
const user = computed(() => page.props.auth.user)

const messageContainer = ref(null);

const scrollToBottom = () => {
    if (messageContainer.value) {
        messageContainer.value.scrollTop = messageContainer.value.scrollHeight;
    }
}

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

    scrollToBottom();
}

function sendMessage() {
    router.post('/new-message', {
        conversation_id: idChat.value,
        content: content.value
    })

    content.value = ''
}

onMounted(() => {
    scrollToBottom();

    window.Echo.channel('messages')
        .listen('MessageSent', async (e) => {
            console.log(e);
            if (e.message.conversation_id == idChat.value) { // && page.props.auth_id != e.message.user_id
                await messagesChat.value.push(e.message);
                scrollToBottom();
            }
        });
})
</script>

<template>
    <AppLayout>
        <section class="lg:grid lg:grid-cols-4 lg:gap-2 bg-white">
            <div class="antialiased bg-gray-50 text-gray-600 lg:h-screen overflow-y-auto lg:p-4">
                <div class="relative w-full lg:max-w-[340px] mx-auto bg-white shadow-lg h-full rounded-lg">
                    <div class="mb-2 py-3 px-4">
                        <h3 class="text-lg lg:text-xl font-semibold text-gray-400">Messages</h3>
                    </div>

                    <div class="py-3 px-5">
                        <h3 class="text-xs font-semibold uppercase text-gray-400 mb-1">Chats</h3>
                        <div class="divide-y divide-gray-200 lg:h-[530px] lg:overflow-y-auto">
                            <div v-for="chat in chats" :key="chat.id">
                                <button v-if="chat.user_1.id == user.id"
                                    class="w-full text-left py-2 focus:outline-none focus-visible:bg-indigo-50">
                                    <div @click="messages(chat.id, chat.user_2)" class="flex items-center">
                                        <img class="rounded-full items-start flex-shrink-0 mr-3"
                                            src="https://res.cloudinary.com/dc6deairt/image/upload/v1638102932/user-32-01_pfck4u.jpg"
                                            width="32" height="32" alt="Marie Zulfikar" />
                                        <div>
                                            <h4 class="text-sm font-semibold text-gray-900">{{ chat.user_2.name
                                                }}
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
                                            <h4 class="text-sm font-semibold text-gray-900">{{ chat.user_1.name
                                                }}
                                            </h4>
                                            <div class="text-[13px]">The video chat ended · 2hrs</div>
                                        </div>
                                    </div>
                                </button>
                            </div>
                        </div>
                    </div>

                    <!-- Bottom right button -->
                    <Link href="/chat">
                    <button
                        class="absolute bottom-5 right-5 inline-flex items-center text-sm font-medium text-white bg-blue-500 hover:bg-blue-600 rounded-full text-center px-3 py-2 shadow-lg focus:outline-none focus-visible:ring-2">
                        <LeafIcon />
                        <span>New Chat</span>
                    </button>
                    </Link>
                </div>
            </div>

            <Transition>
                <div v-show="idChat" :class="idChat ? 'lg:block' : 'hidden lg:block'"
                    class="fixed lg:relative z-30 lg:col-span-3 left-0 bottom-0 lg:bottom-0 lg:right-4 w-full h-screen bg-white">
                    <div class="flex-1 p:2 sm:p-6 justify-between flex flex-col h-screen">
                        <div class="flex gap-2 items-center py-3 border-b border-gray-200 px-2 lg:px-0">
                            <div class="lg:hidden flex items-center">
                                <button @click="idChat = ''">
                                    <BackIcon />
                                </button>
                            </div>
                            <div class="relative flex items-center gap-4">
                                <div class="relative">
                                    <span class="absolute text-green-500 right-0 bottom-0">
                                        <svg width="20" height="20">
                                            <circle cx="8" cy="8" r="8" fill="currentColor"></circle>
                                        </svg>
                                    </span>
                                    <img src="https://images.unsplash.com/photo-1549078642-b2ba4bda0cdb?ixlib=rb-1.2.1&amp;ixid=eyJhcHBfaWQiOjEyMDd9&amp;auto=format&amp;fit=facearea&amp;facepad=3&amp;w=144&amp;h=144"
                                        alt="" class="w-10 sm:w-16 h-10 sm:h-16 rounded-full">
                                </div>
                                <div class="leading-tight">
                                    <div class="flex items-center">
                                        <p class="text-lg lg:text-2xl text-gray-700">{{ userInfos.name }}</p>
                                    </div>
                                    <p class="lg:text-lg text-gray-600">{{ userInfos.email }}</p>
                                </div>
                            </div>
                        </div>

                        <div class="flex flex-col space-y-4 p-3 overflow-y-auto" ref="messageContainer">
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

                                <div v-else class="flex gap-1">
                                    <img class="w-8 h-8 rounded-full"
                                        src="https://images.unsplash.com/photo-1590031905470-a1a1feacbb0b?ixlib=rb-1.2.1&amp;ixid=eyJhcHBfaWQiOjEyMDd9&amp;auto=format&amp;fit=facearea&amp;facepad=3&amp;w=144&amp;h=144"
                                        alt="Jese image">
                                    <div
                                        class="flex flex-col w-full max-w-[320px] leading-1.5 p-4 border-gray-200 bg-gray-100 rounded-e-xl rounded-es-xl">
                                        <div class="flex items-center space-x-2 rtl:space-x-reverse">
                                            <span class="text-sm font-semibold text-gray-900">
                                                {{ message.user.name }}
                                            </span>
                                            <span class="text-sm font-normal text-gray-500">{{ message.id
                                                }}</span> <!-- message.created_at -->
                                        </div>
                                        <p class="text-sm font-normal py-2.5 text-gray-900">
                                            {{ message.content }}
                                        </p>
                                        <span class="text-sm font-normal text-gray-500">Delivered</span>
                                    </div>

                                    <Dropdown align="left" width="48" position="center-lg:bottom">
                                        <template #trigger>
                                            <button
                                                class="inline-flex self-center items-center text-sm font-medium text-center text-gray-900"
                                                type="button">
                                                <EllipsisIcon />
                                            </button>
                                        </template>

                                        <template #content>
                                            <DropdownLink :href="route('profile.edit')"> Profile </DropdownLink>
                                            <DropdownLink :href="route('logout')" method="post" as="button">
                                                Log Out
                                            </DropdownLink>
                                        </template>
                                    </Dropdown>

                                </div>
                            </div>
                        </div>

                        <div class="border-t border-gray-200 px-4 pt-4 mb-2 sm:mb-0">
                            <div class="relative flex">
                                <span class="absolute inset-y-0 flex items-center">
                                    <button type="button"
                                        class="inline-flex items-center justify-center rounded-full h-12 w-12 transition duration-500 ease-in-out text-gray-500 hover:bg-gray-300 focus:outline-none">
                                        <MicrophoneIcon />
                                    </button>
                                </span>

                                <form @submit.prevent="sendMessage" class="w-full flex">
                                    <input v-model="content" type="text" placeholder="Write your message"
                                        class="w-full focus:outline-none focus:placeholder-gray-400 border border-r-0 border-black text-gray-600 placeholder-gray-600 pl-12 bg-gray-200 rounded-full rounded-r-none py-3">

                                    <button type="submit"
                                        class="flex gap-2 items-center justify-center border-black border-l-0 border rounded-r-full px-4 py-3 transition duration-500 ease-in-out text-white bg-blue-500 hover:bg-blue-400">
                                        <span class="font-bold hidden lg:flex">Send</span>
                                        <SendIcon />
                                    </button>
                                </form>
                            </div>
                        </div>
                    </div>
                </div>
            </Transition>
        </section>
    </AppLayout>
</template>
