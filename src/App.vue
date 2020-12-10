<template>
  <div id="app">
    <router-view />
    <el-input v-model="text" placeholder="asdasdasd" variant="lightblue">
      <el-button slot="append-btn">asdas as das das</el-button>
    </el-input>
    <el-dropdown v-model="select" :options="selectOptions" placeholder="asdasdasd" />
    <el-datepicker v-model="date" placeholder="asdasdasd" />
    <el-file-upload v-model="file" title="asd" name="qwe" />
  </div>
</template>
<script>
import ElInput from './elements/ElInput'
import ElDropdown from './elements/ElDropdown'
import ElDatepicker from './elements/ElDatepicker'
import ElButton from './elements/ElButton'
import ElFileUpload from './elements/ElFileUpload'
export default {
  name: 'App',
  components: {
    ElInput,
    ElDropdown,
    ElDatepicker,
    ElButton,
    ElFileUpload,
  },
  data() {
    return {
      date: null,
      text: null,
      select: [],
      file: [],
    }
  },
  computed: {
    selectOptions() {
      return [
        {
          title: 'asd1',
          value: 1,
        },
        {
          title: 'asd2',
          value: 2,
        },
        {
          title: 'asd3',
          value: 3,
        },
        {
          title: 'asd4',
          value: 4,
        },
      ]
    },
  },
}
</script>
<style lang="scss">
@import 'assets/scss/main';
@import 'assets/scss/indents';
#app {
  padding: 100px;
}
</style>
