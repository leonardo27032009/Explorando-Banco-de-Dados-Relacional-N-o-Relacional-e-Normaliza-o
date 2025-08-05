
# Explorando Banco de Dados Relacional, Não Relacional e Normalização 📖 

## Banco de dados **Relacional**. 🟩

- Um banco de dados relacional é um sistema que organiza dados em tabelas interconectadas. Cada tabela tem colunas e linhas, e a principal característica desse modelo é a forma como ele estabelece e gerencia as relações entre essas tabelas.

- Essa interconexão é feita através de:

1. Chave primária: Um identificador único para cada linha em uma tabela. Pense nisso como um RG que cada registro tem e que nunca se repete.

2. Chave estrangeira: Um campo em uma tabela que se refere à chave primária de outra tabela. É essa chave que cria o "link" ou a "relação" entre as informações.

### Exemplo de uso: Sistema de biblioteca.

- Pense que você quer gerenciar uma biblioteca. Em vez de criar uma tabela enorme com todos os dados, você divide as informações em tabelas menores e as conecta:

1. Tabela Autores: Armazena os dados dos autores, como IDAutor (chave primária), Nome e Nacionalidade.

2. Tabela Livros: Contém os dados dos livros, como IDLivro (chave primária), Título, Editora e um campo IDAutor (chave estrangeira) que aponta para a tabela Autores.

- Com essa estrutura, se você quiser encontrar todos os livros de um autor específico, não precisa procurar o nome dele em cada linha da tabela de livros. Você simplesmente usa a chave estrangeira ID_Autor para vincular as duas tabelas e obter as informações de forma rápida e precisa.

## Banco de dados **Não relacional**. 🟥

- Um banco de dados não relacional, ou NoSQL, é um sistema que armazena dados de forma flexível, sem o uso de tabelas e relações rígidas como no modelo relacional. Em vez disso, ele organiza os dados em estruturas mais dinâmicas, como documentos.

### Exemplo de uso: Perfil em uma rede social.💻

- Imagine uma rede social onde cada usuário tem um perfil com diversas informações. Em vez de espalhar esses dados por várias tabelas (uma para dados pessoais, outra para fotos, outra para postagens, etc.), um banco de dados não relacional pode armazenar todas as informações de um usuário em um único "documento".
### Exemplo em **JSON**:

```{
  "id_usuario": "usuario286",
  "nome": "Leonardo",
  "email": "Leonardo.Silva@email.com",
  "idade": 30,
  "cidade": "São Paulo",
  "posts": [
    {
      "id_post": "post456",
      "conteudo": "Meu primeiro post!",
      "data": "2025-08-05"
    },
    {
      "id_post": "post789",
      "conteudo": "Adorei essa viagem!",
      "data": "2025-08-06",
      "fotos": ["foto1.jpg", "foto2.jpg"]
    }
  ],
  "seguidores": [
    "usuario456",
    "usuario789"
  ]
}
```
### Exemplo de tabela não normalizada 🖥️

|Pedido_id|Cliente_Nome|Cliente_endereço|Produto_id|Produto_Nome|Quantidade|Preço_Total|
|------------|----------------|-------------------|------------|-----------------|-------------|-----------|
|1|Leo|Rua A,123|10|Chuteira|1|R$300,00|
|1|Tiago|Rua B,124|11|Caneleira|2|R$50,00|
|2|Gabriel|AV. C,125|12|Chuteira|2|R$600,00|

## Oque é **Normalização** e qual seu objetivo?🆗

- A Normalização é um processo de organização de dados em um banco de dados relacional. Seu objetivo é dividir tabelas grandes em tabelas menores e mais coesas para:

1. Evitar a redundância de dados: Impede que a mesma informação seja armazenada em vários lugares.

2. Garantir a consistência: Ao atualizar um dado, a mudança só precisa ser feita em um único local.

3. Reduzir anomalias: Elimina problemas ao inserir, atualizar ou excluir dados, tornando o banco mais confiável.

