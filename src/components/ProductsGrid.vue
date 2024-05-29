<template>
    <div v-if="data.length > 0" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
        <Card v-for="item in data" :key="item.id">
            <CardHeader class="px-0 pt-0">
                <img :src="item.src" class="w-100 rounded-t-lg border-b">
            </CardHeader>
            <CardContent class="flex flex-col gap-4">
                <p class="font-bold">{{ item.name }}</p>
                <div class="flex gap-2">
                    <Badge variant="secondary" v-for="variant in item.variants" :key="variant.id">{{ variant.value }}</Badge>
                </div>
                
            </CardContent>
            <CardFooter>
                <div>
                    <Badge variant="outline" v-for="category in item.categories" :key="category.id">{{ category.name }}</Badge>
                </div>
            </CardFooter>
        </Card>
    </div>
    <div v-else>
        <p>Nenhum produto encontrado.</p>
    </div>
</template>

<script>
import axios from 'axios';
import { Button } from '@/components/ui/button'
import { Badge } from '@/components/ui/badge'
import {
    Card,
    CardContent,
    CardDescription,
    CardFooter,
    CardHeader,
    CardTitle,
} from '@/components/ui/card'
import { Plus } from 'lucide-vue-next';

export default {
    components: {
        Card,
        CardContent,
        CardDescription,
        CardFooter,
        CardHeader,
        CardTitle,
        Button,
        Badge,
        Plus,
    },
    data() {
        return {
            data: [], // Inicializa o estado com um array vazio
        };
    },
    mounted() {
        this.fetchData(); // Chama o método fetchData quando o componente é montado
    },
    methods: {
        async fetchData() {
            try {
                // Faz uma requisição POST usando Axios
                const response = await axios.post('https://apibgdev.nswebservice.com.br/store/10/products', {
                    limit: 100,
                });
                this.data = response.data; // Atualiza o estado com os dados da resposta
                console.log(this.data);
            } catch (error) {
                console.error('Erro ao buscar dados:', error);
            }
        },
    },
};
</script>

<style scoped>
/* Seu estilo aqui */
</style>