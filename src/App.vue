<template>
  <div id="app">
    <main>
      <div class="cartoes">
        <div class="cartao-frente">
          <img 
            src="@/assets/img/bg-card-front.png" 
            alt="Frente de um cartão na cor azul"
            width="447"
            height="245"
          >
          <div class="cartao-frente-informacoes">
            <img 
              src="@/assets/img/card-logo.svg" 
              alt="Dois círculos branco"
              width="84"
              height="47"
            >
            <div class="cartao-frente-informacoes-container">
              <span class="cartao-frente-informacoes-numero">
                {{ numero || '0000 0000 0000 0000' }}
              </span>
              <div class="cartao-frente-informacoes-nomeData">
                <p class="cartao-frente-informacoes-nome">
                  {{ nome || 'Josh Ramalho' }}
                </p>
                <span class="cartao-frente-informacoes-data">
                  {{ dataMM || '00' }}/{{ dataAA || '00'}}
                </span>
              </div>
            </div>
          </div>
        </div>

        <div class="cartao-verso">
          <img 
            src="@/assets/img/bg-card-back.png" 
            alt="Verso de um cartão na cor branca"
            width="447"
            height="245"
          >
          <div class="cartao-verso-informacoes">
            <p class="cartao-verso-informacoes-cvc">
              {{ cvc || '000'}}
            </p>
          </div>
        </div>
      </div>


      <form @submit.prevent="enviaFormulario()">
        <div class="form-item">
          <label for="nome">Nome do titular</label>
          <input 
            id="nome" 
            type="text" 
            v-model="nome" 
            placeholder="ex: Josh Ramalho"
            :class="{ error: msgNome }"
            @input="verificaNome()"
            @focus="removeError()"
          >
          <transition>
            <span v-if="msgNome" class="msgError">{{ msgNome }}</span>
          </transition>
        </div>

        <div class="form-item">
          <label for="numero">Número do cartão</label>
          <input 
            id="numero" 
            ref="inputNumero"
            type="text" 
            v-model="numero" 
            placeholder="ex: 4859 2350 1093 9921"
            maxlength="19"
            inputmode="numeric"
            :class="{ error: msgNumero }"
            @input="verificaNumero()"
            @focus="removeError()"
          >
          <transition>
            <span v-if="msgNumero" class="msgError">{{ msgNumero }}</span>
          </transition>
        </div>

        <div class="form-linha">
          <div class="form-item data">
            <label for="dataMM">Validade (MM/AA)</label>
            <div class="form-item-dataAAMM">
              <input 
                id="dataMM" 
                type="text" 
                ref="inputDataMM"
                v-model="dataMM" 
                placeholder="MM"
                maxlength="2"
                inputmode="numeric"
                :class="{ error: msgDataMM }"
                @input="verificaDataMM()"
                @focus="removeError()"
              >
              <input 
                id="dataAA" 
                type="text"
                ref="inputDataAA"
                v-model="dataAA" 
                placeholder="AA"
                maxlength="2"
                inputmode="numeric"
                :class="{ error: msgDataAA }"
                @input="verificaDataAA()"
                @focus="removeError()"
              >
            </div>
            <transition>
              <span v-if="msgDataMM || msgDataAA" class="msgError">{{ msgDataMM || msgDataAA }}</span>
            </transition>
          </div>

          <div class="form-item">
            <label for="cvc">CVC</label>
            <input 
              id="cvc" 
              type="text" 
              ref="inputCVC"
              v-model="cvc" 
              placeholder="838"
              maxlength="3"
              inputmode="numeric"
              :class="{ error: msgCVC }"
              @input="verificaCVC()"
              @focus="removeError()"
            >
            <transition>
              <span v-if="msgCVC" class="msgError">{{ msgCVC }}</span>
            </transition>
          </div>
        </div>

        <button type="submit">Confirmar</button>
      </form>
    </main>
  </div>
</template>

<script>

