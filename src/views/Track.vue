<template>
  <div>
    <actionResult v-if="showActionResult"/>
    <div v-if="track" class="track">

      <div
        @click="hideModal"
        ref="popupPageDark"
        class="popup-page-darken"></div>

      <!--    <Preloader></Preloader>-->

      <!-- <img :src="previewPicture"
           class="track-cover" alt="preview picture"/> -->
      <div class="track-bg-img" style="overflow: hidden; position: relative;">
        <img
          :src="previewPicture"
          class="track-cover-bg"
          alt="preview picture" data-v-7ef61a01=""
          style="
        position: absolute;
        top:  0px;
        left: 0px;
        max-height: 510px;
        width: 101%;

        filter: blur(10px);">
        <img
          :src="previewPicture"
          class="track-cover"
          alt="preview picture"
          data-v-7ef61a01="">
      </div>

      <div class="track-content container">
        <router-link
          :to="{ name: 'Tracks' }"
          class="link-back"
        >
          <i class="fas fa-arrow-left"></i>
          В каталог
        </router-link>
        <div v-if="isMaster" class="admin-btns d-flex flex-column gap-2">
          <Button
            :btn-orange="true"
            class="redact-btn"
            @click="this.$router.push({name : 'EditTrack'})"
          >
            <i class="fas fa-pencil"></i>
            Редактировать
          </Button>
          <Button
            :btn-danger="true"
            class="redact-btn"
            @click="showDeleteModal"
          >
            <i class="fas fa-times"></i>
            Удалить
          </Button>

        <div class="publish-cnt d-flex align-center">
          <label for="publish">Опубликовано :</label>
          <input
            type="checkbox"
            class="publish-btn"
            id="publish"
            :checked="track.data.published"
            @click="publish"
          >
        </div>   </div>
        <ConfirmDelete
          @callConfirm="callConfirm"
          v-if="showConfirmDelete"
          :deletion-target="deletionTarget"
        />

        <TrackInfoMain
          :name="track.data.name"
          :description="track.data.previewText"
          :isNotAssigned="!track.assigned"
          :completion-percent="completionPercent"
        />

        <TrackInfoSub
          :track-duration="trackDuration"
          :date-start-prop="track.data.dateTimeStart"
          :date-finish-prop="track.data.dateTimeFinish"
        />
        <div class="track-manage-btns">
          <Button v-if="isMaster"
                  :btn-orange="true"
                  class="add-btn"
                  @click="this.$router.push(`/track/${this.track.id}/extend`);"
          >
            <i class="fas fa-plus"></i>
            Добавить элемент
          </Button>
          <Button v-if="isMaster"
                  :btn-blue="true"
                  class="enroll-btn"
                  @click="addStudents"
          >
            <img src="../assets/student.svg" alt="student">
            Записать студента
          </Button>
        </div>
        <!-- if ordered -->
        <!-- сменить на track.assigned как будет функционал -->

        <div
          v-if="!track.assigned"
          class="track-content-ordered">
          <Button
            :class="{'btn-test': true, 'btn-disabled': false}">
            Входное тестирование
          </Button>
        </div>
        <TrackItemList :track-id="track.id" @durationCounted="onCounted"></TrackItemList>

      </div>
    </div>
  </div>
</template>

<script>
import { mapActions, mapGetters } from 'vuex';
// import Track from '../services/track/track';
// import Preloader from '../components/Preloader';

import Button from '@/components/Button.vue';
import TrackInfoMain from '@/components/trackRelated/TrackInfoMain.vue';
import TrackInfoSub from '@/components/trackRelated/TrackInfoSub.vue';
import placeholderBig from '../../public/placeholderBig.png';
import TrackItemList from '@/components/trackRelated/TrackItemList.vue';
import ActionResult from '@/components/ActionResult.vue';
import ConfirmDelete from '@/components/trackRelated/ConfirmDelete.vue';

