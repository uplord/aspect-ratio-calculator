<template>
  <div class="inner-container">

    <div class="row">
      <div class="col-6">
        <BaseInput label="Width 1" id="w1" type="text" v-model.number="original_width" @input="updateRatio()" />
      </div>
      <div class="col-6">
        <BaseInput label="Height 1" id="h1" type="text" v-model.number="original_height" @input="updateRatio()" />
      </div>
      <div class="col-6">
        <BaseInput label="Width 2" id="w2" type="text" v-model.number="new_width" @input="updateRatio('new_width')" />
      </div>
      <div class="col-6">
        <BaseInput label="Height 2" id="h2" type="text" v-model.number="new_height" @input="updateRatio('new_height')" />
      </div>
    </div>

    <div>Aspect Ratio {{ aspect_ratio }}</div>

    <div class="ratio-box" :style="{ width: example_width, height: example_height }"></div>

  </div>
</template>

<script>
  export default {
    data() {
      return {
        original_width: 500,
        original_height: 500,
        new_width: 0,
        new_height: 0,
        aspect_ratio: '',
        example_size: 200,
        example_width: '',
        example_height: ''
      }
    },
    created() {
      this.calculateExample()
      this.calculateRatio()
    },
    methods: {
      updateRatio(type = '') {

        if (this.original_width == '') {
          this.original_width = 0
        }
        if (this.original_height == '') {
          this.original_height = 0
        }

        this.calculateRatio()

        if (type == 'new_width') {
          if (this.new_width != '') {
            this.new_height = Math.floor(this.new_width * (this.original_height / this.original_width))
          }
        } else if (type == 'new_height') {
          if (this.new_height != '') {
            this.new_width = Math.floor(this.new_height * (this.original_width / this.original_height))
          }
        } else {
          if (this.original_height != '' && this.original_width != '') {
            this.new_height = Math.floor(this.new_width * (this.original_height / this.original_width))
            this.new_width = Math.floor(this.new_height * (this.original_width / this.original_height))
          } else {
            this.new_height = 0
            this.new_width = 0
          }
        }

        this.calculateExample()
      },
      calculateExample() {

        if (this.original_height < this.original_width) {
          this.example_height = (this.example_size * (this.original_height / this.original_width)) + 'px'
          this.example_width = this.example_size + 'px'
        } else {
          this.example_height = this.example_size + 'px'
          this.example_width = (this.example_size * (this.original_width / this.original_height)) + 'px'
        }
      },
      calculateRatio() {

        this.aspect_ratio = ''

        if (!this.isInteger(this.original_width) || !this.isInteger(this.original_height)) {
          this.aspect_ratio = '? : ?'
        }
        if (this.original_width === this.original_height) {
          this.aspect_ratio = '1 : 1'
        }

        const divisor = this.divisorCalc(this.original_width, this.original_height)

        this.aspect_ratio = this.original_width / divisor + ' : ' + this.original_height / divisor
      },
      isInteger(value) {
        return /^[0-9]+$/.test(value);
      },
      divisorCalc(a, b) {
        if (b === 0) return a
        return this.divisorCalc(b, a % b)
      }
    }
  }
</script>

<style lang="less" scoped>
  @import 'style';
</style>