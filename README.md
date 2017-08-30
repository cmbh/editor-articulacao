# Editor de Articulação

O editor de articulação é uma biblioteca javascript elaborada pela Assembleia Legislativa de Minas Gerais,
como parte do Sistema de Informação Legislativa de Minas Gerais (Silegis-MG).

Ele permite a edição de texto articulado, formatando e numerando automaticamente artigos, parágrafos,
incisos, alíneas e itens, bem como as divisões em títulos, capítulos, seções e subseções. O texto articulado
é estruturado em formato XML, conforme elemento `Articulacao` definido pelo schema do [LexML](https://github.com/lexml/lexml-xml-schemas/tree/master/src/main/resources/xsd).

## Funcionalidades

* Criação de **rótulo e numeração automática** para dispositivos (artigo, parágrafo, inciso, alínea e item);

* Formatação padrão dos dispositivos, conforme regras de redação definidas no art. 12 da [LCP 78/2004](http://www.almg.gov.br/consulte/legislacao/completa/completa.html?tipo=LCP&num=78&comp=&ano=2004).

  * A formatação pode ser **configurável**, para atender ao padrão de redação federal, sem alteração no código.
    
* Divisão dos artigos em títulos, capítulos, seções e subseções;
* Validação automática de:
  * **caixa alta:** artigos e parágrafos não devem ser escritos usando apenas caixa alta;
  * **uso de aspas:** citações devem estar entre aspas e terminar com ponto final (.);
  * **enumerações:** enumerações devem possuir mais de um elemento;
  * **letra maiúscula:** artigos e parágrafos devem ser iniciados com letra maiúscula;
  * **pontuação:** artigos e parágrafos devem ser terminados com ponto final (.) ou dois pontos (:), sem espaço antes da pontuação, e enumerações devem ser terminadas com ponto final (.), ponto e vírgula (;) ou dois pontos (:), sem espaço antes da pontuação.;
  * **sentença única:** dispositivos devem conter uma única sentença.
* Auto-formatação:
  * **ao abrir aspas**, formata-se automaticamente como um texto dentro do caput, permitindo múltiplas linhas dentro das aspas;
  * **ao fechar aspas**, formata-se a próxima linha como um novo artigo;
  * **ao finalizar com dois pontos**, inicia-se uma enumeração (de artigo ou parágrafo para inciso, de inciso para alínea e de alínea para item);
  * **ao finalizar com ponto final**, finaliza-se a enumeração (de item para alínea, de alínea para inciso, de inciso para artigo ou parágrafo);
  * **ao dar enter em uma linha vazia**, troca a formatação da linha vazia para artigo, quando formatado como parágrafo, ou encerra a enumeração, quando formatado como inciso, alínea ou item.
* Articulação pode ser importada/exportada de/para XML, seguindo especificação do **LexML**;
* Interpretação de conteúdo colado, de forma a permitir a formatação e numeração automática em dispositivos estruturados.

## Como usar a partir do código fonte

### Pré-requisitos para compilação

* [NodeJS com npm](https://nodejs.org/en/download/)
* Grunt (`npm install -g grunt`)

### Instalação das dependências

```
npm install
```

### Executando exemplo

Após clonar o repositório, execute a tarefa padrão do grunt.

```
grunt
```

Em seguida, basta abrir o navagedor no endereço http://localhost:8080/exemplo.html

### Testando

O editor de articulação possui testes automatizados utilizando karma e protractor. Para executá-los, basta iniciar a tarefa test pelo grunt.

```
grunt test
```

### Gerando pacote para aplicações finais

O javascript minificado é gerado por meio do webpack, a partir de uma tarefa do grunt. Existem dois empacotamentos para uso em aplicações finais:

#### Plain-JS

O empacotamento `plain-js` define `silegismgEditorArticulacao` como uma função global para transformar um elemento no DOM em um editor de articulação.

##### Gerando pacote

```
grunt build-plain
```

É possível incluir o polyfill do babel também, utilizando:

```
grunt build-plain-polyfill
```

##### Utilizando plain-js

Sintaxe: `silegismgEditorArticulacao(elemento, opcoes)`

##### Exemplo

```html
<script src="build/silegismg-editor-articulacao-plain-js.js"></script>
<div id="editor"></div>
<script>
  silegismgEditorArticulacao(document.getElementById('editor'));
</script>
```

#### Angular 1

O empacotamento `angular1` registra a diretiva `silegismgEditorArticulacaoConteudo` no módulo `silegismg-editor-articulacao` para AngularJS 1.x.

##### Gerando pacote

```
grunt build-angular1
```

##### Exemplo

```html
<script src="build/silegismg-editor-articulacao-angular1.js"></script>
<silegismg-editor-articulacao-conteudo id="editor" opcoes="opcoes"></silegismg-editor-articulacao-conteudo>
```

<a name="opcoes"></a>

## Opções do editor de articulação

| Atributo | Tipo | Valor padrão | Descrição |
| -------- | ---- | ------------ | --------- |
| shadowDOM | Boolean | false | **(Experimental)** Determina se deve adotar o Shadow DOM, se suportado pelo navegador. |
| transformacaoAutomatica | Boolean | true | Determina se o editor de articulação deve aplicar transformação automática. |
| escaparXML | Boolean | false | Determina o escapamento de caracteres de código alto unicode durante a exportação para lexmlString. |
| rotulo | [Object](#opcoes.rotulo) | | Determina o sufixo para os rótulos dos dispositivos. |
| validarAoAtribuir | Boolean | true | Determina se deve validar o conteúdo atribuído ao componente. |
| validacao | [Object](#opcoes.validacao) | | Determina as validações que devem ocorrer. |

<a name="opcoes.rotulo"></a>

### Opções de rótulo

Todos os atributos de rótulo são do tipo literal (String).

| Atributo | Valor padrão | Descrição |
| -------- | ------------ | --------- |
| separadorArtigo | &#8211; | Separador do rótulo do artigo 1º ao 9º |
| separadorArtigoSemOrdinal | &#8211; | Separador do rótulo do artigo 10 em diante |
| separadorParagrafo | &#8211; | Separador do rótulo do parágrafo 1º ao 9º |
| separadorParagrafoSemOrdinal | &#8211; | Separador do rótulo do parágrafo 10 em diante |
| separadorParagrafoUnico | &#8211; | Separador do rótulo parágrafo único |
| separadorInciso | &#8211; | Separador do rótulo de inciso |
| separadorAlinea | ) | Separador do rótulo da alínea |
| separadorItem | ) | Separador do rótulo do item |

<a name="opcoes.validacao"></a>

### Opções de validação

Todas as opções de validação são habilitadas (valor true) por padrão.

| Atributo | Descrição |
| -------- | --------- |
| caixaAlta | Determina se deve validar o uso de caixa alta. |
| citacao | Determina se deve validar o uso de aspas em citações. |
| enumeracaoElementos | Determina se deve validar a presença de múltiplos elementos em uma enumeração. |
| inicialMaiuscula | Determina se deve validar o uso de letra maiúscula no caput do artigo e em parágrafos. |
| pontuacao | Determina se deve validar as pontuações. |
| pontuacaoEnumeracao | Determina se deve validar pontuação de enumeração. |
| sentencaUnica | Determina se deve exigir sentença única no dispositivo. |

Contribuições desejadas
-----------------------
* Identificação de remissões;
* Renumeração automática de remissões, em caso de alterações nos dispositivos;
* Modo de edição de norma, em que alterações a um texto original são consideradas emendas.

Limitações conhecidas (aceita-se contribuições)
-----------------------------------------------
As limitações conhecidas correspondem a um conjunto de funcionalidade que não funciona como deveria, mas seu comportamento é aceitável para a proposta do editor. Contribuições são bem-vindas.

* Copiar do editor de articulação e colar em editor externo omite os rótulos;
* Interpretação de artigo com emenda (exemplo: Art. 283-A da Constituição do Estado de Minas Gerais), apesar de haver suporte para importação de LexML com este tipo de dispositivo.