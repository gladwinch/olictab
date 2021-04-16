<template>
	<div>
		<!-- header -->
		<div class="bg-base-400 px-5 pt-5">
			<TableHeader 
				v-model="query.q"
				:title="title"
			/>
			<div class="divider my-4"></div>

			<!-- table tabs -->
			<div class="w-16 h-10 border-b-2 border-primary-400 flex items-center justify-between cursor-pointer">
				<img src="../assets/img/case.png" class="h-3" alt="case" >
				<span class="uppercase text-base-600 text-xs font-bold text-base-700 uppercase">All</span>
				<span class="font-bold text-xs text-primary-400">64</span>
			</div>
			<!-- table tabs end-->
		</div>
		<!-- info bar -->
		<div class="divider"></div>

		<div 
			class="h-14 bg-white flex items-center justify-between text-xs text-base-700 px-6">
			<div>Page {{ query._page }} of 10 results</div>
			<div class="flex items-center">

				<!-- pagination -->
				<div class="flex items-center">
					<img 
						src="../assets/img/left-arrow.png" 
						alt="go-left" class="h-3 mr-2 cursor-pointer" 
						@click="paginate('prev')">
					<div class="border-2 border-base-200 bg-white text-center py-1 px-2 rounded-md shadow-sm">{{ query._page }}</div>
					<!-- <span class="ml-2">Out of {{ Math.floor(10/query._limit) }}</span> -->
					<img src="../assets/img/right-arrow.png" 
						alt="go-left" class="h-3 ml-2 mr-6 cursor-pointer" @click="paginate('next')">
				</div>


				<!-- select result limit-->
				<span>Results per page: </span>
				<div 
					tabindex="0" 
					@blur="showlimit=false"
					class="ml-2 relative focus:outline-none z-50" 
				>
					<span 
						@click="showlimit=!showlimit"
						index="1"
						class="flex justify-between items-center rounded-md border-2 border-base-200 shadow-sm px-2 py-1 bg-white flex-1 w-12 text-base-700 cursor-pointer"
					>
						{{ query._limit }}
						<img src="../assets/img/drop.png" alt="drop" class="h-2" >
					</span>

					<div 
						class="border-2 border-base-200 bg-white text-center py-2 rounded-md w-12 absolute mt-1 shadow-sm"
						:class="[showlimit ? 'block' : 'hidden']"
					>
						<span 
							class="block px-2 py-1 cursor-pointer hover:bg-gray-200"
							v-for="num in 10" :key="num"
							@click="setpage(num)"
						>
							{{ num }}
						</span>
					</div>
				</div>
				<!-- dropdown end-->
			</div>
		</div>
		<!-- header end -->

		<table class="table font-medium text-xs text-base-700 mix-blend-400">
			<thead class="text-base-700 uppercase bg-base-400 text-xs h-20">
				<th class="font-medium" v-for="head in heads" :key="head">
					<div class="flex items-center cursor-pointer md:px-12">
						<span 
							class="mr-2" :class="head == query._sort ? 'text-base-800' : ''"
							@click="() => sortResult(head)">{{ head }}</span>
						<div>
							<div class="arr-up" 
								:class="query._order == 'asc' && query._sort == head ? 'active-sort-top' : ''">
							</div>
							<div class="arr-down"
								:class="query._order == 'desc' && query._sort == head ? 'active-sort-down' : ''">
							</div>
						</div>
					</div>
				</th>
			</thead>
			<tbody>
				<tr v-for="i in list" :key="i.id" class="hover:bg-primary-200">
					<td v-for="x in heads" :key="x" :data-label="x">
						<div v-if="loading"
							class="w-34 ml-11 bg-gray-200 h-6 rounded animate-pulse"></div>
						<span class="md:ml-12" v-else>{{ i[x] }}</span>
					</td>
				</tr>
			</tbody>
		</table>
	</div>
</template>

<script>
	import axios from 'axios'
	import qs from 'qs'

	import TableHeader from './TableHeader'

	export default {
		components: {
			TableHeader
		},
		props: ['title','endpoint','points'],
		data: () => ({
			list: [],
			query: {
				_limit: 3,
				_page: 1,
				_sort: 'none',
				_order: 'desc',
				q: ''
			},
			showlimit: false,
			cursort: '',
			loading: true
		}),
		computed: {
			heads() {
				return this.points.split(",")
			},
			uri() {
				return this.endpoint+'?'+ qs.stringify(this.query)
			}
		},
		methods: {
			fetchData() {
				this.loading = true
				axios.get(this.uri)
					.then(res => {
						this.list = res.data.map(x => {
							let obj = {}

							// flatten object
							for(let i in x) {
								if(typeof x[i] !== 'object') {
									obj[i] = x[i]
								} else {
									let keys = Object.keys(x[i])
									obj[i] = x[i][keys[0]]
								}
							}

							// return key value pair (no nested)
							return obj
						})
						this.loading = false
					})
					.catch(err => console.error(err))
			},
			setpage(num) {
				this.query._limit = num
				this.query._page = 1
				this.showlimit = false
			},
			sortResult(name) {
				this.query._page = 1
				this.query._sort = name
				this.cursort == name && this.query._order == 'asc' ?
					this.query._order = 'desc' :
					this.query._order = 'asc'

				this.cursort = name
			},
			paginate(direct) {
				if(direct == 'next' && this.list.length >= this.query._limit) 
				{ this.query._page++ }

				if(direct == 'prev' && this.query._page > 1) 
				{ this.query._page-- }
			}
		},
		watch: {
			query: {
				deep: true,
				handler() {
					this.fetchData()
				}
			}
		},
		mounted() {
			this.fetchData()
		}
	}
</script>

<style src="../assets/css/main.css">
