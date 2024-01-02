# Biblioteca
Criação e gerenciamento básico de Livros, Autores e Assuntos dentro de uma biblioteca.

## Requisitos
- Node 12.22.12
- Yarn 1.22.21

## Tecnologias utilizadas no projeto
- VueJs
- Boostrarp
- BootstrapVue

## Passos para rodar o projeto

Clonar este repositório na sua pasta de preferência executando no Terminal ou CMD o seguinte comando:

```console
git clone git@github.com:Edumatsu/bookstore-frontend.git
```

Acessar o projeto:
```console
cd bookstore-frontend
```

Copiar o .env.example para o .env do projeto:
```console
cp .env.example .env
```

Modificar o .env incluindo a URL correta da API:
```console
VUE_APP_API_URL="http://localhost:8000/api/"
```

Executar o projeto no modo de desenvolvimento:
```console
yarn serve
```