<template>
  <div>
    <template v-if="position.length">
      <div class="f-title-1 mx-4 mb-2">{{ $t("common.assets") }}</div>
      <f-panel padding="0" class="mx-4">
        <f-list>
          <f-list-item
            v-for="item in sortedPosition"
            :key="item.asset_id"
            :title="`${item.amount.toString()} ${item.symbol}`"
            :subtitle="`$${item.totalUsd.toFixed(2)}`"
            @click="handleClick(item)"
          >
            <template #head>
              <f-mixin-asset-logo
                :logo="item.icon_url"
                :size="32"
                class="mt-1"
              />
            </template>
          </f-list-item>
        </f-list>
      </f-panel>
    </template>
    <div v-else class="f-body-2 f-greyscale-3 text-center mt-10">
      {{ $t("hint.no_asset_1") }} <br />{{ $t("hint.no_asset_2") }}
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue, Prop } from "vue-property-decorator";
import { Mutation, State } from "vuex-class";

@Component
class AssetList extends Vue {
  @State((state) => state.global.position) position;

  get sortedPosition() {
    const ret = this.position.slice();
    return ret.sort((a, b) => {
      if (a.totalUsd > b.totalUsd) {
        return -1;
      } else if (a.totalUsd < b.totalUsd) {
        return 1;
      }
      return 0;
    });
  }

  handleClick(item) {
    this.$emit("click", item);
  }
}
export default AssetList;
</script>

<style lang="scss" scoped></style>
