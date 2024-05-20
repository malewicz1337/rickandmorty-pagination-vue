<script setup lang="ts">
defineProps<{
    character: {
        name: string;
        status: string;
        image: string;
    };
}>();
</script>

<template>
    <section class="character-card">
        <img class="character-image" :src="character.image" :alt="character.name" />
        <div class="character-info">
            <h2>{{ character.name }}</h2>
            <p>Status: {{ character.status }}</p>
        </div>
    </section>
</template>

<style scoped>
.character-card {
    width: 500px;
    display: flex;
    align-items: center;
    border: 1px solid #ccc;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    overflow: hidden;
    margin: 16px;
    background-color: grey;
    transition: transform 0.3s, box-shadow 0.3s;
}

.character-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
}

.character-image {
    width: 200px;
    height: 200px;
    border-radius: 8px 0 0 8px;
    object-fit: cover;
    margin-right: 16px;
}

.character-info {
    height: 100%;
    display: flex;
    flex-direction: column;
    align-items: start;
    padding: 16px;
}

.character-info h2 {
    margin: 0 0 8px;
    font-size: 1.5em;
    color: whitesmoke;
}

.character-info p {
    margin: 0;
    font-size: 1em;
    color: whitesmoke;
}
</style>
