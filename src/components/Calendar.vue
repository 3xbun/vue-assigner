<template>
  <div class="container">
    <div class="months">
      <span @click="monthHandler('p')">
        {{ monthFormatter(global.currentMonth - 1) }}</span
      >
      <span class="active"> {{ monthFormatter(global.currentMonth) }}</span>
      <span @click="monthHandler('f')">
        {{ monthFormatter(global.currentMonth + 1) }}</span
      >
    </div>
    <div class="calendar">
      <div class="days-header">
        <span>Sun</span>
        <span>Mon</span>
        <span>Tue</span>
        <span>Wed</span>
        <span>Thu</span>
        <span>Fri</span>
        <span>Sat</span>
      </div>
      <div class="days">
        <span
          :id="day"
          v-for="(day, index) in state.dayOfMonth"
          :key="index"
          @click="
            global.selectedDay = day;
            global.workOfDay = state.workOfMonth.filter(
              (work) => work.workDay == global.selectedDay
            );
          "
          >{{ day }}
          <span class="dots">
            <div v-for="(work, index) in state.workOfMonth" :key="index">
              <span
                class="dot"
                v-if="work.workDay == day"
                :style="{ backgroundColor: work.work.color }"
              ></span>
            </div>
          </span>
        </span>
      </div>
    </div>
  </div>
</template>

<script>
import { inject, onBeforeMount, onMounted, reactive } from "vue";
import Months from "./Months.js";
import Homeworks from "./Homeworks.js";

export default {
  name: "Calendar",
  setup() {
    const global = inject("global");
    const dayjs = require("dayjs");
    const utc = require("dayjs/plugin/utc");

    dayjs.extend(utc);
    const state = reactive({
      currentMonth: 0,
      firstDayOfMonth: "",
      lastDayOfMonth: "",
      dayOfMonth: [],
      workOfMonth: [],
    });

    const monthHandler = (method) => {
      reset();

      if (method === "p") {
        global.currentMonth -= 1;

        const date = `${monthFormatter(global.currentMonth)} ${dayjs().year()}`;
        state.firstDayOfMonth = dayjs(date, "MMM YYYY").startOf("M").day();
        state.lastDayOfMonth = dayjs(date, "MMM YYYY").endOf("M").date();
        setDay(state.firstDayOfMonth, state.lastDayOfMonth);

        setWorks(Homeworks);
      } else if (method === "f") {
        global.currentMonth += 1;

        const date = `${monthFormatter(global.currentMonth)} ${dayjs().year()}`;
        state.firstDayOfMonth = dayjs(date, "MMM YYYY").startOf("M").day();
        state.lastDayOfMonth = dayjs(date, "MMM YYYY").endOf("M").date();
        setDay(state.firstDayOfMonth, state.lastDayOfMonth);

        setWorks(Homeworks);
      }
    };

    const monthFormatter = (month) => {
      if (month < 0) {
        if (month % 12 !== 0) {
          return Months[(month % 12) + 12].abbreviation.toUpperCase();
        } else {
          return Months[0].abbreviation.toUpperCase();
        }
      } else if (month > 11) {
        return Months[month % 12].abbreviation.toUpperCase();
      }

      return Months[month].abbreviation.toUpperCase();
    };

    const setDay = (firstDay, lastDay) => {
      let counter = 0;
      let day = 1;

      while (day <= lastDay) {
        if (counter < firstDay) {
          state.dayOfMonth.push("");
          counter += 1;
        } else {
          state.dayOfMonth.push(day);
          day += 1;
        }
      }

      while (state.dayOfMonth.length % 7 !== 0) {
        state.dayOfMonth.push("");
      }
    };

    const setWorks = (works) => {
      works.forEach((work) => {
        const duration = dayjs(work.dueDate).diff(
          dayjs(work.assignDate),
          "day"
        );

        for (let counter = 0; counter <= duration; counter++) {
          const day = dayjs(work.assignDate).add(counter, "day").date();
          const month = dayjs(work.assignDate).add(counter, "day").month();

          if (month == global.currentMonth) {
            state.workOfMonth.push({
              work: work,
              workDay: day,
            });
          }
          state.workOfMonth.sort((a, b) => {
            return a - b;
          });
        }
      });
    };

    const reset = () => {
      state.firstDayOfMonth = "";
      state.lastDayOfMonth = "";
      state.dayOfMonth = [];
      state.workOfMonth = [];
      global.selectedDay = "";
      global.workOfDay = {};
    };

    onBeforeMount(() => {
      state.firstDayOfMonth = dayjs().startOf("M").day();
      state.lastDayOfMonth = dayjs().endOf("M").date();
      global.currentMonth = dayjs().month();

      setDay(state.firstDayOfMonth, state.lastDayOfMonth);
    });

    onMounted(() => {
      setWorks(Homeworks);
    });

    return { monthFormatter, state, monthHandler, Homeworks, global };
  },
};
</script>

<style scoped>
.container {
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  -o-user-select: none;
  user-select: none;
}

.months {
  display: flex;
  max-width: 350px;
  font-size: 1.5em;
  text-align: right;
  font-weight: bold;
  margin: auto;
  padding: 0 1em;
}

.months span {
  color: var(--grey);
  flex: 1;
  text-align: center;
  cursor: pointer;
}

.months .active {
  color: white;
}

.calendar {
  background-color: white;
  border: 1px solid #efefef;
  border-radius: 0.5em;
  box-shadow: 0 5px 25px -5px rgba(0, 0, 0, 0.15);
  max-width: 350px;
  padding: 0.5em;
  width: 100%;
  margin: auto;
}

.days-header,
.days {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
}

.days-header > span,
.days > span {
  position: relative;
  width: 45px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  color: black;
}

.days > span {
  height: 45px;
  border-radius: 8px;
}
.days > span:hover {
  background-color: #f7fcff;
  cursor: pointer;
}
.days-header {
  height: 30px;
  text-transform: uppercase;
  font-size: 12px;
  font-weight: 600;
}

.dots {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  width: 100%;
  bottom: 8px;
}

.dots div {
  display: inline-flex;
}

.dots .dot {
  display: inline-flex;
  height: 4px;
  width: 4px;
  margin: 0 2px;
  border-radius: 4px;
  background-color: red;
}

@media only screen and (max-width: 375px) {
  .days-header > span,
  .days > span {
    width: 40px;
  }
}
</style>
