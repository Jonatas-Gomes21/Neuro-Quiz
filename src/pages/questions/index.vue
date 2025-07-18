<template>
  <div
    v-if="currentQuestion"
    class="container d-flex flex-column align-items-center justify-content-center min-vh-100 bg-light text-center p-0"
  >
 
    <div class="logo-image mt-5">
      <img src="/src/assets/img/Logo Secundaria.svg" alt="Logo" height="90" />
    </div>
    
    <div class="card m-0 p-0 align-items-center mt-5 mb-4">
      <div class="card-body-title p-2 rounded-3 fw-bolder text-white">
        {{ currentQuestion.statement }}
      </div>
    </div>
    
    <div class="container align-items-center d-flex flex-column" style="max-width: 400px;">
      <label
      v-for="alt in currentQuestion.alternatives"
      :key="alt.id"
      class="option-box p-3 mb-3 rounded-3 fw-semibold d-block text-center"
      :class="{ 'option-select bg-success text-white': selected === alt.id }"
      @click="selected = alt.id"
      style="cursor: pointer;"
      >
      <input
      type="radio"
      :name="'question-' + currentQuestion.id"
      class="form-check-input me-2"
      :value="alt.id"
      v-model="selected"
      hidden
      />
      <span class="fw-bold me-2">{{ alt.id }}</span> {{ alt.text }}
    </label>
  </div>

  <div class="bar-progress mt-2">
    <div class="w-100 mt-2 mb-3" style="max-width: 500px;">
  <div class="position-relative w-100 mb-2">
<div class="progress" style="height: 8px; border-radius: 5px;">
  <div
    class="progress-bar bg-purple"
    role="progressbar"
    :style="{ width: progressPercent + '%' }"
    :aria-valuenow="progressPercent"
    aria-valuemin="0"
    aria-valuemax="100"
  ></div>
</div>
<div
  class="number position-absolute"
  style="font-size: 14px; font-weight: 500; color: #666;"
>
  {{ progressPercent }}%
</div>
  </div>
</div>
  </div>
  

    <button
      class="btn btn-primary mb-5 mt-2"
      @click="nextQuestion"
      :disabled="!selected"
    >
      Próxima pergunta
    </button>
    <p
      class="text-muted text-decoration-underline"
      style="cursor: pointer;"
      @click="skipQuestion"
      >
        Pular
    </p>

  </div>
  

  
</template>

<script setup>
import { ref, onMounted, computed } from 'vue'

const personalityCount = ref({
  Fleumatico: 0,
  Sanguineo: 0,
  Colerico: 0,
  Melancolico: 0
})

const questions = ref([])
const currentIndex = ref(0)
const currentQuestion = ref(null)
const selected = ref(null)
import { useRouter } from 'vue-router'

const router = useRouter()

function navigateToResult() {
  const result = Object.entries(personalityCount.value).reduce((a, b) => 
    a[1] > b[1] ? a : b
  )[0]

  router.push({ name: 'result', query: { result } })
}



const progressPercent = computed(() => {
  if (questions.value.length === 0) return 0
  return Math.round((currentIndex.value / questions.value.length) * 100)
})  



