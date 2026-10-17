# Especificação Técnica: epub2docx-by-jair-lima

Este documento define a especificação técnica para o conversor de e-books EPUB para documentos do Microsoft Word (DOCX) no padrão A5 para impressão física.

## 1. Objetivo
O objetivo principal deste projeto é criar uma ferramenta CLI em Python que converta arquivos EPUB para documentos DOCX formatados de acordo com os padrões tipográficos e estéticos de um livro impresso de alta qualidade: especificamente no tamanho de folha A5.

## 2. Escopo
* **Extração do Pacote:** Descompactação programática do arquivo EPUB.
* **Leitura de Metadados:** Extração automática de dados como título, autor e idioma do e-book.
* **Processamento de HTML:** Parsing de conteúdo XHTML com BeautifulSoup: identificando capítulos, parágrafos, listas, tabelas e formatação em linha (negrito, itálico, cores de fonte, etc.).
* **Geração do DOCX:** Criação de documento no tamanho A5 com margens espelhadas ajustadas para encadernação de livro físico.
* **Preservação Visual:** Manutenção da cor de destaque vermelho para as palavras de Jesus e outras formatações importantes.
* **Páginas de Controle:** Inclusão automática de página de rosto e sumário dinâmico.
* **Paginação:** Inserção de cabeçalhos e rodapés com paginação automática.
* **Automação COM:** Capacidade de forçar a atualização imediata das páginas do sumário no Windows se o Microsoft Word estiver instalado.

## 3. Arquitetura do Projeto
O utilitário é implementado como um script CLI Python autônomo.
O fluxo de execução segue os seguintes passos:
1. **Validação de Argumentos:** A CLI aceita o caminho do arquivo EPUB de entrada, o caminho de saída DOCX e argumentos opcionais de configuração (fonte, tamanho da fonte, margens, atualização de TOC, etc.).
2. **Leitura e Extração:** O EPUB é carregado usando a biblioteca zipfile. O arquivo `content.opf` é lido para mapear os metadados do livro e a ordem lógica de leitura (spine).
3. **Mapeamento de Mídia:** Todas as imagens são extraídas em memória ou diretório temporário para inserção no documento Word.
4. **Montagem do Documento Word:**
   * Configuração das propriedades de página da primeira seção (papel A5, margens espelhadas).
   * Criação e formatação dos estilos básicos (Normal, Heading 1, Heading 2, Heading 3, List Bullet, etc.) utilizando fontes serifadas elegantes: Georgia por padrão.
   * Criação da Página de Rosto e do Sumário Dinâmico (TOC) com instruções de campo do Word.
   * Varredura dos arquivos XHTML em ordem cronológica: injetando o conteúdo de texto no Word com a formatação adequada.
5. **Automação de Atualização de Campos (Opcional):** Se o Word estiver disponível e a opção for ativada: o arquivo é aberto via interface COM, atualizado e salvo novamente.

## 4. Dependências
* Python 3.x
* `python-docx` para geração de documentos Word.
* `beautifulsoup4` e `lxml` para parsing estruturado de XHTML.
* `pillow` para redimensionamento e validação de imagens.
* `pywin32` para automação do Word via COM no Windows.

## 5. Decisões de Design
* **Margens Espelhadas (Mirror Margins):** O padrão para livro físico exige margens internas maiores. O script configura margens internas de 2.2 cm e externas de 1.8 cm para permitir encadernação sem cortar texto.
* **Tipografia do Livro:** A fonte padrão é Georgia: uma fonte serifada que oferece excelente legibilidade em papel impresso A5. O tamanho padrão é 10.5 pt com espaçamento entre linhas de 1.25.
* **Preservação de Estilos Inline:** O parser de HTML mapeia cores CSS (como `color: red` ou tags com a classe `red`) e aplica a cor vermelha no run correspondente do DOCX.
* **Quebras de Página Inteligentes:** Cada novo arquivo XHTML que representa um capítulo começa em uma nova página.

## 6. Critérios de Aceite
* O arquivo DOCX gerado deve ser válido e legível no Microsoft Word e LibreOffice.
* As páginas do documento devem estar configuradas no formato A5.
* O sumário dinâmico deve refletir os títulos do livro.
* A paginação automática deve ser exibida no rodapé.
* As palavras de Jesus e marcações em vermelho devem ser perfeitamente reproduzidas.

## 7. Instruções Operacionais
* Para instalar dependências: `pip install -r requirements.txt`
* Uso básico da ferramenta:
  ```powershell
  python epub2docx.py "caminho/do/livro.epub" "caminho/do/resultado.docx"
  ```
* Parâmetros avançados suportados:
  * `--update-toc`: Atualiza os números de página do sumário usando automação COM (requer Microsoft Word instalado).
  * `--font "Nome da Fonte"`: Altera a fonte principal (padrão: Georgia).
  * `--size 11`: Altera o tamanho do texto do corpo (padrão: 10.5).
  * `--margin-inner 2.2`: Margem interna em centímetros.
  * `--margin-outer 1.8`: Margem externa em centímetros.
  * `--title "Título Customizado"`: Substitui o título extraído do EPUB.
  * `--author "Autor Customizado"`: Substitui o autor extraído do EPUB.