- O processo de normalização é guiado pelas Formas Normais, que são um conjunto de regras progressivas para estruturar o banco de dados. As mais comuns são:

  - Primeira Forma Normal (1FN): Garante que cada célula da tabela contenha apenas um valor atômico, ou seja, que não possa ser dividido.

  - Segunda Forma Normal (2FN): Exige que a tabela já esteja na 1FN e que todos os atributos não-chave dependam inteiramente da chave primária completa.

  - Terceira Forma Normal (3FN): Exige que a tabela esteja na 2FN e que não haja dependências transitivas, ou seja, que um atributo não-chave não dependa de outro atributo não-chave.

## Exemplo de tabela **Normalizadas**.

## **1FN** 🥇

A 1FN garante que cada célula contenha apenas um valor atômico (indivisível). Para isso, a tabela Pedidos original, que tinha vários produtos em uma única linha, é transformada em uma tabela onde cada linha representa a combinação de um pedido com um produto.



| ID_Pedido | Data       | ID_Cliente | Endereço_Cliente | ID_Produto | Nome_Produto | Preço_Unitario | Quantidade |   |   |
|-----------|------------|------------|------------------|------------|--------------|----------------|------------|---|---|
| 1         | 05/08/2025 | Leo        | Rua A,10         | 101        | Camiseta     | R$50,00        | 2          |   
| 1         | 05/08/2025 | Leo        | Rua A,10         | 102        | Calça        | R$120,00       | 1          |   
| 2         | 05/08/2025 | Tiago      | Rua B,25         | 103        | Tenis        | R$200,00       | 1          
         
## **2FN**🥈

A 2FN exige que a tabela esteja na 1FN e que todos os atributos não-chave dependam integralmente da chave primária composta. Para isso, os dados que dependem apenas de parte da chave são movidos para novas tabelas.

Aqui, separamos as informações do cliente e dos produtos em suas próprias tabelas.

| ID_Pedido | Data       | ID_Cliente | Nome_Cliente | Endereço_Cliente |      
|-----------|------------|------------|--------------|------------------|
| 1         | 05/08/2025 | C1         | Leo          | Rua A, 10        |
| 2         | 05/08/2025 | C2         | Tiago        | Rua B, 25        |    


| ID_Pedido | ID_Produto | Nome_Produto | Preço_Unitario | Quantidade |   
|-----------|------------|--------------|----------------|------------|
| 1         | 101        | Camiseta     | R$50,00        | 2          |   
| 1         | 102        | Calça        | R$120,00       | 1          |   
| 2         | 103        | Tenis        | R$200,00       | 1          |   


## **3FN** 🥉

A 3FN exige que a tabela esteja na 2FN e que não haja dependências transitivas, ou seja, nenhum atributo não-chave pode depender de outro atributo não-chave.

Para isso, separamos as informações de clientes e produtos em tabelas dedicadas. A tabela Pedidos agora armazena apenas a data e a chave do cliente. A tabela Itens_Pedido armazena as chaves de pedido e produto com a quantidade, enquanto as informações detalhadas de clientes e produtos ficam em suas próprias tabelas.

| ID_Cliente | Nome_Cliente | Endereço_Cliente |   
|------------|--------------|------------------|
| C1         | Leo          | Rua A,10         |   
| C2         | Tiago        | Rua B,25         |   


| ID_Pedido | ID_Cliente | Data       |
|-----------|------------|------------|
| 1         | C1         | 05/08/2025 |   
| 2         | C2         | 05/08/2025 |   


| ID_Produto | Nome_Produto | Preço_Unitario |   
|------------|--------------|----------------|
| 101        | Camiseta     | R$50,00        |   
| 102        | Calça        | R$120,00       |   
| 103        | Tenis        | R$200,00       | 

| ID_Pedido | ID_Produto | Quantidade |     
|-----------|------------|------------|
| 1         | 101        | 2          |   
| 1         | 102        | 1          |   
| 2         | 103        | 1          |  












