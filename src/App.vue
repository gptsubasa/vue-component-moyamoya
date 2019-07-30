<template>
  <div id="app">
    <span v-for="moya in moyamoyas">
      <Moyamoya :width="moya.width" :height="moya.height" :max="moya.max" :color="moya.color"></Moyamoya>
    </span>
  </div>
</template>

<script>
import Moyamoya from './components/Moyamoya'

export default {
  components: {
    Moyamoya
  },
  data () {
    return {
      moyamax: 40,
      moyamoyas: []
    }
  },
  created () {
    for(var i=0; i<this.moyamax; i++){
      this.moyamoyas.push({
        width: 150,
        height: 150,
        max: 8 + parseInt(Math.random() * 4, 10),
        color: "rgb(" + (~~(256 * Math.random())) + ", " + (~~(256 * Math.random())) + ", " + (~~(256 * Math.random())) + ")"
      })
    }
  }
}
</script>