<template>
  <div id="burguer-table">
    <Message :msg="msg" v-show="msg"/>
    <div>
      <div id="burguer-table-heading">
        <div class="order-id">#</div>
        <div>Cliente:</div>
        <div>Pão:</div>
        <div>Carne:</div>
        <div>Opcionais:</div>
        <div>Ações:</div>
      </div>
      <div id="buruger-table-rows">
        <div class="burguer-table-row" v-for="burger in burgers" :key="burger_id">
          <div class="order-number">{{ burger_id }}</div>
          <div>{{ burger.nome }}</div>
          <div>{{ burger.pao }}</div>
          <div>{{ burger.carne }}</div>
          <div>
            <ul>
              <li v-for="(opcional, index) in burger.opcionais" :key="index">{{ opcional }}</li>
            </ul>
          </div>
          <div>
            <select @change="updateBurger($event, burger.id)" name="status" class="status">
              <option value="">Selecione</option>
              <option v-for="s in status" :key="s.id" :value="s.tipo" :selected="burger.status === s.tipo">{{ s.tipo }}</option>
            </select>
            <button @click="deleteBurger(burger.id)" class="delete-btn">Cancelar</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Message from "./Message";
export default {
  name: "Dashboard",
  components: {
    Message
  },
  data() {
    return {
      burgers: null,
      burger_id: null,
      status: [],
      msg: null
    }
  },
  methods: {
    async getPedidos() {
      const req = await fetch("http://localhost:3000/burgers");

      this.burgers = await req.json();

      console.log(this.burgers);

      //resgatar os status
      await this.getStatus();
    },
    async getStatus() {
      const req = await fetch("http://localhost:3000/status");

      this.status = await req.json();
    },
    async deleteBurger(id) {
      const req = await fetch("http://localhost:3000/burgers/" + id,{
        method: 'DELETE'
      });

      const res = await req.json();

      await this.getPedidos();

      this.msg = `Pedido cancelado com sucesso!`;

      setTimeout(() => {
        this.msg = ""
      }, 3000)
    },
    async updateBurger(event, id) {
      const option = event.target.value;
      const dataJson = JSON.stringify({status: option});

      const req = await fetch("http://localhost:3000/burgers/" + id, {
        method: "PATCH",
        headers: {"Content-Type": "application/json"},
        body: dataJson
      });

      const res = await req.json();

      this.msg = `Pedido Nº${res.id} foi atualizado para ${res.status}!`;

      setTimeout(() => {
        this.msg = ""
      }, 3000)
    }
  },
  mounted() {
    this.getPedidos();
  }
}
</script>

<style scoped>
#burguer-table {
  max-width: 1200px;
  margin: 0 auto;
}

#burguer-table-heading, #buruger-table-rows, .burguer-table-row {
  display: flex;
  flex-wrap: wrap;
}

#burguer-table-heading {
  font-weight: bold;
  padding: 12px;
  border-bottom: 3px solid #333;
}

#burguer-table-heading div, .burguer-table-row div {
  width: 19%;
}

.burguer-table-row {
  width: 100%;
  padding: 12px;
  border-bottom: 1px solid #CCC;
}

#burguer-table-heading .order-id, .burguer-table-row .order-number {
  width: 5%;
}

select {
  padding: 12px 6px;
  margin-right: 12px;
}

.delete-btn {
  background-color: #222;
  color: #FCBA03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}

.delete-btn:hover {
  background-color: transparent;
  color: #222;
  border-color: #FCBA03;
}
</style>