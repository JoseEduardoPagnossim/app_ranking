# Brasileirão do Atendimento

Painel web em estilo transmissão/TV para classificação de atendimentos por técnico.

A página importa arquivos CSV, XLS ou XLSX, calcula rankings por período e exibe uma classificação dinâmica com modo apresentação, carrossel automático, temas visuais e persistência opcional em Firebase/Firestore.

## Funcionalidades

- Importação de arquivos `.csv`, `.xls` e `.xlsx`.
- Salvamento local automático via `localStorage`.
- Persistência opcional em Firebase Cloud Firestore.
- Filtro por período:
  - mês atual;
  - mês anterior;
  - todos os dados;
  - período personalizado.
- Classificação geral do período.
- Classificação do último dia da planilha.
- Ranking por grupo.
- Carrossel automático entre telas.
- Temas personalizáveis:
  - Clássico;
  - Soften;
  - Neon;
  - Claro.
- Destaque visual para G5/G6 e Z4, conforme regra configurada no código.
- Cards superiores com:
  - data do dia;
  - quantidade de rodadas;
  - líder do dia;
  - líder geral.
- Modo apresentação para TV.
- Layout em duas colunas.
- Saída do modo apresentação usando `Esc`.
- Filtro por técnico e por grupo.

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

A página também aceita CSV separado por ponto e vírgula (`;`).

## Regras de cálculo

Cada data encontrada na coluna `time` é considerada uma rodada.

A classificação geral soma a quantidade de atendimentos de cada técnico dentro do período filtrado.

A classificação do dia considera apenas a última data encontrada dentro do período selecionado.

O ranking por grupo soma os atendimentos por `grupoAtendimento` dentro do período filtrado.

## Técnicos excluídos automaticamente

A página ignora técnicos sem grupo e também exclui automaticamente os seguintes nomes:

- Jose Eduardo Pagnossim
- Heitor Drago
- João Paulo Cardoso
- Eduardo Thomaz

A comparação dos nomes ignora acentos, maiúsculas, minúsculas e espaços duplicados.

## Como usar localmente

1. Baixe ou clone este repositório.
2. Abra o arquivo `index.html` no navegador.
3. Clique em **Importar Excel/CSV**.
4. Selecione a planilha.
5. Use os filtros de período, grupo ou técnico conforme necessário.
6. Para exibir em TV, clique em **Modo apresentação**.

## Carrossel automático

O botão **Carrossel ON/OFF** controla a troca automática entre as telas:

1. Classificação geral;
2. Classificação do dia;
3. Ranking por grupo.

O intervalo padrão é de 20 segundos.

Para alterar o tempo, edite no arquivo `index.html` o trecho:

```js
}, 20000);
```

O valor está em milissegundos. Por exemplo, `30000` equivale a 30 segundos.

## Temas

O seletor de tema fica no topo da página.

Temas disponíveis:

- Clássico;
- Soften;
- Neon;
- Claro.

A escolha do tema fica salva no navegador.

## Firebase / Firestore

A página funciona normalmente sem Firebase, usando apenas `localStorage`.

Com Firebase configurado, os dados podem ser salvos na nuvem e carregados em outro computador, navegador ou TV.

### Como configurar

1. Acesse o Firebase Console.
2. Crie um projeto ou use um projeto existente.
3. Adicione um app Web ao projeto.
4. Copie o objeto `firebaseConfig`.
5. Abra a página do painel.
6. Clique no botão **Firebase**.
7. Cole o `firebaseConfig` em formato JSON.
8. Clique em **Sincronizar** ou importe uma planilha para salvar na nuvem.

Exemplo de formato esperado:

```json
{
  "apiKey": "...",
  "authDomain": "...",
  "projectId": "...",
  "storageBucket": "...",
  "messagingSenderId": "...",
  "appId": "..."
}
```

### Coleção usada no Firestore

Por padrão, a página salva os dados em:

```txt
paineis/brasileirao-atendimento
```

No arquivo `index.html`, isso pode ser alterado nestas constantes:

```js
const FIREBASE_COLLECTION = "paineis";
const FIREBASE_DOCUMENT = "brasileirao-atendimento";
```

### Regras do Firestore para teste

Para um teste interno rápido, você pode liberar leitura e escrita temporariamente:

```js
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /paineis/{document} {
      allow read, write: if true;
    }
  }
}
```

Atenção: essa regra é aberta. Para produção, use autenticação ou restrinja melhor as permissões.

## Como publicar no GitHub Pages

1. Renomeie o arquivo principal para `index.html`.
2. Crie um repositório no GitHub.
3. Envie o `index.html` e este `README.md`.
4. No GitHub, acesse:
   - **Settings**;
   - **Pages**;
   - **Build and deployment**;
   - Source: **Deploy from a branch**;
   - Branch: `main`;
   - Folder: `/root`.
5. Salve as configurações.
6. Aguarde o GitHub gerar o link da página.

## Uso em TV

Para melhor resultado em uma televisão:

- use o navegador em zoom `100%`;
- abra a página em tela cheia;
- clique em **Modo apresentação**;
- pressione `Esc` para sair do modo apresentação;
- prefira resolução Full HD ou superior.

## Dados e privacidade

Sem Firebase, os dados ficam apenas no navegador usado, via `localStorage`.

Com Firebase configurado, os dados são salvos no Cloud Firestore do projeto informado.

## Observações

A página é estática e pode ser hospedada gratuitamente no GitHub Pages.

Não é necessário backend próprio, banco de dados local ou instalação de dependências.
