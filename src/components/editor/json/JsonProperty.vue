<template>
  <div class="json-flex">
    <ElSelect v-if="editing" v-model="dataType" class="w-100">
      <ElOption v-for="type in typesArr" :key="type" :label="type" :value="type"/>
    </ElSelect>
    <div class="flex-1 flex-v-center">
      <JsonValue
          v-model="targetVal"
          :type="dataType"
          :editing="editing"
          @onFold="onFold"
          @onEditing="$emit('onEditing')"/>
    </div>
  </div>
</template>

<script>
import {dataTypes, FoldMixin, typeOf} from './util';
import JsonValue from './JsonValue';

export default {
  name: 'JsonProperty',
  mixins: [FoldMixin],
  components: {JsonValue},
  props: {
    value: {},
    editing: Boolean,
  },
  watch: {
    value: {
      handler(now) {
        if (now === undefined) {
          this.$emit('input', null);
        } else {
          this.targetVal = now;
          this.dataType = typeOf(now);
        }
      },
      immediate: true,
    },
    targetVal(now, old) {
      if (now !== old) {
        this.$emit('input', now);
      }
    },
    dataType(now) {
      this.$emit('onChangeValueType', now);
    },
  },
  data() {
    return {
      targetVal: null,
      dataType: 'String',
    };
  },
  computed: {
    typesArr: () => [...dataTypes],
  },
};
</script>

<style scoped>

</style>
