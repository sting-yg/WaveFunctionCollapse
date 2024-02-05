<template>
  <svg :width="width" :height="height">
    <g v-for="(cell, rowIndex) in grid" :key="rowIndex">
      <rect
          :x="cell.col * cellSize"
          :y="cell.row * cellSize"
          :width="cellSize"
          :height="cellSize"
          fill="black"
          stroke="white"
          stroke-width="1"
          
      />
      <image v-if="cell.tile != null" :href="cell.tile" height="200" width="200" :transform="`rotate(${cell.rotationG}, 100, 100)`" />
  </g>
  </svg>
  <div>
    <button @click="draw()">click</button>
  </div>
</template>

<script>

  export default{
    data(){
      return{
        grid:[],
        tiles: [],
        width: 1000,
        height: 1000,
        cellSize: 200,
        cols: 3,
        rows: 3,
      }
    },
    created(){
        this.initializeGrid();
        // this.draw();
    },
    methods:{
      initializeGrid(){       
        
        for(let i = 0; i < this.rows; i++){
            for(let j = 0; j <this.cols; j++){
                this.grid.push({row: i, col: j, tile: null, rotationG: 0, rotationX: i*100, rotationY: j*100});

            }
        }  
        for(let i=0 ; i< 13;i++){
          this.tiles.push(`../public/asset/tiles/circuit-coding-train/${i}.png`);
        }
        
        this.grid[0].tile = this.tiles[3];
        this.grid[0].rotationG= 90;


      },
    }
  }

</script>

<style>


</style>