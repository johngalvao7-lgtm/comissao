# A mudança para a Cloudflare — passo a passo

Tempo total da sua parte: uns 40 minutos, em dois momentos. O resto quem faz é o Claude Code na nuvem.

| Passo | Quem | Onde | Tempo |
|---|---|---|---|
| 1. Subir as fontes para o GitHub | Você | Navegador do Mac (Chrome ou Safari) | 10 min |
| 2. Criar a conta na Cloudflare | Você | Navegador do Mac | 10 min |
| 3. Construir e testar | Claude Code na nuvem, modelo **Opus** | claude.ai/code | Ele trabalha sozinho |
| 4. Ligar a Cloudflare ao GitHub | Você | Navegador do Mac | 10 min |
| 5. Teste no endereço real | Claude Code na nuvem, **Opus** | mesma conversa do passo 3 | Ele trabalha sozinho |
| 6. Ensaio com 3 celulares | Você | Celulares de verdade | 10 min |
| 7. Atualizar o livro | Chat do Claude, **Opus** | claude.ai | Uma conversa |

---

## 1. Subir as fontes para o GitHub

**Agora:**
1. No Mac, dê dois cliques no arquivo **Fontes_Discernindo.zip**. Aparece uma pasta.
2. Abra **github.com**, entre no repositório **comissao** (o mesmo do site).
3. Clique em **Add file** e depois **Upload files**.
4. Arraste para a página **tudo o que está dentro da pasta** (as pastas `app` e `livro`, o `textos.json` e o `LEIA-ME.md`). Arraste também os arquivos **Continuidade_Discernindo.md**, **Padrao_de_construcao.md** e **este arquivo**.
5. Clique em **Commit changes**.

Se o Safari não aceitar arrastar pastas, use o Google Chrome só para este passo.

**Por quê:** o Claude Code na nuvem trabalha dentro do repositório. Com as fontes lá, ele parte do que já existe, sem reconstruir nada.

## 2. Criar a conta na Cloudflare

**Agora:**
1. Abra **dash.cloudflare.com/sign-up**. Cadastre seu e-mail e uma senha forte. Confirme o e-mail.
2. Se perguntar o plano, escolha **Free** (gratuito).
3. Se pedir um "subdomínio de workers", escreva **discernindo**. O endereço do site vai ficar parecido com `comissao.discernindo.workers.dev`.

**Por quê:** a Cloudflare vai hospedar o site e o "cérebro" de cada reunião no mesmo lugar. O plano gratuito cobre uma igreja por dia com folga.

Guarde a senha no seu gerenciador de senhas. Ninguém além de você precisa dela.

## 3. Construir e testar — Claude Code na nuvem

**Agora:**
1. Abra **claude.ai/code**. Escolha o repositório **comissao**.
2. Nas configurações do ambiente, se houver opção de acesso à internet, escolha **acesso completo**. Ele vai precisar baixar ferramentas e, no passo 5, testar o endereço real.
3. Escolha o modelo **Opus**.
4. Copie e cole o pedido abaixo, inteiro.

