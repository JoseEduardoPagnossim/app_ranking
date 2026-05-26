# Brasileirão do Atendimento

Painel web em estilo transmissão/TV para classificação de atendimentos por técnico, com carrossel, temas visuais e sincronização opcional via Firebase/Firestore.

## Funcionalidades principais

- Importação de arquivos `.csv`, `.xls` e `.xlsx`.
- Senha simples para liberar a importação de nova planilha.
- Salvamento local automático com `localStorage`.
- Sincronização opcional com Firebase/Firestore.
- Filtro por período:
  - mês atual;
  - mês anterior;
  - todos os dados;
  - período personalizado.
- Classificação geral.
- Classificação do último dia.
- Ranking por grupo.
- Carrossel automático entre telas.
- Temas visuais:
  - Clássico;
  - Soften;
  - Neon;
  - Claro.
- Modo apresentação para TV.
- Saída do modo apresentação com `Esc`.

## Senha de importação

A senha padrão para importar uma nova planilha é:

```text
1234
```

Para alterar a senha, abra o arquivo `index.html`, procure por:

```js
const IMPORT_PASSWORD = "1234";
```

E troque `1234` pela senha desejada.

Exemplo:

```js
const IMPORT_PASSWORD = "minha-senha";
```

Essa é uma senha simples no próprio front-end. Ela serve para evitar importações acidentais, mas não substitui autenticação profissional.

## Estrutura esperada da planilha

A planilha deve conter as seguintes colunas:

| Coluna | Descrição |
|---|---|
| `time` | Data do atendimento ou rodada |
| `Tecnico` | Nome do técnico |
| `grupoAtendimento` | Grupo ou equipe do técnico |
| `Quantidade` | Quantidade de atendimentos |

Exemplo:

```csv
time,Tecnico,grupoAtendimento,Quantidade
2026-05-01,João Silva,Grupo A,12
2026-05-01,Maria Souza,Grupo B,18
```

Também são aceitos arquivos CSV separados por ponto e vírgula (`;`).

## Regras de cálculo

Cada data encontrada na coluna `time` é considerada uma rodada.

A classificação geral soma a quantidade de atendimentos de cada técnico dentro do período filtrado.

A classificação do dia considera apenas a última data encontrada dentro do período selecionado.

O ranking por grupo soma a quantidade de atendimentos por `grupoAtendimento`.

## Técnicos excluídos automaticamente

A página ignora técnicos sem grupo e também exclui automaticamente os seguintes nomes:

- Jose Eduardo Pagnossim
- Heitor Drago
- João Paulo Cardoso
- Eduardo Thomaz

A comparação dos nomes ignora acentos, maiúsculas, minúsculas e espaços duplicados.

## Firebase

A sincronização em Firebase é opcional.

Para configurar:

1. Crie um projeto no Firebase.
2. Ative o Firestore Database.
3. Crie um app Web no Firebase.
4. Copie o `firebaseConfig`.
5. Abra a página do ranking.
6. Clique no botão **Firebase**.
7. Cole o `firebaseConfig` em formato JSON.
8. Clique em **Sincronizar**.

Exemplo de formato aceito:

```json
{
  "apiKey": "SUA_API_KEY",
  "authDomain": "seu-projeto.firebaseapp.com",
  "projectId": "seu-projeto",
  "storageBucket": "seu-projeto.firebasestorage.app",
  "messagingSenderId": "123456789",
  "appId": "1:123456789:web:abcdef"
}
```

## Regras sugeridas para Firestore

Para uso inicial, você pode liberar apenas o documento usado pelo painel:

```js
rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {
    match /paineis/brasileirao-atendimento {
      allow read, write: if true;
    }

    match /{document=**} {
      allow read, write: if false;
    }
  }
}
```

Essa regra ainda permite que quem tenha acesso à página altere o ranking. Para uso mais seguro, o ideal é implementar Firebase Authentication.

## Publicação no GitHub Pages

1. Renomeie o arquivo principal para `index.html`.
2. Envie `index.html` e `README.md` para o repositório.
3. No GitHub, acesse:
   - **Settings**
   - **Pages**
   - **Build and deployment**
   - Source: **Deploy from a branch**
   - Branch: `main`
   - Folder: `/root`
4. Salve e aguarde o link ser gerado.

## Observações

A página é estática e pode ser hospedada gratuitamente no GitHub Pages.  
Sem Firebase, os dados ficam salvos apenas no navegador em que a planilha foi importada.  
Com Firebase configurado, os dados podem ser compartilhados entre computadores e TVs.
