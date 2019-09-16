<template>
    <div class="day" @click="openPopup">
        <div class="date">
            <span class="number">{{day.date.getDate()}}</span>
            <span class="month">({{day.date.getMonth() + 1}}월)</span>
        </div>
        <div class="list">
            <div class="item" v-for="(todo, idx) in day.list" @click.stop="openEditPopup(idx)">{{todo.name}}</div>
        </div>
    </div>
</template>

<script>
    export default {
        name:'day',
        props:['day'],
        data(){
            return {
                
            }
        },
        methods:{
            openPopup(){
                this.$emit("open");
            },
            openEditPopup(idx){
                this.$emit("edit", event, idx, this.day.idx);
            }
        }
    }
</script>

<style scoped>
    

    .day:hover {
        border: 1px solid rgb(255, 0, 255);
    }
    
::-webkit-scrollbar {
  width: 10px;  
  height: 50px;  
}
::-webkit-scrollbar-track {
  background-color: lightblue;
}
::-webkit-scrollbar-track-piece {
  background-color: rgb(255, 255, 255);
}
::-webkit-scrollbar-thumb {
  border-radius: 8px;
  background-color: rgb(255, 170, 241);
}
::-webkit-scrollbar-button {
  background-color: rgb(240, 240, 240);
  width: 20px;
  height: 10px;
}
::-webkit-scrollbar-button:start {
  background-color: rgb(255, 104, 255); /* Top, Left 방향의 이동버튼 */
}
::-webkit-scrollbar-button:end {
  background-color: rgb(255, 104, 255); /* Bottom, Right 방향의 이동버튼 */
}

    .list {
        overflow-y: auto;
    }

    .date {
        padding:8px;
    }

    .number {
        font-size:25px;
        font-weight: bold;
    }

    .month {
        font-size:15px;
        color:rgb(110, 109, 109);
    }

    .item {
        background-color: rgb(243, 154, 255);
        color:#fff;
        border-radius: 0.25rem;
        padding:0.2rem 0.25rem;
        margin:4px;
        cursor: pointer;
        overflow-y: auto;
        
    }

    .day {
        width:100%;
        height:120px;
        display: flex;
        justify-content: flex-start;
        flex-direction: column;
        background-color: rgb(252, 201, 245);
        border-radius: 10px;
    }
    .week .day:first-child .number{
        color: red;
    }
    .week .day:last-child .number{
        color: blue;
    }
    
</style>