```text
Você vai migrar e elevar o site "A Comissão" deste repositório. Leia primeiro, por inteiro:
Continuidade_Discernindo.md, Padrao_de_construcao.md, Migracao_Cloudflare.md, LEIA-ME.md,
app/app.js e app/molde.html. O usuário (pastor, não programador) exige qualidade máxima e
praticidade extraordinária desde a primeira entrega: antes de construir, compare as
alternativas reais de cada decisão técnica e escolha a melhor; registre o porquê.

PROBLEMA A RESOLVER
No Firebase, em uso real, os membros ficaram presos em "Você entrou" depois que o presidente
começou. Causa provável: a conexão contínua (streaming) do Firestore caiu em silêncio em
celulares/Safari/redes móveis. O objetivo é eliminar essa classe de falha e a incerteza.

ARQUITETURA PEDIDA
- Cloudflare Workers (plano Free) servindo o site (static assets) e uma API no mesmo domínio.
- Um Durable Object com backend SQLite por reunião (exigência do plano Free). Ele é a única
  fonte da verdade e serializa todas as ações.
- Transporte principal: requisições HTTP comuns (consulta a cada ~1,5 s, ou long-poll curto).
  WebSocket só se ficar comprovadamente mais estável nos testes, sempre com recuo automático
  para HTTP. Regra de ouro: se a página abre no celular, a reunião funciona.
- Estado com número de versão; o cliente pede "o que mudou desde a versão X".
- Identidade sem conta: token aleatório por aparelho (localStorage). O presidente recebe um
  token próprio ao criar a reunião.
- O servidor impõe as regras: só o presidente avança passos; um registro por aparelho por
  rodada; voto até N nomes (vagas) e só 1 quando a função é de uma pessoa.
- Sigilo garantido pelo servidor: nunca devolver quem propôs quem, quem marcou o quê, nem quem
  votou em quem. Aos clientes vão só agregados (listas unidas, contagens, notas consolidadas)
  e os registros do próprio aparelho. Anotações pessoais nunca saem do aparelho.
- Apagamento automático dos dados da reunião 60 dias depois (alarme do Durable Object), além
  do botão "Apagar agora".
- Remover o Firebase por completo (sem código para colar, sem configuração no link).
- QR code e link curtos: https://<endereço>/?r=CODIGO.

PRESERVAR TUDO O QUE JÁ EXISTE (comportamento e textos)
Dois tipos de reunião (nomeações e organizadora, com quadro de equilíbrio); abertura, quatro
combinados, cinco frases; função com características e "quantas pessoas"; propostas em
silêncio unidas automaticamente; observações desde a lista grande com as três
características (70/15/15), nota de 20 a 100, média sem a maior e a menor com 5+ marcações,
alerta acima de 30 pontos, aviso de empate ≤5 pontos com o princípio da alternância na lista
pequena; afinar pela nota (~metade, sem cortar empates, presidente ajusta) até vagas+1 ou 3;
oração; voto secreto com redação de dependência de Deus ("Em sua consciência, quem você
discerne ser a escolha de Deus...", "ainda não discerni"); maioria = mais da metade dos votos
dados; empate na última vaga fica para nova oração; resultado, quem conversa (anuência),
"discernir as vagas que faltam", relatório final. Mesmo visual (tokens de cor, gota dourada,
uma coisa por tela, jeito de conversa). Modo "só experimentar" continua existindo.

ACRESCENTAR
1. Ensaio de um minuto antes de começar: cada membro toca "Estou vendo"; o presidente vê
   "7 de 7 prontos". Durante a reunião, presença viva (sinal a cada poucos segundos) e um
   ponto discreto de quem caiu; "x de N" conta só quem está presente.
2. Quórum: o presidente informa o quórum votado; o site avisa se os presentes ficarem abaixo.
3. Modo "um aparelho só" (sem internet): tudo no aparelho do presidente; nas propostas,
   observações e voto, o aparelho passa de mão em mão, com tela de privacidade entre um membro
   e outro ("Passe para o próximo"). Mesma conta e mesma apuração.
4. Modo telão: no Mac ligado à TV, mostra o passo e os textos em letra grande.
5. Aplicativo na tela inicial (manifest, ícone da gota dourada, nome "A Comissão").
6. Relatório em PDF e botão para enviar pelo WhatsApp.
7. Impressos gerados pelo site, prontos para imprimir: ficha de observações já com os nomes,
   cédula com o número certo de linhas, folha de indicação de cada classe.
8. Letra maior e alto contraste com um toque.
9. Cronômetro discreto de 1–2 minutos por nome nas observações da lista grande.

TESTES (obrigatórios antes de considerar pronto)
- Playwright com Chromium e WebKit (motor do Safari), 10 aparelhos simulados, rodando contra
  `wrangler dev` local e, depois do passo 4, contra o endereço real publicado.
- Cenários: entrar por QR/link; ensaio 10/10; reunião completa de nomeações com lista grande,
  afinar, observações, quadro, oração, voto com 1 vaga e com 3 vagas; tentativa de marcar
  além das vagas; sem maioria; empate na última vaga; organizadora; anuência e relatório;
  modo um aparelho; recarregar a página no meio; aparelho que perde a rede por 30 s e volta
  (deve se recuperar sozinho); latência alta simulada; dois cliques rápidos (sem voto duplo).
- Teste de sigilo: nenhuma resposta da API pode conter autor de proposta, marcação ou voto.
- Estimar pedidos por reunião (10 aparelhos, 3 horas) e confirmar que cabe no plano Free.

PUBLICAÇÃO
Use a integração "Workers Builds" da Cloudflare ligada a este repositório (cada push na
branch principal publica sozinho). Deixe o wrangler.jsonc pronto para isso. Quando a parte
local estiver testada, pare e me diga exatamente o que o usuário deve clicar na Cloudflare
para ligar o repositório (passo 4 do Migracao_Cloudflare.md), em português simples.

ENTREGA
Código, README em português, relatório dos testes (o que passou, com números) e uma lista
curta do que muda no livro (instalação nova, modo um aparelho, ensaio, telão, impressos).
```

