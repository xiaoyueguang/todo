<template>
	<h1>待办事项</h1>
	<div id="todo">
		<input v-model="todo" type="text" v-on:keyup.enter="add" placeholder="请输入待办事项"/>
		<div class="bar">
			<span>共有 {{items.length}} 条代办事项</span>
			<br/><br/>
			<span @click='all' :class="{active:filter.all}">全部事项</span>
			<span @click='undone' :class="{active:filter.undone}">未完成</span>
			<span @click='done' :class="{active:filter.done}">已完成事项</span>
		</div>
		<ul>
			<template v-for="item in items | orderBy 'todoid' | filterBy filter.status ">
				<li transition="fade" :class="{'checked':item.status}">
					<input type="hidden" v-model='item.todoid'>
					<input type="checkbox" v-on:click="status($index)" v-model='item.status'>
					<span>{{item.content}}</span>
					<!-- 获取INDEX -->
					<a v-on:click="remove($index)">+</a>
				</li>
			</template>
		</ul>
	</div>
	<span v-on:click="getall">获取所有数据</span>
</template>

<style src="./todo.css">
</style>

<script>
	export default {
		data: ()=> {
			return {
				test: false,
				filter: {
					all: true,
					done: false,
					undone: false,
					status: '',
				},
				todoid: 1,
				items: [],
				todo: ''
			}
		},
		methods: {
			remove: function(index) {
				this.items.splice(index, 1);
			},
			add: function() {
				if (this.todo.trim()) {
					this.items.push({
						content: this.todo,
						todoid:this.todoid,
						status:false
					});
					this.todoid ++ ;
					localStorage.todoId = JSON.stringify(this.todoid);
				}
				this.todo = "";
			},
			status: function(index){
				this.items[index].status = !this.items[index].status;
				console.log(this);;
			},
			all:function(){
				this.filter = {
					all:true,
					done:false,
					undone:false,
					status:'',
				}
			},
			undone:function(){
				this.filter = {
					all:false,
					done:false,
					undone:true,
					status:false,
				}
			},
			done:function(){
				this.filter = {
					all:false,
					done:true,
					undone:false,
					status:true,
				}
			},
			getall: function(){

			}
		},
		watch: {
			items: function(value){
				console.log(value[0].status)
				localStorage.todo = JSON.stringify(value);
			}
		},
		ready: function(){
			if(localStorage.todoId){
				this.items = JSON.parse(localStorage.todo);
				this.todoid = JSON.parse(localStorage.todoId);
			}
		},
	}
</script>
