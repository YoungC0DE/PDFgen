<template>
  <div id="mainApp">
    <nav class="navBar">
      <a href="#" class="logo text-dark">
        <i class="bi bi-file-earmark-text"></i> PDFGen
      </a>

      <div class="menuOptions">
        <a href="https://reciboonline.com/orcamento/" target="_blank"
          >References</a
        >
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
          <input
            type="text"
            class="form-control"
            id="Emit_Name"
            v-model="EmitData.Name"
            placeholder="data"
            required
          />
          <label for="Emit_Name">Nome ou Razão Social</label>
        </div>

        <div class="form-floating">
          <input
            type="tel"
            class="form-control"
            id="Emit_Tel"
            v-model="EmitData.Tel"
            v-maska="['(##) ####-####', '(##) #####-####']"
            placeholder="data"
            required
          />
          <label for="Emit_Tel">Telefone ou Celular</label>
        </div>

        <div class="form-floating">
          <input
            type="email"
            class="form-control"
            id="Emit_Email"
            placeholder="data"
            v-model="EmitData.Email"
            required
          />
          <label for="Emit_Email">Email</label>
        </div>

        <div class="multSpace">
          <div class="moreSpace form-floating">
            <input
              type="url"
              class="form-control"
              id="Emit_Site"
              placeholder="data"
              v-model="EmitData.Site"
            />
            <label for="Emit_Site">Website</label>
          </div>

          <div class="chooseLogo">
            <label :for="hasLogo ? '' : 'Emit_logo'">
              <span class="deleteLogo" v-if="hasLogo" v-on:click="deletLogo()"
                >Remove Logo</span
              >
              <span v-else>Upload Logo</span>
              <i
                class="deleteLogo bi bi-file-check-fill"
                v-if="hasLogo"
                v-on:click="deletLogo()"
              ></i>
              <i class="bi bi-file-earmark-image" v-else></i>
            </label>
            <input
              class="form-control d-none"
              type="file"
              id="Emit_logo"
              accept="image/png, image/jpg, image/jpeg"
              v-on:input="injectNewPic()"
            />
          </div>
        </div>
      </section>

      <section>
        <p><i class="bi bi-info-square-fill"></i> Dados do Destinatário</p>
        <div class="form-floating">
          <input
            type="text"
            class="form-control"
            id="Dest_Name"
            placeholder="data"
            v-model="DestData.Name"
            required
          />
          <label for="Dest_Name">Nome ou Razão Social</label>
        </div>

        <div class="form-floating">
          <input
            type="tel"
            class="form-control"
            id="Dest_Tel"
            placeholder="data"
            v-model="DestData.Tel"
            v-maska="['(##) ####-####', '(##) #####-####']"
            required
          />
          <label for="Dest_Tel">Telefone ou Celular</label>
        </div>

        <div class="multSpace">
          <div class="moreSpace form-floating">
            <input
              type="text"
              class="form-control"
              id="Dest_Address"
              placeholder="data"
              v-model="DestData.Address"
              required
            />
            <label for="Dest_Address">Endereço</label>
          </div>

          <div class="form-floating">
            <input
              type="number"
              class="form-control"
              id="Dest_Addnum"
              placeholder="data"
              min="0"
              v-model="DestData.Num"
              required
            />
            <label for="Dest_Addnum">Número</label>
          </div>
        </div>

        <div class="multSpace">
          <div class="form-floating">
            <input
              type="text"
              class="form-control"
              id="Dest_Address"
              placeholder="data"
              v-maska="'#####-###'"
              min="0"
              max="8"
              v-model="DestData.Cep"
              required
            />
            <label for="Dest_Address">CEP</label>
          </div>

          <div class="moreSpace form-floating">
            <input
              type="text"
              class="form-control"
              id="Dest_City"
              placeholder="data"
              v-model="DestData.City"
              required
            />
            <label for="Dest_City">Cidade</label>
          </div>
        </div>
      </section>

      <section>
        <p><i class="bi bi-info-square-fill"></i> Dados do orçamento</p>
        <div class="multSpace">
          <div class="form-floating">
            <input
              type="number"
              class="form-control"
              id="BudgetID"
              v-maska="'###'"
              placeholder="data"
              v-model="BudgetData.ID"
              required
            />
            <label for="BudgetID">Número do orçamento</label>
          </div>

          <div class="form-floating">
            <input
              type="date"
              class="form-control"
              id="Budget_Valid"
              placeholder="data"
              v-model="BudgetData.Valid"
            />
            <label for="Budget_Valid">Validade</label>
          </div>
          <div class="form-floating">
            <input
              type="date"
              class="form-control"
              id="Budget_Warranty"
              placeholder="data"
              v-model="BudgetData.Warranty"
            />
            <label for="Budget_Warranty">Garantia</label>
          </div>
        </div>

        <div class="multSpace">
          <div class="moreSpace form-floating">
            <select
              class="form-select"
              id="Budget_Payment"
              aria-label="payment"
              placeholder="data"
              v-model="BudgetData.payment"
              required
            >
              <option
                v-for="option in payment"
                selected="{{option == BudgetData.Payment}}"
                :key="option"
              >
                {{ option }}
              </option>
            </select>
            <label for="Budget_Payment">Forma de Pagamento</label>
          </div>

          <div class="form-floating">
            <input
              type="number"
              class="form-control"
              id="Budget_fractPay"
              min="1"
              placeholder="data"
              v-model="BudgetData.PayTimes"
              required
            />
            <label for="Budget_fractPay">Quantas vezes</label>
          </div>
        </div>

        <div class="form-floating">
          <textarea
            class="form-control"
            placeholder="data"
            id="Budget_Desc"
            maxlength="250"
            style="height: 100px"
            v-model="BudgetData.Info"
          ></textarea>
          <label for="Budget_Desc">Informações Adicionais</label>
        </div>

        <div class="areaAddRemoveItem">
          <button
            id="addProd"
            class="addValue"
            type="button"
            v-on:click="addItem('product')"
            v-show="prodCount < 10"
          >
            Adicionar Produto ({{ prodCount }}/10)
            <i class="bi bi-plus-circle"></i>
          </button>
          <button
            id="removeProd"
            class="addValue"
            v-if="prodCount > 0"
            type="button"
            v-on:click="removeItem('product')"
          >
            Remover Produto <i class="bi bi-dash-circle"></i>
          </button>
        </div>
        <hr />

        <div
          class="product"
          :show="prodCount > 0"
          v-for="i in prodCount"
          :key="i"
        >
          <div class="form-floating">
            <input
              type="text"
              class="form-control"
              id="Prod_Name"
              maxlength="20"
              placeholder="data"
              v-model="ProdData.Name[i - 1]"
              required
            />
            <label for="Prod_Name">Nome do Produto</label>
          </div>

          <div class="multSpace">
            <div class="form-floating">
              <input
                type="text"
                class="form-control"
                id="Prod_value"
                min="0"
                placeholder="data"
                v-model="ProdData.Value[i - 1]"
                v-maska="[
                  'R$ #,##',
                  'R$ ##,##',
                  'R$ ###,##',
                  'R$ #.###,##',
                  'R$ ##.###,##',
                  'R$ ###.###,##',
                  'R$ #.###.###,##',
                  'R$ ##.###.###,##',
                  'R$ ###.###.###,##',
                ]"
                required
              />
              <label for="Prod_value">Valor</label>
            </div>

            <div class="form-floating">
              <input
                type="number"
                class="form-control"
                id="Prod_count"
                placeholder="data"
                min="1"
                v-model="ProdData.Count[i - 1]"
                required
              />
              <label for="Prod_count">Quantidade</label>
            </div>
          </div>
          <span>Medida:</span>
          <select
            class="form-select"
            aria-label="Metrics"
            v-model="ProdData.Metric[i - 1]"
            required
          >
            <option
              v-for="option in metricOptionProd"
              selected="{{option == ProdData.Metric[i - 1]}}"
              :key="option"
            >
              {{ option }}
            </option>
          </select>
        </div>

        <div class="areaAddRemoveItem" style="margin-top: 3%">
          <button
            id="addServ"
            class="addValue"
            type="button"
            v-on:click="addItem('service')"
            v-show="servCount < 10"
          >
            Adicionar Serviço ({{ servCount }}/10)<i
              class="bi bi-plus-circle"
            ></i>
          </button>
          <button
            id="removeServ"
            class="addValue"
            v-if="servCount > 0"
            type="button"
            v-on:click="removeItem('service')"
          >
            Remover Serviço <i class="bi bi-dash-circle"></i>
          </button>
        </div>
        <hr />

        <div
          class="service"
          :show="servCount > 0"
          v-for="i in servCount"
          :key="i"
        >
          <div class="form-floating">
            <input
              type="text"
              class="form-control"
              id="Serv_Name"
              maxlength="20"
              placeholder="data"
              v-model="ServData.Name[i - 1]"
              required
            />
            <label for="Serv_Name">Serviço</label>
          </div>

          <div class="multSpace">
            <div class="form-floating">
              <input
                type="text"
                class="form-control"
                id="Serv_value"
                placeholder="data"
                min="0"
                max="15"
                v-model="ServData.Value[i - 1]"
                v-maska="[
                  'R$ #,##',
                  'R$ ##,##',
                  'R$ ###,##',
                  'R$ #.###,##',
                  'R$ ##.###,##',
                  'R$ ###.###,##',
                  'R$ #.###.###,##',
                  'R$ ##.###.###,##',
                  'R$ ###.###.###,##',
                ]"
                required
              />
              <label for="Serv_value">Valor Base</label>
            </div>

            <div class="form-floating">
              <input
                type="number"
                class="form-control"
                id="Serv_count"
                placeholder="data"
                min="1"
                v-model="ServData.Count[i - 1]"
                required
              />
              <label for="Serv_count">Quantidade</label>
            </div>
          </div>
          <span>Medida:</span>
          <select class="form-select" v-model="ServData.Metric[i - 1]" required>
            <option
              v-for="option in metricOptionServ"
              selected="{{option == ServData.Metric[i - 1]}}"
              :key="option"
            >
              {{ option }}
            </option>
          </select>
        </div>
      </section>

      <div
        class="alert alert-danger w-75 text-center"
        role="alert"
        v-show="errorEmpty"
      >
        Há campos importantes não estão preenchidos!
      </div>

      <button
        id="DonwloadPDF"
        type="submit"
        class="DownloadPDF btn btn-success"
        v-on:click.prevent="SavePDF"
      >
        Baixar PDF <i class="bi bi-download"></i>
      </button>
    </form>

    <footer id="footer">
      <div class="descFooter">
        Projeto desenvolvido utilizando: VueJs, Boostrap, CSS e Javascript
        <span>Mande um Email para: <b>rafaelwolowitz@gmail.com</b></span>
        <span
          >Link do repositório:
          <a href="https://github.com/YoungC0DE/PDFgen" target="_blank"
            >Github/YoungC0DE <i class="bi bi-box-arrow-up-right"></i></a
        ></span>
      </div>
      <div class="AjudaEu">
        Me ajude a evoluir o projeto =)<br />
        Chave Pix:
        <img class="pix" src="./assets/pix-pro-pai.png" width="auto" />
      </div>
    </footer>
  </div>