**Por quê:** o pedido traz a arquitetura, tudo o que precisa ser preservado e os testes obrigatórios. Assim o Claude Code entrega certo da primeira vez e só considera pronto o que passou nos testes.

## 4. Ligar a Cloudflare ao GitHub

Faça quando o Claude Code avisar que a parte local está testada. Ele vai dizer os cliques exatos. Em geral, é assim:

1. Na Cloudflare, abra **Workers e Pages** (Workers & Pages) e clique em **Criar** (Create).
2. Escolha **Importar um repositório** (Import a repository) e conecte sua conta do GitHub.
3. Dê acesso **só ao repositório comissao**.
4. Confirme. A Cloudflare publica e mostra o endereço do site.
5. Copie o endereço e cole na conversa do Claude Code.

**Por quê:** ligando o repositório uma vez, toda melhoria futura do Claude Code chega ao site sozinha, sem você subir arquivo nenhum.

## 5. Teste no endereço real

O Claude Code roda os mesmos testes, agora contra o endereço publicado, inclusive com o motor do Safari. Só siga para o passo 6 quando ele disser que **todos passaram**.

## 6. Ensaio com 3 celulares de verdade

1. No Mac ou no iPhone, abra o endereço e toque em **Conduzir uma reunião**. Crie uma reunião de teste com cinco nomes inventados.
2. Peça a duas pessoas, cada uma no próprio celular e de preferência com operadoras diferentes, que leiam o QR code.
3. Confira no ensaio: **"3 de 3 prontos"**.
4. Faça uma proposta, uma observação e um voto. Durante o teste, desligue o Wi-Fi de um celular por meio minuto e religue. Ele tem de voltar sozinho.
5. Apague a reunião no relatório final.

Se algo falhar, tire um print e mande na conversa do Claude Code.

Depois de tudo validado, você pode apagar o projeto do Firebase, no console do Firebase, em Configurações do projeto. Ele não será mais usado.

## 7. Atualizar o livro

Num chat novo do Claude, modelo **Opus**, anexe **Continuidade_Discernindo.md**, **Fontes_Discernindo.zip** e a lista do que mudou que o Claude Code entregar. Peça: "Atualize o livro com estas mudanças, no padrão de sempre." A parte de instalação vai ficar bem mais curta.

---

### Endereço próprio (opcional)

- **Padrão recomendado:** o endereço gratuito da Cloudflare. Os membros entram pelo QR code ou pelo link do WhatsApp e não digitam nada, então o endereço longo não atrapalha.
- **Melhoria opcional:** um domínio próprio e curto, como `discernir.com.br`, por cerca de R$ 40 por ano no registro.br. Ele dá cara mais oficial para outras igrejas e para a Associação. Pode ser ligado depois, sem mudar nada no site.
