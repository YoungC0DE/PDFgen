<template>
  <div id="mainApp">
    <nav class="navBar">
      <div class="logo">
        <i class="bi bi-file-earmark-text"></i> PDFGen
      </div>

      <div class="menuOptions">
        <a href="https://reciboonline.com/orcamento/" target="_blank">References</a>
        <a href="#footer">About</a>
      </div>
    </nav>

    <main class="contentMain">

      <div class="titleInfo">
        <h1>Gerador de Orçamento</h1>
        <p>Preencha os campos abaixo e clique em Baixar PDF</p>
      </div>

      <section>
        <p><i class="bi bi-info-square-fill"></i> Dados do Emissor</p>
        <div class="form-floating">
          <input type="text" class="form-control" id="Emit_Name" placeholder="Ex: John">
          <label for="Emit_Name">Nome ou Razão Social</label>
        </div>

        <div class="form-floating">
          <input type="tel" class="form-control" id="Emit_Tel" placeholder="(99) 99999-9999">
          <label for="Emit_Tel">Telefone ou Celular</label>
        </div>

        <div class="form-floating">
          <input type="email" class="form-control" id="Emit_Email" placeholder="name@example.com">
          <label for="Emit_Email">Email</label>
        </div>

        <div class="multSpace">
          <div class="moreSpace form-floating">
            <input type="url" class="form-control" id="Emit_Site" placeholder="Ex: https://yoursite.com.br">
            <label for="Emit_Site">Website</label>
          </div>

          <div class="chooseLogo">
            <label for="Emit_logo">Logo Marca <i class="bi bi-file-earmark-image"></i></label>
            <input class="form-control d-none" type="file" id="Emit_logo" accept="image/png, image/jpg, image/jpeg" />
          </div>
        </div>
      </section>

      <section>
        <p><i class="bi bi-info-square-fill"></i> Dados do Destinatário</p>
        <div class="form-floating">
          <input type="text" class="form-control" id="Dest_Name" placeholder="Ex: John">
          <label for="Dest_Name">Nome ou Razão Social</label>
        </div>

        <div class="form-floating">
          <input type="tel" class="form-control" id="Dest_Tel" placeholder="(99) 99999-9999">
          <label for="Dest_Tel">Telefone ou Celular</label>
        </div>

        <div class="multSpace">
          <div class="moreSpace form-floating">
            <input type="text" class="form-control" id="Dest_Address" placeholder="Ex: Avenida">
            <label for="Dest_Address">Endereço</label>
          </div>

          <div class="form-floating">
            <input type="number" class="form-control" id="Dest_Addnum" placeholder="Ex: 288" min="0">
            <label for="Dest_Addnum">Número</label>
          </div>
        </div>

        <div class="multSpace">
          <div class="form-floating">
            <input type="text" class="form-control" id="Dest_Address" placeholder="Ex: 00000-000">
            <label for="Dest_Address">CEP</label>
          </div>

          <div class="moreSpace form-floating">
            <input type="text" class="form-control" id="Dest_Addnum" placeholder="Ex: 00000-000">
            <label for="Dest_Addnum">Cidade</label>
          </div>
        </div>
      </section>

      <section>
        <p><i class="bi bi-info-square-fill"></i> Dados do orçamento</p>
        <div class="multSpace">
          <div class="form-floating">
            <input type="number" class="form-control" id="Budget_id" placeholder="Ex: 1" min="1" value="1">
            <label for="Budget_id">Número do orçamento</label>
          </div>

          <div class="form-floating">
            <input type="date" class="form-control" id="Budget_Valid" placeholder="dd/mm/yyyy">
            <label for="Budget_Valid">Validade</label>
          </div>
          <div class="form-floating">
            <input type="date" class="form-control" id="Budget_Warranty" placeholder="dd/mm/yyyy">
            <label for="Budget_Warranty">Garantia</label>
          </div>
        </div>

        <div class="multSpace">
          <div class="moreSpace form-floating">
            <select class="form-select" id="Budget_Payment" aria-label="payment">
              <option selected>Boleto</option>
              <option value="1">Cartão</option>
              <option value="2">Pix</option>
              <option value="3">Transferência</option>
              <option value="4">Outros</option>
            </select>
            <label for="Budget_Payment">Forma de Pagamento</label>
          </div>

          <div class="form-floating">
            <input type="number" class="form-control" id="Budget_fractPay" placeholder="1x" min="1" value="1">
            <label for="Budget_fractPay">Quantas vezes</label>
          </div>
        </div>

        <div class="form-floating">
          <textarea class="form-control" placeholder="Informações adicionais" id="Budget_info"></textarea>
          <label for="Budget_info">Informações adicionais</label>
        </div>

        <div class="form-floating">
          <textarea class="form-control" placeholder="Descrição.." id="Budget_Desc" style="height: 100px"></textarea>
          <label for="Budget_Desc">Descrição do produto ou serviço</label>
        </div>

        <div class="areaAddRemoveItem">
          <button id="addProd" class="addValue" type="button" v-on:click="addItem('product')">Adicionar Produto <i
              class="bi bi-plus-circle"></i></button>
          <button id="removeProd" class="addValue" v-if="prodCount > 0" type="button"
            v-on:click="removeItem('product')">Remover Produto <i class="bi bi-dash-circle"></i></button>
        </div>
        <hr>

        <div class="product" :show="prodCount > 0" v-for="i in prodCount">
          <div class="form-floating">
            <input type="text" class="form-control" id="Prod_Name" placeholder="Smart Phone">
            <label for="Prod_Name">Nome do Produto</label>
          </div>

          <div class="multSpace">
            <div class="form-floating">
              <input type="number" class="form-control" id="Prod_value" placeholder="Ex: 125" min="0" value="0">
              <label for="Prod_value">Valor</label>
            </div>

            <div class="form-floating">
              <input type="number" class="form-control" id="Prod_count" placeholder="Ex: 1" min="1" value="1">
              <label for="Prod_count">Quantidade</label>
            </div>
          </div>
          <select class="form-select" aria-label="Metrics">
            <option selected>Selecione a medida</option>
            <option value="1">Metros</option>
            <option value="2">Metros Quadrados</option>
            <option value="3">Centímetros</option>
            <option value="4">Quilômetros</option>
            <option value="5">Gramas</option>
            <option value="6">Litros</option>
            <option value="7">Quilo</option>
            <option value="8">Minutos</option>
            <option value="9">Horas</option>
            <option value="10">Dias</option>
            <option value="11">Meses</option>
            <option value="12">Anos</option>
          </select>
        </div>

        <div class="areaAddRemoveItem" style="margin-top: 3%;">
          <button id="addServ" class="addValue" type="button" v-on:click="addItem('service')">Adicionar Serviço <i
              class="bi bi-plus-circle"></i></button>
          <button id="removeServ" class="addValue" v-if="servCount > 0" type="button"
            v-on:click="removeItem('service')">Remover Serviço <i class="bi bi-dash-circle"></i></button>
        </div>
        <hr>

        <div class="service" :show="servCount > 0" v-for="i in servCount">
          <div class="form-floating">
            <input type="text" class="form-control" id="Serv_Name" placeholder="Smart Phone">
            <label for="Serv_Name">Serviço</label>
          </div>

          <div class="multSpace">
            <div class="form-floating">
              <input type="number" class="form-control" id="Serv_value" placeholder="Ex: 125" min="0" value="0">
              <label for="Serv_value">Valor Base</label>
            </div>

            <span class="align-self-center">Cobra por:</span>

            <select class="form-select" aria-label="ValueJob" style="margin-bottom: 2%; width: auto;">
              <option value="1" selected>Minutos</option>
              <option value="2">Horas</option>
              <option value="3">Dias</option>
              <option value="4">Meses</option>
              <option value="5">Anos</option>
            </select>
          </div>
        </div>
      </section>

      <button id="DonwloadPDF" type="button" class="DownloadPDF btn btn-success" v-on:click="SavePDF">Baixar PDF <i
          class="bi bi-download"></i></button>
    </main>

    <footer id="footer">
      <span>Projeto desenvolvido utilizando: VueJs, Boostrap, CSS e Javascript</span>
      <span>Link do repositório: <a href="#">github/YoungC0DE</a></span>
    </footer>
  </div>

</template>

<script>
import jsPDF from 'jspdf'

export default {
  name: 'App',
  data() {
    return {
      prodCount: 0,
      servCount: 0
    }
  },
  methods: {
    addItem(prop) {
      prop == 'product' ? this.prodCount += 1 : this.servCount += 1
    },
    removeItem(prop) {
      prop == 'product' ? this.prodCount -= 1 : this.servCount -= 1
    },
    SavePDF(){
      var pdf = new jsPDF
      pdf.text('Em construção', 10, 10)
      pdf.save("a4.pdf");
    }
  }
}
</script>