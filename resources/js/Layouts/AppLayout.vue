<script setup>
import UserIcon from '@/Components/Icons/UserIcon.vue'
import BubbleIcon from '@/Components/Icons/BubbleIcon.vue'
import GroupIcon from '@/Components/Icons/GroupIcon.vue'
import CogIcon from '@/Components/Icons/CogIcon.vue'

import Dropdown from '@/Components/Dropdown.vue';
import DropdownLink from '@/Components/DropdownLink.vue';
import { Link, usePage } from '@inertiajs/vue3';

const page = usePage()
</script>

<template>
    <aside
        class="fixed left-0 bottom-0 h-fit lg:h-screen border border-t lg:border-r w-full lg:w-fit px-5 py-3 lg:px-3 lg:py-8 z-20 bg-white">
        <div class="flex flex-row lg:flex-col justify-between lg:justify-start lg:gap-5 text-black">
            <Link href="/chat" class="relative">
            <button :class="{ 'text-white bg-blue-500': page.url == '/chat', 'text-black bg-white': page.url != '/chat' }"
                class="p-2 rounded-full lg:rounded-xl">
                <BubbleIcon />
            </button>
            </Link>

            <Link href="/contacts">
            <button
                :class="{ 'text-white bg-blue-500': page.url == '/contacts', 'text-black bg-white': page.url != '/contacts' }"
                class="p-2 rounded-full lg:rounded-xl">
                <UserIcon />
            </button>
            </Link>

            <Link href="/group-chats">
            <button
                :class="{ 'text-white bg-blue-500': page.url == '/group-chats', 'text-black bg-white': page.url != '/group-chats' }"
                class="p-2 rounded-full lg:rounded-xl">
                <GroupIcon />
            </button>
            </Link>

            <Dropdown align="left" width="48" position="top-lg:bottom">
                <template #trigger>
                    <button class="p-2">
                        <CogIcon class="cursor-pointer" />
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
    </aside>

    <div class="lg:ml-16">
        <slot />
    </div>
</template>
