<template>
  <div>
    <div id="gantt-header" class="h-12 p-2 flex items-center">
      <h1 class="text-xl font-bold">ガントチャート</h1>
    </div>
    <div id="gantt-content" class="flex">
      <div id="gantt-task">
        <div
          id="gantt-task-title"
          class="flex items-center bg-green-600 text-white h-20"
          ref="task"
        >
          <div
            class="border-t border-r border-b flex items-center justify-center font-bold text-xs w-48 h-full"
          >
            タスク
          </div>
          <div
            class="border-t border-r border-b flex items-center justify-center font-bold text-xs w-24 h-full"
          >
            開始日
          </div>
          <div
            class="border-t border-r border-b flex items-center justify-center font-bold text-xs w-24 h-full"
          >
            完了期限日
          </div>
          <div
            class="border-t border-r border-b flex items-center justify-center font-bold text-xs w-16 h-full"
          >
            担当
          </div>
          <div
            class="border-t border-r border-b flex items-center justify-center font-bold text-xs w-12 h-full"
          >
            進捗
          </div>
        </div>
      </div>
      <div id="gantt-calendar" class="overflow-x-scroll w-1/2">
        <div id="gantt-date" class="h-20">
          <div id="gantt-year-month" class="relative h-8">
            <div v-for="(calendar, index) in calendars" :key="index">
              <div
                class="bg-indigo-700 text-white border-b border-r border-t h-8 absolute font-bold text-sm flex items-center justify-center"
                :style="`width:${calendar.calendar * block_size}px;left:${
                  calendar.start_block_number * block_size
                }px`"
              >
                {{ calendar.date }}
              </div>
            </div>
          </div>
          <div id="gantt-day" class="relative h-12">
            <div v-for="(calendar, index) in calendars" :key="index">
              <div v-for="(day, index) in calendar.days" :key="index">
                <div
                  class="border-r h-12 absolute flex items-center justify-center flex-col font-bold text-xs"
                  :style="`width:${block_size}px;left:${
                    day.block_number * block_size
                  }px`"
                >
                  <span>{{ day.day }}</span>
                  <span>{{ day.dayOfWeek }}</span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import moment from "moment";

export default defineComponent({
  name: "ProgressManagement",
  data() {
    return {
      start_month: "2020-10",
      end_month: "2021-02",
      block_size: 30,
      block_number: 0,
      calendars: [{}],
      inner_width: 0,
      inner_height: 0,
      task_width: 0,
      task_height: 0,
    };
  },
  mounted() {
    this.getCalendar();
    this.getWindowSize();
    window.addEventListener("resize", this.getWindowSize);
  },
  methods: {
    getDays(year: string, month: string, block_number: number) {
      const dayOfWeek = ["日", "月", "火", "水", "木", "金", "土"];
      let days = [];
      let date = moment(`${year}-${month}-01`);
      let num = date.daysInMonth();
      for (let i = 0; i < num; i++) {
        days.push({
          day: date.date(),
          dayOfWeek: dayOfWeek[date.day()],
          block_number,
        });
        date.add(1, "day");
        block_number++;
      }
      return days;
    },
    getCalendar() {
      let block_number = 0;
      let days;
      let start_month = moment(this.start_month);
      let end_month = moment(this.end_month);
      let between_month = end_month.diff(start_month, "months");
      for (let i = 0; i <= between_month; i++) {
        days = this.getDays(
          start_month.year() as any,
          start_month.format("MM"),
          block_number
        );
        this.calendars.push({
          date: start_month.format("YYYY年MM月"),
          year: start_month.year(),
          month: start_month.month(), //month(), 0,1..11と表示
          start_block_number: block_number,
          calendar: days.length,
          days: days,
        });
        start_month.add(1, "months");
        block_number = days[days.length - 1].block_number;
        block_number++;
      }
      return block_number;
    },
    getWindowSize() {
      this.inner_width = window.innerWidth;
      this.inner_height = window.innerHeight;
      console.log(this.$refs.task);
      // this.task_width = this.$refs.task.offsetWidth;
      // this.task_height = this.$refs.task.offsetHeight;
    },
  },
});
</script>
