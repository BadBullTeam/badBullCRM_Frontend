<template>
    <div @drop="onDrop($event, 1)" class="droppable">
        <Head>
            <Title>Вкладка: {{ route.params.tabs }}/{{ route.params.subtabs }} | {{ user.lastname }} {{ user.firstname }}</Title>
        </Head>
        <h1>{{ tab.title }}</h1>
        <div class="components-items" >
            <views-templateView class="component-item" v-for="component in tab.components" :key="component.id" :component="component"  :size="component.size"/>
            <div class="add-component" :class="isAddComponent ? 'close' : ''" @click="onAddComponent()">
                <svg width="24" height="24" viewBox="0 0 24 24"  xmlns="http://www.w3.org/2000/svg"><g clip-path="url(#clip0_0_2895)"><path d="M19 13H13V19H11V13H5V11H11V5H13V11H19V13Z" /></g><defs><clipPath id="clip0_0_2895"><rect width="24" height="24" fill="white"/></clipPath></defs></svg>
            </div>
        </div>
    </div>
</template>

<script>
import { useCrmStore } from '@/stores/crm'

export default{
    data(){
        return{
            tab: {},
            route: {},
            isAddComponent: false,
            user: {},
        }
    },
    methods:{
        onAddComponent(){
            this.isAddComponent = !this.isAddComponent
        }
    },
    
    beforeMount(){
        this.route = useRoute()
        
        
        if(!useCrmStore().user.id){
            this.$router.push(`/`);
        }
        else{
            if(this.$route.params.user_id != useCrmStore().user.id){
                this.$router.push(`/${useCrmStore().user.id}/${this.route.params.tabs}/${this.route.params.subtabs}`);
            }
            else{
                this.user = useCrmStore().user
            }
        }

        this.tab = useCrmStore().tabs.filter((tab)=> this.route.params.tabs == tab.name)[0].subtabs.filter(subtab=> this.route.params.subtabs == subtab.name)[0]
    },
}
</script>

<style>
    .add-component{
        width: 28%;
        height: 20rem;
        background-color: #f3f3f3;
        margin: 2rem;
        display: flex;
        justify-content: center;
        align-items: center;
        cursor: pointer;
    }

    .add-component svg{
        transform: scale(3);
        fill: black;
        transition: .3s;
    }

    .add-component.close svg{
        transform: scale(3) rotate(135deg);
    }
</style>