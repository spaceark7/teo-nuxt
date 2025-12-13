<template>
  <div class="hidden gap-1 md:gap-2 lg:gap-4 xl:flex xl:gap-6">
    <AppNavItem v-for="(item, index) in mockData" :key="index" :label="item.label" :icon="item.icon"
      :items="item?.items" @click="onItemClick" />
  </div>

  <div v-if="props.isMobileNavVisible" class="flex flex-col">
    <AppNavItem v-for="(item, index) in mockData" :key="index" :label="item.label" :icon="item.icon"
      :items="item?.items" isMobile @click="onItemClick" />

  </div>

</template>

<script setup lang="ts">
import AppNavItem from './AppNavItem.vue';
import VideoLibraryImage from '~/assets/images/video-library.png';
import DryEyeArticlesImage from '~/assets/images/dry-eye.png';
import ExpertOpinionsImage from '~/assets/images/expert-op.png';
import EyeStoriesImage from '~/assets/images/eye-stories.png';

const props = defineProps<{
  isMobileNavVisible?: boolean;
}>()


const emits = defineEmits<{
  (e: 'itemClick', payload?: any): void;
}>()




const mockData = [
  {
    label: 'Home',
    icon: '',
  },
  {
    label: 'Resources',
    icon: '',
    items: [
      {
        label: 'Video Library',
        icon: 'pi pi-file',
        media: {
          type: 'image',
          src: VideoLibraryImage
        },
        description: 'A collection of videos to help you see dry eye from every angle.'
      },
      {
        label: 'Dry Eyes Articles',
        icon: 'pi pi-book',
        media: {
          type: 'image',
          src: DryEyeArticlesImage
        },
        description: 'Read and learn about causes, symptoms, and treatments.'
      },
      {
        label: 'Expert Opinions',
        icon: 'pi pi-users',
        media: {
          type: 'image',
          src: ExpertOpinionsImage
        },
        description: 'Article written by Eye Specialists from Singapore and Malaysia.'
      },
      {
        label: 'Eye Stories',
        icon: 'pi pi-users',
        media: {
          type: 'image',
          src: EyeStoriesImage
        },
        description: 'Read how others live with and manage dry eye in their own way.'
      },
      // {
      //   label: 'Expert Opinions',
      //   icon: 'pi pi-users',
      //   media: {
      //     type: 'image',
      //     src: ExpertOpinionsImage
      //   },
      //   description: 'Article written by Eye Specialists from Singapore and Malaysia.'
      // },
      // {
      //   label: 'Eye Stories',
      //   icon: 'pi pi-users',
      //   media: {
      //     type: 'image',
      //     src: EyeStoriesImage
      //   },
      //   description: 'Read how others live with and manage dry eye in their own way.'
      // },
      // {
      //   label: 'Expert Opinions',
      //   icon: 'pi pi-users',
      //   media: {
      //     type: 'image',
      //     src: ExpertOpinionsImage
      //   },
      //   description: 'Article written by Eye Specialists from Singapore and Malaysia.'
      // },
      // {
      //   label: 'Eye Stories',
      //   icon: 'pi pi-users',
      //   media: {
      //     type: 'image',
      //     src: EyeStoriesImage
      //   },
      //   description: 'Read how others live with and manage dry eye in their own way.'
      // },
    ],
  },
  {
    label: 'Ask the Expert',
    icon: '',
  }, {
    label: 'Eye test'
  },
  {
    label: 'Forum'
  },

]

const onItemClick = (items: any) => {
  console.log('Clicked item with subitems:', items);
  if (props.isMobileNavVisible) {

  }
  emits('itemClick', items);
}

</script>

<style scoped></style>