export default {
  name: 'card',

  data(){
    return{
      nome: '',
      numero: '',
      dataMM: '',
      dataAA: '',
      cvc: '',

      msgNome: '',
      msgNumero: '',
      msgDataMM: '',
      msgDataAA: '',
      msgCVC: ''
    }
  },
  methods: {
    apenasLetras (string) {
      return string.replace(/[^a-z\s]/gi, "");
    },
    apenasNumeros (string){
      return string.replace(/[^0-9]/g, "");
    },

    verificaNome () {
      this.nome = this.apenasLetras(this.nome);
    },
    verificaNumero () {
      this.numero = this.apenasNumeros(this.numero);
      this.numero = this.numero.replace(/([0-9]{4})/g, "$1 ").trimEnd();

      if(this.numero.length === 19){
        this.$refs.inputDataMM.focus();
      }
    },
    verificaDataMM () {
      this.dataMM = this.apenasNumeros(this.dataMM);
      this.dataMM = this.dataMM

      if(event.inputType === 'deleteContentBackward' && this.dataMM.length === 0){
        this.$refs.inputNumero.focus();
      }
      else if(this.dataMM.length === 2){
        this.$refs.inputDataAA.focus();
      }
    },
    verificaDataAA () {
      this.dataAA = this.apenasNumeros(this.dataAA);

      if(event.inputType === 'deleteContentBackward' && this.dataAA.length === 0){
        this.$refs.inputDataMM.focus();
      }
      else if(this.dataAA.length === 2){
        this.$refs.inputCVC.focus();
      }
    },
    verificaCVC () {
      this.cvc = this.apenasNumeros(this.cvc);

      if(event.inputType === 'deleteContentBackward' && this.cvc.length === 0){
        this.$refs.inputDataAA.focus();
      }
    },

    removeError () {
      const element = event.target;
      const id = event.target.id;

      if (element.classList.contains('error')){
        element.classList.remove('error');

        switch (id) {
          case 'nome':
            this.msgNome = '';
            break;
          case 'numero':
            this.msgNumero = '';
            break;
          case 'dataMM':
            this.msgDataMM = '';
            break;
          case 'dataAA':
            this.msgDataAA = '';
            break;
          case 'cvc':
            this.msgCVC = '';
            break;
        }
      }
    },

    enviaFormulario () {
      if (this.nome.length === 0) {
        this.msgNome = 'Não pode ficar em branco';
      } else if (!(this.nome.length > 2)) {
        this.msgNome = 'Digite um nome verdadeiro';
      } else if (!(/\s\w{3}/i.test(this.nome))) {
        this.msgNome = 'Digite seu nome e sobrenome';
      }

      if (this.numero.length === 0) {
        this.msgNumero = 'Não pode ficar em branco';
      } else if (this.numero.length === 18){
        this.msgNumero = 'Falta um dígito do seu cartão'
      } else if (!(this.numero.length === 19)){
        this.msgNumero = 'Digite um número válido'
      }

      if (this.dataMM.length === 0) {
        this.msgDataMM = 'Não pode ficar em branco';
      } else if (!(this.dataMM.length === 2)){
        this.msgDataMM = 'Data inválida';
      } else if (this.dataMM === '00'){
        this.msgDataMM = 'Data inválida';
      }

      if (this.dataAA.length === 0) {
        this.msgDataAA = 'Não pode ficar em branco';
      } else if (!(this.dataAA.length === 2)){
        this.msgDataAA = 'Data inválida';
      } else if (this.dataAA === '00'){
        this.msgDataAA = 'Data inválida';
      }

      if (this.cvc.length === 0) {
        this.msgCVC = 'Não pode ficar em branco';
      } else if (!(this.cvc.length === 3)){
        this.msgCVC = 'CVC inválido';
      }
    }
  }
}
</script>

<style>
/* RESETS */
* {
  margin: 0;
  padding: 0;
  font-family: 'Space Grotesk', sans-serif;
  box-sizing: border-box;
}
img{
  display: block;
  max-width: 100%;
}
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
input[type=number] {
  -moz-appearance: textfield;
}

/* MAIN */
#app{
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-image: url('../src/assets/img/bg-main-desktop.jpg');
  background-repeat: no-repeat;
  background-position: left;
  background-size: contain;
}
main{
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 20px;
  max-width: 1049px;
  flex-grow: 1;
}

/* CARTÕES */
.cartoes{
  flex-shrink: 0;
  width: 100%;
  max-width: 541px;
  display: flex;
  flex-direction: column;
  gap: 37px;
}
.cartao-frente{
  position: relative;
  max-width: 447px;
}
.cartao-frente-informacoes{
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  padding: 28px 33px 26px 32px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  color: #FFFFFF;
}
.cartao-frente-informacoes-numero{
  font-weight: 500;
  font-size: 28px;
  letter-spacing: 0.12em;
}
.cartao-frente-informacoes-nomeData{
  display: flex;
  justify-content: space-between;
  gap: 10px;
  margin-top: 26px;
  letter-spacing: 0.14em;
  text-transform: uppercase;
}
.cartao-frente-informacoes-nome{
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  max-width: 100%;
}
.cartao-verso{
  align-self: flex-end;
  position: relative;
  max-width: 447px;
  color: #FFFFFF;
}
.cartao-verso-informacoes{
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  padding: 101px 60px 106px;
  display: flex;
  justify-content: flex-end;
  align-items: center;
  letter-spacing: 0.14em;
}

/* FORMULÁRIO */
form {
  max-width: 381px;
}
.form-linha,
form > .form-item + .form-item{
  margin-top: 26px;
}
.form-item label{
  font-weight: 500;
  font-size: 12px;
  line-height: 15px;
  letter-spacing: 0.16em;
  text-transform: uppercase;
  color: #0B0C12;
}
.form-item input{
  width: 100%;
  margin-top: 9px;
  border-radius: 8px;
  outline: none;
  border: 1px solid #CED3D9;
  padding: 10px 16px;
  font-weight: 500;
  font-size: 18px;
  line-height: 23px;
  letter-spacing: 0.01em;
  transition: .2s;
}
.form-item input.error{
  border-color: #FF5252;
}
.form-item input:focus{
  border-color: #068BE4;
}
.form-item input::placeholder{
  color: #CED3D9;
}
.form-linha {
  display: flex;
  gap: 20px;
}
.form-item-dataAAMM{
  display: flex;
  gap: 10px;
}
.form-item-dataAAMM > input{
  width: 80px;
}
button {
  margin-top: 41px;
  border-radius: 7px;
  border: none;
  padding: 15px;
  text-align: center;
  width: 100%;
  font-weight: 500;
  font-size: 18px;
  line-height: 23px;
  background-color: #068BE4;
  color: #F5F7FA;
  cursor: pointer;
  transition: .2s;
}
button:hover {
  background-color: #0061A7;
}

.msgError{
  display: flex;
  align-items: flex-end;
  height: 23px;
  font-weight: 500;
  font-size: 12px;
  line-height: 15px;
  color: #FF5252;
  position: relative;
  z-index: -1;
}

.v-enter-active, .v-leave-active {
  transition: .4s;
  max-height: 23px;
}
.v-enter,
.v-leave-to {
  max-height: 0;
  opacity: 0;
}
</style>
