<template>
	<div class="container-fluid">
		<div class="row">
			<div class="pr-0 w-100">
				<div class="card card-chan mb-2">
					<div class="card-header bg-chan p-0 border-0">
						<div class="row">
							<div class="col pl-4 py-1">
								<p class="mb-0 font-chan-red font-weight-bold">Create A New Board</p>
							</div>
							<div class="col-auto pl-auto pr-4 pt-1 small">
								<i class="fal fa-gas-pump"></i> Gas Prices &mdash; Fast: {{gasPrice.fast/10}} | Average: {{gasPrice.average/10}} | Safe Low: {{gasPrice.safeLow/10}}
								<button class="btn btn-outline-chan-red py-0 px-1" @click="getGas()"><i class="fal fa-redo"></i></button>
							</div>
						</div>
					</div>
					<div class="card-header bg-chan-light">
						<p class="lead mb-2 text-center font-chan-normal">Board Details</p>
						<div class="input-group mb-2">
							<div class="input-group-prepend">
								<span class="input-group-text bg-chan text-chan border-chan-red">Ticker</span>
							</div>
							<input type="text" class="form-control border-chan-red border-right-0" placeholder="i.e. a, b, wsg" v-model="boardTicker" maxlength="4">
							<div class="input-group-append">
								<span class="input-group-text border-chan-red" style="background-color:white!important;">{{boardTicker.length}}/4</span>
							</div>
						</div>
						<div class="input-group mb-3">
							<div class="input-group-prepend">
								<span class="input-group-text bg-chan text-chan border-chan-red">Tagline</span>
							</div>
							<input type="text" class="form-control border-chan-red border-right-0" placeholder="i.e. anime &amp; manga, random, work safe gif" v-model="boardName" maxlength="32">
							<div class="input-group-append">
								<span class="input-group-text border-chan-red" style="background-color:white!important;">{{boardName.length}}/32</span>
							</div>
						</div>
						<alert>
							<template #content>
								Before you submit please make sure you entered the details above correctly as a change of these inputs after you submit the transaction is not possible. The transaction will be rejected if the board ticker already exists.
							</template>
						</alert>
						<div class="row">
							<div class="col-12 col-lg-6 mb-2 mb-lg-0" v-if="getPayment == 'none' || getPayment == 'zch'">
								<button class="btn btn-outline-chan btn-block" @click="createBoard()"><i class="fab fa-creative-commons-zero"></i> Create with 100 ZCH</button>
							</div>
							<div class="col-12 col-lg-6" v-if="getPayment == 'none' || getPayment == 'eth'">
								<button class="btn btn-outline-chan btn-block"><i class="fab fa-ethereum"></i> Create with 0.01 ETH</button>
							</div>
							
						</div>
					</div>
				</div>
			</div>
		</div>
	</div> 
</template>

<script>
	import Axios from 'axios'
	import Alert from '../components/helpers/Alert.vue'
	import {mapGetters, mapActions} from 'vuex'

	export default{
		data(){
			return{
				gasPrice: null,
				boardTicker: '',
				boardName: ''
			}
		},
		components: {
			Alert
		},
		methods:{
			...mapActions([
				'setBoard'
			]),
			createBoard(){
				this.setBoard({"board": this.boardTicker, "punchline": this.boardName})
				this.$router.push({name: 'ticker', params: {ticker: this.boardTicker}})
			},
			getGas(){
				Axios.get("https://ethgasstation.info/json/ethgasAPI.json")
					.then(response => {this.gasPrice = response.data})
			}
		},
		computed:{
			...mapGetters([
				'getPayment'
			])
		},
		mounted(){
			this.getGas()
		}
	}
</script>