<template>
	<nav aria-label="Page navigation"  class="col-md-8 offset-2">
		<ul class="pagination justify-content-center">
			<li class="page-item" >
				<a class="page-link"
					@click="onClickFirstPage"
					:disabled="isInFirstPage"
					aria-label="Go to first page"
				>
					Fisrt Page
				</a>
			</li>

			<li class="page-item" >
				<a class="page-link"
					@click="onClickPreviousPage"
					:disabled="isInFirstPage"
				>
					<span aria-hidden="true">&laquo;</span>
					<span class="sr-only">Next</span>
				</a>
			</li>

			<li v-for="page in pages" :key="page.name" class="page-item"
				:class="{ active: isPageActive(page.name) }"
				:disabled="page.isDisabled"
			>
				<a class="page-link"
					@click="onClickPage(page.name)"
				>
					{{ page.name }}
				</a>
			</li>

			<li class="page-item">
				<a class="page-link"
					@click="onClickNextPage"
					:disabled="isInLastPage"
					aria-label="Go to next page"
				>
					<span aria-hidden="true">&raquo;</span>
					<span class="sr-only">Next</span>
				</a>
			</li>

			<li class="page-item">
				<a class="page-link"
					@click="onClickLastPage"
					:disabled="isInLastPage"
					aria-label="Go to last page"
				>
					Last
				</a>
			</li>
		</ul>
	</nav>
</template>

<script>
export default {
	props: [
		'totalPages',
		'total',
		'perPage',
		'currentPage',
	],
	data(){
		return{
			maxVisibleButtons:5
		}
	},

  computed: {
    startPage() {
      if (this.currentPage === 1) {
        return 1;
      }

      if (this.currentPage === this.totalPages) {
        return this.totalPages - this.maxVisibleButtons + 1;
      }

			if (this.currentPage === 82) {
				return this.currentPage - 3
			}

			if (this.currentPage > 2) {
				return this.currentPage - 2
			}

      return this.currentPage - 1;

    },
    endPage() {
      return Math.min(this.startPage + this.maxVisibleButtons - 1, this.totalPages);

    },
    pages() {
      const range = [];

      for (let i = this.startPage; i <= this.endPage; i+= 1 ) {
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
		onClickFirstPage() {
			this.$emit('pagechanged', 1);
		},
		onClickPreviousPage() {
			this.$emit('pagechanged', this.currentPage - 1);
		},
		onClickPage(page) {
			this.$emit('pagechanged', page);
		},
		onClickNextPage() {
			this.$emit('pagechanged', this.currentPage + 1);
		},
		onClickLastPage() {
			this.$emit('pagechanged', this.totalPages);
		},
		isPageActive(page) {
			return this.currentPage === page;
		},
	}
};
</script>
