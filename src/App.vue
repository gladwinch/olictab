<template>
    <div id="app" class="flex flex-row h-screen">
		<div class="w-14 hide-sm">
			<side-bar></side-bar>
		</div>
		<div class="flex-auto overflow-y-scroll">
			<Datatable 
				title="Customers"
				endpoint="https://jsonplaceholder.typicode.com/users"
				points="id,name,email,company,address,website"
			/>
		</div>
    </div>
</template>

<script>
import Datatable from './components/DataTable.vue'
import SideBar from './components/SideBar.vue'

export default {
	name: 'App',
	components: {
		Datatable,
		SideBar
	}
}
</script>

<style src="./assets/css/tailwind.css">
