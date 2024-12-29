<template>
    <div class="gallery-container">
        <div class="gallery-grid">
            <div class="gallery-item" v-for="(image, index) in images" :key="index">
                <img ref="imageRefs" :data-src="image.src" :alt="image.alt" class="gallery-image"
                    :class="{ 'fade-in': image.loaded }" @load="handleImageLoad(index)" />
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent, onMounted, ref } from 'vue';

export default defineComponent({
    name: 'Gallery',
    data() {
        return {
            images: [
                { src: '/assets/gallery/1.png', alt: 'Image 1', loaded: false },
                { src: '/assets/gallery/2.png', alt: 'Image 2', loaded: false },
                { src: '/assets/gallery/3.png', alt: 'Image 3', loaded: false },
                { src: '/assets/gallery/4.png', alt: 'Image 4', loaded: false },
                { src: '/assets/gallery/5.png', alt: 'Image 5', loaded: false },
                { src: '/assets/gallery/6.png', alt: 'Image 6', loaded: false },
                { src: '/assets/gallery/7.png', alt: 'Image 7', loaded: false },
            ]
        };
    },
    methods: {
        handleImageLoad(index: number) {
            this.images[index].loaded = true;
        },
        lazyLoadImages() {
            const imageRefs = this.$refs.imageRefs as HTMLImageElement[];
            const observer = new IntersectionObserver((entries) => {
                entries.forEach(entry => {
                    if (entry.isIntersecting) {
                        const img = entry.target as HTMLImageElement;
                        img.src = img.dataset.src!;
                        observer.unobserve(img);
                    }
                });
            });

            imageRefs.forEach(img => {
                observer.observe(img);
            });
        }
    },
    mounted() {
        this.lazyLoadImages();
    }
});
</script>

<style scoped>
.gallery-container {
    padding: 20px;
}

.gallery-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
    gap: 20px;
    padding: 0;
    margin: 0;
}

.gallery-item {
    position: relative;
    overflow: hidden;
    border-radius: 10px;
    box-shadow: 0 0 15px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.gallery-item:hover {
    transform: scale(1.05);
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
}

.gallery-image {
    width: 100%;
    height: 100%;
    object-fit: cover;
    display: block;
    opacity: 0;
    transition: opacity 0.6s ease;
}

.gallery-image.fade-in {
    opacity: 1;
}
</style>
