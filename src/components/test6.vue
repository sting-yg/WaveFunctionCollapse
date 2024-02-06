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
      <image v-if="cell.collapsed == true" 
        :href="cell.tile.path" 
        height="100" 
        width="100" 
        :x="cell.col*100" 
        :y="cell.row*100" 
        :transform="`rotate(${cell.rotation}, ${cell.col*100+50}, ${cell.row*100+50})`" />
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
        cols: 10,
        rows: 10,
        BLANK: 0,
        UP:    1,
        DOWN:  2,
        RIGHT: 3,
        LEFT:  4,
        currentGrid:[],
        nextGrid: [],
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
              options: [this.BLANK, this.UP, this.DOWN, this.RIGHT, this.LEFT],              
              row: i, 
              col: j, 
              tile: [{path: "", options: ""}], 
              rotation: 0,
              collapsed: false,        
            });
          }
        }  
        // defin tiles type :  up -> right -> down -> left
        this.tiles[0] = {path: "../public/asset/tiles/demo/blank.png", options: ['0', '0', '0', '0']}; 
        this.tiles[1] = {path: "../public/asset/tiles/demo/down.png",  options: ['0', '1', '1', '1']};
        this.tiles[2] = {path: "../public/asset/tiles/demo/left.png",  options: ['1', '0', '1', '1']};
        this.tiles[3] = {path: "../public/asset/tiles/demo/right.png", options: ['1', '1', '1', '0']};
        this.tiles[4] = {path: "../public/asset/tiles/demo/up.png",    options: ['1', '1', '0', '1']};

        for(let i=0; i<1; i++){
          let randomIndex = Math.floor(Math.random()*(this.cols * this.rows));
          this.grid[randomIndex].tile = this.tiles[Math.floor((Math.random()*4))];    
          this.grid[randomIndex].collapsed = true
          this.currentGrid = this.grid[randomIndex]
        }        
        console.log("currentGrid", this.currentGrid);
      },   

      getDraw(currentGrid){

        let currentRowIndex = currentGrid.row * this.cols + currentGrid.col;
        let upIndex         = currentRowIndex-this.cols;
        let rightIndex      = currentRowIndex + 1;
        let downIndex       = currentRowIndex + this.cols;
        let leftIndex       = currentRowIndex - 1;

        console.log("currentRowIndex",              currentRowIndex);
        console.log("upIndex",                      upIndex);
        console.log("rightIndex",                   rightIndex);
        console.log("downIndex",                    downIndex);
        console.log("leftIndex",                    leftIndex);
        console.log("currentGrid.options[2]",       currentGrid.options[2]);
        console.log("currentGrid.tile.options[2]",  currentGrid.tile.options[2])

        for(let i=0; i< 2; i++){
          this.getUp(currentGrid);
          this.getRight(currentGrid);
          this.getDown(currentGrid);
          this.getLeft(currentGrid);
        }

        

        // if (upIndex >= 0){
        //   let upCell = this.grid[upIndex];
        //   for(let i=0; i<4; i++){
        //     if(this.tiles[i].options[2] == this.grid[currentRowIndex].tile.options[0]){
        //       upCell.tile = this.tiles[i];
        //       console.log("upIndex tile", i, this.tiles[i]);
        //       upCell.collapsed = true;
        //       currentGrid = upCell
        //       this.getDraw(currentGrid)
        //       // this.newCurrentGrid.push(upCell);
        //     }
        //   }  
        // }  
        // if (rightIndex % this.cols !== 0){
        //   let rightCell = this.grid[rightIndex]
        //   for(let i=0; i<4; i++){
        //     if(this.tiles[i].options[3] == this.grid[currentRowIndex].tile.options[1]){
        //       rightCell.tile = this.tiles[i]
        //       console.log("rightIndex tile", i, this.tiles[i]);
        //       rightCell.collapsed = true;
        //       currentGrid = rightCell
        //       this.getDraw(currentGrid)
        //       // this.newCurrentGrid.push(rightCell);
        //     }
        //   }
        // } 
        // if(downIndex < this.rows * this.cols){
        //   let downCell = this.grid[downIndex]
        //   for(let i=0; i<4; i++){
        //     if(this.tiles[i].options[0] == this.grid[currentRowIndex].tile.options[2]){
        //       downCell.tile = this.tiles[i]
        //       console.log("downIndex tile", i, this.tiles[i]);
        //       downCell.collapsed = true;
        //       currentGrid = downCell
        //       this.getDraw(currentGrid)
        //       // this.newCurrentGrid.push(downCell);        
        //     }
        //   }
        // }
        // if(leftIndex % this.cols !== this.cols - 1 && leftIndex >= 0){
        //   let leftCell = this.grid[leftIndex]
        //   for(let i=0; i<4; i++){
        //     if(this.tiles[i].options[1] == this.grid[currentRowIndex].tile.options[3]){
        //       leftCell.tile = this.tiles[i]
        //       console.log("lefIndex tile", i, this.tiles[i]);
        //       leftCell.collapsed = true;
        //       currentGrid = leftCell
        //       this.getDraw(currentGrid)
        //       // this.newCurrentGrid.push(leftCell);
        //     }
        //   } 
        // }
      },

      getUp(currentGrid){
        let currentRowIndex = currentGrid.row * this.cols + currentGrid.col;
        let upIndex         = currentRowIndex-this.cols;
        if (upIndex >= 0){
          let upCell = this.grid[upIndex];
          for(let i=0; i<4; i++){
            if(this.tiles[i].options[2] == this.grid[currentRowIndex].tile.options[0]){
              upCell.tile = this.tiles[i];
              console.log("upIndex tile", i, this.tiles[i]);
              upCell.collapsed = true;
              currentGrid = upCell
              // this.getDraw(currentGrid)
              // this.newCurrentGrid.push(upCell);
            }
          }  
        }
        else{
          return 0;
        }

      },
      
      getRight(currentGrid){
        let currentRowIndex = currentGrid.row * this.cols + currentGrid.col;
        let rightIndex      = currentRowIndex + 1;
        if (rightIndex % this.cols !== 0){
          let rightCell = this.grid[rightIndex]
          for(let i=0; i<4; i++){
            if(this.tiles[i].options[3] == this.grid[currentRowIndex].tile.options[1]){
              rightCell.tile = this.tiles[i]
              console.log("rightIndex tile", i, this.tiles[i]);
              rightCell.collapsed = true;
              currentGrid = rightCell
              // this.getDraw(currentGrid)
              // this.newCurrentGrid.push(rightCell);
            }
          }
        }
        else{
          return 0;
        }

      },

      getDown(currentGrid){
        let currentRowIndex = currentGrid.row * this.cols + currentGrid.col;
        let downIndex       = currentRowIndex + this.cols;
        if(downIndex < this.rows * this.cols){
          let downCell = this.grid[downIndex]
          for(let i=0; i<4; i++){
            if(this.tiles[i].options[0] == this.grid[currentRowIndex].tile.options[2]){
              downCell.tile = this.tiles[i]
              console.log("downIndex tile", i, this.tiles[i]);
              downCell.collapsed = true;
              currentGrid = downCell
              // this.getDraw(currentGrid)
              // this.newCurrentGrid.push(downCell);        
            }
          }
        }

      },

      getLeft(currentGrid){
        let currentRowIndex = currentGrid.row * this.cols + currentGrid.col;
        let leftIndex       = currentRowIndex - 1;
        if(leftIndex % this.cols !== this.cols - 1 && leftIndex >= 0){
          let leftCell = this.grid[leftIndex]
          for(let i=0; i<4; i++){
            if(this.tiles[i].options[1] == this.grid[currentRowIndex].tile.options[3]){
              leftCell.tile = this.tiles[i]
              console.log("lefIndex tile", i, this.tiles[i]);
              leftCell.collapsed = true;
              currentGrid = leftCell
              // this.getDraw(currentGrid)
              // this.newCurrentGrid.push(leftCell);
            }
          } 
        }
        else{
          return 0;
        }

      }
      

        //这是测试代码
        // this.grid[2].options  = [this.BLANK, this.LEFT]
        // this.grid[3].options  = [this.BLANK, this.LEFT]
        // this.grid[5].options  = [this.BLANK, this.LEFT]
        // this.grid[7].options  = [this.BLANK, this.LEFT]
        // this.grid[11].options = [this.BLANK, this.LEFT]

        // let gridCopy = this.grid.slice();
        // gridCopy = gridCopy.filter((a) =>!a.collapsed);
        // gridCopy.sort((a, b) => {
        //   return a.options.length - b.options.length;
        // })

        // console.log("grid", this.grid)
        // console.log("gridCopy", gridCopy)

        // let len = gridCopy[0].length
        // let stopIndex = 0
        // for(let i=1; i < gridCopy.length; i++){
        //   if(gridCopy[i].options.length > len){
        //     stopIndex = i;
        //     break;
        //   }
        // }
        // console.log("stopIndex", stopIndex)
        // if (stopIndex > 0) gridCopy.splice(stopIndex);
        // console.log("gridCopy:", gridCopy)

        // const cell = gridCopy[(Math.random() * gridCopy.length).toFixed(0)]
        // //
        // console.log("cell", cell)
        // cell.collapsed = true
        // const pick = cell.options[(Math.random() * cell.options.length).toFixed(0)];
        // //
        // console.log("pick", pick)
        // cell.options = [pick]
        // console.log("cell", cell)


        // let currentRowIndex = currentGrid.row * this.cols + currentGrid.col;
        // let upIndex         = currentRowIndex-this.cols;
        // let rightIndex      = currentRowIndex + 1;
        // let downIndex       = currentRowIndex + this.cols;
        // let leftIndex       = currentRowIndex - 1;

        // console.log("currentRowIndex",              currentRowIndex);
        // console.log("upIndex",                      upIndex);
        // console.log("rightIndex",                   rightIndex);
        // console.log("downIndex",                    downIndex);
        // console.log("leftIndex",                    leftIndex);
        // console.log("currentGrid.options[2]",       currentGrid.options[2]);
        // console.log("currentGrid.tile.options[2]",  currentGrid.tile.options[2])

        // const nextGrid = [];
        // for(let i=0; i < this.rows; i++){
        //   for(let j=0; j <this.cols.length; j++){
        //     if(grid[currentRowIndex].collapsed){
        //       nextGrid[currentRowIndex] = this.grid[currentRowIndex]
        //     }
        //     else {
        //       let options = [BLANK, UP, RIGHT, DOWN, LEFT];
              
        //       // look up
        //       if (upIndex >= 0){
        //         let upCell = this.grid[upIndex];
        //         let validOptions = [];
        //         for(let i=0; i<4; i++){
        //           let valid = this.tiles[i].options[2]
        //           console.log("this.tiles[i].options[2]", this.tiles[i].options[2])
        //           console.log("valid:", valid)
        //           validOptions = validOptions.concat(valid);         
        //         }  
        //         checkValid(options, validOptions);
        //       }
        //       // look right
        //       if (rightIndex % this.cols !== 0){
        //         let rightCell = this.grid[rightIndex]
        //         let validOptions = [];
        //         for(let i=0; i<4; i++){
        //           let valid = tiles[i].options[3]
        //           validOptions = validOptions.concat(valid);
                  
        //         }
        //         checkValid(options, validOptions);
        //       }
        //       // look down
        //       if(downIndex < this.rows * this.cols){
        //         let downCell = this.grid[downIndex]
        //         let validOptions = [];
        //         for(let i=0; i<4; i++){
        //           let valid = tiles[i].options[0]
        //           validOptions = validOptions.concat(valid);
                                   
        //         }
        //         checkValid(options, validOptions);
        //       }
        //       //look left
        //       if(leftIndex % this.cols !== this.cols - 1 && leftIndex >= 0){
        //         let leftCell = this.grid[leftIndex]
        //         let validOptions = [];
        //         for(let i=0; i<4; i++){
        //           let valid = tiles[i].options[1]
        //           validOptions = validOptions.concat(valid);
                  
        //         }
        //         checkValid(options, validOptions);
        //       } 
        //     } 
        //       nextGrid[index] = {
        //         options,
        //         collapsed: false,
        //       }
        //     }
        //   }  
        // }
      
      // checkValid(arr, valid){
      //   for(let i=arr.length - 1; i >= 0; i--){
      //     let element = arr[i];
      //     if(!valid.includes(element)){
      //       arr.splice(i,1);
      //     }
      //   } 
      // }
    }
  }  
                

      
 
</script>
<style>
</style>