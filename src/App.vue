<template>
  <div id="app">
    <div class="logo">
      <img src="./assets/imgs.jpg" alt width="100px" height="100px" />
      <h2>Cherry Blossom Calendar</h2>
    </div>
    <div class="button-row">
      <button @click="prev"><</button>
      <div class="month-label">{{current.getMonth() + 1}} 월</div>
      <button @click="next">></button>
    </div>

    <transition name="slide">
      <div class="calendar">
        <div class="box">
          <div class="days">일</div>
          <div class="days">월</div>
          <div class="days">화</div>
          <div class="days">수</div>
          <div class="days">목</div>
          <div class="days">금</div>
          <div class="days">토</div>
        </div>
        <div v-for="week in dayList" class="week">
          <day
            v-for="day in week"
            :day="day"
            :key="day.idx"
            @edit="openEditPopup"
            @open="openPopupWindow(day.idx)"
          >{{day}}</day>
        </div>
      </div>
    </transition>

    <transition name="fade">
      <div id="popup" v-if="popupOpen">
        <div class="inner">
          <div class="form-group">
            <input v-model="todoTitle" type="text" placeholder="할 일을 입력하세요" />
            <textarea v-model="todoContent" placeholder="상세한 내용을 입력하세요"></textarea>
            <div class="btn-group">
              <button class="btn" type="button" @click="saveTodo">입력</button>
              <button class="btn1" type="button" @click="popupOpen = false">닫기</button>
            </div>
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  name: "app",
  created() {
    let now = new Date();
    this.drawCalendar(now);
  },
  methods: {
    drawCalendar(now) {
      this.current = now;
      let first = now.getFirstDate();
      first.addDay(-first.getDay()); //달력의 최초 시작일이 나온다.
      this.dayList = [];
      while (true) {
        let arr = [];
        for (let i = 0; i < 7; i++) {
          arr.push({ idx: first.getTime(), date: first.clone(), list: [] });
          first.addDay(1);
        }
        this.dayList.push(arr);
        if (first.getMonth() != now.getMonth()) {
          break;
        }
      }
    },
    prev() {
      let pre = this.current.clone();
      pre.setMonth(pre.getMonth() - 1);
      this.drawCalendar(pre);
    },
    next() {
      let nex = this.current.clone();
      nex.setMonth(nex.getMonth() + 1);
      this.drawCalendar(nex);
    },
    openPopupWindow(day) {
      console.log(day);
      this.currentPopupData = day;
      this.popupOpen = true;
    },
    saveTodo() {
      if (this.currentEditItem != null) {
        this.currentEditItem.name = this.todoTitle;
        this.currentEditItem.content = this.todoContent;
        this.currentEditItem = null;
      } else {
        this.dayList.forEach(week => {
          week.forEach(day => {
            if (day.idx == this.currentPopupData) {
              day.list.push({
                name: this.todoTitle,
                content: this.todoContent
              });
            }
          });
        });
      }

      this.popupOpen = false;
      this.todoTitle = "";
      this.todoContent = "";
    },
    openEditPopup(e, idx, dIdx) {
      this.dayList.forEach(week => {
        week.forEach(day => {
          if (day.idx == dIdx) {
            this.todoTitle = day.list[idx].name;
            this.todoContent = day.list[idx].content;
            this.currentEditItem = day.list[idx];
            this.popupOpen = true;
          }
        });
      });
    }
  },
  data() {
    return {
      current: null,
      dayList: [],
      popupOpen: false,
      currentPopupData: null,
      todoTitle: "",
      todoContent: "",
      currentEditItem: null
    };
  }
};
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Anton|Lato|Sunflower:300&display=swap");
body {
  background-image: url(./assets/img.jpg);
}

.logo {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  padding-top: 30px;
  padding-bottom: 30px;
}
.box {
  display: grid;
  grid-gap: 10px;
  margin-top: 10px;
  grid-template-columns: repeat(7, 1fr);
  grid-template-rows: minmax(40px, auto);
  text-align: center;
}
.days{
  width: 180.13px;
  height: 40px;
  background-color: #fab4eb;
  border-radius: 10px;
  margin-bottom: 10px;
  display: flex;
  justify-content: center;
  align-items: center;
}
.month-label {
  font-family: "Sunflower", sans-serif;
}
.logo > h2 {
  font-family: "Lato", sans-serif;
}
.button-row {
  width: 70%;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
}

.button-row button {
  border: none;
  background-color: rgb(216, 248, 186);
  font-size: 20px;
  cursor: pointer;
  font-weight: bold;
  color: rgb(80, 80, 80);
  border-radius: 5px;
}

.month-label {
  font-size: 25px;
}
.calendar {
  width: 70%;
  margin: 0 auto;
  margin-top: 20px;
}
.week {
  display: grid;
  grid-gap: 10px;
  margin-bottom: 10px;
  grid-template-columns: repeat(7, 1fr);
  grid-template-rows: minmax(120px, auto);
}

#popup {
  position: fixed;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.3);
  display: flex;
  justify-content: center;
  align-items: center;
}

#popup > .inner {
  width: 400px;
  height: 300px;
  background-image: url(./assets/back.jpg);
  border-radius: 7px;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px;
  border: 3px solid pink;
}

#popup > .inner > .form-group {
  display: grid;
  grid-template-columns: 1fr;
  width: 80%;
  height: 100%;
  grid-gap: 10px;
}

#popup > .inner > .form-group input {
  height: 25px;
  padding: 1px 9px;
  border-radius: 4px;
  border: 2px solid pink;
  font-size: 15px;
}

#popup > .inner > .form-group textarea {
  border-radius: 4px;
  border: 2px solid pink;
  padding: 9px;
  font-size: 15px;
}

.form-group > textarea {
  height: 150px;
}

#popup > .inner > .form-group .btn-group {
  width: 100%;
  justify-content: space-around;
  display: flex;
  height: 35px;
}

#popup > .inner > .form-group .btn-group button {
  width: 70px;
  border-radius: 5px;
  color: #fff;
  cursor: pointer;
}

#popup > .inner > .form-group .btn-group button:first-child {
  background-color: #eb98d9;
  border-color: #eb98d9;
}

#popup > .inner > .form-group .btn-group button:last-child {
  background-color: #eb98d9;
  border-color: #eb98d9;
}

#popup > .inner > .form-group .btn-group button:first-child:hover {
  background-color: #fff;
  border-color: #fff;
  color: #000;
}

#popup > .inner > .form-group .btn-group button:last-child:hover {
  background-color: #fff;
  border-color: #fff;
  color: #000;
}

.days:first-child{ 
        color: red;
    }
    .days:last-child   {
        color: blue;
    }
</style>
