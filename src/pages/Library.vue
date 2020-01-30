<template>
  <div class="parent">
    <div class="col dashboard-list grid">
      <div
        v-for="i in 64"
        :key="i"
        class="q-card dashboard-item column"
        :class="{ selected: selected == i }"
      ></div>
    </div>
  </div>
</template>

<script>
export default {
  name: "game-library",
  data() {
    return {
      selected: 1,
      minSize: 397,
      padding: 60,
      gap: 10
    };
  },
  mounted() {
    document.addEventListener("keydown", this.move);
  },
  beforeDestroy() {
    document.removeEventListener("keydown", this.move);
  },
  methods: {
    move(evt) {
      const cwi = document.documentElement.clientWidth;
      let numberOfCols = parseInt(cwi / this.minSize);
      console.log("cols", numberOfCols);
      const card = document.getElementsByClassName("q-card").length;

      evt.preventDefault();
      if (evt.keyCode == 40) {
        var myElement = document.getElementsByClassName("selected");
        const allElements = document.querySelectorAll(".q-card");
        
        const myCol = this.showFound(myElement[0], allElements);
        let idx = 0;
        for (const it of allElements) {
          const classes = Object.values(it.classList);
          if (!classes.includes('selected')) {
            const col = this.showFound(it, allElements);
            if (col == myCol) {
              this.selected = idx+1;
              break;
            }
          }
          idx += 1;
        }
        //down
      } else if (evt.keyCode == 38) {
         var myElement = document.getElementsByClassName("selected");
        const allElements = document.querySelectorAll(".q-card");
        
        const myCol = this.showFound(myElement[0], allElements);
        let idx = 0;
        for (const it of allElements) {
          const classes = Object.values(it.classList);
          if (!classes.includes('selected')) {
            const col = this.showFound(it, allElements);
            if (col == myCol) {
              this.selected = idx+1;
              break;
            }
          }
          idx += 1;
        }
        //up
      } else if (evt.keyCode == 37) {
        console.log(evt);
        this.selected -= 1;
        //right
      } else if (evt.keyCode == 39) {
        console.log(evt);
        this.selected += 1;
        //left
      }
    },
    showFound(myElement, allElements) {
      let maxcolpos = -1,
        colposCount = 0;

      for (const elem of allElements) {
        let l = elem.getBoundingClientRect().left;
        if (l > maxcolpos) {
          maxcolpos = l;
          if (myElement.getBoundingClientRect().left > l) colposCount++;
        }
      }

      const columnIndex = colposCount + 1; //zero based, leave +1 if you want 0 based
      return columnIndex ;
    }
  }
};
</script>

<style>
.selected {
  border: 2px solid #008845;
}

.dashboard-item {
  height: 255px;
}

.parent {
  padding: 00px;
}

.dashboard-list {
  display: grid;
  grid-gap: 10px;
  grid-auto-flow: column;
  grid-auto-columns: 397px;
  grid-template-rows: 670px;
}

.dashboard-list.grid {
  padding: 0px;
  grid-template-columns: repeat(auto-fill, minmax(254px, 1fr));
  grid-template-rows: repeat(auto-fill, minmax(254px, 1fr));
  grid-auto-flow: row;
  grid-auto-columns: auto;
}
</style>
