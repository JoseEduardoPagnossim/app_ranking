# Brasileirão do Atendimento

Painel web em estilo transmissão/TV para classificação de atendimentos por técnico.  
A página importa uma planilha CSV, XLS ou XLSX, calcula o ranking por período e exibe uma classificação dinâmica com destaque para melhores e piores posições.

## Funcionalidades

- Importação de arquivos `.csv`, `.xls` e `.xlsx`.
- Salvamento automático dos dados no navegador usando `localStorage`.
- Ao importar um novo arquivo, os dados anteriores são substituídos.
- Filtro por período:
  - mês atual;
  - mês anterior;
  - todos os dados;
  - período personalizado.
- Classificação geral do período.
- Classificação do último dia da planilha.
- Destaque visual para G6 e Z4.
- Cards superiores com:
  - data do dia;
  - quantidade de rodadas;
  - líder do dia;
  - líder geral;
  - nome do arquivo importado.
- Modo apresentação para TV.
- Layout em duas colunas.
- Saída do modo apresentação usando a tecla `Esc`.
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

A página também aceita arquivos separados por ponto e vírgula (`;`).

## Regras de cálculo

Cada data encontrada na coluna `time` é considerada uma rodada.

A classificação geral soma a quantidade de atendimentos de cada técnico dentro do período filtrado.

A classificação do dia considera apenas a última data encontrada dentro do período selecionado.

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

## Como publicar no GitHub Pages

1. Crie um repositório no GitHub.
2. Envie o arquivo principal com o nome `index.html`.
3. Envie também este arquivo `README.md`.
4. No GitHub, acesse:
   - **Settings**
   - **Pages**
   - **Build and deployment**
   - Source: **Deploy from a branch**
   - Branch: `main`
   - Folder: `/root`
5. Salve as configurações.
6. Aguarde o GitHub gerar o link da página.

## Uso em TV

Para melhor resultado em uma televisão:

- use o navegador em zoom `100%`;
- abra a página em tela cheia;
- clique em **Modo apresentação**;
- pressione `Esc` para sair do modo apresentação;
- caso use uma TV com navegador próprio, prefira resolução Full HD ou superior.

## Dados e privacidade

Os dados importados não são enviados para servidor.  
Tudo fica salvo apenas no navegador usado, através de `localStorage`.

Se você abrir a página em outro computador, navegador ou TV, será necessário importar a planilha novamente.

## Observações

A página é estática e pode ser hospedada gratuitamente no GitHub Pages.  
Não é necessário backend, banco de dados ou instalação de dependências.
