<template>
	<div
		v-show="isVisible"
		class="dynamic-context-menu"
		:style="style"
		tabindex="-1"
		@blur="close"
		@click="close"
		@contextmenu.capture.prevent
	>
		<slot :user-data="userData"></slot>
	</div>
</template>

<script>
const Vue = require('vue')

module.exports = {
	name: 'dynamic-context-menu',
	mounted() {
		window.addEventListener('scroll', this.close);
	},
	data () {
		return {
			x: null,
			y: null,	
			userData: null,
			isVisible: false,
		}
	},
	computed: {
		style () {
			return this.isVisible ? {
				top: this.y	+ 'px',
				left: this.x + 'px'
			} : {}
		},
	},
	methods: {
		open (evt, userData) {
			this.userData = userData;
			let x = evt.clientX, y = event.clientY;
			this.isVisible = true;
			this.$nextTick(() => {
				this.setMenu(x, y);
				this.$el.focus()
			})
		},
		close (evt) {
			setTimeout(() => {
				this.x = null
				this.y = null
				this.userData = null
				this.isVisible = false
			}, 200)
		},
		setMenu (x, y) {
			let largestHeight = window.innerHeight - this.$el.offsetHeight - 25;
			let largestWidth = window.innerWidth - this.$el.offsetWidth - 25;

			if (y > largestHeight) {
				y = largestHeight
			}

			if (x > largestWidth) {
				x = largestWidth
			}

			this.x = x
			this.y = y
		},
	}
};
</script>

<style scoped>
.dynamic-context-menu {
	position: fixed;
	z-index: 999;
}
.dynamic-context-menu:focus {
	outline: none;
}
</style>
