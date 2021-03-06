<template>
  <div class="tabs">
    <ul class="tabs__list">
      <li
        class="tabs__link"
        v-for="(tab, index) in tabsData"
        :key="tab.id"
        :class="[
          { ['tabs__link_' + theme + '_active']: show == index },
          `tabs__link_${theme}`,
        ]"
        @click.prevent="
          show = index;
          $emit('tab-changed', index);
        "
      >
        {{ tab.title }}
      </li>
    </ul>
    <div class="tabs__content">
      <p
        class="tabs__text"
        :class="`tabs__text_${theme}`"
        v-for="(tab, index) in tabsData"
        v-if="show === index"
        :key="tab.id"
        v-html="tab.text"
      ></p>
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  props: ['tabsData', 'theme'],
  data() {
    return {
      show: 0,
    };
  },
};
</script>

<style scoped>
.tabs {
  display: flex;
  justify-content: space-between;
  margin: 37px 0 0;
}
.tabs__list {
  list-style: none;
  margin: 0;
  text-align: right;
}
.tabs__link {
  cursor: pointer;
  margin-bottom: 10px;
  font-style: normal;
  font-weight: normal;
  font-size: 18px;
  line-height: 22px;
}

.tabs__link_call-to-action {
  color: #a2a2a2;
}
.tabs__link_call-to-action:hover {
  color: #000;
  transition: color 0.3s linear;
}
.tabs__link_call-to-action_active {
  color: #000;
}

.tabs__link_about {
  color: #c9c9c9;
}
.tabs__link_about:hover {
  color: #fff;
  transition: color 0.3s linear;
}
.tabs__link_about_active {
  color: #fff;
}
.tabs__content {
  display: flex;
  flex-direction: column;
  margin-left: 40px;
}
.tabs__text {
  display: flex;
  flex-direction: column;
  max-width: 640px;
  font-style: normal;
  font-weight: normal;
  font-size: 18px;
  line-height: 22px;
  margin: 0 0 16px;
}
.tabs__text:last-child {
  margin-bottom: 0;
}

.tabs__text >>> p {
  margin: 0;
}

.tabs__text_call-to-action {
  color: #666;
}
.tabs__text_about {
  color: #dedede;
  margin-bottom: 22px;
}

@media screen and (max-width: 1280px) {
  .tabs {
    margin: 30px 0 0;
  }
  .tabs__text {
    max-width: 570px;
  }
}
@media screen and (max-width: 1024px) {
  .tabs {
    margin: 27px 0 0;
  }
  .tabs__link {
    margin-bottom: 8px;
    font-size: 15px;
    line-height: 1.27;
  }
  .tabs__content {
    margin-left: 30px;
  }
  .tabs__text {
    max-width: 447px;
    font-size: 15px;
    line-height: 1.27;
  }
}

@media screen and (max-width: 768px) {
  .tabs {
    margin: 80px 0 0;
    flex-direction: column;
    flex: auto;
  }
  .tabs__link {
    margin-bottom: 24px;
    margin-right: 30px;
    padding-bottom: 6px;
  }
  .tabs__link:last-child {
    margin-right: 0px;
  }
  .tabs__link_call-to-action_active {
    border-bottom: #613a93 solid 2px;
  }
  .tabs__link_about_active {
    border-bottom: #fff solid 2px;
  }
  .tabs__content {
    margin-left: 0px;
    flex: auto;
    justify-content: space-between;
  }
  .tabs__list {
    display: flex;
    padding: 0;
  }
  .tabs__text {
    max-width: 380px;
  }
}
@media screen and (max-width: 350px) {
  .tabs {
    margin: 40px 0 0;
  }
  .tabs__text_call-to-action {
    font-size: 13px;
    line-height: 1.23;
  }
  .tabs__text {
    max-width: 290px;
    font-size: 13px;
    line-height: 1.23;
    margin-bottom: 20px;
  }
  .tabs__link {
    font-size: 13px;
    line-height: 1.46;
    margin-bottom: 16px;
    padding-bottom: 4px;
  }
  .tabs__text_about {
    font-size: 15px;
    line-height: 1.27;
  }
}
</style>
