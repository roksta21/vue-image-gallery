<template>
  <div class="vue-image-gallery">
    <div class="row">
      <div
        :class="`col-md-${bootstrap_col}`"
        v-for="(image, i) in images"
        :key="`vue-image-gallery-${i}`"
      >
        <div class="vue-gallery-image">
          <a href="#" @click.prevent="selected_image = image; show_preview = true">
            <img :src="image" :alt="image" :style="`height: ${imageHeight}; border: ${imageBorder}`" />
          </a>
        </div>
      </div>
    </div>
    <image-preview
      :showPreview="show_preview"
      @close="show_preview = false;"
      :image="selected_image"
      @next="nextImage"
      @previous="previousImage"
    />
  </div>
</template>

<script>
import ImagePreview from './ImagePreview';

export default {
  components: {
    ImagePreview,
  },

  data() {
    return {
      selected_image: '',
      show_preview: false,
    };
  },

  props: {
    images: {
      type: Array,
      required: true,
    },
    columns: {
      type: Number,
      default: 3,
    },
    imageHeight: {
      type: String,
      default: 'auto'
    },
    imageBorder: {
      type: String,
      default: 'solid 1px #ddd'
    }
  },

  methods: {
    nextImage() {
      let current_index = this.images.indexOf(this.selected_image);

      if (current_index < this.images.length - 1) {
        this.selected_image = this.images[current_index + 1];
      } else {
        this.selected_image = this.images[0];
      }
    },

    previousImage() {
      let current_index = this.images.indexOf(this.selected_image);

      if (current_index == 0) {
        this.selected_image = this.images[this.images.length - 1];
      } else {
        this.selected_image = this.images[current_index - 1];
      }
    },
  },

  computed: {
    bootstrap_col() {
      return Math.floor(12 / this.columns);
    },
  },
};
</script>

<style scoped>
.vue-image-gallery .vue-gallery-image {
  margin-bottom: 1rem;
  text-align: center;
}

.vue-image-gallery .vue-gallery-image img {
  max-width: 100%;
}
</style>