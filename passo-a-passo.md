# Passo a Passo: Explorando um Índice do Azure AI Search (UI)

Este documento detalha o processo de criação, configuração e exploração de um índice no Azure AI Search, utilizando a interface do portal Azure. O guia foi adaptado do laboratório [Explore an Azure AI Search index](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/11-ai-search.html) da Microsoft.

## Pré-requisitos

* Uma assinatura do Azure.
* Um recurso do **Azure AI Search** provisionado.
* Um recurso do **Azure Blob Storage** com dados para indexação (ex: documentos, imagens).

---

### **Etapa 1: Provisionar um Recurso do Azure AI Search**

1.  No portal do Azure, clique em **+ Criar um recurso**.
2.  Pesquise por "Azure AI Search" e clique em **Criar**.
3.  Configure os detalhes do recurso:
    * **Assinatura**: Selecione sua assinatura.
    * **Grupo de Recursos**: Crie um novo ou selecione um existente (ex: `rg-ai-search-lab`).
    * **Nome do Serviço**: Forneça um nome globalmente exclusivo (ex: `meu-search-service-unique`).
    * **Localização**: Escolha a região mais próxima de você.
    * **Tipo de Preço**: Selecione `Básico` ou `Gratuito (F)` para este laboratório.
4.  Clique em **Revisar + criar** e, em seguida, em **Criar**.

### **Etapa 2: Criar uma Conta de Armazenamento e Fazer Upload de Dados**

1.  Crie um recurso do tipo **Conta de Armazenamento** no Azure.
2.  Após a criação, navegue até a conta de armazenamento e selecione **Contêineres** no menu à esquerda.
3.  Crie um novo contêiner (ex: `coffee-reviews`).
4.  Faça o upload dos arquivos de dados de exemplo para este contêiner. O laboratório da Microsoft fornece um conjunto de reviews de cafeterias.

### **Etapa 3: Indexar os Dados**

A indexação é o processo de ingestão de conteúdo para o Azure AI Search.

1.  Navegue até o seu recurso do **Azure AI Search**.
2.  Na página de visão geral, clique em **Importar dados**.
3.  **Conectar aos seus dados**:
    * **Fonte de Dados**: Selecione **Azure Blob Storage**.
    * **Nome da fonte de dados**: Dê um nome, como `coffee-reviews-datasource`.
    * **Conexão**: Escolha sua conta de armazenamento e o contêiner `coffee-reviews`.
4.  **Adicionar habilidades cognitivas (Opcional, mas recomendado)**:
    * Nesta etapa, você pode enriquecer os dados com IA. Habilite o **OCR** para extrair texto de imagens e adicione outras habilidades, como:
        * **Extração de frases-chave**: Para identificar os principais tópicos.
        * **Análise de Sentimento**: Para determinar se o texto é positivo, negativo ou neutro.
        * **Reconhecimento de Entidades**: Para identificar lugares, pessoas, etc.
    * Salve os enriquecimentos em um novo campo no índice.
5.  **Personalizar o índice de destino**:
    * O **Índice** é a estrutura que armazena os dados pesquisáveis.
    * **Nome do Índice**: Dê um nome, como `coffee-reviews-index`.
    * **Chave**: Defina um campo único para cada documento (geralmente `metadata_storage_path`).
    * Revise os campos do índice. Marque os campos como `Recuperável`, `Filtrável`, `Classificável` e `Pesquisável` conforme a necessidade.
6.  **Criar um indexador**:
    * O **Indexador** é o processo que executa a ingestão de dados.
    * **Nome**: Dê um nome, como `coffee-reviews-indexer`.
    * **Agenda**: Defina como `Uma vez`.
7.  Clique em **Enviar** para iniciar o processo de criação e execução do indexador.

### **Etapa 4: Explorar o Índice e Realizar Consultas**

1.  Dentro do seu recurso do Azure AI Search, selecione **Explorador de pesquisa** no menu.
2.  Certifique-se de que o **`coffee-reviews-index`** está selecionado na guia **Índice**.

#### **Exemplos de Consultas:**

* **Pesquisa de Texto Simples**:
    * Na caixa **Cadeia de caracteres de consulta**, digite `london` e clique em **Pesquisar**. Os resultados incluirão todos os documentos que mencionam a palavra "london".

    ```json
    search=london
    ```

* **Filtragem com a Sintaxe OData**:
    * Para encontrar reviews com uma avaliação de sentimento (sentiment) maior que 0.8 (muito positivo), use o parâmetro `$filter`.

    ```
    search=*&$filter=sentiment gt 0.8
    ```

* **Combinação de Pesquisa e Filtro**:
    * Para encontrar reviews positivos sobre `london`:

    ```
    search=london&$filter=sentiment gt 0.7
    ```

* **Selecionar Campos Específicos**:
    * Para retornar apenas as frases-chave e a pontuação de sentimento dos resultados, use o parâmetro `$select`.

    ```
    search=london&$select=keyphrases,sentiment
    ```

Este guia prático demonstra como o Azure AI Search pode ser utilizado para ingerir, enriquecer e explorar dados não estruturados de forma eficiente, transformando-os em informações valiosas e pesquisáveis.
