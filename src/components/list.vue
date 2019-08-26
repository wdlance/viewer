<template>
  <div class="list-wrap">
    <div class="sort-method flex">
      <button :class="{active:activeIndex==1}" @click="changeMethod(1)">按时间</button>
      <button :class="{active:activeIndex==2}" @click="changeMethod(2)">按分类</button>
    </div>
    <div class="list flex">
      <div class="item" v-for="(item,index) in list" :key="item.id" @drop="dropEnd($event,index)" @dragover="allowDrop($event)">
        <div class="img-wrap relative" draggable @dragstart="dragStart($event,index)">
          <img :src="item.image" :draggable="activeIndex==2?true:false">
          <!-- <select @change="changeType" v-model="item.tId">
            <option v-for="(type,index) in types" :key="type.id" :value="type.id">
              {{type.name}}
            </option>
          </select> -->
        </div>
        <div class="info-box">
          <div class="name">{{item.name}}</div>
          <div class="flex">
            <div class="time">{{item.time}}</div>
            <div class="time">{{getTypeById(item.tId).name}}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script type="text/javascript">
import { list, types } from "./mock.js"
export default {
  data() {
    return {
      activeIndex: 1,
      list: [],
      type: [],
      dragIndex:""
    }
  },
  created() {
    this.list = list
    this.types = types
    this.sortByTime()
  },
  methods: {
    allowDrop(e) {
      e.preventDefault();
    },
    dropEnd(e,index) {
      e.preventDefault();
      this.$set(this.list[this.dragIndex],"tId",this.list[index].tId)
      this.list.splice(index+1, 0, this.list[this.dragIndex]);
      this.list.splice(this.dragIndex,1)
    },
    dragStart(e, index) {
      this.dragIndex = index
    },
    getTypeById(id) {
      return this.types.find(v => v.id === id)
    },
    changeMethod(type) {
      this.activeIndex = type
      if (type == 1) {
        this.sortByTime()
      } else {
        this.sortByType()
      }
    },
    changeType() {
      this.sortByType()
    },
    sortByTime(order = 0) { /*0表示倒叙,1表示顺序*/
      if (order == 0) {
        if (this.list instanceof Array) {
          let list = this.list
          list.sort((a, b) => {
            return b.time - a.time
          })
        }
      }
    },
    sortByType(order = 0) { /*0表示倒叙,1表示顺序*/
      if (order == 0) {
        if (this.list instanceof Array) {
          let list = this.list
          list.sort((a, b) => {
            return b.tId - a.tId
          })
        }
      }
    }
  }


}

</script>
<style type="text/css">
.list-wrap {
  padding: 20px 10px;
  max-width: 240px;
  margin: auto;
}

.relative {
  position: relative;
}

.flex {
  display: flex;
}

.sort-method {
  margin-bottom: 20px;
  justify-content: center;
}

.sort-method button {
  margin-right: 20px;
  padding: 0 15px;
  height: 30px;
  line-height: 30px;
}

.sort-method button.active {
  background: red;
  color: #fff;
}

.list {
  flex-wrap: wrap;
  justify-content: space-around;
}

.img-wrap {
  height: 0px;
  padding-bottom: 50%;
  background: #f8f8f8;
  overflow: hidden;
}

select {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: rgba(0, 0, 0, 0.7);
  height: 30px;
  color: #fff;
  padding: 0 15px;
}

.item {
  width: 100%;
  min-width: 150px;
  margin-bottom: 20px;
  padding: 0 10px;
  box-sizing: border-box;
}

.info-box {
  color: #c1c1c1;
  font-size: 12px;
  padding: 10px 0px;
}

img {
  width: 100%;
}

.name {
  color: #333;
  font-size: 14px;
  margin-bottom: 10px;
}

.info-box .flex {
  justify-content: space-between;
}

</style>
