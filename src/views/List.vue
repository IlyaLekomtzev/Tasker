<template>
  <div>
    <h1>List</h1>
    <hr>

    <table v-if="tasks.length">
      <thead>
        <tr>
          <th>#</th>
          <th>Title</th>
          <th>Date</th>
          <th>Descr</th>
          <th>Status</th>
          <th>Open</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(task, idx) of tasks"
          :key="task.id"
        >
          <td>{{idx + 1}}</td>
          <td>{{task.title}}</td>
          <td>{{new Date(task.date).toLocaleDateString()}}</td>
          <td class="max"><div class="text">{{task.descr}}</div></td>
          <td style="color: aquamarine;" v-if="task.status == 'active'">{{task.status}}</td>
          <td style="color: red;" v-else>{{task.status}}</td>
          <td>
            <router-link tag="button" class="btn btn-small" :to="'/task/' + task.id">
              Open
            </router-link>
          </td>
        </tr>
      </tbody>
    </table>
    <p v-else>No tasks</p>
  </div>
</template>

<script>
export default {
  computed: {
    tasks(){
      return this.$store.getters.tasks
    }
  },
}
</script>

<style lang="scss" scoped>
  .max{
    max-width: 400px;
  }
  .text{
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
  }
</style>