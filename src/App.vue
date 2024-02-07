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
      <image v-if="cell.option != null && cell.collapsed == true" 
        :href="tiles[cell.option]" 
        height="50" 
        width="50" 
        :x="cell.col*50" 
        :y="cell.row*50" />
    </g>
    <rect v-if="currentCell"
        :x="currentCell.col * cellSize"
        :y="currentCell.row * cellSize"
        :width="cellSize"
        :height="cellSize"
        stroke="red"
        stroke-width="3"        
      /> 
  </svg>
  <div>
    <button @click="createMaze">TestButton</button>
  </div>
  <div>
    <p>{{ message }}</p>
  </div>
</template>
<script>

export default{
    data(){
      return{
        grid:[],
        tiles: {
          blank: "../public/asset/tiles/demo/blank.png",
          up: "../public/asset/tiles/demo/up.png",
          right: "../public/asset/tiles/demo/right.png", 
          down: "../public/asset/tiles/demo/down.png",
          left: "../public/asset/tiles/demo/left.png",
        },
        width: 500,
        height: 500,
        cellSize: 50,
        cols: 3,
        rows: 3,
        possibleOptions: {
          'blank' : {
            'u': ['up', 'blank'],
            'r': ['right', 'blank'],
            'd': ['down', 'blank'],
            'l': ['left', 'blank'],
          },
          'up' : {
            'u': ['down', 'left', 'right'],
            'r': ['left', 'up', 'down'],
            'd': ['down', 'blank'],
            'l': ['right', 'up', 'down'],
          },
          'right' : {
            'u': ['down', 'left', 'right'],
            'r': ['left', 'up', 'down'],
            'd': ['up', 'left', 'right'],
            'l': ['left', 'blank'],
          },
          'down' : {
            'u': ['up', 'blank'],
            'r': ['left', 'up', 'down'],
            'd': ['up', 'left', 'right'],
            'l': ['right', 'up', 'down'],
          },
          'left' : {
            'u': ['down', 'left', 'right'],
            'r': ['right', 'blank'],
            'd': ['up', 'left', 'right'],
            'l': ['right', 'up', 'down'],
          },
        },
        options: ['blank', 'up', 'right', 'down', 'left'],
        currentCell: null,
        snapshots: [],
        message: "",
      }
    },
    created(){
      this.initializeGrid();
    },
    methods:{
      initializeGrid(){             
        for(let i=0; i<this.rows; i++){
          for(let j=0; j<this.cols; j++){
            this.grid.push({  
              options: ['blank', 'up', 'right', 'down', 'left'],
              option: null,
              row: i, 
              col: j, 
              collapsed: false,
              });
          }
        }
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

      createMaze() {  
        this.grid.forEach(x => {
          x.options = ['blank', 'up', 'right', 'down', 'left'];
          x.option = null;
          x.collapsed = false;
        });
        let randomIndex = Math.floor(Math.random()*(this.cols * this.rows));
        this.grid[randomIndex].option = this.options[Math.floor((Math.random()*this.options.length))];    
        this.grid[randomIndex].collapsed = true;
        console.log(randomIndex, this.grid[randomIndex]);
        this.snapshots = [];
        
        const timer = setInterval(() => {
          if(this.grid.every(x => x.collapsed)) {
            this.message = "done";
              clearInterval(timer);
              return;
          }

          const snapshot = this.grid.map(x => ({cell:x, options:x.options.slice(), collapsed:x.collapsed}));

          this.currentCell = this.tryCollapse();
          if(this.currentCell) {
            this.snapshots.push({before: snapshot, cell:this.currentCell, option:this.currentCell.option});
          }
          else {
            if(this.snapshots.length == 0) {
              this.message = "no solution";
              clearInterval(timer);
              return;
            }

            const lastSnapshot = this.snapshots.pop();
            lastSnapshot.before.forEach(x => {
              x.cell.options = x.options.slice();
              x.cell.collapsed = x.collapsed;
            });

            //lastSnapshot.cell.options = lastSnapshot.options;
            const optionIndex = lastSnapshot.cell.options.indexOf(lastSnapshot.option);
            if(optionIndex >= 0) {
              lastSnapshot.cell.options.splice(optionIndex, 1);
            }
            lastSnapshot.cell.collapsed = false;
          }
        }, 100);
      },
      
      tryCollapse() {     
        const candis = this.grid.filter(x => x.collapsed == false);
        if(candis.length == 0) {
          return null;
        }

        const current = candis[Math.floor((Math.random()*candis.length))];
        const neighbor = [
          {dir:'up', opposite:'d', cell: this.getCell(current.row-1, current.col)},    
          {dir:'right', opposite:'l', cell:this.getCell(current.row, current.col+1)},
          {dir:'down', opposite:'u', cell:this.getCell(current.row+1, current.col)},
          {dir:'left', opposite:'r', cell:this.getCell(current.row, current.col-1)}
        ];

        let myOptions = [...current.options];
        neighbor.filter(x => x.cell && x.cell.collapsed).forEach(x => {
          const options = this.possibleOptions[x.cell.option][x.opposite];
          //console.log(x, options);
          myOptions = myOptions.filter(y => options.includes(y));
        });

        if(myOptions.length == 0)
        {
          return null;
        }

        current.option = myOptions[Math.floor((Math.random()*myOptions.length))];
        current.collapsed = true;
        return current;
      },
    }
  }   
</script>
<style>
</style>