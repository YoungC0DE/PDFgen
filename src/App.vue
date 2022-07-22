<template>
  <div id="mainApp">
    <nav class="navBar">
      <a href="#" class="logo text-dark">
        <i class="bi bi-file-earmark-text"></i> PDFGen
      </a>

      <div class="menuOptions">
        <a href="https://reciboonline.com/orcamento/" target="_blank">References</a>
        <a href="#footer">About</a>
      </div>
    </nav>

    <form class="contentMain needs-validation" novalidate>

      <div class="titleInfo">
        <h1>Gerador de Orçamento</h1>
        <p>Preencha os campos abaixo e clique em Baixar PDF</p>
      </div>

      <section>
        <p><i class="bi bi-info-square-fill"></i> Dados do Emissor</p>
        <div class="form-floating">
          <input type="text" class="form-control" id="Emit_Name" v-model="EmitData.Name" required>
          <label for="Emit_Name">Nome ou Razão Social</label>
        </div>

        <div class="form-floating">
          <input type="tel" class="form-control" id="Emit_Tel" v-model="EmitData.Tel" v-maska="'(##) #####-####'"
            required>
          <label for="Emit_Tel">Telefone ou Celular</label>
        </div>

        <div class="form-floating">
          <input type="email" class="form-control" id="Emit_Email" v-model="EmitData.Email" required>
          <label for="Emit_Email">Email</label>
        </div>

        <div class="multSpace">
          <div class="moreSpace form-floating">
            <input type="url" class="form-control" id="Emit_Site" v-model="EmitData.Site">
            <label for="Emit_Site">Website</label>
          </div>

          <div class="chooseLogo">
            <label :for="hasLogo ? '' : 'Emit_logo'">
              <span class="deleteLogo" v-if="hasLogo" v-on:click="deletLogo()">Remove Logo</span>
              <span v-else>Upload Logo</span>
              <i class="deleteLogo bi bi-file-check-fill" v-if="hasLogo" v-on:click="deletLogo()"></i>
              <i class="bi bi-file-earmark-image" v-else></i>
            </label>
            <input class="form-control d-none" type="file" id="Emit_logo" accept="image/png, image/jpg, image/jpeg"
              v-on:input="injectNewPic()" />
          </div>
        </div>
      </section>

      <section>
        <p><i class="bi bi-info-square-fill"></i> Dados do Destinatário</p>
        <div class="form-floating">
          <input type="text" class="form-control" id="Dest_Name" v-model="DestData.Name" required>
          <label for="Dest_Name">Nome ou Razão Social</label>
        </div>

        <div class="form-floating">
          <input type="tel" class="form-control" id="Dest_Tel" v-model="DestData.Tel" v-maska="'(##) #####-####'"
            required>
          <label for="Dest_Tel">Telefone ou Celular</label>
        </div>

        <div class="multSpace">
          <div class="moreSpace form-floating">
            <input type="text" class="form-control" id="Dest_Address" v-model="DestData.Address" required>
            <label for="Dest_Address">Endereço</label>
          </div>

          <div class="form-floating">
            <input type="number" class="form-control" id="Dest_Addnum" min="0" v-model="DestData.Num" required>
            <label for="Dest_Addnum">Número</label>
          </div>
        </div>

        <div class="multSpace">
          <div class="form-floating">
            <input type="text" class="form-control" id="Dest_Address" v-maska="'#####-###'" min="0" max="8"
              v-model="DestData.Cep" required>
            <label for="Dest_Address">CEP</label>
          </div>

          <div class="moreSpace form-floating">
            <input type="text" class="form-control" id="Dest_City" v-model="DestData.City" required>
            <label for="Dest_City">Cidade</label>
          </div>
        </div>
      </section>

      <section>
        <p><i class="bi bi-info-square-fill"></i> Dados do orçamento</p>
        <div class="multSpace">
          <div class="form-floating">
            <input type="number" class="form-control" id="BudgetID" min="1" v-model="BudgetData.ID" required>
            <label for="BudgetID">Número do orçamento</label>
          </div>

          <div class="form-floating">
            <input type="date" class="form-control" id="Budget_Valid" v-model="BudgetData.Valid">
            <label for="Budget_Valid">Validade</label>
          </div>
          <div class="form-floating">
            <input type="date" class="form-control" id="Budget_Warranty" v-model="BudgetData.Warranty">
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
            <input type="number" class="form-control" id="Budget_fractPay" min="1" v-model="BudgetData.PayTimes"
              required>
            <label for="Budget_fractPay">Quantas vezes</label>
          </div>
        </div>

        <div class="form-floating">
          <textarea class="form-control" id="Budget_info"></textarea>
          <label for="Budget_info">Informações adicionais</label>
        </div>

        <div class="form-floating">
          <textarea class="form-control" id="Budget_Desc" style="height: 100px"></textarea>
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
            <input type="text" class="form-control" id="Prod_Name" v-model="ProdData.Name" required>
            <label for="Prod_Name">Nome do Produto</label>
          </div>

          <div class="multSpace">
            <div class="form-floating">
              <input type="text" class="form-control" id="Prod_value" min="0" v-model="ProdData.Value"
                v-maska="'R$ ###.###,###,##'" required>
              <label for="Prod_value">Valor</label>
            </div>

            <div class="form-floating">
              <input type="number" class="form-control" id="Prod_count" min="1" v-model="ProdData.Count" required>
              <label for="Prod_count">Quantidade</label>
            </div>
          </div>
          <select class="form-select" aria-label="Metrics" required>
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
            <input type="text" class="form-control" id="Serv_Name" v-model="ServData.Name" required>
            <label for="Serv_Name">Serviço</label>
          </div>

          <div class="multSpace">
            <div class="form-floating">
              <input type="text" class="form-control" id="Serv_value" min="0" v-model="ServData.Value"
                v-maska="'R$ ###.###,###,##'" required>
              <label for="Serv_value">Valor Base</label>
            </div>

            <span class="align-self-center">Cobra por:</span>

            <select class="form-select" aria-label="ValueJob" style="margin-bottom: 2%; width: auto;" required>
              <option value="1" selected>Minutos</option>
              <option value="2">Horas</option>
              <option value="3">Dias</option>
              <option value="4">Meses</option>
              <option value="5">Anos</option>
            </select>
          </div>
        </div>
      </section>

      <div class="alert alert-danger w-75 text-center" role="alert" v-show="errorEmpty">
        Há campos importantes não estão preenchidos!
      </div>

      <button id="DonwloadPDF" type="submit" class="DownloadPDF btn btn-success" v-on:click.prevent="SavePDF">Baixar PDF
        <i class="bi bi-download"></i></button>
    </form>

    <footer id="footer">
      <div class="descFooter">
        Projeto desenvolvido utilizando: VueJs, Boostrap, CSS e Javascript
        <span>Link do repositório: <a href="https://github.com/YoungC0DE/PDFgen" target="_blank">Github</a></span>
      </div>
      <div class="AjudaEu">
        Me ajude a evoluir o projeto =)<br>
        Chave Pix:
        <img class="pix" src="./assets/pix-pro-pai.png" width="auto">
      </div>
    </footer>
  </div>

