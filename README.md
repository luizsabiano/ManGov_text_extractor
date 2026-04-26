# Extrator de texto para manuais normativos governamentais
Extrai texto dos manuais normativos governamentais e persiste no banco de dados vetorial


---
<p style="text-align:justify;">

## 🛠️ Funcionamento do Utilitário

Este utilitário foi construído para automatizar a leitura, extração, fragmentação e geração de perguntas e respostas sintéticas a partir de manuais normativos governamentais armazenados localmente. O fluxo de execução é dividido nas seguintes etapas:


### 1. Extração e Filtragem de Conteúdo
A partir do diretório raiz, o script lê os arquivos dentro de `data/manuals_content/` e extrai seus conteúdos textuais. Para mitigar ruídos como anexos, fluxogramas, tabelas e figuras (bem como conteúdos de baixa densidade informacional), implementou-se um filtro baseado na quantidade de palavras.
* **Regra:** Apenas arquivos contendo uma quantidade igual ou superior a 1000 palavras (*tokens*) são selecionados para compor o *corpus* final.
* **Customização:** Esse limite pode ser modificado alterando a variável `min_tokens_per_docs` no arquivo `src/get_chunks.py`.

### 2. Fragmentação (*Chunking*)
Os documentos aprovados no filtro são submetidos ao processo de fragmentação em blocos de texto (*chunks*).
* **Parâmetros:** A divisão em segmentos segue a configuração estabelecida no atributo `list_splitting_parameters` (o padrão atual está fixado em 3500 *tokens*).
* **Continuidade Semântica:** Para evitar a perda de contexto nas extremidades dos blocos, foi aplicada uma sobreposição (*overlap*) de 10% do tamanho total do fragmento entre blocos consecutivos.
* **Persistência e Backup:** Para garantir a rastreabilidade, todos os *chunks* gerados são persistidos em um arquivo JSON único dentro do diretório `chunks/`, nomeado como `chunk_list_comprimento-do-fragmento.json`. A estrutura do arquivo contém:
  * `id`: Identificação numérica crescente.
  * `source`: Fonte original do manual.
  * `chunks`: Lista contendo os fragmentos extraídos.

</p>

Autores: Luiz Sabiano F. Medeiros


## Estrutura de diretórios


<pre>├── <font color="#12488B"><b>data</b></font>
│   ├── <font color="#12488B"><b>chunks</b></font> <font color="#7B68EE"> => Armazena os fragmentos salvos em Json</font>
│   ├── <font color="#12488B"><b>manuals_content</b></font> <font color="#7B68EE"> => Armazena os manuais a serem extraídos e fragmentados</font>
├── <font color="#12488B"><b>src</b></font>
│   ├── directories.py <font color="#7B68EE"> => Arquivo de caminhos (PATHs)</font>
│   ├── embedding_model.py <font color="#7B68EE"> => Configura modelo de embeddings</font>
│   ├── get_chunks.py <font color="#7B68EE"> => Recebe o conteúdo extraído e o fragmenta</font>
│   ├── get_manual_content.py <font color="#7B68EE"> => Extrai o conteúdo armazenado em manuals_conten</font>
│   ├── tools.py  <font color="#7B68EE"> => funções de apoio</font>
├── README.md
└── requirements.txt <font color="#7B68EE">O arquivo de requisitos para reproduzir os experimentos</font>
</pre>


### Instalação das dependências: 

$ pip install pip==24.0

$ pip install -r requirements.txt



## Execução 
Antes da execução crie um arquivo .env com a variável HF_TOKEN="sua chave do Hugging Face".

Execute o arquivo main.py.

## Configuração 

1) list_splitting_parameters: Lista de parâmetros que configura o tamanho dos fragmentos
2) is_extract_chunks: Se True, realiza a extração e fragmentação dos documentos
3) convert_docx_to_doc: Se True, recebe documentos docx e converte para doc (necessário pois o extrator só trabalha com doc)
4) remove_docx: Se True, remove os arquivos docx (só use depois da conversão para doc).

 
Necessário GPU Nvidia.
