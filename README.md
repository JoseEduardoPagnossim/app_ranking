# Performance Suporte Técnico

Painel web estático para acompanhamento da performance do suporte técnico da Soften Sistemas.

O painel consolida chamados finalizados, avaliações por técnico, rankings por grupo de atendimento e indicadores gerais. A visualização é pública para quem possui o link, enquanto ações de escrita são protegidas por login via Firebase Authentication e regras do Firestore.

---

## Status atual da versão

Esta versão possui:

- Firebase config embutido no código.
- Leitura pública dos dados.
- Escrita protegida por Firebase Authentication.
- Regras do Firestore com autorização por e-mail.
- Dados históricos salvos em subcoleção `registros`.
- Botão **Sync** com loading e bloqueio de clique duplo.
- Botão **Atualizar da nuvem** para leitura direta do Firebase.
- Faixa de status dos dados.
- Botão **Firebase** visível apenas para usuário logado.
- Sidebar lateral retrátil com as ações do painel.
- Layout responsivo para desktop, TV e celular.
- Correção dos atalhos de teclado no modal de login.
- Tema claro como padrão.
- Favicon e logo da Soften Sistemas.

---

## Arquivos necessários no GitHub Pages

Mantenha estes arquivos na raiz do repositório:

```text
index.html
favicon.png
soften-logo.png
README.md
```

O arquivo principal do painel deve ser publicado obrigatoriamente como:

```text
index.html
```

Exemplo de URL publicada:

```text
https://joseeduardopagnossim.github.io/app_ranking/
```

---

## Visão geral

O painel permite importar planilhas, consolidar dados históricos e exibir rankings de suporte em formato visual, ideal para acompanhamento interno e exibição em TV.

Principais recursos:

- Importação de CSV, XLS e XLSX.
- Ranking diário de chamados finalizados.
- Ranking diário de notas.
- Ranking geral de chamados finalizados.
- Ranking geral de notas.
- Ranking de chamados finalizados por grupo.
- Ranking de notas por grupo.
- Indicadores superiores com resumo do período.
- Filtros por período, grupo e busca.
- Carrossel automático entre abas.
- Modo apresentação para TV.
- Temas visuais.
- Sincronização com Firebase.
- Atualização direta da nuvem.
- Autenticação para ações administrativas.
- Sidebar lateral retrátil para organizar as ações.
- Layout responsivo para telas menores.

---

## Interface

### Topo do painel

O topo agora é mais limpo e mantém foco em:

- botão **☰ Menu**;
- logo da Soften Sistemas;
- título **Performance Suporte Técnico**;
- mensagens de retorno do sistema.

As ações administrativas foram movidas para uma sidebar lateral retrátil.

---

## Sidebar lateral retrátil

A antiga barra superior de ações foi substituída por uma sidebar lateral.

Para abrir, clique em:

```text
☰ Menu
```

A sidebar possui as seguintes seções:

### Dados

- **Importar CSV**
- **Recarregar**
- **Atualizar da nuvem**
- **Sync**
- Status Firebase

### Visualização

- Seletor de tema
- Carrossel ON/OFF
- Modo apresentação
- Alternância entre 1 coluna e 2 colunas

### Acesso

- Entrar/Sair
- Firebase

### Perigo

- Limpar

A sidebar pode ser fechada de três formas:

- clicando no botão **×**;
- clicando fora da sidebar;
- pressionando **Esc**.

No modo apresentação, a sidebar e o botão Menu ficam ocultos automaticamente.

---

## Botão Firebase

O botão **Firebase** foi mantido apenas para reconfiguração manual em caso de necessidade.

Comportamento atual:

- visitante sem login não vê o botão;
- usuário logado vê o botão;
- a função também bloqueia abertura caso não exista usuário logado;
- a conexão principal já acontece automaticamente pela configuração embutida.

Caso queira restringir o botão Firebase apenas para e-mails específicos, use a constante no código:

```js
const FIREBASE_CONFIG_ALLOWED_EMAILS = [
  // "contato@softensistemas.com.br"
].map(email => email.trim().toLowerCase());
```

Com a lista vazia, qualquer usuário logado no Firebase Authentication consegue visualizar o botão.  
Com e-mails definidos na lista, apenas esses usuários logados conseguem visualizar/reconfigurar.

---

## Firebase config embutido

O painel já possui a configuração do Firebase embutida no código, portanto o usuário não precisa colar o JSON manualmente ao abrir o painel.

