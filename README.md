# Performance Suporte Técnico

Painel web para acompanhamento da performance do suporte técnico da Soften Sistemas.

O painel consolida chamados finalizados, avaliações por técnico, rankings por grupo de atendimento e indicadores gerais, com leitura pública via Firebase/Firestore e escrita protegida por login autorizado.

---

## Visão geral

O projeto é um painel HTML estático, publicado via GitHub Pages, com integração ao Firebase.

Principais recursos:

- Importação de planilhas CSV, XLS e XLSX.
- Ranking diário de chamados finalizados.
- Ranking diário de notas.
- Ranking geral de chamados finalizados.
- Ranking geral de notas.
- Ranking de chamados finalizados por grupo.
- Ranking de notas por grupo.
- Cards de indicadores principais.
- Carrossel automático entre abas.
- Modo apresentação para TV.
- Temas visuais configuráveis.
- Sincronização com Firebase/Firestore.
- Atualização direta da nuvem.
- Login por Firebase Authentication para ações de escrita.
- Leitura pública para visualização do painel.

---

## Arquivos necessários no GitHub Pages

Para publicação, mantenha estes arquivos na raiz do repositório:

```text
index.html
favicon.png
soften-logo.png
README.md
```

O arquivo principal do painel deve estar obrigatoriamente com o nome:

```text
index.html
```

Exemplo de URL publicada:

```text
https://joseeduardopagnossim.github.io/app_ranking/
```

---

## Estrutura dos dados importados

A planilha importada deve conter, no mínimo, as seguintes colunas:

```text
time
Tecnico
grupoAtendimento
Quantidade
```

Para uso das abas de avaliações, a planilha também deve conter as colunas:

```text
Nota 5
Nota 4
Nota 3
Nota 2
Nota 1
```

---

## Indicadores exibidos

O painel possui cards superiores com os principais indicadores:

1. Última data importada.
2. Dias no período.
3. Líder em atendimentos.
4. Líder em notas.
5. Média das avaliações.
6. Total de avaliações.

Também existe uma faixa de status dos dados, indicando:

- origem dos dados;
- última sincronização;
- total de registros históricos carregados.

---

## Abas do painel

O painel possui as seguintes abas:

### Finalizados do Dia

Mostra o ranking de técnicos com maior quantidade de chamados finalizados no dia selecionado ou no período filtrado.

### Notas do dia

Mostra o ranking diário de avaliações por técnico, considerando a média ponderada das notas.

### Qtd. Finalizados Geral

Mostra o ranking geral de chamados finalizados por técnico no período selecionado.

### Qtd. Notas Geral

Mostra o ranking geral de avaliações por técnico no período selecionado.

### Finalizados por grupo

Mostra o total de chamados finalizados agrupados por grupo de atendimento.

### Notas por grupo

Mostra a performance de avaliações agrupada por grupo de atendimento, incluindo:

- média ponderada das notas;
- total de avaliações;
- percentual de participação no total de avaliações do período.

---

## Cálculo da média das avaliações

A média de avaliações é calculada de forma ponderada:

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

## Filtros disponíveis

O painel possui filtros para análise por período e grupo:

- Mês atual.
- Mês anterior.
- Tudo.
- Datas personalizadas.
- Grupo de atendimento.
- Busca por técnico ou grupo.

---

## Carrossel automático

O painel possui alternância automática entre as abas principais, ideal para exibição em TV.

Abas contempladas no carrossel:

```text
Finalizados do Dia
Notas do dia
Qtd. Finalizados Geral
Qtd. Notas Geral
Finalizados por grupo
Notas por grupo
```

O carrossel pode ser ativado ou desativado no próprio painel.

---

## Modo apresentação

O modo apresentação foi pensado para TV ou monitores de acompanhamento.

Quando ativo, o painel:

- esconde ações administrativas;
- reduz elementos desnecessários;
- melhora o aproveitamento da tela;
- alterna entre rankings;
- ajusta a densidade das tabelas conforme o volume de dados.

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

O tema claro é usado como padrão.

---

## Firebase

O painel usa Firebase para armazenar o histórico de registros e permitir que diferentes dispositivos visualizem os mesmos dados.

### Estrutura no Firestore

Os dados são armazenados na seguinte estrutura:

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

Subcoleção de registros:

```text
paineis/brasileirao-atendimento/registros
```

Essa estrutura evita ultrapassar o limite de tamanho de documento do Firestore, já que os registros históricos ficam distribuídos em documentos individuais.

---

## Firebase config embutido

O painel já possui a configuração do Firebase embutida no código.

Isso significa que o usuário não precisa colar o JSON do Firebase manualmente ao abrir o painel.

A configuração pública do Firebase Web pode ficar no front-end. A proteção real dos dados deve ser feita pelas regras do Firestore e pelo Firebase Authentication.

---

## Autenticação e autorização

A visualização do painel é pública para quem possui o link.

A escrita é protegida por login autorizado.

Ações que exigem login autorizado:

- importar planilha;
- sincronizar dados;
- salvar histórico no Firebase;
- limpar histórico do Firebase.