onMounted(() => {
  const data = [
      {
          "id": 1,
          "statement": "Como você se sente ao participar de atividades em grupo?",
          "alternatives": [
              {"id":"a", "text":"Prefiro observar e colaborar de forma discreta.", "personality":"Fleumático"},
              {"id":"b", "text":"Gosto de conversar e me divertir durante as atividades.", "personality":"Sanguíneo"},
              {"id":"c", "text":"Me sinto motivado a liderar ou tomar decisões.", "personality":"Colérico"},
              {"id":"d", "text":"Fico mais confortável com tarefas individuais.", "personality":"Melancólico"}
          ]
      },
      {
          "id": 2,
          "statement": "Quando precisa começar uma nova tarefa, você:",
          "alternatives": [
              {"id":"a", "text":"Espera instruções claras e começa com calma.", "personality":"Fleumático"},
              {"id":"b", "text":"Começa com entusiasmo, mesmo sem ter todos os detalhes.", "personality":"Sanguíneo"},
              {"id":"c", "text":"Toma a frente e inicia o trabalho imediatamente.", "personality":"Colérico"},
              {"id":"d", "text":"Analisa bem antes de dar o primeiro passo.", "personality":"Melancólico"}
          ]
      },
      {
          "id": 3,
          "statement": "Quando você entra em uma nova turma na faculdade, sua reação mais comum é:",
          "alternatives": [
              {"id":"a", "text":"Já puxo papo com alguém, adoro conhecer gente nova!", "personality":"Sanguíneo"},
              {"id":"b", "text":"Observo quem pode ser útil em trabalhos e começo a me aproximar estrategicamente.", "personality":"Colérico"},
              {"id":"c", "text":"Fico mais na minha, levo um tempo para me sentir à vontade.", "personality":"Melancólico"},
              {"id":"d", "text":"Espero que o ambiente se acalme e só interajo se for necessário. ", "personality":"Fleumático"}
          ]
      },
      {
          "id": 4,
          "statement": "Você está sobrecarregado com prazos e provas. Sua reação:",
          "alternatives": [
              {"id":"a", "text":"Tenta relaxar vendo algo ou saindo com amigos, mesmo sabendo que deveria estudar.", "personality":"Sanguíneo"},
              {"id":"b", "text":"Organiza sua agenda e encara tudo com intensidade. Nada vai te parar.", "personality":"Colérico"},
              {"id":"c", "text":"Sente o peso das cobranças e fica ansioso com medo de não dar conta.", "personality":"Melancólico"},
              {"id":"d", "text":"Faz o possível, mas sem desespero. Acha que tudo se resolve com paciência.", "personality":"Fleumático"}
          ]
      },
      {
          "id": 5,
          "statement": "Quando há uma discussão no grupo de estudos, você tende a:",
          "alternatives": [
              {"id":"a", "text":"Intervém tentando descontrair e mudar o assunto.", "personality":"Sanguíneo"},
              {"id":"b", "text":"Toma uma posição firme e tenta resolver de forma direta.", "personality":"Colérico"},
              {"id":"c", "text":"Fica desconfortável e evita se envolver, mas pensa muito sobre aquilo depois..", "personality":"Melancólico"},
              {"id":"d", "text":"Escuta todos os lados e sugere um meio-termo para acalmar a situação.", "personality":"Fleumático"}
          ]
      },
      {
          "id": 6,
          "statement": "Em uma apresentação de trabalho, você:",
          "alternatives": [
              {"id":"a", "text":"Gosta de falar, se expressa com entusiasmo e até faz piadas.", "personality":"Sanguíneo"},
              {"id":"b", "text":"Usa a oportunidade para se destacar e impressionar.", "personality":"Colérico"},
              {"id":"c", "text":"Fica nervoso e ensaia bastante para se sentir seguro.", "personality":"Melancólico"},
              {"id":"d", "text":"Prefere falar por último ou deixar que outros apresentem.", "personality":"Fleumático"}
          ]
      },
      {
          "id": 7,
          "statement": "Quando o professor é rude ao corrigir sua dúvida ou trabalho, você:",
          "alternatives": [
              {"id":"a", "text":"Fica indignado, comenta com os colegas, mas depois esquece o assunto.", "personality":"Sanguíneo"},
              {"id":"b", "text":"Responde na hora ou guarda aquilo como motivação para se superar.", "personality":"Colérico"},
              {"id":"c", "text":"Leva para o lado pessoal e fica remoendo a situação por dias.", "personality":"Melancólico"},
              {"id":"d", "text":"Tenta entender o lado do professor e evita confrontos.", "personality":"Fleumático"}
          ]
      },
      {
          "id": 8,
          "statement": "Quando pensa sobre seu futuro na carreira, você:",
          "alternatives": [
              {"id":"a", "text":"Tem muitas ideias, mas nem sempre foca em uma só.", "personality":"Sanguíneo"},
              {"id":"b", "text":"Já tem metas claras e faz de tudo para atingi-las.", "personality":"Colérico"},
              {"id":"c", "text":"Se preocupa com as incertezas e prefere caminhos mais seguros.", "personality":"Melancólico"},
              {"id":"d", "text":"Vai deixando rolar e toma decisões com calma, conforme as coisas se mostram.", "personality":"Fleumático"}
          ]
      },
      {
          "id": 9,
          "statement": "Em relação a mudanças inesperadas no conteúdo ou formato das aulas, você:",
          "alternatives": [
              {"id":"a", "text":"Aceita, mas prefere que não seja com frequência.", "personality":"Fleumático"},
              {"id":"b", "text":"Gosta, pois traz novidade.", "personality":"Sanguíneo"},
              {"id":"c", "text":"Se adapta rapidamente e assume o controle.", "personality":"Colérico"},
              {"id":"d", "text":"Fica desconfortável, pois precisa de previsibilidade.", "personality":"Melancólico"}
          ]
      },
      {
          "id": 10,
          "statement": "Quando você está em aula, o que mais chama sua atenção?",
          "alternatives": [
              {"id":"a", "text":"A organização e a clareza do conteúdo.", "personality":"Fleumático"},
              {"id":"b", "text":"A dinâmica e o envolvimento com os colegas.", "personality":"Sanguíneo"},
              {"id":"c", "text":"Os desafios e a chance de se destacar.", "personality":"Colérico"},
              {"id":"d", "text":"Os detalhes e a profundidade dos temas abordados.", "personality":"Melancólico"}
          ]
      },
      {
          "id": 11,
          "statement": "Quando um colega erra durante uma apresentação, você normalmente:",
          "alternatives": [
              {"id":"a", "text":"Pensa em como pode ajudar sem chamar atenção.", "personality":"Fleumático"},
              {"id":"b", "text":"Ri, mas depois tenta animá-lo.", "personality":"Sanguíneo"},
              {"id":"c", "text":"Corrige ou orienta na hora se puder. ", "personality":"Colérico"},
              {"id":"d", "text":"Fica nervoso só de pensar que poderia ser você.", "personality":"Melancólico"}
          ]
      },
      {
          "id": 12,
          "statement": "Quando está em um projeto longo, você:",
          "alternatives": [
              {"id":"a", "text":"Prefere manter o ritmo constante até o final.", "personality":"Fleumático"},
              {"id":"b", "text":"Se motiva com novidades no meio do caminho.", "personality":"Sanguíneo"},
              {"id":"c", "text":"Fica focado em alcançar os resultados.", "personality":"Colérico"},
              {"id":"d", "text":"Revisa tudo com cuidado para garantir perfeição.", "personality":"Melancólico"}
          ]
      },
      {
          "id": 13,
          "statement": "Em um evento ou aula mais movimentada, você:",
          "alternatives": [
              {"id":"a", "text":"Observa, participa discretamente.", "personality":"Fleumático"},
              {"id":"b", "text":"Interage, conversa e se diverte.", "personality":"Sanguíneo"},
              {"id":"c", "text":"Lidera ou organiza a atividade.", "personality":"Colérico"},
              {"id":"d", "text":"Se sente sobrecarregado e prefere ambientes calmos.", "personality":"Melancólico"}
          ]
      },
      {
          "id": 14,
          "statement": "Como você se sente em atividades com prazos curtos e pressão?",
          "alternatives": [
              {"id":"a", "text":"Trabalha bem, desde que tudo esteja organizado.", "personality":"Fleumático"},
              {"id":"b", "text":"Se atrapalha um pouco, mas dá um jeito.", "personality":"Sanguíneo"},
              {"id":"c", "text":"Se motiva com o desafio e entrega rápido.", "personality":"Colérico"},
              {"id":"d", "text":"Fica estressado e teme não conseguir fazer perfeito.", "personality":"Melancólico"}
          ]
      },
      {
          "id": 15,
          "statement": "Um professor tem uma didática confusa e as aulas não fazem sentido. Você:",
          "alternatives": [
              {"id":"a", "text":"Reclama com os colegas e até ri da situação, tentando levar na esportiva.", "personality":"Sanguíneo"},
              {"id":"b", "text":"Vai direto falar com o professor ou coordenação para exigir uma solução.", "personality":"Colérico"},
              {"id":"c", "text":"Fica frustrado e tenta aprender tudo sozinho, mesmo que isso te desgaste.", "personality":"Melancólico"},
              {"id":"d", "text":"Tolera a situação com paciência e evita conflitos, mesmo achando injusto.", "personality":"Fleumático"}
          ]
      },
      {
          "id": 16,
          "statement": "Quando você estuda por conta porque não entendeu nada da aula, você:",
          "alternatives": [
              {"id":"a", "text":"Procura um colega para estudar junto ou assistir um vídeo no YouTube, mas se distrai fácil.", "personality":"Sanguíneo"},
              {"id":"b", "text":"Cria seu próprio cronograma e vai atrás de outros materiais para dominar o conteúdo.", "personality":"Colérico"},
              {"id":"c", "text":"Sente que nunca está suficientemente preparado e fica inseguro para a prova.", "personality":"Melancólico"},
              {"id":"d", "text":"Estuda no seu ritmo, com tranquilidade, mesmo que o aprendizado leve mais tempo.", "personality":"Fleumático"}
          ]
      },
      {
          "id": 17,
          "statement": "Como você se sente ao ser chamado para responder uma pergunta na frente da turma?",
          "alternatives": [
              {"id":"a", "text":"Fico nervoso e penso muito no que vou dizer.", "personality":"Melancólico"},
              {"id":"b", "text":"Fico animado e respondo com naturalidade.", "personality":"Sanguíneo"},
              {"id":"c", "text":"Respondo com firmeza e gosto de mostrar que sei.", "personality":"Colérico"},
              {"id":"d", "text":"Respondo com calma, mesmo que não saiba tudo.", "personality":"Fleumático"}
          ]
      },
      {
          "id": 18,
          "statement": "Ao receber uma nota abaixo da esperada, sua reação mais comum é:",
          "alternatives": [
              {"id":"a", "text":"Fico frustrado e reflito onde errei.", "personality":"Melancólico"},
              {"id":"b", "text":"Levo na esportiva e digo que vou melhorar da próxima.", "personality":"Sanguíneo"},
              {"id":"c", "text":"Me irrito, mas uso isso como motivação para superar.", "personality":"Colérico"},
              {"id":"d", "text":"Aceito com tranquilidade e sigo em frente.", "personality":"Fleumático"}
          ]
      },
      {
          "id": 19,
          "statement": "Quando o semestre começa, você costuma:",
          "alternatives": [
              {"id":"a", "text":"Criar um plano de estudos detalhado desde o início.", "personality":"Melancólico"},
              {"id":"b", "text":"Se animar com as aulas e os colegas.", "personality":"Sanguíneo"},
              {"id":"c", "text":"Ficar motivado a se destacar em tudo.", "personality":"Colérico"},
              {"id":"d", "text":"Seguir o ritmo das aulas com calma e consistência.", "personality":"Fleumático"}
          ]
      },
      {
          "id": 20,
          "statement": "Durante aulas longas, você geralmente:",
          "alternatives": [
              {"id":"a", "text":"Se distrai com facilidade, mas tenta manter a atenção.", "personality":"Sanguíneo"},
              {"id":"b", "text":"Se concentra bem e mantém o foco até o fim.", "personality":"Colérico"},
              {"id":"c", "text":"Sente cansaço mental, mesmo prestando atenção.", "personality":"Melancólico"},
              {"id":"d", "text":"Mantém-se calmo e acompanha o conteúdo no seu tempo.", "personality":"Fleumático"}
          ]
      }
  ]

  questions.value = data
  currentQuestion.value = data[currentIndex.value]
})



