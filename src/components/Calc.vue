<template>
  <v-app>
    <Home></Home>
      <main>
        <v-container>
          <v-card elevation="5">
            <v-card-title>
              <div>
                <v-sheet class="d-flex mb-6">
                  <v-sheet class="ma-2 pa-2 me-auto">
                    <h1>Calculadora de Água Diária</h1>
                  </v-sheet>
                </v-sheet>
              </div>
            </v-card-title>

            <v-container>
              <v-form v-model="valid" @submit.prevent="calcularAgua">
                <v-row>
                  <v-col cols="4">
                    <v-text-field
                      variant="solo-filled"
                      density="comfortable"
                      label="Peso Corporal (kg)"
                      v-model="peso"
                      :rules="[v => !!v || 'Peso é obrigatório']"
                      required
                    ></v-text-field>
                  </v-col>

                  <v-col cols="4">
                    <v-text-field
                      variant="solo-filled"
                      density="comfortable"
                      label="Altura (cm)"
                      v-model="altura"
                      :rules="[v => !!v || 'Altura é obrigatória']"
                      required
                    ></v-text-field>
                  </v-col>

                  <v-col cols="4">
                    <v-text-field
                      variant="solo-filled"
                      density="comfortable"
                      label="Idade (anos)"
                      v-model="idade"
                      :rules="[v => !!v || 'Idade é obrigatória']"
                      required
                    ></v-text-field>
                  </v-col>
                </v-row>

                <v-row>
                  <v-col cols="4">
                    <v-select
                      multiple
                      variant="solo-filled"
                      clearable
                      chips
                      density="comfortable"
                      label="Sexo"
                      v-model="sexo"
                      :items="['Masculino', 'Feminino']"
                      :rules="[v => !!v || 'Sexo é obrigatório']"
                      required
                    ></v-select>
                  </v-col>

                  <v-col cols="4">
                    <v-select
                      multiple
                      variant="solo-filled"
                      clearable
                      chips
                      density="comfortable"
                      label="Tipo de Atividade Física"
                      v-model="tipoAtividade"
                      :items="['Nenhuma', 'Musculação', 'Aeróbica']"
                      :rules="[v => !!v || 'Tipo de atividade é obrigatório']"
                      required
                    ></v-select>
                  </v-col>

                  <v-col cols="4">
                    <v-text-field
                      variant="solo-filled"
                      density="comfortable"
                      label="Horas de Atividade Física por Dia"
                      v-model="horasAtividade"
                      type="number"
                      required
                    ></v-text-field>
                  </v-col>
                </v-row>

                <v-row>
                  <v-col cols="4">
                    <v-select
                      multiple
                      variant="solo-filled"
                      clearable
                      chips
                      density="comfortable"
                      label="Clima Quente ou Úmido?"
                      v-model="clima"
                      :items="['Sim', 'Não']"
                      required
                    ></v-select>
                  </v-col>

                  <v-col  cols="4">
                    <v-select
                      multiple
                      variant="solo-filled"
                      clearable
                      chips
                      density="comfortable"
                      label="Consome Cafeína?"
                      v-model="cafeina"
                      :items="['Sim', 'Não']"
                      required
                    ></v-select>
                  </v-col>

                  <v-col cols="4">
                    <v-select
                      multiple
                      variant="solo-filled"
                      clearable
                      chips
                      density="comfortable"
                      label="Consome Álcool?"
                      v-model="alcool"
                      :items="['Sim', 'Não']"
                      required
                    ></v-select>
                  </v-col>
                </v-row>

                <v-row>
                  <v-col>
                    <v-text-field
                      variant="solo-filled"
                      density="comfortable"
                      label="Hora que Acordou"
                      v-model="horaAcordar"
                      type="time"
                      required
                    ></v-text-field>
                  </v-col>

                  <v-col>
                    <v-text-field
                      variant="solo-filled"
                      density="comfortable"
                      label="Hora que Vai Dormir"
                      v-model="horaDormir"
                      type="time"
                      required
                    ></v-text-field>
                  </v-col>
                </v-row>
                <v-sheet class="mr-8">
                  <div>
                    <div class="d-flex flex-row-reverse mr-n11">
                      <v-sheet class="ma-2 pa-2">
                        <v-btn color="primary" @click="calcularAgua">Calcular</v-btn>
                      </v-sheet>

                      <v-sheet class=" mr-n3 ma-2 pa-2">
                        <v-btn color="red" @click="irParaExplicacao">Ir para Explicação</v-btn>
                      </v-sheet>
                    </div>
                  </div>
                </v-sheet>

              </v-form>
            </v-container>

            <v-card v-if="resultado" class="container mt-5">
              <v-hover>
                <template v-slot:default="{ isHovering, props }">
                  <v-card
                    v-bind="props"
                    :color="isHovering ? 'primary' : undefined"
                    title="Resultado:"
                  >
                    <p>Total de água a ser consumida: {{ resultado.totalAgua }} litros</p>
                    <p>Número de copos: {{ resultado.numeroCopos }}</p>
                    <p>Quantidade de ml por copo: {{ resultado.mlPorCopo }} ml</p>
                    <p>Você deve beber 1 copo a cada {{ resultado.intervaloPorCopo }} horas.</p>
                    <p>Horários sugeridos para beber água:</p>
                    <ul>
                      <li v-for="(horario, index) in resultado.horarios" :key="index">{{ horario }}</li>
                    </ul>
                  </v-card>
                </template>
              </v-hover>
            </v-card>
          </v-card>
        </v-container>
      </main>
    </v-app>
</template>

<script>
import axios from 'axios';
import Home from "@/components/Home.vue";

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: 'Calc',
  components: {Home},
  data() {
    return {
      valid: false,
      peso: null,
      altura: null,
      idade: null,
      sexo: null,
      tipoAtividade: null,
      horasAtividade: 0,
      clima: null,
      cafeina: null,
      alcool: null,
      horaAcordar: null,
      horaDormir: null,
      resultado: null,
    };
  },
  methods: {
    calcularAgua() {
      if (!this.peso || !this.altura || !this.horaAcordar || !this.horaDormir) {
        alert('Por favor, preencha todos os campos.');
        return;
      }

      axios.post('http://localhost:8003/api/calcular-agua', {
        peso: this.peso,
        altura: this.altura,
        idade: this.idade,
        sexo: this.sexo,
        tipoAtividade: this.tipoAtividade,
        horasAtividade: this.horasAtividade,
        clima: this.clima,
        cafeina: this.cafeina,
        alcool: this.alcool,
        horaAcordar: this.horaAcordar,
        horaDormir: this.horaDormir
      }).then(response => {
        this.resultado = response.data;
      }).catch(error => {
        console.error(error);
        alert('Ocorr  eu um erro ao calcular a ingestão de água.');
      });
    },
    irParaExplicacao() {
      this.$router.push({ name: 'Explicacao' });
    }
  }
};
</script>

<style>
body {
  background-color: #8c8b8b;
}
</style>