Ações liberadas sem login:

- visualizar painel;
- alternar abas;
- aplicar filtros;
- atualizar dados da nuvem.

---

## Como criar usuários autorizados

No Firebase Console:

1. Acesse o projeto Firebase.
2. Vá em **Authentication**.
3. Acesse **Sign-in method**.
4. Ative o provedor **Email/Password**.
5. Vá para a aba **Users**.
6. Clique em **Add user**.
7. Informe o e-mail autorizado.
8. Defina uma senha.
9. Salve o usuário.

Depois disso, adicione o e-mail do usuário nas regras do Firestore.

---

## Regras recomendadas do Firestore

Use as regras abaixo em:

```text
Firebase Console → Firestore Database → Rules
```

Troque os e-mails de exemplo pelos usuários autorizados da Soften Sistemas.

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
- apenas usuários autenticados e listados em `isAuthorized()` podem alterar dados;
- registros da subcoleção `registros` também ficam protegidos para escrita;
- o restante do banco permanece bloqueado.

---

## Domínio autorizado no Firebase Auth

Após publicar no GitHub Pages, confirme se o domínio está liberado no Firebase Authentication.

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

## Como testar após publicar

### 1. Teste de visualização pública

Abra o painel em janela anônima.

Resultado esperado:

- o painel deve carregar os dados;
- as abas devem funcionar;
- o botão **Atualizar da nuvem** deve funcionar;
- ações de escrita devem exigir login.

### 2. Teste de login autorizado

Entre com um usuário criado no Firebase Authentication e listado nas regras do Firestore.

Resultado esperado:

- importação de planilha deve funcionar;
- Sync deve funcionar;
- gravação no Firebase deve funcionar;
- limpeza do histórico deve solicitar confirmação e executar corretamente.

### 3. Teste de usuário não autorizado

Crie um usuário no Firebase Authentication, mas não adicione o e-mail nas regras do Firestore.

Resultado esperado:

- o login será aceito;
- a leitura continuará funcionando;
- tentativas de escrita serão bloqueadas pelo Firestore.

### 4. Teste de cache no GitHub Pages

Após subir uma nova versão, abra usando um parâmetro de versão:

```text
https://joseeduardopagnossim.github.io/app_ranking/?v=1
```

Caso publique outra versão, altere o parâmetro:

```text
https://joseeduardopagnossim.github.io/app_ranking/?v=2
```

Isso ajuda a evitar cache do navegador.

---

## Botões principais

### Firebase

Mantido para eventual reconfiguração manual, mas o painel já possui configuração embutida.

### Atualizar da nuvem

Busca os dados diretamente do Firebase.

Não exige login, pois realiza apenas leitura.

### Sync

Sincroniza dados locais e dados da nuvem.

Exige login autorizado, pois pode salvar informações no Firebase.

Durante a execução, o botão exibe estado de carregamento para evitar clique duplo.

### Importar

Importa planilhas CSV, XLS ou XLSX.

Exige login autorizado.

### Limpar

Remove dados locais e pode remover também o histórico salvo no Firebase.

A limpeza da nuvem exige login autorizado.

---

## Técnicos excluídos do ranking

O painel possui uma lista interna de técnicos que não entram nos rankings.

Essa lista é configurada diretamente no código, na constante:

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

## Publicação no GitHub Pages

Passos básicos:

1. Gere ou mantenha o arquivo atualizado como `index.html`.
2. Suba o arquivo para a raiz do repositório.
3. Suba também `favicon.png` e `soften-logo.png`.
4. Confirme que o GitHub Pages está ativo.
5. Abra a URL pública do projeto.
6. Teste visualização sem login.
7. Teste escrita com login autorizado.

---

## Observações de segurança

Este painel permite leitura pública para facilitar o uso em TVs e computadores internos.

A segurança de escrita depende de:

- Firebase Authentication ativo;
- regras do Firestore publicadas corretamente;
- e-mails autorizados mantidos atualizados nas regras;
- domínio do GitHub Pages liberado no Firebase Auth.

A configuração pública do Firebase no HTML não deve ser tratada como senha. Quem protege o acesso aos dados são as regras do Firestore.

---

## Manutenção recomendada

Sempre que adicionar novos usuários autorizados:

1. Crie o usuário em **Firebase Authentication**.
2. Adicione o e-mail dele na função `isAuthorized()` das regras do Firestore.
3. Publique as regras.
4. Teste login e Sync.

Sempre que publicar nova versão:

1. Suba o novo `index.html`.
2. Confirme que `favicon.png` e `soften-logo.png` continuam no repositório.
3. Abra o painel com parâmetro `?v=numero`.
4. Teste leitura sem login.
5. Teste escrita com login autorizado.

---

## Status atual do projeto

Versão atual com:

- Firebase config embutido.
- Login por Firebase Authentication.
- Escrita protegida por usuários autorizados.
- Leitura pública.
- Sync com loading e bloqueio de clique duplo.
- Botão Atualizar da nuvem.
- Faixa de status dos dados.
- Tema claro como padrão.
- Favicon e logo da Soften.
