# Build do Android pelo GitHub

O servidor de build oficial do Defold não aceita mais o Defold 1.9.6, então o
APK é montado numa máquina do GitHub (de graça) pelo arquivo
`.github/workflows/build-android.yml`. Nada roda no seu PC.

## 1. Chave do Google (uma vez só)

As imagens do servidor de build do Defold só baixam com login do Google.
No GitHub esse login é feito com uma "chave de conta de serviço":

1. Entre em https://console.cloud.google.com com qualquer conta Google.
2. No topo, clique no seletor de projeto -> **Novo projeto** -> dê um nome
   (ex: `pokedex-build`) -> **Criar**. Não precisa ativar cobrança.
3. Com esse projeto selecionado, vá em **IAM e administrador** ->
   **Contas de serviço** -> **Criar conta de serviço**.
   - Nome: `defold-build` -> **Criar e continuar** -> pode pular as
     permissões -> **Concluir**.
4. Clique na conta criada -> aba **Chaves** -> **Adicionar chave** ->
   **Criar nova chave** -> **JSON** -> **Criar**. Um arquivo `.json` vai
   ser baixado. Guarde, e não mande pra ninguém.

## 2. Subir o projeto pro GitHub

Pelo **GitHub Desktop**:

1. **File -> Add local repository** -> escolha a pasta `Pokedex5E-master`.
2. Ele vai dizer que a pasta não é um repositório: clique em
   **create a repository** -> **Create repository**.
3. Clique em **Publish repository** e deixe **Keep this code private**
   MARCADO (o projeto tem a chave de assinatura do app, `debug.keystore`).

## 3. Colocar a chave do Google no GitHub

1. No site do GitHub, abra o repositório -> **Settings** ->
   **Secrets and variables** -> **Actions** -> **New repository secret**.
2. Name: `GCP_SA_KEY`
3. Secret: abra o `.json` do passo 1 no Bloco de Notas, copie TUDO e cole.
4. **Add secret**.

## 4. Gerar o APK

1. No repositório, aba **Actions** -> **Build Android** (na esquerda) ->
   **Run workflow** -> **Run workflow**.
2. Espere terminar (a primeira vez demora uns 15-30 minutos).
3. Clique na execução que ficou verde -> lá embaixo, em **Artifacts**,
   baixe **PokedexPMP-android**. É um `.zip` com o APK dentro.

O APK é assinado com o `debug.keystore` do projeto (o mesmo dos builds
anteriores), então instala por cima da versão do celular sem apagar os
Pokémon salvos.

Toda vez que mudar o app: commit + push no GitHub Desktop e repita o passo 4.

## Se der erro

Clique na execução vermelha, abra o passo que falhou e mande o log.
