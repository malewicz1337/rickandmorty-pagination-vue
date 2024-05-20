<template>
    <article class="container">
        <section class="filters">
            <input v-model="filters.name" placeholder="Name" />
            <select v-model="filters.status">
                <option value="">All</option>
                <option value="Alive">Alive</option>
                <option value="Dead">Dead</option>
                <option value="unknown">Unknown</option>
            </select>
            <button @click="applyFilters">Применить</button>
        </section>

        <section class="characters">
            <CharacterCard v-for="character in characters" :key="character.id" :character="character" />
        </section>

        <div class="pagination">
            <button @click="prevPage" :disabled="currentPage === 1">Previous</button>
            <span>Page {{ currentPage }}</span>
            <button @click="nextPage" :disabled="currentPage === totalPages">Next</button>
        </div>
    </article>
</template>

<script setup lang="ts">
import { ref, onMounted, watch } from 'vue';
import axios from 'axios';
import CharacterCard from './CharacterCard.vue';

interface Character {
    id: number;
    name: string;
    status: string;
    image: string;
}

const characters = ref<Character[]>([]);
const currentPage = ref(1);
const totalPages = ref(1);
const filters = ref({ name: '', status: '' });

const fetchCharacters = async () => {
    try {
        const response = await axios.get(`https://rickandmortyapi.com/api/character`, {
            params: {
                page: currentPage.value,
                name: filters.value.name,
                status: filters.value.status,
            },
        });
        characters.value = response.data.results;
        totalPages.value = response.data.info.pages;
    } catch (error) {
        console.error(error);
    }
};

const applyFilters = () => {
    currentPage.value = 1;
    fetchCharacters();
};

const nextPage = () => {
    if (currentPage.value < totalPages.value) {
        currentPage.value += 1;
        fetchCharacters();
        scrollToTop();
    }
};

const prevPage = () => {
    if (currentPage.value > 1) {
        currentPage.value -= 1;
        fetchCharacters();
        scrollToTop();
    }
};

const scrollToTop = () => {
    window.scrollTo({
        top: 0,
        behavior: 'smooth'
    });
};

watch([filters, currentPage], fetchCharacters);

onMounted(fetchCharacters);
</script>

<style scoped>
.container {
    display: flex;
    flex-direction: column;
    align-items: center;
}

.filters {
    margin: 16px;
    display: flex;
    gap: 0.5rem;
    align-items: center;

}

.characters {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 16px;
}

.pagination {
    margin: 16px;
    display: flex;
    flex-direction: row;
    gap: 0.5rem;
    align-items: center;
}
@media (max-width: 1080px) {
    .characters {
        grid-template-columns: repeat(1, 1fr);
    }
}
</style>
