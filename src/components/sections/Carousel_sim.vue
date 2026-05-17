<script>
import { Swiper, SwiperSlide } from 'swiper/vue'
import { Navigation, Pagination, Autoplay } from 'swiper/modules'
import 'swiper/css/bundle'

export default {
  components: {
    Swiper,
    SwiperSlide,
  },

  data() {
    return {
      modules: [
        Navigation,
        Pagination,
        Autoplay,
      ],

      video_pairs: [
        {
          source: './sim_tasks/phone_on_base_sawyer.mp4',
          target: './sim_tasks/phone_on_base_franka.mp4',
        },
        {
          source: './sim_tasks/pick_up_cup_sawyer.mp4',
          target: './sim_tasks/pick_up_cup_franka.mp4',
        },
        {
          source: './sim_tasks/put_rubbish_in_bin_sawyer.mp4',
          target: './sim_tasks/put_rubbish_in_bin_franka.mp4',
        },
        {
          source: './sim_tasks/slide_block_to_target_sawyer.mp4',
          target: './sim_tasks/slide_block_to_target_franka.mp4',
        },
        {
          source: './sim_tasks/pick_and_lift_sawyer.mp4',
          target: './sim_tasks/pick_and_lift_franka.mp4',
        },
        {
          source: './sim_tasks/basketball_in_hoop_sawyer.mp4',
          target: './sim_tasks/basketball_in_hoop_franka.mp4',
        },
      ],
    }
  },

  methods: {
    onSwiper(swiper) {
      this.$nextTick(() => {
        if (swiper.autoplay) {
          swiper.autoplay.start()
        }
      })
    },
  },
}
</script>

<template>
  <div class="carousel-section">
    <el-divider />

    <el-row justify="center">
      <h1 class="section-title">Simulation Tasks</h1>
    </el-row>

    <el-row justify="center">
      <el-col :span="24">
        <div class="row-title-wrap">
          <h2 class="row-title">Source Demonstration</h2>
        </div>

        <swiper
          class="task-swiper"
          :loop="true"
          :slides-per-view="1"
          :space-between="16"
          :speed="800"
          :observer="true"
          :observe-parents="true"
          :breakpoints="{
            600: {
              slidesPerView: 2,
            },
            800: {
              slidesPerView: 3,
            },
          }"
          :modules="modules"
          :navigation="{
            hideOnClick: true,
          }"
          :pagination="{
            hideOnClick: true,
            clickable: true,
            type: 'bullets',
          }"
          :autoplay="{
            delay: 5000,
            disableOnInteraction: false,
            pauseOnMouseEnter: false,
          }"
          @swiper="onSwiper"
        >
          <swiper-slide
            v-for="item in video_pairs"
            :key="item.source"
          >
            <div class="video-pair">
              <video
                class="carousel-video"
                :src="item.source"
                autoplay
                muted
                loop
                playsinline
                preload="metadata"
              ></video>

              <video
                class="carousel-video target-video"
                :src="item.target"
                autoplay
                muted
                loop
                playsinline
                preload="metadata"
              ></video>
            </div>
          </swiper-slide>
        </swiper>

        <div class="target-title-wrap">
          <h2 class="row-title">Target Execution</h2>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<style scoped>
.carousel-section {
  padding: 10px 0 20px 0;
}

.section-title {
  margin: 20px 0 24px 0;
  text-align: center;
}

.row-title-wrap {
  margin: 0 0 8px 0;
}

.target-title-wrap {
  margin: 8px 0 0 0;
}

.row-title {
  margin: 0;
  text-align: center;
  font-size: 20px;
}

.task-swiper {
  --swiper-theme-color: white;
  width: 100%;
}

.video-pair {
  width: 100%;
}

.carousel-video {
  width: 100%;
  aspect-ratio: 16 / 9;
  display: block;
  object-fit: cover;
  border-radius: 8px;
}

.target-video {
  margin-top: 8px;
}
</style>