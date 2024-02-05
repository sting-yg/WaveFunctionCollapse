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
        <image v-if="cell.tile != null" :href="cell.tile.path" height="100" width="100" :x="cell.col*100" :y="cell.row*100" :transform="`rotate(${cell.rotation}, ${cell.col*100+50}, ${cell.row*100+50})`" />
      </g>
    </svg>
    <div>
      <button @click="getDraw">TestButton</button>
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
          cellSize: 100,
          cols: 7,
          rows: 7,
          BLANK: 0,
          UP: 1,
          DOWN: 2,
          RIGHT: 3,
          LEFT: 4,
          currentGrid:[],
          
        }
      },
      created(){
          this.initializeGrid();
          // this.draw();
      },
      methods:{
        initializeGrid(){       
          
          let index = 0
                 
          for(let i=0; i<this.rows; i++){
            for(let j=0; j<this.cols; j++){
              this.grid.push({
                Index : index++,
                row: i, 
                col: j, 
                tile: [], 
                rotation: 0,
                collapsed: false,         
              });
            }
          }  
          // defin tiles type : up -> right -> down -> left
          this.tiles[0] = {path: "../public/asset/tiles/demo/blank.png", options: ['0', '0', '0', '0']}; 
          this.tiles[1] = {path: "../public/asset/tiles/demo/down.png",  options: ['0', '1', '1', '1']};
          this.tiles[2] = {path: "../public/asset/tiles/demo/left.png",  options: ['1', '0', '1', '1']};
          this.tiles[3] = {path: "../public/asset/tiles/demo/right.png", options: ['1', '1', '1', '0']};
          this.tiles[4] = {path: "../public/asset/tiles/demo/up.png",    options: ['1', '1', '0', '1']};
  
  
          let randomIndex 
          let randomIndexs
          for(let i=0; i<2; i++){
            randomIndex = Math.floor(Math.random()*(this.cols * this.rows));
            this.grid[randomIndex].tile = this.tiles[(Math.random()*4).toFixed(0)];
          }
          
          
          this.currentGrid = this.grid[0];
          console.log("currentGrid", this.currentGrid);
          console.log("grdi[0]", this.grid[0]);
          // for(let i=0; i<4; i++){
          //   if(this.tiles[i].options[3] == this.grid[0].tile.options[1]){
          //     this.grid[0+1].tile = this.tiles[i]
          //   }
          // }
          // this.checkRight(this.currentGrid)
        },
  
        getDraw(){
  
          const gridCopy = this.grid.slice(0)
          
          console.log(this.grid);
          console.log(gridCopy);
  
           
        },
        //check up
        checkUp(currentGrid){
          let currentRowIndex = currentGrid.row * this.cols + currentGrid.col;
          let upIndex = currentRowIndex-this.cols;
          if (upIndex >= 0){
            let upCell = this.grid[upIndex];
            for(let i=0; i<4; i++){
              if(this.tiles[i].options[2] == currentGrid.tile.options[0]){
                upCell.tile = this.tiles[i];
              }
            }  
          }       
        },
        //check right
        checkRight(currentGrid){
          let currentRowIndex = currentGrid.row * this.cols + currentGrid.col;
          let rightIndex = currentRowIndex + 1;
          if (rightIndex % this.cols !== 0){
            let rightCell = this.grid[rightIndex]
            for(let i=0; i<4; i++){
              if(this.tiles[i].options[3] == currentGrid.tile.options[1]){
                this.grid[rightIndex].tile = this.tiles[i]
              }
            }
          }        
        },
        //check down
        checkDown(currentGrid){
          let currentRowIndex = currentGrid.row * this.cols + currentGrid.col;
          let downIndex = currentRowIndex + this.cols;
          if(downIndex <= this.rows * this.cols){
            let downCell = this.grid[downIndex]
            for(let i=0; i<4; i++){
              if(this.tiles[i].options[0] == currentGrid.tile.options[2]){
                this.grid[downIndex] = this.tiles[i]
              }
            }
          }
        },
        // check left
        checkLeft(currentGrid){
          let currentRowIndex = currentGrid.row * this.cols + currentGrid.col;
          let leftIndex = currentRowIndex - 1;
          if(leftIndex % this.cols !== this.cols - 1 && leftIndex >= 0){
            let leftCell = this.grid[leftIndex]
            for(let i=0; i<4; i++){
              if(this.tiles[i].options[1] == currentGrid.tile.options[3]){
                leftCell.tile = this.tiles[i]
              }
            } 
          }
        },
      }
    }
  </script>
  <style>
  </style>