export default {
  name: 'track',
  components: {
    ActionResult,
    // Track
    // Preloader,
    Button,
    TrackInfoMain,
    TrackInfoSub,
    TrackItemList,
    ConfirmDelete,
  },
  mounted() {
  },
  computed: {
    ...mapGetters([
      'getUser',
      'getTrackByIdStore',
    ]),

    track() {
      return this.getTrackByIdStore(+this.$route.params.id);
    },

    isMaster() {
      return this.getUser.role === 'teacher';
    },

    previewPicture() {
      if (this.track.data.previewPicture) {
        return `${this.baseUrl}${this.track.data.previewPicture}`;
      }
      return this.placeholderBig;
    },
  },

  methods: {
    ...mapActions(['removeTrack', 'editTrack']),

    showDeleteModal() {
      this.showConfirmDelete = true;
      this.deletionTarget = 'трек';
      this.$refs.popupPageDark.style.display = 'block';
      document.getElementsByTagName('body')[0].style.overflow = 'hidden';
    },

    async publish(event) {
      this.showActionResult = false;
      const dataClone = { ...this.track.data, published: event.target.checked };

      await this.editTrack({
        id: this.track.id,
        form: dataClone,
      });
      this.showActionResult = true;
    },
    hideModal() {
      this.showConfirmDelete = false;
      this.$refs.popupPageDark.style.display = 'none';
      document.getElementsByTagName('body')[0].style.overflowY = 'auto';
    },

    callConfirm(userResp) {
      if (userResp) {
        // console.log('удалить');
        this.removeTrack(this.track.id);
      }
      this.hideModal();
    },

    addStudents() {
      this.$router.push(`/track/${this.track.id}/enroll`);
    },

    onCounted(val) {
      this.trackDuration = val.duration;
      this.completionPercent = val.completion;
    },
  },

  data() {
    return {
      baseUrl: 'https://tml9.rosatom.ru',
      placeholderBig,
      trackDetail: null,
      trackDuration: 0,
      completionPercent: 0,
      showConfirmDelete: false,
      deletionTarget: 'трек',
      // typeOfitem: null,
      showActionResult: true,

    };
  },

};
</script>

<style lang="scss" scoped>
.popup-page-darken {
  width: 100%;
  height: 100%;
  background: #000;
  // position: absolute;
  // top: 0px;
  // left: 0px;
  z-index: 1000;
  opacity: 0.6;
  display: none;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translateY(-50%) translateX(-50%);
}

.track {
  color: #1f2041;
}

.track-bg-img {
  background-color: lightgrey;
  max-height: 500px;
}

.track-cover {
  max-height: 500px;
  width: calc(100% + 80px);
  object-fit: contain;
  filter: brightness(90%);
  background: rgba(0, 0, 0, 0.1);
}

.link-back {
  display: flex;
  align-self: start;
  gap: 10px;
  align-items: center;
  border-radius: 10px;
  font-weight: 700;
  font-size: 14px;
}

.redact-btn,
.add-btn,
.enroll-btn {
  border: unset;
  box-shadow: 0px 2px 4px rgba(139, 164, 249, 0.3);
}

.add-btn,
.enroll-btn {
  gap: 15px;
  border-radius: 22px;
  width: 250px;
  grid-column: 2;
}

.publish-cnt {
  gap: 12px;

  label {
    font-size: 22px;
    font-weight: 500;
  }

  input {
    width: 20px;
    height: 20px;
  }
}

.track-content {
  margin-top: 16px;
  padding-bottom: 30px;
  display: grid;
  grid-template-columns: 1fr;
  gap: 60px;

  padding: 30px 50px;
}

.track-manage-btns {
  display: flex;
  flex-wrap: wrap;
  gap: 24px;
  justify-content: center;
}

@media (min-width: 978px) {
  .track-content {
    grid-template-columns: 1fr 6fr 2fr;
  }
  // ИЗ TRACK-INFO-MAIN COMPONENT
  .track-info-main {
    grid-column-start: 2;
  }
  // ИЗ TRACK-INFO-SUB COMPONENT
  .track-info-sub {
    text-align: left;
    align-self: center;
  }

  .link-back {
    grid-column: span 2;
  }
  .track-manage-btns,
  .track-content-ordered,
  .track-item-list {
    grid-column-start: 2;
  }
  .track-manage-btns {
    justify-content: space-between;
  }
}

.track-content-ordered {
  display: flex;
  justify-content: center;
}

.track-item-list {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.track-item {
}

// .track-content-item-main {
//   display: flex;
// }

// .track-content-item-side {}
</style>
