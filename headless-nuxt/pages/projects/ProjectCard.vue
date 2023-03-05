<template>
    <article :title="`Aller vers ${project.name}`"
    class="shadow-lg p-4 rounded cursor-pointer border"
    @click="navigateTo(`/projects/${project.slug}`)">
<img :src="project.image.url" class="object-cover w-[300px] h-[200px]" alt="">
<h2 class="mt-2">{{ project.name }}</h2>
<p>{{ project.type }}</p>
</article>
</template>

<script setup>
    defineProps(['project'])
</script>
