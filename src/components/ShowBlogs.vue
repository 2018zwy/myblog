<template>
	<div v-theme:column="'narrow'" id="show-blogs">
		<h1>博客总览</h1>
		<input type="text" v-model='search' placeholder="搜索" />
		<div v-for="blog in filteredBlogs" class="single-blog">
			<router-link v-bind:to="'/blog/' + blog.id">
				<h2 v-rainbow>{{blog.title | to-uppercase}}</h2>
			</router-link>
			<article> {{blog.content | snippet}}</article>
			
		</div>
	</div>
</template>

<script>
	export default {
		name:'show-blogs',
		data(){
			return {
				blogs:[],
				search:""
			}
		},
		created(){
			//请求本地数据
			this.$http.get('https://wd0574951885ycuwsw.wilddogio.com/posts.json')
			.then(function(data){
				console.log(data.json());
				return data.json()
//				this.blogs = data.body.slice(1,10);
//				console.log(this.blogs );
				})
			.then(function(data){
				var blogsArray =[];
				for(let key in data){
//					console.log(key);
//					console.log(data[key]);
					data[key].id = key;
					blogsArray.push(data[key])
				}
//				console.log(blogsArray);
				this.blogs = blogsArray;
			})
		},
		computed:{
			//搜索
			filteredBlogs:function(){
				return this.blogs.filter((blog) =>{
					return blog.title.match(this.search);
				})
			}
		},
		filters:{
//			"to-uppercase":function(value){
//				return value.toUpperCase();
//			},
//			"snippet":function(value){
//				return value.slice(0,100) + "..."
//			}
			toUppercase(value){
				return value.toUpperCase();
			},
			snippet(value){
				return value.slice(0,100) + "...";
			}
	},
	directives:{
		'rainbow':{
			bind(el,binding,vnode){
				el.style.color = "#" + Math.random().toString(16).slice(2,8);
			}
		}
	}
	}
</script>

<style>
	#show-blogs{
		max-width: 800px;
		margin: 0 auto;
	}
	.single-blog{
		padding: 20px;
		margin: 20px 0;
		box-sizing: border-box;
		background-color: #eee;
		border:1px dotted #aaa;
	}

</style
>