<template>
	<div class="container">
		<v-paginate
			:totalPages="pagination.last_page"
			:total="pagination.total"
			:perPage="pagination.per_page"
			:currentPage="pagination.current_page"
			@pagechanged="fetchArticle"
		>
		</v-paginate>
		<ul v-for="(item, i) in Article" :key="i" :id="'var article_'+item.article_id">
			<li>{{ i+1 }}</li>
			<li>{{ item.article_id }}</li>
			<li>{{ item.slug }}</li>
			<li>{{ item.content }}</li>
			<li>{{ item.created_at }}</li>
			<li>{{ item.updated_at }}</li>
			<li><button @click="deleteArticle(item.article_id)" type="button" class="btn btn-md btn-warning">Delete</button></li>
		</ul>
	</div>
</template>

<script>
import Paginate from "./Paginate";
export default {
	components:{
		'v-paginate':Paginate
	},
	data() {
		return {
			Article: {},
			url: '/api/article',
			pagination: {},
			current_page: 1,
		}
	},

	created() {
		this.fetchArticle(this.current_page);
	},
	methods: {

		fetchArticle(page) {
			//get index
			this.current_page = page;
			axios.get(this.url+'?page=' + page)
				.then(
					(res) => {
						this.Article = res.data.data,
						this.pagination = res.data
					}
				)
				.catch(err => {
					console.log(err)
				});
		},
		deleteArticle(id) {
			//delete index
			if (confirm('Xóa')) {
				axios.delete(this.url + "/" + id).then(
					document.getElementById('article_' + id).remove()
				)
			}
		},
	},


}
</script>
