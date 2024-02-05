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
        
        <image v-if="cell.tile != null" :href="cell.tile" height="100" width="100" :x="cell.col*100" :y="cell.row*100" :transform="`rotate(${cell.rotation}, ${cell.col*100+50}, ${cell.row*100+50})`" />
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
        }
      },
      created(){
          this.initializeGrid();
          // this.draw();
      },
      methods:{
  
        // <image v-if="cell.tile != null" :href="cell.tile" height="100" width="100" :x="cell.x*100" :y="cell.y*100" :transform="`rotate(${cell.rotation}, ${cell.x + cellSize/2}, ${cell.y + cell.x*50})`" />
        initializeGrid(){            
          
          
          for(let i=0; i<this.rows; i++){
              for(let j=0; j<this.cols; j++){
                  this.grid.push({row: i, 
                    col: j, 
                    tile: "", 
                    rotation: 0,
                    options: [this.BLANK, this.UP, this.DOWN, this.RIGHT, this.LEFT],
                    collapsed: false,
                  });
              }
          }  
          for(let i=0; i< 13; i++){
            this.tiles.push(`../public/asset/tiles/circuit-coding-train/${i}.png`);
          }
          // this.grid[0].tile = this.tiles[7];
          // this.grid[0].rotation = 90;  
  
          this.grid[0].collapsed = true;
          this.grid[0].options  = [this.BLANK, this.RIGHT]
          this.grid[2].options  = [this.BLANK, this.UP, this.DOWN]
          this.grid[7].options  = [this.BLANK, this.DOWN];
          this.grid[14].options = [this.BLANK, this.DOWN];
          this.grid[20].options = [this.BLANK, this.DOWN];
          
          // this.grid[1].tile = this.tiles[7];
          // this.grid[1].rotation = 90;  
          // this.grid[7].tile = this.tiles[7];
          // this.grid[7].rotation = 90; 
          // this.grid[8].tile = this.tiles[7];
          // this.grid[8].rotation = 90; 
          // this.grid[14].tile = this.tiles[7];
          // this.grid[14].rotation = 90; 
          // this.grid[15].tile = this.tiles[7];
          // this.grid[15].rotation = 90; 
          // this.grid[21].tile = this.tiles[7];
          // this.grid[21].rotation = 90; 
          // this.grid[22].tile = this.tiles[7];
          // this.grid[22].rotation = 90; 
  
          // for(let i=0; i<this.rows * this.cols; i++){
          //   console.log(Math.random().toFixed(1) * 10)
          //   this.grid[i].tile = this.tiles[Math.random().toFixed(1) * 10] 
          //   this.grid[i].rotation = 90 
          // }
        },
  
        getDraw(){
  
          const gridCopy = this.grid.slice(0);
          gridCopy.sort((a, b)=>{
            return a.options.length - b.options.length;
          });
          let len = gridCopy[0].options.length;
          let stopIndex = 0;
          for(let i=1; i<gridCopy.length; i++){
            if(gridCopy[i].options.length > len)
            {
              stopIndex = i;
              break;
            }
          }
          
          const gridCopy2 = gridCopy.slice(0, stopIndex);     
          
          console.log("this.grid: ", this.grid);
          console.log("gridCopy: ", gridCopy);
          console.log("gridCopy2", gridCopy2);
          
          console.log("(Math.random() * gridCopy2.length).toFixed(0)", (Math.random() * gridCopy2.length).toFixed(0))
          // gridCopy2[(Math.random() * gridCopy2.length).toFixed(0)];
          console.log("gridCopy2", gridCopy2[(Math.random() * gridCopy2.length).toFixed(0)]);
          const gridCopy3 = gridCopy2[(Math.random() * gridCopy2.length).toFixed(0)];
          console.log("gridCopy3", gridCopy3.options[(Math.random() * gridCopy3.options.length).toFixed(0)]);
          console.log("gridCopy3.options", gridCopy3.options)
          const pick =  gridCopy3.options[(Math.random() * gridCopy3.options.length).toFixed(0)];
          gridCopy3.options = [pick];
          console.log("gridCopy3.options", gridCopy3.options)
          // const cell = gridCopy2[(Math.random() * gridCopy2.length).toFixed(0)];
          // console.log("cell: ", cell);
          // cell.collapsed = true;
          // const pick =cell.options[(Math.random() * cell.options.length).toFixed(0)]; 
          // console.log((Math.random() * cell.options.length).toFixed(0));
          // console.log("pick", pick)
          // cell.options = [pick];
          // console.log("cell.options: ", cell.options);
  
  
          const nextTiles = [];
          for(let i=0; i<this.grid.length; i++){
            let index i = 0; i<
          }
  
  
          
          
  
          
  
          
          // for(let i=0; i<this.rows; i++){
          //   if(grid[i].collapsed){
          //     let index = this.grid[i].options[0];
          //   } 
          //   else{
          //   }
          // }
        },
      }
    }
  </script>
  <style>
  </style>