</template>

<script>
import jsPDF from 'jspdf'
import { maska } from 'maska'

export default {
  name: 'App',
  data() {
    return {
      EmitData: {},
      DestData: {},
      BudgetData: {},
      ProdData: {},
      ServData: {},
      prodCount: 0,
      servCount: 0,
      hasLogo: false,
      errorEmpty: false
    }
  },
  directives: { maska },
  methods: {
    addItem(prop) { prop == 'product' ? this.prodCount += 1 : this.servCount += 1 },
    removeItem(prop) { prop == 'product' ? this.prodCount -= 1 : this.servCount -= 1 },

    validing() {
      var forms = document.querySelectorAll('.needs-validation')
      forms.forEach(form => form.classList.add('was-validated'))

      // validing inputs
      var inputData = !this.EmitData.Name || !this.EmitData.Tel || !this.EmitData.Email ||
        !this.DestData.Name || !this.DestData.Tel || !this.DestData.Address ||
        !this.DestData.City || !this.DestData.Cep

      if (inputData) this.errorEmpty = true
      else this.errorEmpty = false
    },

    SavePDF() {
      this.validing()
      //if (this.errorEmpty) return

      var pdf = new jsPDF,
        DateToday = new Date(),
        dataTime = DateToday.getDay().toString() + "/" +
          DateToday.getMonth().toString() + "/" +
          DateToday.getFullYear().toString(),
        image = this.BudgetData.logo

      pdf.setFont('arial')
      pdf.setFontSize(10)
      pdf.text(dataTime, 10, 9)
      pdf.text('Orçamento', 185, 9)

      if (this.hasLogo) {
        pdf.line(200, 10, 10, 10, 'DF')
        pdf.setFontSize(20)
        pdf.setLineWidth(0.2);
        pdf.rect(15, 15, 45, 45);
        pdf.addImage(image, 'PNG', 17.5, 17.5, 40, 40)
        pdf.text(`Orçamento ${this.BudgetData.ID == undefined ? '1' : this.BudgetData.ID}`, 80, 20)
        pdf.line(200, 65, 10, 65, 'DF')
      }
      else {
        pdf.setFontSize(35)
        pdf.setFont('arial','bold')
        pdf.text('Orçamento', 15, 22)
        pdf.setFontSize(15)
        pdf.text(`Nº ${this.BudgetData.ID == undefined ? '1' : this.BudgetData.ID}`, 17, 30)
        pdf.line(200, 40, 10, 40, 'DF')
      }

      pdf.save("Orçamento.pdf")
    },
    injectNewPic() {
      localStorage.setItem('logo', window.URL.createObjectURL(document.getElementById('Emit_logo').files[0]))
      this.BudgetData.logo = localStorage.getItem('logo')
      this.hasLogo = document.getElementById('Emit_logo').files[0] != null || localStorage.getItem('logo')
    },
    deletLogo() {
      this.hasLogo = false
      localStorage.removeItem('logo')
      console.log(this.hasLogo, localStorage.getItem('logo'))
    }
  }
}
</script>