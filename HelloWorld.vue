<template>
  <div class="pojemnik">
    <h1>Formularz awari</h1>
    <div>
      <label for="textbox">Rodzaj awarii</label>
      <textarea name="rodzaj_awari" id="1" cols="30" rows="1"></textarea>
      <label class="priorytet" for="combobox1">Priorytet awari</label>
      <select id="2">
        <option value="">wybierz</option>
        <option value="Niski">Niski</option>
        <option value="Średni">Średni</option>
        <option value="Wysoki">Wysoki</option>
      </select>
    </div>

    <div class="okolicznosc">
      <label for="">Okoliczności zdarzenia</label>
      <textarea name="okolicznosci" id="3" cols="50" rows="5">
 Przykład: W trakcie pracy zaczął lecieć czarny dym z maszyny</textarea
      >
    </div>

    <div>
      <p>Czy można pracować na tym stanowisu pomimo awarii?</p>
      <input type="checkbox" id="4" value="Tak" name="wybur" />
      <label for="scales">Tak</label>
      <input type="checkbox" id="4" value="Nie" name="wybur" />
      <label for="horns">Nie</label>
    </div>

    <button @click="zgl">Zgłoś awarię</button>
    <button @click="zmien">Wypełnij Raport</button>
    <div v-for="(awaria, index) in awarie" :key="awaria">
      <p>
        {{ index + 1 }}. 
        <strong>Rodzaj awarii: </strong> {{ awaria.rodzaj_awari }} 
        <strong>Priorytet awari: </strong> {{ awaria.priorytet_awari }} 
        <strong>Okoliczności zdarzenia: </strong> {{ awaria.okoliczności }}
        <strong>Czy można pracować: </strong> {{ awaria.praca }}
      </p>
    </div>

    <div class="raport" v-show="x == 1">
      <h2>Raport</h2>
      <textarea name="" id="" cols="100" rows="10"></textarea>
      <button>Zatwierdź raport</button>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  name: "HelloWorld",
  setup() {
    class awaria {
      constructor(rodzaj_awari, priorytet_awari, okoliczności, praca) {
        this.rodzaj_awari = rodzaj_awari;
        this.priorytet_awari = priorytet_awari;
        this.okoliczności = okoliczności;
        this.praca = praca;
      }
    }

    const awarie = ref([]);

    function zgl() {
      var v = "";
      var f = document.getElementsByName("wybur");

      if (f[0].checked && f[1].checked) {
        alert("nie można wybrać 2");
        v = null;
      } else if (f[0].checked) {
        v = f[0].value;
      } else if (f[1].checked) {
        v = f[1].value;
      }
      if (!f[0].checked && !f[1].checked) {
        v = null;
      }

      const aw = new awaria(
        document.getElementById("1").value,
        document.getElementById("2").value,
        document.getElementById("3").value,
        v
      );
      awarie.value.push(aw);
    }

    const x = ref(0);

    function zmien() {
      if (x.value == 0) {
        x.value = 1;
      } else {
        x.value = 0;
      }
    }

    return { zmien, x, zgl, awarie };
  },
};
</script>

<style lang="scss">
body {
  background-color: rgb(120, 132, 223);
}

.priorytet {
  margin-left: 30px;
}

.pojemnik {
  background-color: white;
  margin-left: auto;
  margin-right: auto;
  max-width: 960px;
  padding: 20px 40px;
}

label {
  padding-right: 10px;
}

div {
  margin-bottom: 40px;
}

.okolicznosc {
  display: flex;
  align-items: flex-start;
}

button {
  margin-right: 5%;
}

.raport button {
  display: block;
}

textarea {
  resize: none;
}
</style>

