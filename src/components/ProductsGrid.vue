<template>
    <div class="flex w-100 pb-20 px-80">
        <Input v-model="searchTerm" placeholder="Pesquisar" />
    </div>
    <div v-if="data.length > 0" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
        <Card v-for="item in filteredProducts" :key="item.id" class="transition-all">
            <CardHeader class="px-0 pt-0">
                <img :src="item.src" class="w-100 rounded-t-lg border-b">
            </CardHeader>
            <CardContent class="flex flex-col gap-4">
                <p class="font-bold">{{ item.name }}</p>
                <div class="flex flex-wrap gap-1 items-center">
                    <span class="text-xs px-1 bg-muted rounded text-muted-foreground" variant="outline"
                        v-for="category in item.categories" :key="category.id">{{ category.name }}</span>
                </div>


                <div class="flex gap-2 flex-wrap">
                    <div v-for="variant in item.variants" :key="variant.id">
                        <Badge variant="outline" v-if="variant.stock > 0">{{ variant.value }}</Badge>
                        <Badge variant="secondary" class="text-muted-foreground" v-else>{{ variant.value }}</Badge>
                    </div>
                </div>

            </CardContent>
        </Card>
    </div>
    <div v-else class="w-100 text-center">
        <p>Nenhum produto encontrado</p>
    </div>
</template>

<script>
import axios from 'axios';
import { Badge } from '@/components/ui/badge'
import { Input } from '@/components/ui/input'
import {
    Card,
    CardContent,
    CardDescription,
    CardFooter,
    CardHeader,
    CardTitle,
} from '@/components/ui/card'

export default {
    components: {
        Card,
        CardContent,
        CardDescription,
        CardFooter,
        CardHeader,
        CardTitle,
        Badge,
        Input,
    },
    data() {
        return {
            data: [],
            searchTerm: '',
        };
    },
    mounted() {
        this.fetchData();
    },
    methods: {
        async fetchData() {
            try {
                const response = await axios.post('https://apibgdev.nswebservice.com.br/store/10/products', {
                    limit: 100,
                });
                this.data = response.data;
                console.log(this.data);
            } catch (error) {
                console.error('Erro ao buscar dados:', error);
            }
        },
    },
    computed: {
        filteredProducts() {
            return this.data.filter(product =>
                product.name.toLowerCase().includes(this.searchTerm.toLowerCase())
            );
        },
    },
};
</script>