A configuração pública do Firebase Web pode ficar no front-end. A segurança dos dados deve ser feita pelas regras do Firestore e pelo Firebase Authentication.

---

## Firebase Authentication

O painel usa Firebase Authentication para proteger ações de escrita.

A visualização do painel é pública para quem possui o link.

Ações liberadas sem login:

- visualizar rankings;
- alternar abas;
- aplicar filtros;
- atualizar dados da nuvem;
- usar modo apresentação, se disponível na interface.

Ações que exigem login autorizado:

- importar planilha;
- sincronizar dados com Firebase;
- salvar histórico no Firebase;
- limpar histórico do Firebase;
- acessar/reconfigurar Firebase pela interface.

---

## Como criar usuários autorizados

No Firebase Console:

1. Acesse o projeto Firebase.
2. Vá em **Authentication**.
3. Acesse **Sign-in method**.
4. Ative o provedor **Email/Password**.
5. Vá até a aba **Users**.
6. Clique em **Add user**.
7. Informe o e-mail autorizado.
8. Defina uma senha.
9. Salve o usuário.

Depois disso, adicione o e-mail nas regras do Firestore.

---

## Regras recomendadas do Firestore

Acesse:

```text
Firebase Console → Firestore Database → Rules
```

Cole as regras abaixo, trocando os e-mails de exemplo pelos usuários autorizados.

```js
rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {

    function isAuthorized() {
      return request.auth != null
        && request.auth.token.email in [
          "usuario1@softensistemas.com.br",
          "usuario2@softensistemas.com.br"
        ];
    }

    match /paineis/brasileirao-atendimento {
      allow read: if true;
      allow write: if isAuthorized();

      match /registros/{registroId} {
        allow read: if true;
        allow write, delete: if isAuthorized();
      }
    }

    match /{document=**} {
      allow read, write: if false;
    }
  }
}
```

Com essas regras:

- qualquer pessoa com o link pode visualizar o painel;
- apenas usuários autenticados e listados na função `isAuthorized()` podem escrever;
- a subcoleção `registros` também fica protegida para escrita;
- o restante do banco permanece bloqueado.

---

## Domínio autorizado no Firebase Auth

Após publicar no GitHub Pages, confirme se o domínio está autorizado no Firebase Authentication.

Caminho:

```text
Firebase Console → Authentication → Settings → Authorized domains
```

Adicione o domínio:

```text
joseeduardopagnossim.github.io
```

Sem isso, o login pode funcionar localmente, mas falhar no GitHub Pages.

---

## Estrutura dos dados no Firestore

Os dados são armazenados nesta estrutura:

```text
paineis
└── brasileirao-atendimento
    └── registros
        └── {registroId}
```

Documento principal:

```text
paineis/brasileirao-atendimento
```

Subcoleção histórica:

```text
paineis/brasileirao-atendimento/registros
```

Essa estrutura evita o limite de tamanho de documento do Firestore, pois os registros ficam distribuídos em documentos individuais dentro da subcoleção `registros`.

---

## Estrutura da planilha importada

A planilha deve conter, no mínimo, as colunas:

```text
time
Tecnico
grupoAtendimento
Quantidade
```

Para uso completo das abas de notas, a planilha também deve conter:

```text
Nota 5
Nota 4
Nota 3
Nota 2
Nota 1
```

---

## Cálculo da média das avaliações

A média é calculada de forma ponderada:

```text
(Nota 5 × 5 + Nota 4 × 4 + Nota 3 × 3 + Nota 2 × 2 + Nota 1 × 1) / Total de avaliações
```

Exemplo:

```text
Nota 5 = 10
Nota 4 = 5
Nota 3 = 2
Nota 2 = 0
Nota 1 = 0

Média = (10×5 + 5×4 + 2×3) / 17
Média = 4,47
```

---

## Indicadores superiores

O painel exibe cards com:

1. Última data importada.
2. Dias no período.
3. Líder em atendimentos.
4. Líder em notas.
5. Média das avaliações.
6. Total de avaliações.

Também existe uma faixa de status dos dados com:

- fonte dos dados;
- última sincronização;
- total de registros históricos carregados.

Exemplo:

```text
Fonte: Firebase • Última sincronização: 15/06/2026, 14:54 • Registros históricos: 7.399
```

---

## Abas disponíveis

### Finalizados do Dia

Mostra o ranking de técnicos por chamados finalizados na última data do período.

### Notas do dia

Mostra o ranking diário de avaliações por técnico.

### Qtd. Finalizados Geral

Mostra o ranking geral de chamados finalizados por técnico no período selecionado.

