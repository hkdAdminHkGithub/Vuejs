<style>
.disabled {
	background: #559da3 !important;
	color: rgb(235, 235, 235) !important;
	font-weight: bold;
}
</style>

<template>
<nav aria-label="Page navigation" class="col">
	<ul class="pagination justify-content-center">
		<li class="page-item" :class="{'disabled': isInFirstPage}">
			<a class="page-link" @click="onNavPagintion($event, 1)" aria-label="Go to first page">
					Fisrt Page
				</a>
		</li>

		<li class="page-item" :class="{'disabled': isInFirstPage}">
			<a class="page-link" @click="onNavPagintion($event, currentPage - 1)">
				<span aria-hidden="true">
					<i class="fa fa-caret-square-o-left" aria-hidden="true"></i>
				</span>
			</a>
		</li>

		<li v-for="page in pages" :key="page.name" class="page-item" :class="{ 'disabled' : isPageActive(page.name) }">
			<a class="page-link" @click="onNavPagintion($event , page.name)" :class="{ 'disabled' : isPageActive(page.name) }">
					{{ page.name }}
				</a>
		</li>

		<li class="page-item" :class="{'disabled': isInLastPage}">
			<a class="page-link" @click="onNavPagintion($event, currentPage + 1)">
				<span aria-hidden="true"><i class="fa fa-caret-square-o-right" aria-hidden="true"></i></span>
			</a>
		</li>

		<li class="page-item" :class="{'disabled': isInLastPage}">
			<a class="page-link" @click="onNavPagintion($event, totalPages)" aria-label="Go to last page">
					Last
				</a>
		</li>
	</ul>
</nav>
</template>

<script>
export default {
	props: {
		totalPages: Number,
		total: Number,
		perPage: Number,
		currentPage: Number,
		maxVisibleButtons: {
			type: Number,
			default: 5
		}
	},

	computed: {
		startPage() {
			const ceilVisible = Math.ceil(this.maxVisibleButtons/2);
			const floorVisible = Math.floor(this.maxVisibleButtons/2);
			if (this.currentPage === 1) {
				return 1;
			} //first

			if (this.currentPage === this.totalPages) {
				return this.totalPages - this.maxVisibleButtons + 1;
			}	// last

			if (this.currentPage -floorVisible < 1) {
				return this.currentPage - 1
			}
			if (this.totalPages - this.currentPage < floorVisible )
				return this.currentPage - ceilVisible
			return this.currentPage -floorVisible // center
		},
		endPage() {
			const ceilVisible = Math.ceil(this.maxVisibleButtons/2);
			const floorVisible = Math.floor(this.maxVisibleButtons/2);

			if (this.startPage === 1)
				return this.maxVisibleButtons // first

			if (this.currentPage === this.totalPages)
				return this.totalPages // last

			if (this.currentPage + floorVisible > this.totalPages) {
				return this.currentPage + 1
			}
			return this.currentPage + floorVisible 	// center
		},
		pages() {
			const range = [];

			for (let i = this.startPage; i <= this.endPage; i += 1) {
				range.push({
					name: i,
					isDisabled: i === this.currentPage
				});
			}

			return range;
		},
		isInFirstPage() {
			return this.currentPage === 1;
		},
		isInLastPage() {
			return this.currentPage === this.totalPages;
		},
	},
	methods: {
		onNavPagintion(ev, page) {
			ev.preventDefault();
			this.$emit('pagechanged', page)
		},
		isPageActive(page) {
			return this.currentPage === page;
		},
	}
};
</script>
