<template>
	<div class="col-12 pl-sm-0 pr-0 pr-sm-4">
		<div class="row bg-chan">
			<div class="col text-center">
				<h4 class="font-chan-red font-roboto">Newest Posts</h4>
			</div>
		</div>
		<div class="row pb-2" v-for="(post, index) in sortedItems()" :key="index+1" :class="{'bg-chan-light': index % 2 !== 0}">
			<router-link tag="div" class="col-8" :to="{path: '/board/'+post.board+'/thread/'+post.thread}"><a>{{post.content}}</a></router-link>
			
			<router-link tag="div" class="col text-center" :to="{path: '/board/'+post.board}"><a>/{{post.board}}/</a></router-link>
			<div class="col text-right">{{post.replies}} {{post.replies==1?'reply':'replies'}}</div>
		</div>
		<div class="row">
		</div>
	</div>
</template>

<script>
	import {mapGetters} from 'vuex'

	export default {
		data(){
			return{
				date: []
			}
		},
		methods: {
			sortedItems() {
				return this.date.slice().sort((a, b) => (a.datum > b.datum) ? 1 : ((b.datum > a.datum) ? -1 : 0)).slice(-10)
			}
		},
		computed: {
			...mapGetters([
				'getDemo'
			]),
			punchline() {
				return this.getDemo.boards.map((item) => {
					return item.punchline
				})
			}
			// timestamp() {
			// 	return Threads.map((item) => {
			// 		return item.timestamp
			// 	})
			// },
			// boardTicker() {
			// 	return Threads.map((item) => {
			// 		return item.board
			// 	})
			// },
			// replies() {
			// 	return Threads.map((item) => {
			// 		return item.replies.length
			// 	})
			// },
			// opContent() {
			// 	return Threads.map((item) => {
			// 		if(item.subject == ""){
			// 			return item.text.slice(0,100)
			// 		}
			// 		else {
			// 			return item.subject
			// 		}
			// 	})
			// },
			// threadID() {
			// 	return Threads.map((item) => {
			// 		return item.thread
			// 	})
			// },
		},
		mounted(){
			// retarded parsing neccessary because of my JSON file for demo purposes
			// on-chain we can just parse the last 10 or so threads that are indexed
			for (let i = 0; i < this.timestamp.length; i++) {
				let year = '20'+this.timestamp[i].slice(6,8)
				let month = this.timestamp[i].slice(0,2)
				let day = this.timestamp[i].slice(3,5)
				let comDate = year+'-'+month+'-'+day+'T'+this.timestamp[i].slice(13)
				let board = this.boardTicker[i]
				let reply = this.replies[i]
				let content = this.opContent[i]
				let id = this.threadID[i]
				this.date.push({thread: id, datum: comDate, board: board, replies: reply, content: content})
			}	
		}
	}
</script>