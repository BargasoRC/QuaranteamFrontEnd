<template>
  <div style="padding: 1%; margin-top: 3%;">
    <v-item-group>
      <h1
        class="text-center"
        v-bind:style=" loading ? 'font-style: italic' : 'font-style: none'"
      >{{loading ? "Shuffling task..." : text}}</h1>
      <v-container>
        <v-row>
          <v-col v-for="task in tasks" :key="task.tasking" cols="12" md="4">
            <v-item>
              <v-skeleton-loader
                :loading="loading"
                :transition="transition"
                height="200"
                type="card"
              >
                <TaskingCard :TaskDetails="task" v-show="!loading"></TaskingCard>
              </v-skeleton-loader>
            </v-item>
          </v-col>
        </v-row>
      </v-container>
      <div class="text-center" style="padding: 2%;">
        <v-pagination
          v-model="page"
          :length="4"
          prev-icon="mdi-menu-left"
          next-icon="mdi-menu-right"
          v-show="!loading"
        ></v-pagination>
      </div>
    </v-item-group>
    <AddTask/>
    <Buttons/>
  </div>
</template>

<script>
import TaskingCard from "@/components/Cards/TaskingCard.vue";
import AddTask from "@/components/Modals/AddTask.vue";
import Buttons from "@/components/Buttons/FabSpeedDial.vue";

export default {
  name: "studenttasking",
  data() {
    return {
      page: 3,
      loading: false,
      transition: "fab-transition",
      text: "Center 1 Tasking"
    };
  },
  computed: {
    tasks() {
      return this.$store.state.tasks_center1;
    },
    testShuffle() {
      var temp_list = [];
      this.$store.state.tasks_center1.forEach(element => {
        temp_list.push(element.members);
      });
      return temp_list;
    }
  },
  components: {
    TaskingCard,
    AddTask,
    Buttons
  },
  mounted() {
    this.$bus.$on("shuffle", isShuffle => {
      this.loading = isShuffle;
      setTimeout(() => {
        this.loading = !isShuffle;
        this.testShuffle.forEach(temp => {
          this.ShuffleTask(temp);
        });
      }, 4000);
    });
  },
  methods: {
    ShuffleTask(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
    }
  }
};
</script>

