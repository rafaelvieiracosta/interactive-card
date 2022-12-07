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
                0000 0000 0000 0000
              </span>
              <div class="cartao-frente-informacoes-nomeData">
                <p class="cartao-frente-informacoes-nome">
                  Jane Appleseed
                </p>
                <span class="cartao-frente-informacoes-data">
                  00/00
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
            <p class="cartao-verso-informacoes-cvc">000</p>
          </div>
        </div>
      </div>

      <form action="">
        <div class="form-item">
          <label for="nome">Nome do titular</label>
          <input id="nome" type="text" v-model="nome" placeholder="ex: Josh Ramalho">
          <span class="error">Coloque um nome</span>
        </div>

        <div class="form-item">
          <label for="numero">Número do cartão</label>
          <input id="numero" type="text" v-model="numero" placeholder="ex: 4859 2350 1093 9921">
          <span class="error">Coloque um nome</span>
        </div>

        <div class="form-linha">
          <div class="form-item data">
            <label for="dataMM">Validade (MM/AA)</label>
            <div class="form-item-dataAAMM">
              <input id="dataMM" type="text" v-model="dataMM" placeholder="MM">
              <input id="dataAA" type="text" v-model="dataAA" placeholder="AA">
            </div>
            <span class="error">Coloque um nome</span>
          </div>

          <div class="form-item">
            <label for="cvc">CVC</label>
            <input id="cvc" type="text" v-model="cvc" placeholder="838">
            <span class="error">Coloque um nome</span>
          </div>
        </div>

        <button type="submit">Confirmar</button>
      </form>
    </main>
  </div>
</template>

<script>

export default {
  name: 'Dados do cartão',

  data(){
    return{
      nome: null,
      numero: null,
      dataMM: null,
      dataAA: null,
      cvc: null
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
  margin-top: 26px;
  letter-spacing: 0.14em;
  text-transform: uppercase;
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
.error{
  display: none;
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
</style>
