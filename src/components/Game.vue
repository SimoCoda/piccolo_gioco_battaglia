<template>
  <div>
    <HeaderComponent :numberRounds="gameRound" />

    <GameOverComponent
      v-if="isGameOver"
      :whoWin="winOrNot()"
      :rounds="gameRound"
      @resetGame="resetGame"
    />
    <section v-if="!isGameOver">
      <HealthBarComponent
        v-if="selectedName"
        :name="firstName"
        :life="life_nemico"
        @newName="() => (selectedName = false)"
      />
      <div v-else class="container flex items-center gap-8 h-16">
        <input
          type="text"
          class="bg-gray-100 w-[100%] rounded-lg pl-3"
          v-model="firstName"
          @keydown.enter="confirmName"
          placeholder="Inserisci il nome dell'avversario"
        />
      </div>
      <HealthBarComponent
        v-if="selectedName"
        :name="secondName"
        :life="life_me"
        @newName="() => (selectedName = false)"
      />
      <div v-else class="container flex items-center gap-8 h-16">
        <input
          type="text"
          class="bg-gray-100 w-[100%] rounded-lg pl-3"
          v-model="secondName"
          @keydown.enter="confirmName"
          placeholder="Inserisci il tuo nome"
        />
        <button
          class="w-[20%] h-[5px] flex items-center justify-center text-black bg-green-300 hover:bg-green-400 transition-all text-sm"
          @click="confirmName"
          :disabled="isEmtyInputName"
        >
          Conferma
        </button>
      </div>

      <section
        v-if="selectedName"
        class="grid grid-cols-2 gap-8 max-w-xl mx-auto my-10"
      >
        <UiButtonComponent
          class="attack"
          title="Attacco"
          @evtClick="evtAttacco"
        ></UiButtonComponent>
        <UiButtonComponent
          class="attack"
          title="Attacco speciale"
          @evtClick="evtAttaccoSpeciale"
          :disabled="isSpecialAttack"
        ></UiButtonComponent>
        <UiButtonComponent
          class="medikit"
          title="Medikit"
          @evtClick="evtMedikit"
          :disabled="isMedikit"
        ></UiButtonComponent>
        <UiButtonComponent
          class="medikit"
          title="Mi Arrendo!"
          @evtClick="evtMiArrendo"
        ></UiButtonComponent>
      </section>
    </section>

    <BattleLogComponent v-if="selectedName" :actions="logMessages" />
  </div>
</template>

