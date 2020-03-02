<template>
  <div class="row">
    <div v-if="task" class="col s6 offset-s3">
      <h1>{{task.title}}</h1>

      <div v-if="task.status != 'completed'">
        <form @submit.prevent="submitHandler">
          <div class="chips" ref="chips"></div>

          <div class="input-field">
            <textarea v-model="descr" id="descr" class="materialize-textarea" required></textarea>
            <span class="character-counter" style="float: right; font-size: 12px;">{{descr.length}}/2048</span>
          </div>

          <input type="text" ref="datepicker">

          <button class="btn" type="submit">Update</button>
          <button class="btn blue darken-3" type="submit" @click="completeTask">Complete</button>
        </form>
      </div>
      <div v-else>
        <h3>{{new Date(task.date).toLocaleDateString()}}</h3>
        <hr>
        <h4>{{task.status}}</h4>
        <hr>
        <h5 v-for="tag of task.tags">{{tag.tag}}</h5>
        <hr>
        <p>{{task.descr}}</p>
      </div>
    </div>
    <div v-else>Not found</div>
  </div>
</template>

<script>
export default {
  computed: {
    task(){
      return this.$store.getters.taskById(+this.$route.params.id)
    }
  },
  data: () => ({
    descr: '',
    chips: null,
    date: null,
  }),
  mounted() {
    this.descr = this.task.descr
    this.chips = M.Chips.init(this.$refs.chips, {
      placeholder: 'Task tags',
      data: this.task.tags
    })
    this.date = M.Datepicker.init(this.$refs.datepicker, {
      format: 'dd.mm.yyyy',
      defaultDate: new Date(this.task.date),
      setDefaultDate: true
    })
  },
  methods: {
    submitHandler(){  
      this.$store.dispatch('updateTask', {
        id: this.task.id,
        descr: this.descr,
        date: this.date.date
      })
      this.$router.push('/list')
    },
    completeTask(){
      this.$store.dispatch('completeTask', this.task.id)
      this.$router.push('/list')
    }
  },
  destroyed() {
    if(this.date && this.date.destroy){
      this.date.destroy()
    }

    if(this.chips && this.chips.destroy){
      this.chips.destroy()
    }
  },
}
</script>

<style lang="scss" scoped>
    
</style>