</template>

<script>
import jsPDF from "jspdf";
import { maska } from "maska";

export default {
  name: "App",
  data() {
    return {
      EmitData: {}, // Dados do Emissor
      DestData: {}, // Dados do Destinatario
      BudgetData: {}, // Dados do orçamento
      ProdData: {
        // Dados do produto
        Name: [],
        Value: [],
        Count: [],
        Metric: [],
      },
      ServData: {
        // Dados do serviço
        Name: [],
        Value: [],
        Count: [],
        Metric: [],
      },
      prodCount: 0, // Contagem de quantos produtos foram incluidos
      servCount: 0, // Contagem de quantos serviços foram incluidos
      hasLogo: false, // Valida se tem logo
      errorEmpty: false, // Valida se teve campo vazio ou algum erro
      // Valores para os campos de option no html
      payment: ["Boleto", "Cartão", "Pix", "Transferência", "Outros"],
      metricOptionProd: ["un", "m", "m²", "cm", "km", "g", "L", "kg"],
      metricOptionServ: ["Minuto", "Hora", "Dia", "Mês", "Ano"],
    };
  },
  directives: { maska }, // Mascara
  methods: {
    // Metodo para adicionar produto/serviço
    addItem(prop) {
      prop == "product" ? (this.prodCount += 1) : (this.servCount += 1);
    },
    // Metodo para remover produto/serviço
    removeItem(prop) {
      prop == "product" ? (this.prodCount -= 1) : (this.servCount -= 1);
    },
    // Metodo para validar campos vazios
    validing() {
      var forms = document.querySelectorAll(".needs-validation");
      forms.forEach((form) => form.classList.add("was-validated"));

      // validing inputs
      var inputData =
        !this.EmitData.Name ||
        !this.EmitData.Tel ||
        !this.EmitData.Email ||
        !this.DestData.Name ||
        !this.DestData.Tel ||
        !this.DestData.Address ||
        !this.DestData.City ||
        !this.DestData.Cep ||
        !this.ProdData.Name ||
        !this.ServData.Name ||
        !this.BudgetData.ID ||
        !this.BudgetData.PayTimes;

      // Caso algum campo esteja vazio ele retorna verdadeiro na variavel
      if (inputData) this.errorEmpty = true;
      else this.errorEmpty = false;
    },

    SavePDF() {
      //this.validing();
      //if (this.errorEmpty) return;

      // Instancia do jsPDF
      var pdf = new jsPDF("p", "mm", [297, 210]),
        font = "./src/fonts/CourierPrime-Regular.ttf",
        fontBold = "./src/fonts/CourierPrime-Bold.ttf",
        // ======== Gerando data do cabeçalho =========
        DateToday = new Date(),
        hour = DateToday.getHours().toString(),
        minutes = DateToday.getMinutes().toString(),
        seconds = DateToday.getSeconds().toString(),
        day = DateToday.getDate().toString().padStart(2, "0"),
        month = (DateToday.getMonth() + 1).toString().padStart(2, "0"),
        year = DateToday.getFullYear().toString(),
        // =============================================
        image = this.BudgetData.logo, // Pegando logo
        // Listagem de meses para validação
        months = [
          "Janeiro",
          "Fevereiro",
          "Março",
          "Abril",
          "Maio",
          "Junho",
          "Julho",
          "Agosto",
          "Setembro",
          "Outubro",
          "Novembro",
          "Dezembro",
        ];

      // =========== Definindo cabeçalho ==================
      pdf.addFileToVFS("CourierPrime-Regular.ttf", font);
      pdf.addFileToVFS("CourierPrime-Bold.ttf", fontBold);
      pdf.addFont("CourierPrime-Regular.ttf", "Courier");
      pdf.addFont("CourierPrime-Bold.ttf", "CourierBold");
      pdf.setFont("Courier", "");
      pdf.setFontSize(8);
      pdf.text(`${day}/${month}/${year} ${hour}:${minutes}:${seconds}`, 10, 9);
      pdf.text("Orçamento", 185, 9);
      // ===================================================

      // Se tiver logo, o layout vai ser outro.
      if (this.hasLogo) {
        pdf.line(200, 10, 10, 10, "DF");
        pdf.setLineWidth(0.2);
        pdf.rect(15, 15, 45, 45);
        pdf.addImage(image, "PNG", 17.5, 17.5, 40, 40);
        pdf.setFontSize(40);
        pdf.setFont("Courier", "Bold");
        pdf.text(`Orçamento Nº ${this.BudgetData.ID}`, 70, 25);

        pdf.setFontSize(12);
        pdf.setFont("Courier", "Bold");
        pdf.text("Dados do Emissor", 70, 38, "left");
        pdf.setFont("Courier", "");
        pdf.text(`Nome: ${this.EmitData.Name}`, 70, 45, "left");
        pdf.text(`Contato: ${this.EmitData.Tel}`, 70, 50, "left");
        pdf.text(`Email: ${this.EmitData.Email}`, 70, 55, "left");
        if (this.EmitData.Site) {
          pdf.text(`Site: ${this.EmitData.Site}`, 70, 60, "left");
        }
        pdf.line(200, 65, 10, 65, "DF");
      } else {
        pdf.setFontSize(40);
        pdf.setFont("Courier", "Bold");
        pdf.text("Orçamento", 15, 25);
        pdf.setFontSize(20);
        pdf.text(
          `Nº ${this.BudgetData.ID == undefined ? "1" : this.BudgetData.ID}`,
          17,
          35
        );

        pdf.setFontSize(12);
        pdf.setFont("Courier", "Bold");
        pdf.text("Dados do Emissor", 200, 20, "right");
        pdf.setFont("Courier", "");
        pdf.text(`Nome: ${this.EmitData.Name}`, 200, 25, "right");
        pdf.text(`Contato: ${this.EmitData.Tel}`, 200, 30, "right");
        pdf.text(`Email: ${this.EmitData.Email}`, 200, 35, "right");
        if (this.EmitData.Site)
          pdf.text(`Site: ${this.EmitData.Site}`, 200, 40, "right");
        pdf.line(200, 42, 10, 42, "DF");
      }

      pdf.setFontSize(11);
      pdf.setFont("Courier", "Bold");
      pdf.text("Data da emissão: ", 10, this.hasLogo ? 72 : 48, "left");
      pdf.setFont("Courier", "");
      pdf.text(
        `${day} de ${months[DateToday.getMonth()]} de ${year}`,
        48,
        this.hasLogo ? 72 : 48
      );
      pdf.setFont("Courier", "Bold");
      pdf.text("Situação do Orçamento: ", 105, this.hasLogo ? 72 : 48);
      pdf.setFont("Courier", "");
      pdf.text("Aguardando Retorno.", 158, this.hasLogo ? 72 : 48);

      pdf.setFontSize(12);
      pdf.setFont("Courier", "Bold");
      pdf.text("Dados do Cliente", 10, this.hasLogo ? 83 : 63);
      pdf.setFont("Courier", "");
      pdf.text(
        `Nome: ${this.DestData.Name}`,
        10,
        this.hasLogo ? 90 : 70,
        "left"
      );
      pdf.text(
        `Contato: ${this.DestData.Tel}`,
        10,
        this.hasLogo ? 95 : 75,
        "left"
      );
      pdf.text(
        `Endereço: ${this.DestData.Address}, ${this.DestData.Num} - ${this.DestData.City} `,
        10,
        this.hasLogo ? 100 : 80,
        "left"
      );
      pdf.text(
        `CEP: ${this.DestData.Cep}`,
        10,
        this.hasLogo ? 105 : 85,
        "left"
      );
      this.hasLogo
        ? pdf.line(200, 110, 10, 110, "DF")
        : pdf.line(200, 90, 10, 90, "DF");

      // Caso tenha produtos ou serviços no orçamento, o layout vai mudar.
      if (this.prodCount > 0 || this.servCount > 0) {
        var TotalValueServ = 0,
          TotalValueProd = 0,
          QtaProd = 0,
          QtaServ = 0;

        // Aqui validamos se tem produtos e serviços. Caso tenha os dois o layout muda pra ficar responsivo.
        if (this.prodCount > 0 && this.servCount > 0) {
          for (let x = 0; x < this.prodCount; x++) {
            pdf.setFont("Courier", "Bold");
            pdf.text("Produto:", 10, this.hasLogo ? 120 + x * 5 : 100 + x * 5);
            pdf.setFont("Courier", "");
            pdf.text(
              `${this.ProdData.Name[x]} .... ${this.ProdData.Value[x]} .... Qtda: ${this.ProdData.Count[x]} ${this.ProdData.Metric[x]}`,
              32,
              this.hasLogo ? 120 + x * 5 : 100 + x * 5
            );
            let totalTemp = this.ProdData.Value[x]
              .toString()
              .replaceAll("R$ ", "")
              .replaceAll(".", "")
              .replaceAll(",", "");
            TotalValueProd += parseInt(
              totalTemp.substring(0, totalTemp.length - 2)
            );
            QtaProd += this.ProdData.Count[x];
          }

          for (let y = 0; y < this.servCount; y++) {
            if (this.prodCount <= 2) {
              pdf.setFont("Courier", "Bold");
              pdf.text(
                "Serviço:",
                10,
                this.hasLogo ? 130 + y * 5 : 110 + y * 5
              );
              pdf.setFont("Courier", "");
              pdf.text(
                `${this.ServData.Name[y]} .... ${this.ServData.Value[y]} /${this.ServData.Count[y]} ${this.ServData.Metric[y]}`,
                32,
                this.hasLogo ? 130 + y * 5 : 110 + y * 5
              );
            } else if (this.prodCount >= 3 && this.prodCount <= 6) {
              pdf.setFont("Courier", "Bold");
              pdf.text(
                "Serviço:",
                10,
                this.hasLogo
                  ? 135 + y * 5 + this.prodCount * 2
                  : 115 + y * 5 + this.prodCount * 2
              );
              pdf.setFont("Courier", "");
              pdf.text(
                `${this.ServData.Name[y]} .... ${this.ServData.Value[y]} /${this.ServData.Count[y]} ${this.ServData.Metric[y]}(s)`,
                32,
                this.hasLogo
                  ? 135 + y * 5 + this.prodCount * 2
                  : 115 + y * 5 + this.prodCount * 2
              );
            } else if (this.prodCount > 6 && this.prodCount <= 10) {
              pdf.setFont("Courier", "Bold");
              pdf.text(
                "Serviço:",
                10,
                this.hasLogo
                  ? 140 + y * 5 + this.prodCount * 3
                  : 115 + y * 5 + this.prodCount * 4
              );
              pdf.setFont("Courier", "");
              pdf.text(
                `${this.ServData.Name[y]} .... ${this.ServData.Value[y]} /${this.ServData.Count[y]} ${this.ServData.Metric[y]}(s)`,
                32,
                this.hasLogo
                  ? 140 + y * 5 + this.prodCount * 3
                  : 115 + y * 5 + this.prodCount * 4
              );
            }
            let totalTemp = this.ServData.Value[y]
              .toString()
              .replaceAll("R$ ", "")
              .replaceAll(".", "")
              .replaceAll(",", "");
            TotalValueServ += parseInt(
              totalTemp.substring(0, totalTemp.length - 2)
            );
            this.ServData.Count[y] > 1
              ? (QtaServ += this.ServData.Count[y])
              : 0;
          }
          let total = TotalValueProd * QtaProd + TotalValueServ * QtaServ;
          console.log(TotalValueServ, TotalValueProd);
          total = total.toLocaleString("pt-br", {
            style: "currency",
            currency: "BRL",
            minimumFractionDigits: 2,
          });

          pdf.setFont("Courier", "Bold");
          pdf.setFontSize(18);
          pdf.text(
            "Valor Total",
            195,
            this.hasLogo
              ? 125 + (this.servCount + this.prodCount) * 5
              : 110 + (this.servCount + this.prodCount) * 5,
            "right"
          );
          pdf.setFont("Courier", "");
          pdf.setFontSize(16);
          pdf.text(
            total,
            195,
            this.hasLogo
              ? 132 + (this.servCount + this.prodCount) * 5
              : 118 + (this.servCount + this.prodCount) * 5,
            "right"
          );

          this.hasLogo
            ? pdf.line(
                200,
                140 + (this.servCount + this.prodCount) * 5,
                10,
                140 + (this.servCount + this.prodCount) * 5,
                "DF"
              )
            : pdf.line(
                200,
                122 + (this.servCount + this.prodCount) * 5,
                10,
                122 + (this.servCount + this.prodCount) * 5,
                "DF"
              );

          if (this.BudgetData.Info) {
            pdf.setFont("Courier", "Bold");
            pdf.setFontSize(12);
            pdf.text(
              "Mais detalhes",
              10,
              this.hasLogo
                ? 150 + (this.servCount + this.prodCount) * 5
                : 130 + (this.servCount + this.prodCount) * 5
            );
            pdf.setFont("Courier", "");
            var InfoBudget = pdf.splitTextToSize(this.BudgetData.Info, 195);
            pdf.text(
              InfoBudget,
              10,
              this.hasLogo
                ? 155 + (this.servCount + this.prodCount) * 5
                : 135 + (this.servCount + this.prodCount) * 5
            );

            if (this.hasLogo) {
              pdf.setFont("Courier", "Bold");
              pdf.text(
                "Validade do orçamento:",
                10,
                190 + (this.servCount + this.prodCount) * 5
              );
              pdf.setFont("Courier", "");
              pdf.text(
                this.BudgetData.Valid ? this.BudgetData.Valid : "indefinido.",
                70,
                190 + (this.servCount + this.prodCount) * 5
              );

              pdf.setFont("Courier", "Bold");
              pdf.text(
                "Garantia:",
                10,
                200 + (this.servCount + this.prodCount) * 5
              );
              pdf.setFont("Courier", "");
              pdf.text(
                this.BudgetData.Warranty
                  ? this.BudgetData.Warranty
                  : "sem garantia.",
                70,
                200 + (this.servCount + this.prodCount) * 5
              );

              pdf.setFont("Courier", "Bold");
              pdf.text(
                "Forma de Pagamento:",
                10,
                210 + (this.servCount + this.prodCount) * 5
              );
              pdf.setFont("Courier", "");
              pdf.text(
                `${this.BudgetData.payment} /${this.BudgetData.PayTimes}x`,
                70,
                210 + (this.servCount + this.prodCount) * 5
              );
            } else {
              pdf.setFont("Courier", "Bold");
              pdf.text(
                "Validade do orçamento:",
                10,
                160 + (this.servCount + this.prodCount) * 5
              );
              pdf.setFont("Courier", "");
              pdf.text(
                this.BudgetData.Valid ? this.BudgetData.Valid : "indefinido.",
                70,
                160 + (this.servCount + this.prodCount) * 5
              );

              pdf.setFont("Courier", "Bold");
              pdf.text(
                "Garantia:",
                10,
                170 + (this.servCount + this.prodCount) * 5
              );
              pdf.setFont("Courier", "");
              pdf.text(
                this.BudgetData.Warranty
                  ? this.BudgetData.Warranty
                  : "sem garantia.",
                70,
                170 + (this.servCount + this.prodCount) * 5
              );

              pdf.setFont("Courier", "Bold");
              pdf.text(
                "Forma de Pagamento:",
                10,
                180 + (this.servCount + this.prodCount) * 5
              );
              pdf.setFont("Courier", "");
              pdf.text(
                `${this.BudgetData.payment} /${this.BudgetData.PayTimes}x`,
                70,
                180 + (this.servCount + this.prodCount) * 5
              );
            }
          } else {
            if (this.hasLogo) {
              pdf.setFontSize(12);
              pdf.setFont("Courier", "Bold");
              pdf.text(
                "Validade do orçamento:",
                10,
                160 + (this.servCount + this.prodCount) * 5
              );
              pdf.setFont("Courier", "");
              pdf.text(
                this.BudgetData.Valid ? this.BudgetData.Valid : "indefinido.",
                70,
                160 + (this.servCount + this.prodCount) * 5
              );

              pdf.setFont("Courier", "Bold");
              pdf.text(
                "Garantia:",
                10,
                170 + (this.servCount + this.prodCount) * 5
              );
              pdf.setFont("Courier", "");
              pdf.text(
                this.BudgetData.Warranty
                  ? this.BudgetData.Warranty
                  : "sem garantia.",
                70,
                170 + (this.servCount + this.prodCount) * 5
              );

              pdf.setFont("Courier", "Bold");
              pdf.text(
                "Forma de Pagamento:",
                10,
                180 + (this.servCount + this.prodCount) * 5
              );
              pdf.setFont("Courier", "");
              pdf.text(
                `${this.BudgetData.payment} /${this.BudgetData.PayTimes}x`,
                70,
                180 + (this.servCount + this.prodCount) * 5
              );
            } else {
              pdf.setFontSize(12);
              pdf.setFont("Courier", "Bold");
              pdf.text(
                "Validade do orçamento:",
                10,
                130 + (this.servCount + this.prodCount) * 5
              );
              pdf.setFont("Courier", "");
              pdf.text(
                this.BudgetData.Valid ? this.BudgetData.Valid : "indefinido.",
                70,
                130 + (this.servCount + this.prodCount) * 5
              );

              pdf.setFont("Courier", "Bold");
              pdf.text(
                "Garantia:",
                10,
                140 + (this.servCount + this.prodCount) * 5
              );
              pdf.setFont("Courier", "");
              pdf.text(
                this.BudgetData.Warranty
                  ? this.BudgetData.Warranty
                  : "sem garantia.",
                70,
                140 + (this.servCount + this.prodCount) * 5
              );

              pdf.setFont("Courier", "Bold");
              pdf.text(
                "Forma de Pagamento:",
                10,
                150 + (this.servCount + this.prodCount) * 5
              );
              pdf.setFont("Courier", "");
              pdf.text(
                `${this.BudgetData.payment} /${this.BudgetData.PayTimes}x`,
                70,
                150 + (this.servCount + this.prodCount) * 5
              );
            }
          }
          //=============================== tem produto e serviço - fim =========================
        } else {
          // Caso tenha somente produto, ou somente serviço, o layout é outro.
          for (let x = 0; x < this.prodCount; x++) {
            pdf.setFont("Courier", "Bold");
            pdf.text("Produto:", 10, this.hasLogo ? 120 + x * 5 : 100 + x * 5);
            pdf.setFont("Courier", "");
            pdf.text(
              `${this.ProdData.Name[x]} ..... ${this.ProdData.Value[x]} .... Qtda: ${this.ProdData.Count[x]} ${this.ProdData.Metric[x]}`,
              32,
              this.hasLogo ? 120 + x * 5 : 100 + x * 5
            );
            let totalTemp = this.ProdData.Value[x]
              .toString()
              .replaceAll("R$ ", "")
              .replaceAll(".", "")
              .replaceAll(",", "");
            TotalValueProd += parseInt(
              totalTemp.substring(0, totalTemp.length - 2)
            );
            QtaProd += this.ProdData.Count[x];
          }
          // ================= so produto - fim ================
          // =============== Somente serviço ===================
          for (let y = 0; y < this.servCount; y++) {
            pdf.setFont("Courier", "Bold");
            pdf.text("Serviço:", 10, this.hasLogo ? 120 + y * 5 : 100 + y * 5);
            pdf.setFont("Courier", "");
            pdf.text(
              `${this.ServData.Name[y]} .... ${this.ServData.Value[y]} /${this.ServData.Count[y]} ${this.ServData.Metric[y]}(s)`,
              32,
              this.hasLogo ? 120 + y * 5 : 100 + y * 5
            );
            let totalTemp = this.ServData.Value[y]
              .toString()
              .replaceAll("R$ ", "")
              .replaceAll(".", "")
              .replaceAll(",", "");
            TotalValueServ += parseInt(
              totalTemp.substring(0, totalTemp.length - 2)
            );
            this.ServData.Count[y] > 1
              ? (QtaServ += this.ServData.Count[y])
              : 0;
          }
          // ================= so serviço - fim ================
          // Definindo o valor total de produtos e serviços
          let total = TotalValueProd * QtaProd + TotalValueServ * QtaServ;
          total = total.toLocaleString("pt-br", {
            style: "currency",
            currency: "BRL",
            minimumFractionDigits: 2,
          });
          // =============================================
          // ================= Valor total dos produtos ==========
          if (this.prodCount > 0) {
            pdf.setFont("Courier", "Bold");
            pdf.setFontSize(18);
            pdf.text(
              "Valor Total",
              195,
              this.hasLogo
                ? 125 + this.prodCount * 5
                : 110 + this.prodCount * 5,
              "right"
            );
            pdf.setFont("Courier", "");
            pdf.setFontSize(16);
            pdf.text(
              total,
              195,
              this.hasLogo
                ? 132 + this.prodCount * 5
                : 118 + this.prodCount * 5,
              "right"
            );

            this.hasLogo
              ? pdf.line(
                  200,
                  140 + this.prodCount * 5,
                  10,
                  140 + this.prodCount * 5,
                  "DF"
                )
              : pdf.line(
                  200,
                  122 + this.prodCount * 5,
                  10,
                  122 + this.prodCount * 5,
                  "DF"
                );
          }
          // ===============================================
          // ================= Valor total dos Serviços ==========
          if (this.servCount > 0) {
            pdf.setFont("Courier", "Bold");
            pdf.setFontSize(18);
            pdf.text(
              "Valor Total",
              195,
              this.hasLogo
                ? 125 + this.servCount * 5
                : 110 + this.servCount * 5,
              "right"
            );
            pdf.setFont("Courier", "");
            pdf.setFontSize(16);
            pdf.text(
              total,
              195,
              this.hasLogo
                ? 132 + this.servCount * 5
                : 118 + this.servCount * 5,
              "right"
            );

            this.hasLogo
              ? pdf.line(
                  200,
                  140 + this.servCount * 5,
                  10,
                  140 + this.servCount * 5,
                  "DF"
                )
              : pdf.line(
                  200,
                  122 + this.servCount * 5,
                  10,
                  122 + this.servCount * 5,
                  "DF"
                );
          }

          // Se tiver alguma descrição, o layout muda
          if (this.BudgetData.Info) {
            pdf.setFont("Courier", "Bold");
            pdf.setFontSize(12);
            pdf.text(
              "Mais detalhes",
              10,
              this.hasLogo
                ? 150 + (this.servCount + this.prodCount) * 5
                : 130 + (this.servCount + this.prodCount) * 5
            );
            pdf.setFont("Courier", "");
            var InfoBudget = pdf.splitTextToSize(this.BudgetData.Info, 195);
            pdf.text(
              InfoBudget,
              10,
              this.hasLogo
                ? 157 + (this.servCount + this.prodCount) * 5
                : 137 + (this.servCount + this.prodCount) * 5
            );
            // this.hasLogo
            //   ? pdf.line(200, (180 + (this.servCount * 5)), 10, (180 + (this.servCount * 5)), 'DF')
            //   : pdf.line(200, (160 + (this.servCount * 5)), 10, (160 + (this.servCount * 5)), 'DF')

            if (this.hasLogo) {
              pdf.setFont("Courier", "Bold");
              pdf.text("Validade do orçamento:", 10, 190 + this.servCount * 5);
              pdf.setFont("Courier", "");
              pdf.text(
                this.BudgetData.Valid ? this.BudgetData.Valid : "indefinido.",
                70,
                190 + this.servCount * 5
              );

              pdf.setFont("Courier", "Bold");
              pdf.text("Garantia:", 10, 200 + this.servCount * 5);
              pdf.setFont("Courier", "");
              pdf.text(
                this.BudgetData.Warranty
                  ? this.BudgetData.Warranty
                  : "sem garantia.",
                70,
                200 + this.servCount * 5
              );

              pdf.setFont("Courier", "Bold");
              pdf.text("Forma de Pagamento:", 10, 210 + this.servCount * 5);
              pdf.setFont("Courier", "");
              pdf.text(
                `${this.BudgetData.payment} /${this.BudgetData.PayTimes}x`,
                70,
                210 + this.servCount * 5
              );
            } else {
              pdf.setFont("Courier", "Bold");
              pdf.text("Validade do orçamento:", 10, 170 + this.servCount * 5);
              pdf.setFont("Courier", "");
              pdf.text(
                this.BudgetData.Valid ? this.BudgetData.Valid : "indefinido.",
                70,
                170 + this.servCount * 5
              );

              pdf.setFont("Courier", "Bold");
              pdf.text("Garantia:", 10, 180 + this.servCount * 5);
              pdf.setFont("Courier", "");
              pdf.text(
                this.BudgetData.Warranty
                  ? this.BudgetData.Warranty
                  : "sem garantia.",
                70,
                180 + this.servCount * 5
              );

              pdf.setFont("Courier", "Bold");
              pdf.text("Forma de Pagamento:", 10, 190 + this.servCount * 5);
              pdf.setFont("Courier", "");
              pdf.text(
                `${this.BudgetData.payment} /${this.BudgetData.PayTimes}x`,
                70,
                190 + this.servCount * 5
              );
            }
          } else {
            if (this.hasLogo) {
              pdf.setFontSize(12);
              pdf.setFont("Courier", "Bold");
              pdf.text("Validade do orçamento:", 10, 160 + this.prodCount * 5);
              pdf.setFont("Courier", "");
              pdf.text(
                this.BudgetData.Valid ? this.BudgetData.Valid : "indefinido.",
                70,
                160 + this.prodCount * 5
              );

              pdf.setFont("Courier", "Bold");
              pdf.text("Garantia:", 10, 170 + this.prodCount * 5);
              pdf.setFont("Courier", "");
              pdf.text(
                this.BudgetData.Warranty
                  ? this.BudgetData.Warranty
                  : "sem garantia.",
                70,
                170 + this.prodCount * 5
              );

              pdf.setFont("Courier", "Bold");
              pdf.text("Forma de Pagamento:", 10, 180 + this.prodCount * 5);
              pdf.setFont("Courier", "");
              pdf.text(
                `${this.BudgetData.payment} /${this.BudgetData.PayTimes}x`,
                70,
                180 + this.prodCount * 5
              );
            } else {
              pdf.setFontSize(12);
              pdf.setFont("Courier", "Bold");
              pdf.text("Validade do orçamento:", 10, 140 + this.prodCount * 5);
              pdf.setFont("Courier", "");
              pdf.text(
                this.BudgetData.Valid ? this.BudgetData.Valid : "indefinido.",
                70,
                140 + this.prodCount * 5
              );

              pdf.setFont("Courier", "Bold");
              pdf.text("Garantia:", 10, 150 + this.prodCount * 5);
              pdf.setFont("Courier", "");
              pdf.text(
                this.BudgetData.Warranty
                  ? this.BudgetData.Warranty
                  : "sem garantia.",
                70,
                150 + this.prodCount * 5
              );

              pdf.setFont("Courier", "Bold");
              pdf.text("Forma de Pagamento:", 10, 160 + this.prodCount * 5);
              pdf.setFont("Courier", "");
              pdf.text(
                `${this.BudgetData.payment} /${this.BudgetData.PayTimes}x`,
                70,
                160 + this.prodCount * 5
              );
            }
          }
        }
      }

      pdf.setFontSize(8);
      pdf.setFont("Courier", "Bold");
      pdf.setTextColor(56, 84, 153);
      pdf.textWithLink("Gerado por PDFgen", 10, 285, {
        url: "https://pdf-gen-rho.vercel.app/",
      });
      pdf.text("https://pdf-gen-rho.vercel.app/", 200, 285, "right");

      pdf.save("Orçamento.pdf");
    },
    injectNewPic() {
      localStorage.setItem(
        "logo",
        window.URL.createObjectURL(
          document.getElementById("Emit_logo").files[0]
        )
      );
      this.BudgetData.logo = localStorage.getItem("logo");
      this.hasLogo =
        document.getElementById("Emit_logo").files[0] != null ||
        localStorage.getItem("logo");
    },
    deletLogo() {
      this.hasLogo = false;
      localStorage.removeItem("logo");
      console.log(this.hasLogo, localStorage.getItem("logo"));
    },
  },
};
</script>