<script>
import BattleLogComponent from "./BattleLog.vue";
import GameOverComponent from "./GameOver.vue";
import HeaderComponent from "./HeaderComponent.vue";
import HealthBarComponent from "./HealthBar.vue";
import UiButtonComponent from "./UiButton.vue";
import { ref, watch, computed } from "vue";
export default {
  components: {
    UiButtonComponent,
    HeaderComponent,
    GameOverComponent,
    HealthBarComponent,
    BattleLogComponent,
  },

  setup() {
    const gameRound = ref(0);
    const isGameOver = ref(false);
    const life_nemico = ref(100);
    const life_me = ref(100);
    const logMessages = ref([]);
    const iWin = ref();
    const isSpecialAttack = ref(true);
    const isMedikit = ref(true);
    const selectedName = ref(false);
    const firstName = ref("");
    const secondName = ref("");
    const isEmtyInputName = ref(true);
    const isFirstNameEmty = ref(true);
    const isSecondNameEmty = ref(true);

    const winOrNot = () => {
      debugger;
      if (iWin.value == true) {
        if (secondName == "Tizio" || secondName.trim == "") {
          return "Nemico";
        } else {
          return secondName;
        }
      } else {
        if (firstName == "Tizio" || firstName.trim == "") {
          return "Me";
        } else {
          return firstName;
        }
      }
    };
    const generateRandomNumber = () => {
      return Math.floor(Math.random() * 21) + 1;
    };
    const generateRandomNumberSpecial = () => {
      return Math.floor(Math.random() * 20) + 21;
    };

    watch(firstName,(newName) => {
        isFirstNameEmty.value = newName.trim() === "";
        debugger
        if(!isFirstNameEmty.value && !isSecondNameEmty.value) {
            isEmtyInputName.value = false;
        }else{
            isEmtyInputName.value = true;
        }
    });
    watch(secondName,(newName) => {
        isSecondNameEmty.value = newName.trim() === "";
        debugger
        if(!isFirstNameEmty.value && !isSecondNameEmty.value) {
            isEmtyInputName.value = false;
        }else{
            isEmtyInputName.value = true;
        }
    });
    

    watch(gameRound, (oldGameRound) => {
      if (oldGameRound == 3) {
        isSpecialAttack.value = false;
      }
    });
    watch(life_me, (new_lifeMe) => {
      if (new_lifeMe <= 0) {
        iWin.value = false;
        isGameOver.value = true;
        logMessages.value.push({
          name: `${firstName.value}`,
          damage: `Hai vinto!`,
        });
      }
      if (new_lifeMe <= 50) {
        isMedikit.value = false;
      }
    });
    watch(life_nemico, (new_lifeNemico) => {
      if (new_lifeNemico <= 0) {
        iWin.value = true;
        isGameOver.value = true;
        logMessages.value.push({
          name: `${secondName.value}`,
          health: `Hai vinto!`,
        });
      }
    });

    function evtAttacco() {
      gameRound.value++;
      let dangerAttackNemico = generateRandomNumber();
      let dangerAttackMe = generateRandomNumber();
      if (life_me.value - dangerAttackMe <= 0) {
        life_me.value = 0;
        return;
      } else if (life_nemico.value - dangerAttackNemico <= 0) {
        life_nemico.value = 0;
        return;
      } else {
        life_nemico.value -= dangerAttackNemico;
        life_me.value -= dangerAttackMe;
        console.log(
          `Son stati tolti al nemico ${dangerAttackNemico} e a me ${dangerAttackMe}`
        );
      }
      logMessages.value.push({
        name: `${firstName.value}`,
        damage: `-${dangerAttackNemico}`,
      });
      logMessages.value.push({
        name: `${secondName.value}`,
        damage: `-${dangerAttackMe}`,
      });
    }
    function evtAttaccoSpeciale() {
      gameRound.value++;
      let dangerAttackNemico = generateRandomNumberSpecial();
      let dangerAttackMe = generateRandomNumberSpecial();
      if (life_me.value - dangerAttackMe <= 0) {
        life_me.value = 0;
        return;
      } else if (life_nemico.value - dangerAttackNemico <= 0) {
        life_nemico.value = 0;
        return;
      } else {
        life_nemico.value -= dangerAttackNemico;
        life_me.value -= dangerAttackMe;
      }
      logMessages.value.push({
        name: `${firstName.value}`,
        damage: `-${dangerAttackNemico}`,
      });
      logMessages.value.push({
        name: `${secondName.value}`,
        damage: `-${dangerAttackMe}`,
      });
    }
    function evtMedikit() {
      if(life_me.value + 20 < 100){
        gameRound.value++;
        life_me.value += 20;
        let dangerAttackNemico = generateRandomNumber();
        life_me.value -= dangerAttackNemico;
        logMessages.value.push({
          name: `${firstName.value}`,
          damage: `-${dangerAttackNemico}`,
        });
        logMessages.value.push({ name: `${secondName.value}`, health: `+20` });
        if(life_me.value > 50){
            isMedikit.value = true;
        }
      }else{
        life_me.value = 100;
      }
    }
    function evtMiArrendo() {
      life_me.value = 0;
    }
    function resetGame() {
      life_nemico.value = 100;
      life_me.value = 100;
      gameRound.value = 0;
      isGameOver.value = false;
      logMessages.value = [];
      isSpecialAttack = ref(true);
      isMedikit = ref(true);
      iWin.value = null;
    }
    function confirmName() {
      if(!isEmtyInputName.value){
        selectedName.value = true;
      }
    }

    return {
      gameRound,
      isGameOver,
      life_nemico,
      life_me,
      evtAttacco,
      evtAttaccoSpeciale,
      evtMedikit,
      evtMiArrendo,
      logMessages,
      iWin,
      winOrNot,
      resetGame,
      isSpecialAttack,
      isMedikit,
      firstName,
      secondName,
      confirmName,
      selectedName,
      isEmtyInputName,
      isFirstNameEmty,
      isSecondNameEmty,
    };
  },
};
</script>

<style></style>
