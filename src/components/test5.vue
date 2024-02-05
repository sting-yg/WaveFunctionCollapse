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
        <image v-if="cell.collapsed == true" :href="cell.tile.path" height="100" width="100" :x="cell.col*100" :y="cell.row*100" :transform="`rotate(${cell.rotation}, ${cell.col*100+50}, ${cell.row*100+50})`" />
      </g>
    </svg>
    <div>
      <button @click="getDraw(this.currentGrid)">TestButton</button>
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
          cols: 5,
          rows: 5,
          BLANK: 0,
          UP: 1,
          DOWN: 2,
          RIGHT: 3,
          LEFT: 4,
          currentGrid:[],
          newCurrentGrid:[],
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
                Index : index++,
                row: i, 
                col: j, 
                tile: [{path: "", options:""}], 
                rotation: 0,
                collapsed: false,    
                useOptions: [{BLANK:['0', '0', '0', '0'], DOWN: ['0', '1', '1', '1'], LEFT:['1', '0', '1', '1'], RIGHT:['1', '1', '1', '0'], UP:['1', '1', '0', '1']}],     
              });
            }
          }  
          // defin tiles type : blank-> up -> right -> down -> left
          this.tiles[0] = {path: "../public/asset/tiles/demo/blank.png", options: ['0', '0', '0', '0']}; 
          this.tiles[1] = {path: "../public/asset/tiles/demo/down.png",  options: ['0', '1', '1', '1']};
          this.tiles[2] = {path: "../public/asset/tiles/demo/left.png",  options: ['1', '0', '1', '1']};
          this.tiles[3] = {path: "../public/asset/tiles/demo/right.png", options: ['1', '1', '1', '0']};
          this.tiles[4] = {path: "../public/asset/tiles/demo/up.png",    options: ['1', '1', '0', '1']};
  
          let randomIndex 
          let randomIndexs
          for(let i=0; i<1; i++){
            randomIndex = Math.floor(Math.random()*(this.cols * this.rows));
            this.grid[randomIndex].tile = this.tiles[(Math.random()*4).toFixed(0)];    
            this.grid[randomIndex].collapsed = true
            this.currentGrid = this.grid[randomIndex]
          }      
          
          console.log("currentGrid", this.currentGrid);
        },   
  
        getDraw(currentGrid){
  
          let currentRowIndex = currentGrid.row * this.cols + currentGrid.col;
          let upIndex = currentRowIndex-this.cols;
          let rightIndex = currentRowIndex + 1;
          let downIndex = currentRowIndex + this.cols;
          let leftIndex = currentRowIndex - 1;
  
          console.log("currentRowIndex", currentRowIndex);
          console.log("upIndex",    upIndex);
          console.log("rightIndex", rightIndex);
          console.log("downIndex",  downIndex);
          console.log("leftIndex",  leftIndex);
          // console.log("currentGrid.tile.options[2]", currentGrid.tile.options[2])
  
          if (upIndex >= 0){
            let upCell = this.grid[upIndex];
            for(let i=0; i<4; i++){
              if(this.tiles[i].options[2] == this.grid[currentRowIndex].tile.options[0]){
                upCell.tile = this.tiles[i];
  
                //如果有相同的 则计数 +1
                
  
  
                console.log("upIndex tile", i, this.tiles[i]);
                upCell.collapsed = true;
                // this.newCurrentGrid.push(upCell);
              }
            }  
          }  
          if (rightIndex % this.cols !== 0){
            let rightCell = this.grid[rightIndex]
            for(let i=0; i<4; i++){
              if(this.tiles[i].options[3] == this.grid[currentRowIndex].tile.options[1]){
                rightCell.tile = this.tiles[i]
                console.log("rightIndex tile", i, this.tiles[i]);
                rightCell.collapsed = true;
                // this.newCurrentGrid.push(rightCell);
              }
            }
          } 
          if(downIndex < this.rows * this.cols){
            let downCell = this.grid[downIndex]
            for(let i=0; i<4; i++){
              if(this.tiles[i].options[0] == this.grid[currentRowIndex].tile.options[2]){
                downCell.tile = this.tiles[i]
                console.log("downIndex tile", i, this.tiles[i]);
                downCell.collapsed = true;
                // this.newCurrentGrid.push(downCell);        
              }
            }
          }
          if(leftIndex % this.cols !== this.cols - 1 && leftIndex >= 0){
            let leftCell = this.grid[leftIndex]
            for(let i=0; i<4; i++){
              if(this.tiles[i].options[1] == this.grid[currentRowIndex].tile.options[3]){
                leftCell.tile = this.tiles[i]
                console.log("lefIndex tile", i, this.tiles[i]);
                leftCell.collapsed = true;
                // this.newCurrentGrid.push(leftCell);
              }
            } 
          }  
          console.log(this.grid)
          let gridCopy = this.grid.slice();   
          console.log("slice gridCopy:" ,gridCopy)
          gridCopy = gridCopy.filter((a) => !a.collapsed)
          console.log("filter collapsed gridCopy: " ,gridCopy)
  
          if(gridCopy.length == 0){
            return;
          }    
  
          gridCopy.sort((a, b) => {
            return a.useOptions.length - b.useOptions.length;
          })
  
          console.log("after sort", gridCopy)
  
          let len = gridCopy[0].useOptions.length;
          let stopIndex = 0;
  
          for (let i=1; i<gridCopy.length; i++)
          {
            if(gridCopy[i].useOptions.length >len)
            {
              stopIndex = i;
              
              break;
            }
          }
          console.log("stopIndex: ",stopIndex)
          if(stopIndex > 0) gridCopy.splice(stopIndex);
          const cell = Math.random(gridCopy);
          cell.collapsed = true;
          const pick = Math.random(cell.useOptions);
          if(pikc === undefined){
            startover();
            return;
          }
          cell.useOptions = [pick]
  
  
          
        },
  
        //check up
        checkUp(currentGrid){
          let currentRowIndex = currentGrid.row * this.cols + currentGrid.col;
          let upIndex = currentRowIndex-this.cols;
          if (upIndex >= 0){
            let upCell = this.grid[upIndex];
            for(let i=0; i<4; i++){
              if(this.tiles[i].options[2] == this.grid[currentRowIndex].tile.options[0]){
                upCell.tile = this.tiles[i];
                upCell.collapsed = true;
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
              if(this.tiles[i].options[3] == this.grid[currentRowIndex].tile.options[1]){
                rightCell.tile = this.tiles[i]
                rightCell.collapsed = true;
                
              }
            }
          }        
        },
        //check down
        checkDown(currentGrid){
          let currentRowIndex = currentGrid.row * this.cols + currentGrid.col;
          let downIndex = currentRowIndex + this.cols;
          if(downIndex < this.rows * this.cols){
            let downCell = this.grid[downIndex]
            for(let i=0; i<4; i++){
              if(this.tiles[i].options[0] == this.grid[currentRowIndex].tile.options[2]){
                downCell.tile = this.tiles[i]
                downCell.collapsed = true;
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
              if(this.tiles[i].options[1] == this.grid[currentRowIndex].tile.options[3]){
                leftCell.tile = this.tiles[i]
                leftCell.collapsed = true
              }
            } 
          }
        },
      }
    }
  </script>
  <style>
  </style>