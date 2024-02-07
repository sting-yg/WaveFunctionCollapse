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
      <image v-if="cell.tile != null && cell.collapsed == true" 
        :href="cell.tile.path" 
        height="100" 
        width="100" 
        :x="cell.col*100" 
        :y="cell.row*100" 
        :transform="`rotate(${cell.rotation}, ${cell.col*100+50}, ${cell.row*100+50})`" />
    </g>
  </svg>
  <div>
    <button @click="createMaze">TestButton</button>
  </div>
</template>
<script>
import App_copy from './components/App_copy.vue';


  export default{
    data(){
      return{
        grid:[],
        tiles: [],
        width: 1000,
        height: 1000,
        cellSize: 100,
        cols: 10,
        rows: 10,
        currentGrid: null,
        nextGrid: [],
        openset: [],
        closedset: [],         
      }
    },
    created(){
      this.initializeGrid();
    },
    methods:{
      initializeGrid(){             
        let index = 0            
        for(let i=0; i<this.rows; i++){
          for(let j=0; j<this.cols; j++){
            this.grid.push({  
              options: [0,1,2,3,4],
              useableOptions: [],    
              disableOptions: [],
              row: i, 
              col: j, 
              tile: null, 
              rotation: 0,
              collapsed: false,
              });
          }
        }  
        // defin tiles type :  up -> right -> down -> left
        this.tiles = [
          {path: "../public/asset/tiles/demo/blank.png", options: ['0', '0', '0', '0']},
          {path: "../public/asset/tiles/demo/up.png",    options: ['1', '1', '0', '1']},
          {path: "../public/asset/tiles/demo/right.png", options: ['1', '1', '1', '0']}, 
          {path: "../public/asset/tiles/demo/down.png",  options: ['0', '1', '1', '1']},
          {path: "../public/asset/tiles/demo/left.png",  options: ['1', '0', '1', '1']},
        ];       
        
        let randomIndex = Math.floor(Math.random()*(this.cols * this.rows));
        //this.grid[randomIndex].tile = this.tiles[Math.floor((Math.random()*this.tiles.length))];    
        //this.grid[randomIndex].collapsed = true
        //this.currentGrid = this.grid[randomIndex];
        this.openset.push(this.grid[randomIndex]);
             
        console.log("currentGrid", this.currentGrid);
        console.log("openset", this.openset);
      },   

      getCell(row, col) {
        if(row >= 0 && row < this.rows && col >=0 && col < this.cols)
        {
          return this.grid[this.cols * row + col];
        }
        else {
          return null;
        }
      },
      
      createMaze(){     
        while(this.openset.length > 0) {
          const current = this.openset.pop();

          // 전체 상태 캡처
          
          // const possibleOptions = [...current.options]; //복사

          //const copyGrid = this.grid.slice();

          const up = this.getCell(current.row-1, current.col);    
          const right = this.getCell(current.row, current.col+1);
          const down = this.getCell(current.row-1, current.col);
          const left = this.getCell(current.row, current.col-1);

            if(up != nulle) {
              for(let i=0; i<this.tiles.length; i++) {
                  if(up.tile.options[3] == this.tiles[i].options[0]){
                    current.useableOptions.push(this.tiles[i]);
                    current.options.remove(i)
                    console.log("current.options" , current.options);
                  }
              }
              this.openset.push(up);
            }
            // right 
            // down
            // left

            const random = Math.floor(Math.random() * current.options.length)
            current.tile = this.tiles[random]

          if(up != null){             
            if(up.collapsed == false){
              this.openset.push(up);
            }
          }
          else if(right != null){
            if(right.collapsed == false){
              this.openset.push(right)
            }
          }
          else if(down != null){
            if(right.collapsed == false){
              this.openset.push(down)
            }
          }
          else if(down != null){
            if(right.collapsed == false){
              this.openset.push(left) 
            }
            else{
              this.closedset.push(left)
            }
          }
          else{

          }
          // if(possibleOptions.length == 0) {

          // }
        }
      },
    }
  }   
</script>
<style>
</style>