<template>
    <div class="recommend">
      <div  v-if="recommends.length" class="slider-wrapper" ref="sliderWrapper">
        <slider>
          <div v-for="(item,i) in recommends" :key="i">
              <a :href="item.linkUrl">
                <img :src="item.picUrl" alt="" />
              </a>
          </div>
        </slider>
      </div>
      <div class="recommend-list">
        <h1 class="list-title">热门歌单推荐</h1>
        <ul></ul>
      </div>
    </div>
</template>

<script type="text/ecmascript-6">
import axios from 'axios'
import Slider from '../../base/slider/slider'

export default {
  data () {
    return {
      recommends: []
    }
  },
  created () {
    this._getRecommend()
    this._getDisslist()
  },
  mounted () {},
  methods: {
    _getRecommend () {
      let _this = this
      axios.get('/api/getLists/musichall/fcgi-bin/fcg_yqqhomepagerecommend.fcg').then(function (data) {
        if (data.data.code === 0) {
          _this.recommends = data.data.data.slider
        }
      })
    },
    _getDisslist () {
      axios.get('/api/getLists/splcloud/fcgi-bin/fcg_get_diss_by_tag.fcg', {
        params: {
          g_tk: 247361471,
          jsonpCallback: 'getPlaylistTags',
          loginUin: '',
          hostUin: 0,
          format: 'jsonp',
          inCharset: 'utf8',
          outCharset: 'utf-8',
          notice: 0,
          platform: 'yqq',
          needNewCode: 0
        }
      }).then(function (data) {
        console.log(data)
      })
    }
  },
  components: {
    Slider
  }

}
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "~common/stylus/variable"

  .recommend
    position: fixed
    width: 100%
    top: 88px
    bottom: 0
    .recommend-content
      height: 100%
      overflow: hidden
      .slider-wrapper
        position: relative
        width: 100%
        overflow: hidden
      .recommend-list
        .list-title
          height: 65px
          line-height: 65px
          text-align: center
          font-size: $font-size-medium
          color: $color-theme
        .item
          display: flex
          box-sizing: border-box
          align-items: center
          padding: 0 20px 20px 20px
          .icon
            flex: 0 0 60px
            width: 60px
            padding-right: 20px
          .text
            display: flex
            flex-direction: column
            justify-content: center
            flex: 1
            line-height: 20px
            overflow: hidden
            font-size: $font-size-medium
            .name
              margin-bottom: 10px
              color: $color-text
            .desc
              color: $color-text-d
      .loading-container
        position: absolute
        width: 100%
        top: 50%
        transform: translateY(-50%)
</style>
