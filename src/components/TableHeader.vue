<template>
    <div>
        <div class="flex items-center justify-between">
            <h1 class="text-3xl text-panel-400">{{ title }}</h1>
            <button 
                class="bg-primary-400 border-2 border-primary-600 rounded-md text-xs py-1 font-bold px-2 uppercase text-white cursor-pointer"
            >
                    <span class="text-sm font-bold">+</span>
                    create user
            </button>
        </div>
        <div class="divider my-4"></div>

        <!-- search bar -->
        <div 
            class="flex justify-between items-center rounded-md border-2 border-base-200 shadow-sm px-2 py-2 bg-white flex-1 max-w-xs"
        >
            <img 
                src="../assets/img/search.png" 
                alt="search-icon" 
                class="h-4 mr-2"
            >
            <input 
                class="w-full focus:outline-none text-sm" 
                placeholder="Search by Customer name" 
                :value="value"
                @input="(e) => onChange(e)"
            >
        </div>
    </div>
</template>

<script>
    export default {
        props: ['value','title'],
        methods: {
            onChange(e) {
                this.$emit('input', e.target.value)
            }
        }
    }
</script>