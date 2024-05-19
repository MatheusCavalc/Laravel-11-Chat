<script setup>
import AppLayout from '@/Layouts/AppLayout.vue'
</script>

<template>
    <AppLayout>
        <p>Oi</p>
    </AppLayout>

    <!--
    <aside
        class="fixed left-0 bottom-0 h-fit lg:h-screen border border-t lg:border-r w-full lg:w-fit px-5 py-1 lg:px-3 lg:py-8 z-20 bg-black">
        <div class="flex flex-row lg:flex-col justify-between lg:justify-start lg:gap-5 text-black">
            <Link href="/chat" class="relative">
            <button
                :class="{ 'absolute lg:relative bottom-7 lg:bottom-0 border-white text-white bg-black lg:bg-white lg:text-black': page.url == '/chat', 'text-white bg-black border-blue-500': page.url != '/chat' }"
                class="p-2 rounded-full lg:rounded-xl border-8 lg:border-none">
                <BubbleIcon />
            </button>
            </Link>

            <Link href="/contacts">
            <button
                :class="{ 'text-white bg-black lg:bg-white lg:text-black': page.url == '/contacts', 'text-white bg-black': page.url != '/contacts' }"
                class="p-2 rounded-full lg:rounded-xl">
                <UserIcon />
            </button>
            </Link>

            <Dropdown align="left" width="48">
                <template #trigger>
                    <button class="p-2 text-white">
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
-->
</template>
