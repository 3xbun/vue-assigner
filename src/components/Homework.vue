<template>
  <section>
    <div class="homeworks">
      <div v-if="global.selectedDay == ''">
        <h1>Summary</h1>
        <div
          class="homework"
          v-for="work in Homeworks"
          :key="work.id"
          :style="{ backgroundColor: work.color }"
        >
          <p class="homework-title">{{ work.title }}</p>
          <p>
            Assigner:
            <span class="homework-assigner">{{ work.assigner }}</span>
          </p>
          <hr />
          <p>
            <i class="far fa-calendar-alt"></i>
            {{
              `${dayjs(work.assignDate).format("DD MMMM YYYY")} - ${dayjs(
                work.dueDate
              ).format("DD MMMM YYYY")} ( ${dayjs(work.dueDate).fromNow()} )`
            }}
          </p>
        </div>
      </div>
      <div v-else>
        <h1>Detail</h1>
        <div
          class="homework"
          v-for="work in global.workOfDay"
          :key="work.work.id"
          :style="{ backgroundColor: work.work.color }"
        >
          <p class="homework-title">{{ work.work.title }}</p>
          <p>
            Assigner:
            <span class="homework-assigner">{{ work.work.assigner }}</span>
          </p>
          <hr />
          <p>
            <i class="far fa-calendar-alt"></i>
            {{
              `${dayjs(work.work.assignDate).format("DD MMMM YYYY")} - ${dayjs(
                work.work.dueDate
              ).format("DD MMMM YYYY")} ( ${dayjs(
                work.work.dueDate
              ).fromNow()} )`
            }}
          </p>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import { inject, reactive } from "vue";
import Homeworks from "./Homeworks";

export default {
  name: "Homeworks",
  setup() {
    const dayjs = require("dayjs");
    const relativeTime = require("dayjs/plugin/relativeTime");
    dayjs.extend(relativeTime);

    const global = inject("global");

    const state = reactive({
      selectedHomework: {},
      homeworkInMonth: [],
    });

    return { Homeworks, state, dayjs, global };
  },
};
</script>

<style scoped>
.homeworks {
  margin-top: 3em;
  margin-bottom: 8em;
}

.homework {
  background-color: white;
  color: black;
  margin-bottom: 0.5em;
  border-radius: 0.5em;
  box-shadow: 0 5px 25px -5px rgba(0, 0, 0, 0.15);
  padding: 0.5em;
}

.homework-title {
  font-weight: bold;
}

.english {
  background-color: aqua;
}

.thai {
  background-color: green;
}

.math {
  background-color: red;
}

hr {
  margin: 0.5em 0;
  border: 1px solid rgba(0, 0, 0, 0.1);
}
</style>