function nextQuestion() {
  if (selected.value) {
    const answer = currentQuestion.value.alternatives.find(
      (alt) => alt.id === selected.value
    )
    if (answer) {
      personalityCount.value[answer.personality]++
    }
  }



  selected.value = null
  currentIndex.value++
  if (currentIndex.value < questions.value.length) {
    currentQuestion.value = questions.value[currentIndex.value]
  } else {
    navigateToResult()
  }
}


function skipQuestion() {
  selected.value = null
  currentIndex.value++
  if (currentIndex.value < questions.value.length) {
    currentQuestion.value = questions.value[currentIndex.value]
  } else {
    currentQuestion.value = null
  }
}

</script>

<style scoped lang="scss">
@use "/src/styles/variables.scss" as *;

.card {
  display: flex;
  border: none;
  align-items: center;

  &-body-title {
    display: flex;
    justify-content: center;
    width: 380px;
    background-color: $main-color;
    font-family: 'Montserrat', sans-serif;
    padding: 16px;
    border-radius: 12px;
    font-weight: bold;
    color: white;
  }
}

.option-box {
  background-color: $main-title-color;
  width: 360px;
  cursor: pointer;
  text-align: center;
  transition: background-color 0.3s;

  &.option-select {
    background-color: #7B3EF4 !important;
    opacity: 0.6;
    color: white;
  }
}

.bar-progress {
  width: 300px;
  height: auto;

  .progress {
    height: 8px;
    border-radius: 5px;
  }

  .progress-bar.bg-purple {
    background-color: #8b4dfd !important;
  }

  .number {
    position: absolute;
    top: -20px;
    left: 224px;
    font-size: 14px;
    font-weight: 500;
    color: #666;
  }
}

@media (max-width: 768px) {
  .logo-image img {
    height: 70px;
  }

  .card-body-title {
    width: 340px;
  }

  .option-box {
    width: 340px;
  }

  .bar-progress {
    width: 100%;
    width: 400px;
  }

  .bar-progress .number {
    left: 192px;
  }
}

</style>
    