### Qtd. Notas Geral

Mostra o ranking geral de avaliações por técnico no período selecionado.

### Finalizados por grupo

Mostra a quantidade de chamados finalizados agrupados por grupo de atendimento.

### Notas por grupo

Mostra a performance de avaliações agrupada por grupo de atendimento, incluindo:

- média ponderada;
- total de avaliações;
- percentual de participação no total de avaliações do período.

---

## Filtros disponíveis

O painel possui filtros para análise por:

- mês atual;
- mês anterior;
- todo o histórico;
- período personalizado;
- grupo de atendimento;
- busca por técnico ou grupo.

---

## Carrossel automático

O carrossel alterna automaticamente entre as abas principais.

Abas contempladas:

```text
Finalizados do Dia
Notas do dia
Qtd. Finalizados Geral
Qtd. Notas Geral
Finalizados por grupo
Notas por grupo
```

O carrossel pode ser ativado ou desativado pela sidebar.

---

## Modo apresentação

O modo apresentação é voltado para TV ou monitores de acompanhamento.

Quando ativo, o painel:

- mantém a sidebar e o botão Menu disponíveis;
- esconde ações administrativas;
- melhora o aproveitamento da tela;
- ajusta a densidade das tabelas;
- mantém foco nos rankings e indicadores;
- permite sair com a tecla **Esc**.

---

## Responsividade

Foram adicionados ajustes para telas menores.

Melhorias aplicadas:

- sidebar lateral em vez de barra superior cheia de botões;
- cards superiores se reorganizam em 3, 2 ou 1 coluna conforme a largura;
- abas passam a ter rolagem horizontal no celular;
- filtros e botões evitam quebra visual em telas pequenas;
- ranking se adapta melhor em resoluções mobile;
- modal de login fica ajustado para celular.

---

## Correção dos atalhos de teclado

O painel possui atalhos como:

- `P` para modo apresentação;
- `D` para aba do dia;
- `G` para ranking geral;
- `R` para ranking por grupo;
- `Esc` para sair do modo apresentação ou fechar a sidebar.

Foi aplicada uma correção para que esses atalhos não sejam disparados enquanto o usuário digita em campos de texto, como e-mail e senha no modal de login.

Isso evita o problema de o painel entrar em modo apresentação durante a digitação.

---

## Temas disponíveis

O painel possui os seguintes temas:

- Clássico.
- Soften.
- Neon.
- Claro.
- Oceano.
- Grafite.
- Sunset.
- Roxo.
- Verde.

O tema claro está definido como padrão.

---

## Botões principais

### Importar CSV

Importa arquivos CSV, XLS ou XLSX.

Exige login autorizado.

### Recarregar

Recarrega dados salvos localmente no navegador.

### Atualizar da nuvem

Busca dados diretamente do Firebase.

Não exige login, pois executa apenas leitura.

### Sync

Sincroniza dados locais e dados da nuvem.

Exige login autorizado, pois pode salvar no Firestore.

Durante a execução, o botão entra em estado de loading para evitar clique duplo.

### Entrar/Sair

Permite autenticar um usuário autorizado pelo Firebase Authentication.

### Firebase

Permite visualizar ou reconfigurar manualmente o Firebase config.

Visível apenas para usuário logado.

### Modo apresentação

Ativa a visualização otimizada para TV.

### 1 coluna / 2 colunas

Alterna o layout do ranking quando fora do modo apresentação.

### Limpar

Remove dados locais e permite limpar o histórico do Firebase.

A limpeza da nuvem exige login autorizado.

---

## Técnicos excluídos do ranking

O painel possui uma lista interna de técnicos removidos dos rankings.

Essa lista é configurada diretamente no código:

```js
const EXCLUDED_TECHNICIANS = [
  "Jose Eduardo Pagnossim",
  "Heitor Drago Gois",
  "João Paulo Cardoso",
  "Eduardo Thomaz",
  "Renata Romão",
  "LETICIA SANCHES"
].map(normalizeName);
```

---

## Como publicar no GitHub Pages

1. Renomeie a versão final do painel para `index.html`.
2. Suba o arquivo na raiz do repositório.
3. Suba também:
   - `favicon.png`;
   - `soften-logo.png`;
   - `README.md`.
4. Confirme que o GitHub Pages está ativo.
5. Acesse a URL pública do projeto.
6. Teste a visualização sem login.
7. Teste o login com usuário autorizado.
8. Teste Sync e importação.
9. Teste o painel no celular.
10. Teste o modo apresentação.

