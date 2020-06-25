@using YellowlingAPI.Models.Entidades;
@model YellowlingAPI.Models.ViewModel.FormularioPrincipalViewModel


@{ Layout = "~/Views/Shared/_LayoutFormulario.cshtml"; }

<link href="~/css/cardprincipal.css" rel="stylesheet" />
<link href="~/css/uploadfiles.css" rel="stylesheet" />

<div class="android-screen-section mdl-typography--text-center" style="width:100% !important;margin-bottom: -40px;">
    <div class="mdl-typography--display-2 mdl-typography--font-thin">Traduções Profissionais Facilitadas</div>
    <div class="mdl-card__supporting-text">
        <span class="mdl-typography--font-light mdl-typography--subhead">
            Várias pessoas ao redor do mundo escolheram fazer suas traduções com tradutores profissionais, seja mais um cliente satisfeito. Junte-se a nós.
        </span>
    </div>
</div>

<form asp-controller="Solicitacao" asp-action="Realizar" data-ajax="true" data-ajax-method="”POST”" data-ajax-mode="replace" data-ajax-update="#conteudo" method="POST" enctype="multipart/form-data" id="formulario">

    <input type="hidden" asp-for="Documentos" />
    <div class="container pt-4">
        <div class="android-screen-section mdl-typography--text-center">
            <div class="mdl-card mdl-shadow--3dp col-md-12" style="padding:initial;">
                <div class="mdl-card__title">
                    <h4 class="mdl-typography--font-light mdl-typography--subhead">
                        Preencha o formulário para receber um orçamento detalhado!
                    </h4>
                </div>
                <div class="mdl-card__supporting-text ml-3 pr-5" style="width:100% !important;">
                    <div id="validacao">
                        @Html.ValidationSummary()
                    </div>

                    <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label mdl-cell mdl-cell--6-col mdl-cell--6-col-tablet mdl-cell--12-col-phone">
                        <input class="mdl-textfield__input" type="email" asp-for="Email" placeholder="Digite seu email">
                        <label class="mdl-textfield__label" asp-for="Email" style="top:-15px;"></label>
                        <span asp-validation-for="Email" class="mdl-textfield__error"></span>
                    </div>

                    <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label mdl-cell mdl-cell--6-col mdl-cell--6-col-tablet mdl-cell--12-col-phone">
                        <select asp-for="TipoDeSolicitacao" class="mdl-textfield__input" asp-items="Html.GetEnumSelectList<TiposDeSolicitacao>()" onchange="MostrarAdicionais()"></select>
                        <label class="mdl-textfield__label" style="top:-15px;">Tipo de Tradução</label>
                    </div>

                    <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label mdl-cell mdl-cell--6-col mdl-cell--6-col-tablet mdl-cell--12-col-phone" style="position:relative;">
                        <input asp-for="QuantidadeDeCaracteres" class="mdl-textfield__input" type="number" required onchange="BuscarDatasDisponiveis()" />
                        <div class="word-count-or-upload__upload">
                            <span class="word-count-or-upload__or">ou</span>
                            <div class="word-count-or-upload__button-wrapper">
                                <button type="button" id="sendDocs" class="button--compact btn btn-outline-info" data-toggle="modal" data-target="#exampleModal">Enviar Arquivos</button>
                            </div>
                        </div>
                        <label asp-for="QuantidadeDeCaracteres" class="mdl-textfield__label" style="top:-15px;"></label>
                    </div>


                    <div class="row text-center adicionais" style="display:block;">
                        <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label mdl-cell mdl-cell--6-col mdl-cell--6-col-tablet mdl-cell--12-col-phone">
                            <label class="mdl-checkbox mdl-js-checkbox mdl-js-ripple-effect" for="checkbox-2">
                                <input type="checkbox" id="checkbox-2" class="mdl-checkbox__input" checked disabled>
                                <span class="mdl-checkbox__label">Reconhecimento de Firma / Documento</span>
                            </label>
                        </div>


                        <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label mdl-cell mdl-cell--6-col mdl-cell--6-col-tablet mdl-cell--12-col-phone">
                            <label class="mdl-checkbox mdl-js-checkbox mdl-js-ripple-effect" asp-for="ApostilaDeHaia">
                                <input type="checkbox" asp-for="ApostilaDeHaia" class="mdl-checkbox__input">
                                <span class="mdl-checkbox__label">Apostila de HAIA / Documento</span>
                            </label>
                        </div>
                    </div>



                    <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label mdl-cell mdl-cell--6-col mdl-cell--6-col-tablet mdl-cell--12-col-phone">

                        <select asp-for="Origem" id="IdiomaOrigem" class="custom-select mr-sm-2" asp-items="@(new SelectList(@ViewBag.Origens))">
                           

                        </select>
                        <label asp-for="Origem" class="mdl-textfield__label"></label>
                        


                    </div>






                    <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label mdl-cell mdl-cell--6-col mdl-cell--6-col-tablet mdl-cell--12-col-phone">
                        <select asp-for="Destino" id="IdiomaDestino" class="custom-select mr-sm-2" asp-items="@(new SelectList(string.Empty))">
                            

                        </select>

                        <label asp-for="Destino" class="mdl-textfield__label"></label>
                       
                    </div>

                   






                    <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label mdl-cell mdl-cell--6-col mdl-cell--6-col-tablet mdl-cell--12-col-phone naoAdicional">
                        <select asp-for="Assunto" class="mdl-textfield__input" asp-items="ViewBag.Assuntos"></select> <!--asp-items cria uma caixa de seleção das viewbag com multpiplas escolhas -->
                        <label class="mdl-textfield__label">Assunto da Tradução</label>
                    </div>

                    <div class="mdl-textfield mdl-js-textfield mdl-textfield--floating-label mdl-cell mdl-cell--6-col mdl-cell--6-col-tablet mdl-cell--12-col-phone" data-toggle="modal" data-target="#dateModal">
                        <input id="visualizacaoDaData" class="mdl-textfield__input" type="text" value="@Model.PrevisaoDeEntrega" />
                        <input asp-for="PrevisaoDeEntrega" class="mdl-textfield__input" type="hidden" /> <!--type é o tipo de input-->
                        <div class="mdl-button mdl-button--primary mdl-button--icon mdl-button--file">
                            <i class="material-icons" id="previsao">info</i>
                            <div class="mdl-tooltip" for="previsao">
                                Data selecionada baseada na melhor hipótese de preço.
                            </div>
                        </div>
                        <label asp-for="PrevisaoDeEntrega" class="mdl-textfield__label"></label><!-- recebe a variavel previsao de entrega do modelo-->
                        <span asp-validation-for="PrevisaoDeEntrega" class="mdl-textfield__error"></span> <!-- valida a variavel nos parametros do formulario-->
                    </div>
                </div>

                <div class="mdl-card__actions">
                    <button class="android-link mdl-button mdl-js-button mdl-typography--text-uppercase" type="button" id="buscarPrecos">
                        Analisar Solicitação
                        <i class="material-icons">chevron_right</i>
                    </button>
                </div>
            </div>
        </div>
    </div>
