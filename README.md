# Brasileirão do Atendimento

Painel web em estilo transmissão/TV para acompanhar atendimentos finalizados e avaliações por técnico. O painel importa arquivos `.csv`, `.xls` ou `.xlsx`, calcula rankings por período, exibe destaques em cards e pode sincronizar os dados com Firebase/Firestore.

## Funcionalidades principais

- Importação de arquivos `.csv`, `.xls` e `.xlsx`.
- Senha simples para liberar a importação de nova planilha.
- Salvamento local automático com `localStorage`.
- Sincronização opcional com Firebase/Firestore.
- Histórico cumulativo no Firebase.
- Filtro por período:
  - mês atual;
  - mês anterior;
  - todos os dados;
  - período personalizado.
- Classificação de atendimentos do dia.
- Classificação geral de atendimentos.
- Ranking de notas do dia.
- Ranking geral de notas.
- Ranking por grupo.
- Ranking de notas por grupo, com percentual de participação no total de avaliações do período.
- Ranking de notas por grupo/squad.
- Ranking de notas por grupo/squad.
- Cards superiores com:
  - última data do filtro;
  - dias considerados no período;
  - líder de atendimentos no período;
  - líder de notas no período;
  - média geral das avaliações no período;
  - total de avaliações no período selecionado.
- Coluna de média de nota somente nas abas de notas.
- G6 e Z4 em destaque na classificação atual.
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

Para alterar a senha, abra o arquivo HTML, procure por:

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

A planilha deve conter as colunas obrigatórias abaixo:

| Coluna | Descrição |
|---|---|
| `time` | Data do atendimento ou rodada |
| `Tecnico` | Nome do técnico |
| `grupoAtendimento` | Grupo ou equipe do técnico |
| `Quantidade` | Quantidade de atendimentos finalizados |

Para o ranking de notas, a planilha pode conter também estas colunas:

| Coluna | Descrição |
|---|---|
| `Nota 5` | Quantidade de avaliações com nota 5 |
| `Nota 4` | Quantidade de avaliações com nota 4 |
| `Nota 3` | Quantidade de avaliações com nota 3 |
| `Nota 2` | Quantidade de avaliações com nota 2 |
| `Nota 1` | Quantidade de avaliações com nota 1 |

Exemplo:

```csv
time,Tecnico,grupoAtendimento,Quantidade,Nota 5,Nota 4,Nota 3,Nota 2,Nota 1
2026-06-01,João Silva,Grupo A,12,8,2,1,0,0
2026-06-01,Maria Souza,Grupo B,18,14,3,0,1,0
```

Também são aceitos arquivos CSV separados por ponto e vírgula (`;`).

## Regras de cálculo de atendimentos

Cada data encontrada na coluna `time` é considerada uma rodada ou dia de atendimento.

A classificação geral de atendimentos soma a coluna `Quantidade` de cada técnico dentro do período filtrado.

A classificação do dia considera a última data encontrada dentro do período selecionado.

O ranking por grupo soma a coluna `Quantidade` por `grupoAtendimento` dentro do período filtrado.

O ranking de notas por grupo/squad soma as avaliações (`Nota 5` até `Nota 1`) por `grupoAtendimento` dentro do período filtrado.

## Regras de cálculo de notas

O total de avaliações é calculado pela soma das colunas:

```text
Nota 5 + Nota 4 + Nota 3 + Nota 2 + Nota 1
```

A média de avaliações é calculada por média ponderada:

```text
(Nota 5 × 5 + Nota 4 × 4 + Nota 3 × 3 + Nota 2 × 2 + Nota 1 × 1) ÷ total de avaliações
```

A média geral exibida no card superior respeita o período filtrado.

A média de nota por técnico também respeita o período filtrado e aparece nas abas de notas por técnico. A aba de notas por grupo/squad também exibe a média ponderada do grupo.

- Notas do dia;
- Notas geral.

Nas abas de atendimento, a coluna de média de nota não é exibida.

No ranking **Notas por grupo**, o painel também calcula a participação de cada grupo no total de avaliações do período:

```text
(total de avaliações do grupo ÷ total de avaliações do período) × 100
```

Esse percentual aparece na coluna **% do total**.

## Card de total de avaliações

Além do card de média geral de avaliações, o painel possui um card separado com o total de avaliações consideradas no período selecionado.

Esse total respeita os filtros de data ativos na tela e é calculado pela soma de:

```text
Nota 5 + Nota 4 + Nota 3 + Nota 2 + Nota 1
```

## Histórico cumulativo com Firebase

O painel foi ajustado para trabalhar com histórico cumulativo.

Quando uma nova planilha é importada com Firebase conectado, o sistema:

1. Busca o histórico já salvo no Firebase.
2. Processa os dados da nova planilha.
3. Mescla os dados novos com o histórico existente.
4. Mantém datas antigas mesmo que elas não venham mais no novo CSV.
5. Atualiza/substitui registros que tenham a mesma combinação:

```text
data + técnico + grupo
```

Isso é útil para CSVs de período fixo, em que a exportação sempre remove o dia mais antigo e adiciona um novo dia.

Exemplo:

- Hoje o CSV contém `01/06` até `11/06`.
- Amanhã o CSV contém `02/06` até `12/06`.
- O Firebase manterá `01/06`, atualizará `02/06` até `11/06` e adicionará `12/06`.

Resultado acumulado:

```text
01/06 até 12/06
```

## Atenção sobre correções de dados

Se uma planilha vier com dados incorretos e for importada, os registros com a mesma data, técnico e grupo serão substituídos pelos valores da nova planilha.

Se for necessário começar tudo do zero, use o botão **Limpar**. Quando o Firebase estiver conectado, o painel pergunta se você deseja limpar apenas o navegador ou também o histórico salvo no Firebase.

## Técnicos excluídos automaticamente

A página ignora técnicos sem grupo e também exclui automaticamente os seguintes nomes:

- Jose Eduardo Pagnossim
- Heitor Drago Gois
- João Paulo Cardoso
- Eduardo Thomaz
- Renata Romão

A comparação dos nomes ignora acentos, maiúsculas, minúsculas e espaços duplicados.

## Firebase

A sincronização em Firebase é opcional, mas é recomendada para manter o histórico entre computadores e TVs.

O painel usa o documento:

```text
paineis/brasileirao-atendimento
```

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

      match /registros/{registroId} {
        allow read, write: if true;
      }
    }

    match /{document=**} {
      allow read, write: if false;
    }
  }
}
```

Essa regra ainda permite que quem tenha acesso à página altere o ranking. Para uso mais seguro, o ideal é implementar Firebase Authentication.

## Fluxo recomendado de uso

1. Abra o painel.
2. Configure o Firebase, se ainda não estiver configurado.
3. Importe o CSV ou Excel atualizado.
4. O painel irá mesclar a nova planilha com o histórico existente.
5. Clique em **Sincronizar**, se necessário.
6. Nas TVs ou outros computadores, use **Recarregar** ou **Sincronizar** para buscar o histórico atualizado.

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

Com Firebase configurado, os dados podem ser compartilhados entre computadores e TVs, mantendo o histórico acumulado.

## Ajuste no modo apresentação dos rankings por grupo

No modo apresentação, as abas **Finalizados por grupo** e **Notas por grupo** agora são exibidas em formato de tabela, com as colunas visíveis. Isso permite visualizar as informações completas do período, incluindo total, média e percentual no ranking de notas por grupo.
