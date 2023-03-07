<template>
  <div class="container">
    <div class="buttons">
      <button @click="showForm">Dodaj zgłoszenie</button>
      <button @click="showList">Lista zgłoszeń</button>
    </div>

    <div v-if="showingForm">
      <h2>Formularz awarii</h2>
      <form @submit.prevent="submitForm">
        <div class="form-group">
          <label for="type">Rodzaj zgłoszenia:</label>
          <input type="text" id="type" name="type" v-model="type">
        </div>
        <div class="form-group">
          <label for="priority">Priorytet:</label>
          <div>
            <label>
              <input type="radio" name="priority" value="high" v-model="priority">
              Wysoki
            </label>
            <label>
              <input type="radio" name="priority" value="medium" v-model="priority">
              Średni
            </label>
            <label>
              <input type="radio" name="priority" value="low" v-model="priority">
              Niski
            </label>
          </div>
        </div>

        <div class="form-group">
          <label for="continueWorking">Czy można kontynuować pracę:</label>
          <div>
            <label>
              <input type="radio" name="continueWorking" value="yes" v-model="continueWorking">
              Tak
            </label>
            <label>
              <input type="radio" name="continueWorking" value="no" v-model="continueWorking">
              Nie
            </label>
          </div>
        </div>

        <div class="form-group">
          <label for="circumstances">Okoliczności zdarzenia:</label>
          <textarea id="circumstances" v-model="circumstances"></textarea>
        </div>

        <button type="submit">Wyślij zgłoszenie</button>
      </form>
    </div>

    <div v-if="showingList">
      <h2>Lista zgłoszeń</h2>
      <ul>
        <li v-for="report in reports" :key="report.id" @click="showDetails(report)">
          Rodzaj zgłoszenia: {{ report.type }} | Priorytet: {{ report.priority }} | Czy można kontynuować pracę: {{ report.continueWorking }} | Okoliczności zdarzenia: {{ report.circumstances }}
        </li>
      </ul>

      <div v-if="showingDetails">
        <h3>{{ selectedReport.priority }} - {{ selectedReport.circumstances }}</h3>
        <div class="form-group">
          <label for="report">Raport:</label>
          <textarea id="report" v-model="report"></textarea>
        </div>

        <button @click="closeReport">Dodaj raport i zamknij zgłoszenie</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      showingForm: false,
      showingList: false,
      reports: [],
      type: '',
      priority: '',
      continueWorking: '',
      circumstances: '',
      selectedReport: null,
      report: ''
    };
  },
  methods: {
    showForm() {
      this.showingForm = true;
      this.showingList = false;
    },
    showList() {
      this.showingForm = false;
      this.showingList = true;
    },
    submitForm() {
      const newReport = {
        id: this.reports.length + 1,
        type: this.type,
        priority: this.priority,
        continueWorking: this.continueWorking,
        circumstances: this.circumstances
      };
      this.reports.push(newReport);
      this.type = '',
      this.showingForm = false;
      this.showingList = true;
      this.priority = '';
      this.continueWorking = '';
      this.circumstances = '';
    },
    showDetails(report) {
      this.selectedReport = report;
      this.report = '';
      this.showingDetails = false;
      this.showingDetails = true;
    },
    closeReport() {
      const reportIndex = this.reports.findIndex(report => report.id === this.selectedReport.id);
      this.reports[reportIndex].report = this.report;
      this.selectedReport = null;
      this.report = '';
      this.showingDetails = false;
    }
  }
};
</script>

<style>
.container {
  max-width: 800px;
  margin: 0 auto;
}

.buttons {
  display: flex;
  justify-content: space-between;
  margin-bottom: 20px;
}

.form-group {
  margin-bottom: 20px;
}

label {
  display: block;
  margin-bottom: 5px;
}

textarea {
  width: 100%;
  height: 100px;
}
</style>