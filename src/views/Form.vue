<template>
  <div class="container mt-2">
    <b-form>
      <b-form-group label="Título" label-for="subject">
        <b-form-input id="subject" v-model="form.subject" type="text" placeholder="Ex: análise de contatos" required
          autocomplete="off">
        </b-form-input>
      </b-form-group>

      <b-form-textarea label="Descrição da tarefa" label-for="description">
        <b-form-input id="description" v-model="form.subject" type="text"
          placeholder="Ex: preciso analisar novos contatos" required autocomplete="off">
        </b-form-input>
      </b-form-textarea>

      <b-button type="submit" variant="outline-primary" @click="saveTask"> Salvar </b-button>
    </b-form>
  </div>
</template>

<script>
import ToastMixin from "@/mixins/toastMixin.js";

export default {
  name: "Form",

  mixins: [ToastMixin],

  data() {
    return {
      form: {
        subject: "",
        description: ""
      },
      methodSave: "new",

    }
  },

  created() {
    if (this.$route.params.index === 0 || this.$route.params.index !== undefined) {
      this.methodSave = "update";
      let tasks = JSON.parse(localStorage.getItem("tasks"));
      this.form = tasks[this.$route.params.index];
    }
  },

  methods: {
    saveTask() {
      if (this.methodSave === "update") {
        let tasks = JSON.parse(localStorage.getItem("tasks"));
        tasks[this.$route.params.index] = this.form;
        localStorage.setItem("tasks", JSON.stringify(tasks));
        this.showToast("success", "Sucesso!", "Tarefa atualizada com suceso");
        this.$router.push({ name: "list" });
        return;
      }

      let tasks = (localStorage.getItem("tasks")) ? JSON.parse(localStorage.getItem("tasks")) : [];
      tasks.push(this.form);
      localStorage.setItem("tasks", JSON.stringify(tasks));
      this.showToast("success", "Sucesso!", "Tarefa criada com suceso");
      this.$router.push({ name: "list" });
    }
  }
}
</script>
