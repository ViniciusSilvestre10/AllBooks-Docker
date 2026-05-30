# 🐳 Guia Definitivo Docker — Projeto AllBooks (Front-end)

Este documento reúne o passo a passo completo, boas práticas e comandos essenciais para dockerizar, rodar localmente e publicar no Docker Hub o container da aplicação **AllBooks** (Front-end). O foco aqui é o isolamento do ambiente de desenvolvimento utilizando containers Docker.

---

## 🏗️ 1. Arquitetura do Container

A aplicação é um projeto Front-end estruturado em React + TypeScript. Internamente, o servidor de desenvolvimento do React é configurado para escutar na porta `3000`. Através do Docker, isolamos todas as dependências do Node.js dentro de um ambiente seguro e controlado.

---

## 📦 2. Configurando o Dockerfile

Para garantir um build rápido, limpo e seguro, utilizamos a seguinte estrutura no arquivo `Dockerfile` localizado na raiz do projeto:

```dockerfile
FROM node:20-alpine

# Convenção de mercado para isolamento e segurança dos arquivos
WORKDIR /app

# Copia arquivos de gerenciamento de pacotes primeiro para aproveitar o cache de camadas
COPY package.json 

# Instala as dependências de forma limpa dentro do container
RUN npm install

# Copia os componentes, páginas e estilos (.tsx, .css, etc.)
COPY . .

# Inicia o servidor de desenvolvimento do React utilizando o formato Exec recomendado
ENTRYPOINT ["npm", "start"]

```


## 🔨 3. Comandos de Construção e Execução

### Passo 1: Construir a Imagem (Build)
Para construir a imagem a partir do Dockerfile, etiquetá-la corretamente e definir o contexto no diretório atual, execute o comando abaixo no seu terminal:

```bash
docker build -t viniciussilvestre10/allbooks-frontend:1.0 .
```

## Passo 2: Rodar o Container (Run)

Para criar e iniciar o container baseado na imagem construída, expondo a aplicação para a sua máquina física, utilize o comando:

```bash
docker run -d -p 8080:3000 --name allbooks-front viniciussilvestre10/allbooks-frontend:1.0
```

### 🔍 Entendendo os Parâmetros (Mapeamento de Portas)

- **`-d`**: Roda o container em modo *detached* (segundo plano), liberando o terminal.
- **`-p 8080:3000`**: Faz o mapeamento de portas (**Máquina Física : Dentro do Container**). Toda requisição que chegar na porta **8080** do seu computador será direcionada para a porta **3000** da aplicação React dentro do container.
- **`--name allbooks-front`**: Nomeia o container para facilitar o gerenciamento.

---

## 🌐 4. Acessando a Aplicação

Com o container em execução, abra o seu navegador de preferência e acesse o endereço:

`http://localhost:8080`

---

## 🚀 5. Publicando a Imagem no Docker Hub (Push)

Após validar o funcionamento do container localmente, a imagem foi enviada para o repositório remoto no Docker Hub para garantir a portabilidade do projeto.

### Autenticação no terminal

```bash
docker login
```

### Envio da imagem (Push)

```bash
docker push viniciussilvestre10/allbooks-frontend:1.0
```

**Status atual:** Imagem publicada com sucesso e pronta para ser executada em qualquer máquina com Docker instalado! 

---

## 🛠️ 6. Cheat Sheet (Comandos de Sobrevivência)

### Verificar se o container está rodando e checar as portas

```bash
docker ps
```

### Parar o container de maneira limpa (repassando os sinais do SO corretamente)

```bash
docker stop allbooks
```

### Iniciar novamente o container que foi parado

```bash
docker start allbooks
```

### Remover o container para liberar espaço 

```bash
docker rm  allbooks
```

### Listar as imagens Docker armazenadas no disco rígido do computador

```bash
docker images ls
```