</form>

<pre id="output"></pre>

@section html{
    @await Html.PartialAsync("_modal", Model)

    <div class="modal fade" id="dateModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
        <div class="modal-dialog" role="document">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="exampleModalLabel">Previsão de Entrega</h5>
                    <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                        <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <div class="modal-body">
                    <div class="delivery-date">
                        <div id="areaoption1" class="delivery-date_option delivery-date_option--selected">
                            <div class="delivery-date__option-head">
                                <label class="mdl-radio mdl-js-radio mdl-js-ripple-effect" for="option-1">
                                    <input type="radio" id="option-1" class="mdl-radio__button" name="options" value="Automático (melhor preço)" checked onchange="AlterarVisibilidadeOption1()">
                                    <span class="mdl-radio__label">Automático (melhor preço)</span>
                                </label>
                            </div>
                        </div>
                        <div id="areaoption2" class="delivery-date__option pb-5">
                            <div class="delivery-date__option-head" style="width:100%;">
                                <label class="mdl-radio mdl-js-radio mdl-js-ripple-effect" for="option-2">
                                    <input type="radio" id="option-2" class="mdl-radio__button" name="options" value="choose" onchange="AlterarVisibilidadeOption2()">
                                    <span class="mdl-radio__label">Selecione entre as datas disponíveis para entrega:</span>
                                    <select id="datasPrevistas" class="mdl-textfield__input pb-2" disabled onchange="DefinirValorDeData()"></select>
                                </label>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-primary" data-dismiss="modal">Pronto</button>
                </div>
            </div>
        </div>
    </div>
}
<!--Alterar -->
@section Scripts{

    <script src="https://code.jquery.com/jquery-3.5.1.min.js"
            integrity="sha256-9/aliU8dGd2tb6OSsuzixeV4y/faTqgFtohetphbbj0="
            crossorigin="anonymous"></script>
    <script src="~/lib/jquery-validation/dist/jquery.validate.min.js"></script>
    <script src="~/lib/jquery-validation-unobtrusive/jquery.validate.unobtrusive.min.js"></script>
    <script>
        $(function () {
            $('#IdiomaOrigem').change(function () {
                var data = $("#IdiomaOrigem").val();
                $.ajax({
                    url: '/Solicitacao/CarregarViewBags?IdiomaOrigem=' + data, //Alterar a url por completo
                    type: 'Get',
                    success: function (data) {
                        var items = "";
                        $.each(data, function (i, item) {
                            items += "<option value='" + item.value + "'>" + item.text + "</option>"; //talvez precise alterar
                        });
                        $('#IdiomaDestino').html(items);
                    }
                })
            });
        })
   
        $(function () {

            $("#carregar").hide();
            $("#validacao").hide();
            $(".adicionais").hide();

            $("#CpfOuCnpj").mask('000.000.000-00', { reverse: true });

            document.querySelector('#p3').addEventListener('mdl-componentupgraded', function() {
                this.MaterialProgress.setProgress(33);
                this.MaterialProgress.setBuffer(87);
            });

            BuscarDatasDisponiveis();
        });

        function uploadFiles() {
          $("#carregar").show();
          var input = document.getElementById("Arquivos");
          var files = input.files;
          var formData = new FormData();

            for (var i = 0; i != files.length; i++) {
                formData.append("arquivos", files[i]);
            }

          $.ajax(
            {
              url: '@Url.Action("EnviarArquivo","Documento")',
              data: formData,
              processData: false,
              contentType: false,
              type: "POST",
                  success: function (data) {
                      if (data == null) {
                          Swal.fire('Não foi possível realizar a leitura do seu documento. Por favor, insira a quantidade de caracteres manualmente.')
                      } else {
                          $("#listFiles").empty();
                          $("#listFiles").append(data);

                          quantidadeDeCaracteres = document.getElementById('somaDeCaracteres').value;
                          data = document.getElementsByName('documentoIds');
                          var ids = [];
                          $.each(data, function (index, item) {
                              ids.push(item.value);
                          });

                          $("#QuantidadeDeCaracteres").val(quantidadeDeCaracteres);
                          $("#Documentos").val(ids);

                          BuscarDatasDisponiveis();
                      }
                  }
            }
          );
        }

        $("#buscarPrecos").click(function (e) {
            e.preventDefault();

            $("#formulario").validate()
            var valid = $("#formulario").valid();

            if (!valid) {
                $("#validacao").show();
            } else {
                var total = $("#QuantidadeDeCaracteres").val();
                if (total == '0') {
                    Swal.fire('Insira o anexo para contagem de palavras ou aguarde o fim da análise.')
                } else {
                    $("#validacao").hide();
                    $("#formulario").submit();
                }
            }
        });

        $("#CpfOuCnpj").change(function (e) {
            if ($(this).length > 11) {
                $(this).mask('00.000.000/0000-00', {reverse: true});
            } else {
                $(this).mask('000.000.000-00', {reverse: true});
            }
        });

        $("#Arquivos").change(function (e) {
            e.preventDefault();
            var input = document.getElementById("Arquivos");
            var files = input.files;
            if (files.length > 0) {
                $("#sendDocs").text("Alterar Documentos");
            } else {
                $("#sendDocs").text("Enviar Arquivos");
            }
        });

        function AlterarVisibilidadeOption2() {
            var element = document.getElementById("areaoption2");
            var input = document.getElementById("datasPrevistas");

            element.classList.add("delivery-date__option--selected");
            input.removeAttribute("disabled");

            $("#visualizacaoDaData").val(input.options[input.selectedIndex].text);
            $("#PrevisaoDeEntrega").val(input.options[input.selectedIndex].text);

            var element2 = document.getElementById("areaoption1");
            element2.classList.remove("delivery-date__option--selected");
        }

        function AlterarVisibilidadeOption1() {
            var element = document.getElementById("areaoption2");
            var input = document.getElementById("datasPrevistas");
            var radio = document.getElementById("option-1");

            element.classList.remove("delivery-date__option--selected");
            input.setAttribute("disabled", true);
            $("#visualizacaoDaData").val(radio.value);
            $("#PrevisaoDeEntrega").val(input.options[0].text);

            var element2 = document.getElementById("areaoption1");
            element2.classList.add("delivery-date__option--selected");
        }

        function BuscarDatasDisponiveis() {
            var quantidadeDeCaracteres = document.getElementById('QuantidadeDeCaracteres').value;
            $.ajax(
                {
                    url: '@Url.Action("BuscarDatasDisponiveis","Solicitacao")',
                    data: { quantidadeDeCaracteres: quantidadeDeCaracteres },
                    method: "POST",
                    success: function (data) {
                        console.log(data);
                        if (data == null) {
                            Swal.fire('Sua solicitação não possui a quantidade de laudas necessárias para realizar o cálculo de previsão.')
                        } else {
                            var select = document.getElementById("datasPrevistas");
                            select.innerHTML = "";

                            for (var i = 0; i < data.length; i++) {
                                var option = document.createElement("option");
                                option.text = data[i].text;
                                option.value = data[i].value;
                                select.add(option);
                            }

                            var input = document.getElementById("datasPrevistas");
                            var radio = document.getElementById("option-1");
                            $("#PrevisaoDeEntrega").val(radio.value);
                            $("#PrevisaoDeEntrega").val(input.options[0].text);
                        }
                    }
                });
        }

        function RemoverDocumento(id, quantidade) {
            var elemento = document.getElementById(id);
            elemento.style.display="none";
            elemento.style.visibility="hidden";

            var valor = $("#QuantidadeDeCaracteres").val();
            var total = valor - quantidade;
            $("#QuantidadeDeCaracteres").val(total);

            data = document.getElementsByName('documentoIds');
            var ids = [];
            $.each(data, function (index, item) {
                if (item.value !== id) {
                    ids.push(item.value);
                }
            });
            $("#Documentos").val(ids);

            BuscarDatasDisponiveis();
        }

        function MostrarAdicionais() {
            var tipoDeTraducao = $("#TipoDeSolicitacao").vals();
            if (tipoDeTraducao === '1') {
                $(".adicionais").show();
                $(".naoAdicional").hide();
            } else {
                $(".adicionais").hide();
                $(".naoAdicional").show();
            }
        }

        function DefinirValorDeData() {
            var valor = document.getElementById("datasPrevistas").value;
            $("#visualizacaoDaData").val(valor);
            $("#PrevisaoDeEntrega").val(valor);
        }
    </script>

}
