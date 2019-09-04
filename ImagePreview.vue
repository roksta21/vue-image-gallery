<template>
	<div class="my-image-preview" :id="id">
		<transition name="modal" v-if="showPreview">
			<div class="modal-mask">
				<div class="modal-wrapper" @click="close">
					<div class="modal-container">
						<a href="#" class="modal-close" @click.prevent="$emit('close')"><i class="fa fa-times"></i></a>
						<div class="modal-body">
							<a href="#" @click.prevent="zoom"><img :src="image" alt="" id="image-preview" class="image-preview"></a>
						</div>
            <div class="preview-footer">
              <a href="#" class="btn btn-link" @click.prevent="$emit('previous')">Previous</a>
              <a href="#" class="btn btn-link float-right" @click.prevent="$emit('next')">Next</a>
            </div>
					</div>
				</div>
			</div>
		</transition>
	</div>
</template>

<script>
	export default {
		props: {
			showPreview: {
				type: Boolean,
				default() {
					return false
				}
			},
			id: {
				type: String,
				default: ''
      },
      image: {
        type: String,
        required: true
      }
		},

		methods: {
			close(e) {
				if (e.target.className.indexOf('btn') < 0 && e.target.className.indexOf('image-preview') < 0 && e.target.className.indexOf('preview-footer') < 0) {
					this.$emit('close');
				}
			},

			zoom(e) {
				if(e.target.classList.contains('zoomed')) {
					e.target.classList.remove('zoomed');
				} else {
					e.target.classList.add('zoomed');
				}
				
				// $(e.target).toggleClass('zoomed');
			}
		},

		watch: {
			image() {
				let el = document.getElementById('image-preview');
				if (el) {
					el.classList.remove('zoomed');
				}
			}
		}
	}
</script>

<style lang="scss" scoped>
.my-image-preview {
	.modal-mask {
		position: fixed;
		z-index: 99999999;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-color: rgba(0, 0, 0, .6);
		display: table;
		transition: opacity .3s ease;
	}

	.modal-wrapper {
		display: table-cell;
		vertical-align: top;
	}

	.modal-container {
		width: 1140px;
		margin: 1rem auto 3rem auto;
		padding: 0;
		background: transparent;
		border-radius: 2px;
		box-shadow: 0 2px 8px rgba(0, 0, 0, .33);
		transition: all .3s ease;

		@media(max-width: 768px) {
			width: 90%;
		}
	}

	.modal-close {
    position: absolute;
    right: 20px;
    top: 10px;

		.fa {
			color: #ea4335;
			font-size: 24px;
		}
	}

	.modal-body {
		margin: 0;
		overflow-y: auto;
		height: calc(100vh - 4rem);
		padding: 0;
		text-align: center;
    
    img {
      max-width: 100%;
			max-height: calc(100vh - 4rem);
			cursor: zoom-in;
			border: solid 1px #404040;

			&.zoomed {
				width: 100%;
				height: auto;
				max-height: none;
				overflow-y: auto;
				cursor: zoom-out;
			}
    }
  }
  
  .preview-footer {
    a {
      color: #fff;
      font-weight: bolder;
    }
  }

	/*
	* The following styles are auto-applied to elements with
	* transition="modal" when their visibility is toggled
	* by Vue.js.
	*
	* You can easily play with the modal transition by editing
	* these styles.
	*/

	.modal-enter {
		opacity: 0;
	}

	.modal-leave-active {
		opacity: 0;
	}

	.modal-enter .modal-container,
	.modal-leave-active .modal-container {
		transform: scale(1.1);
	}
}
</style>