---

## Como testar após publicar

### 1. Teste sem login

Abra o painel em janela anônima.

Resultado esperado:

- dados carregam normalmente;
- abas funcionam;
- filtros funcionam;
- botão Atualizar da nuvem funciona;
- ações de escrita solicitam login;
- botão Firebase não aparece.

### 2. Teste com login autorizado

Entre com um usuário criado no Firebase Authentication e listado nas regras do Firestore.

Resultado esperado:

- importação funciona;
- Sync funciona;
- gravação no Firebase funciona;
- limpeza do Firebase funciona após confirmação;
- botão Firebase aparece.

### 3. Teste com usuário não autorizado

Crie um usuário no Firebase Authentication, mas não adicione o e-mail nas regras do Firestore.

Resultado esperado:

- login é aceito;
- leitura funciona;
- escrita é bloqueada pelas regras do Firestore.

### 4. Teste no celular

Use o modo de dispositivo do navegador ou um celular real.

Verifique:

- botão Menu;
- sidebar lateral;
- abas com rolagem horizontal;
- modal de login;
- ranking;
- cards superiores.

### 5. Teste de cache

Após subir uma nova versão, abra com parâmetro de versão:

```text
https://joseeduardopagnossim.github.io/app_ranking/?v=1
```

Em novas publicações, altere o número:

```text
https://joseeduardopagnossim.github.io/app_ranking/?v=2
```

Isso ajuda a evitar cache do navegador.

---

## Observações de segurança

A configuração pública do Firebase no HTML não deve ser tratada como senha.

A proteção real depende de:

- Firebase Authentication ativo;
- regras do Firestore publicadas corretamente;
- e-mails autorizados mantidos atualizados nas regras;
- domínio do GitHub Pages autorizado no Firebase Auth.

A leitura pública foi mantida para facilitar o uso em TVs e computadores internos.

A escrita deve permanecer restrita a usuários autorizados.

---

## Manutenção recomendada

Sempre que adicionar novo usuário autorizado:

1. Crie o usuário em **Firebase Authentication**.
2. Adicione o e-mail na função `isAuthorized()` das regras do Firestore.
3. Publique as regras.
4. Teste login e Sync.

Sempre que publicar nova versão:

1. Suba o novo `index.html`.
2. Confirme que `favicon.png` e `soften-logo.png` estão no repositório.
3. Suba o `README.md` atualizado.
4. Abra o painel com `?v=numero`.
5. Teste leitura sem login.
6. Teste escrita com login autorizado.
7. Teste sidebar e responsividade.

---

## Modo TV/Grafana permanente

Esta versão foi ajustada para uso em dashboard/TV, inclusive dentro de painéis como Grafana.

Comportamento padrão:

- o painel abre automaticamente em **modo apresentação**;
- o layout permanece em modo apresentação mesmo se a página for recarregada;
- a tela não depende mais do clique manual em **Modo apresentação**;
- o painel não tenta entrar em tela cheia automaticamente, evitando instabilidade dentro de iframe/dashboard;
- os filtros continuam visíveis no modo apresentação;
- a sidebar continua disponível no modo TV;
- apenas o botão **Modo apresentação** fica oculto, pois o painel já abre nesse modo.

### URL para TV/Grafana

Use a URL normal do painel:

```text
https://joseeduardopagnossim.github.io/app_ranking/
```

### URL para acessar o painel administrativo

Para abrir o painel fora do modo apresentação permanente, use:

```text
https://joseeduardopagnossim.github.io/app_ranking/?admin=1
```

Essa URL libera a sidebar, menu, importação, Sync, login e demais ações administrativas.

### Tela cheia opcional

Caso queira forçar tentativa de tela cheia fora do Grafana, use:

```text
https://joseeduardopagnossim.github.io/app_ranking/?fullscreen=1
```

No Grafana, a recomendação é não usar `fullscreen=1`, pois iframe/dashboard pode bloquear ou causar comportamento instável.



### Menu lateral no modo TV

Nesta versão TV/Grafana, o menu lateral continua acessível mesmo em modo apresentação permanente.

Ficam disponíveis:

- Importar CSV;
- Recarregar;
- Atualizar da nuvem;
- Sync;
- status Firebase;
- tema;
- carrossel;
- 1 coluna / 2 colunas;
- Entrar/Sair;
- Firebase;
- Limpar.

O único botão removido do menu no modo TV é **Modo apresentação**, porque o painel já abre automaticamente nesse modo.
