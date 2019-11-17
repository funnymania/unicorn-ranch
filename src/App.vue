<template>
  <div id="app-root">
    <h2>Your Unicorn Ranch</h2>
    <h3 class="list-headings">Delightful Alive Unicorns</h3>
    <div id="alive-corns">
      <div class="alive-entry" v-for="entry in aliveCorns" :key="entry.id">
        <div class="unicorn-imagery">
          <div class="display-cont">
            <a :href="entry.src_url" target="_blank">
              <img class="cornIcon" :src="iconDisplay(entry)" />
            </a>
            <img class="cornHorn" src="../assets/unicornHorn.svg" />
          </div>
        </div>
      </div>
    </div>
    <h3 class="list-headings">Unicorn Graveyard</h3>
    <div id="dead-corns">
      <div class="dead-entry" v-for="entry in deadCorns" :key="entry.id">
        <div class="unicorn-imagery">
          <div class="display-cont">
            <img class="cornIcon" :src="iconDisplay(entry)" />
            <img class="cornHorn" src="../assets/unicornHorn.svg" />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'UnicornRanch',
  data () {
    return {
      aliveCorns: [],
      deadCorns: []
    }
  },
  mounted () {
    fetch('/captured-cards', {
      credentials: 'same-origin'
    })
      .then(res => res.json())
      .then(resJ => {
        resJ.active.forEach(el => {
          let { org_name, ...theRest } = el
          theRest.name = org_name
          this.aliveCorns.push(theRest)
        })

        resJ.inactive.forEach(el => {
          let { org_name, ...theRest } = el
          theRest.name = org_name
          this.deadCorns.push(theRest)
        })
      })
      .catch(err => console.log(err))
  },
  methods: {
    iconDisplay (e) {
      return require(`../assets/${e.name}.svg`)
    }
  }
}
</script>

<style lang="scss" scoped>
#ranch-root {
  position: relative;
  font-size: 16px;
  background-color: black;
  font-family: "Monaco", "Fira Mono", "DejaVu Sans Mono", "Consolas",
    "Courier New", Courier, monospace;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: white;
  height: 100%;
  margin: 0 auto;
}
.alive-entry {
  width: 25%;
  display: block;
  height: 50%;
  position: relative;
  margin-right: 20px;
  flex-shrink: 0;
}
.unicorn-imagery {
  height: 100%;
  width: 150px;
}
.display-cont {
  position: relative;
  height: 100%;
}
.dead-entry {
  width: 25%;
  display: block;
  height: 50%;
  position: relative;
  margin-right: 20px;
  flex-shrink: 0;
}
.list-headings {
  text-align: left;
  margin-left: 10%;
}
.cornIcon {
  height: 100%;
  position: absolute;
  left: 0;
}
.cornHorn {
  height: 100%;
  position: absolute;
  left: 47%;
  top: -34%;
  pointer-events: none;
}
#alive-corns {
  display: flex;
  align-items: center;
  overflow: auto;
  width: 80%;
  min-height: 150px;
  height: 25%;
  margin-left: 5%;
}
#dead-corns {
  display: flex;
  align-items: center;
  overflow: auto;
  width: 80%;
  min-height: 150px;
  height: 25%;
  margin-left: 5%;
  filter: grayscale(0.6) contrast(1.5) brightness(0.6);
}
</style>
