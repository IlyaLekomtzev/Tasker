<template>
  <div class="row">
    <div class="col s6 offset-s3">
      <h1>Create</h1>

      <form @submit.prevent="submitHandler">
        <div class="input-field">
          <input v-model="title" placeholder="Title" id="title" type="text" class="validate" required>
          <span class="helper-text" data-error="Title error"></span>
        </div>

        <div class="chips" ref="chips"></div>

        <div class="input-field">
          <textarea v-model="descr" id="descr" class="materialize-textarea" placeholder="Descreiption" required></textarea>
          <span class="character-counter" style="float: right; font-size: 12px;">{{descr.length}}/2048</span>
        </div>

        <input type="text" ref="datepicker">

        <button class="btn" type="submit">Create task</button>
      </form>
      
    </div>
  </div>
</template>

<script>
export default {
  name: 'create',
  data: () => ({
    descr: '',
    title: '',
    chips: null,
    date: null,
  }),
  mounted() {
    this.chips = M.Chips.init(this.$refs.chips, {
      placeholder: 'Task tags',
    })
    this.date = M.Datepicker.init(this.$refs.datepicker, {
      format: 'dd.mm.yyyy',
      defaultDate: new Date(),
      setDefaultDate: true
    })
  },
  methods: {
    submitHandler(){
      const task = {
        title: this.title,
        descr: this.descr,
        id: Date.now(),
        status: 'active',
        tags: this.chips.chipsData,
        date: this.date.date,
      }
      
      this.$store.dispatch('createTask', task)

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
