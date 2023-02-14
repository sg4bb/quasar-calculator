<template>
  <q-page padding>
    <div class="row justify-center q-pt-xl">
      <div class="col-12 col-md-8">
        <q-card>
          <q-card-section class="bg-primary text-white">
            <div class="row text-h6 text-weight-bold">
              <div class="col">Calculadora</div>
              <div class="column-reverse">
                <q-btn icon="history" flat @click="expandedHistory = !expandedHistory"></q-btn>
              </div>
            </div>
          </q-card-section>

          <q-card-section>
            <div class="text-h5 text-grey-5 text-right">
              {{ acumulador + actual }}
            </div>
            <div class="text-h3 text-right">
              {{ resultado }}
            </div>
          </q-card-section>

          <q-card-section class="bg-grey-4">
            <div class="row q-col-gutter-sm">
              <div class="col-3" v-for="(btn, index) in botones" :key="index">
                <q-btn
                  class="full-width text-h6"
                  :color="noEsNumero(btn) ? 'indigo' : 'grey-2'"
                  :text-color="noEsNumero(btn) ? 'white' : 'grey-8'"
                  @click="btnAccion(btn)"
                >
                  {{ btn }}
                </q-btn>
              </div>
              <div class="col-6">
                <q-btn
                  class="full-width text-h6"
                  color="indigo"
                  @click="btnReiniciar"
                  label="Reset"
                >
                </q-btn>
              </div>
              <div class="col-6">
                <q-btn
                  class="full-width text-h6"
                  color="orange"
                  label="="
                  @click="btnResultado"
                >
                </q-btn>
              </div>
            </div>
          </q-card-section>
        </q-card>
      </div>
    </div>

    <q-dialog v-model="expandedHistory" position="bottom">

      <q-card style="width: 350px">

        <q-card-section class="row items-center no-wrap">
          <div>
            <div class=" text-weight-bold">Historial</div>
          </div>
        </q-card-section>

        <q-separator color="primary" inset />

        <q-card-section>
          <div class="text-black" v-for="(result, index) in arrayResultado" :key="index">
            <div class="text-grey-9" style="font-size: 14px;">{{ result }}</div>
          </div>
        </q-card-section>

      </q-card>
    </q-dialog>
  </q-page>
</template>

<script setup>
import { ref } from "vue";
import { evaluate, round } from "mathjs";

import { useQuasar } from "quasar";

const noEsNumero = (valor) => isNaN(valor);
const actual = ref("");
const acumulador = ref("");
const resultado = ref("");
const operadorClick = ref(true);
const arrayResultado = ref([]);
const expandedHistory = ref(false);

const $q = useQuasar();

const botones = [7, 8, 9, "%", 4, 5, 6, "+", 1, 2, 3, "-", ".", 0, "/", "*"];

const btnAccion = (valor) => {
  if (!noEsNumero(valor)) {
    if (operadorClick.value) {
      actual.value = "";
      operadorClick.value = false;
    }
    actual.value = `${actual.value}${valor}`;
  } else {
    ejecutarOperacion(valor);
  }
};

const ejecutarOperacion = (valor) => {
  if (valor === ".") {
    if (actual.value.indexOf(".") === -1) {
      actual.value = `${actual.value}${valor}`;
    }
    return;
  }

  if (valor === "%") {
    if (actual.value !== "") {
      actual.value = `${parseFloat(actual.value) / 100}`;
    }

    return;
  }

  agregamosOperador(valor);
};

const agregamosOperador = (valor) => {
  if (!operadorClick.value) {
    acumulador.value += `${actual.value} ${valor} `;
    actual.value = "";
    operadorClick.value = true;
  }
};

const btnReiniciar = () => {
  actual.value = "";
  acumulador.value = "";
  resultado.value = "";
  operadorClick.value = true;
};

const btnResultado = () => {
  if (!operadorClick.value) {
    resultado.value = evaluate(acumulador.value + actual.value);
    arrayResultado.value.push(
      `${acumulador.value} ${actual.value} = ${resultado.value}`
    );
  } else {
    $q.notify({
      message: "Error, ingresa un numero",
      color: "negative",
      icon: "error",
    });
  }
};
</script>

<style scoped>
.text-h5 {
  height: 23px;
}

.text-h3 {
  margin-top: 15px;
  height: 40px;
}
</style>
