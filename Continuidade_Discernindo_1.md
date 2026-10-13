# Continuidade — "Discernindo a vontade de Deus" (eleição de oficiais da igreja local)

Pr. João Galvão, pastor distrital (Condeúba-BA, ~10 congregações em dois polos). Não é programador: guiar passo a passo, dizendo o que fazer, por quê, e qual modelo usar e onde.

## Como abrir um novo chat sobre este projeto
1. Anexe **este arquivo** e o **Fontes_Discernindo.zip** (fontes do livro e do site). Com eles, o Claude regenera o livro e o site sem reconstruir nada.
2. Para mudanças só de texto ou de decisão: chat do Claude, modelo **Opus**.
3. Para mudanças grandes no site (código): **Claude Code na nuvem**, modelo **Opus**, apontando para o repositório do GitHub onde está o `index.html` (repositório `comissao`). Peça que ele também rode os testes da pasta `app/`.
4. Diga só o que quer mudar. O padrão de construção está no arquivo `Padrao_de_construcao.md`.

## O que existe hoje (versão de 3 de outubro de 2026)
- **Livro** `Discernindo_a_vontade_de_Deus.docx` (+ PDF para imprimir), 47 páginas, padrão Porta e Poço:
  1 Como usar · 2 Calendário (várias semanas; ou tudo num sábado) · 3 Antes da comissão (7 passos) · 4 Preparar a reunião · 5 A reunião da comissão (10 passos) · 6 Depois (anuência, 1ª leitura, objeções, 2ª leitura, posse, vagas) · 7 Tudo em papel (fichas, cédulas, folha da classe, ficha de objeção) · 8 Instalar o site · 9 Textos completos de Ellen White + o que a pesquisa confirma · 10 Por que assim · 11 Resumo de bolso.
- **Site** `index.html` (um arquivo só), hospedado no GitHub Pages + Firebase (Firestore + entrada anônima). Cada membro no próprio celular, presidente no celular ou no Mac. Dois tipos de reunião: comissão de nomeações e comissão organizadora. Modo "só experimentar" funciona sem instalação.
- **regras_firestore.txt** (regras de segurança) e **Eleicoes_2026.ics** (datas no calendário do iPhone).

## Decisões validadas pelo pastor (preservar)
1. Três pilares: escritos de Ellen White (citação exata, nunca inventada) + fidelidade ao Manual da Igreja (cap. 10) + ferramentas práticas.
2. "Samueis" é só imagem para os membros da comissão de nomeações.
3. Classes da Escola Sabatina indicam para a **organizadora** (não para a comissão de nomeações). Padrão: lista impressa da classe; escrever à mão é só opção discreta.
4. Jeito 2 do Manual como padrão (Comissão Diretiva + 5 a 7 indicados).
5. Fluxo da comissão: Função e características → Propostas (em silêncio) → Observações → Oração e voto.
6. Observações desde a lista grande, com as três características e pesos 70/15/15 (ligação com Deus / o que a função pede / chamado discernido). Nota de 20 a 100; com 5 ou mais marcações tira a maior e a menor; alerta acima de 30 pontos de diferença. A lista é afinada pela nota (fica ~metade, sem cortar empates) e revista com mais atenção até 2 ou 3 nomes (ou vagas + 1). Maioria só no voto final.
7. Alternância não elimina: fica quem anda mais perto de Deus, com chamado e habilidade, mesmo já servindo; só no empate em tudo (exceto experiência) vale dar a vez. O site mostra isso sem impor (aviso de empate ≤ 5 pontos na lista pequena).
8. Todas as perguntas em atitude de dependência de Deus ("quem você discerne ser a escolha de Deus", "ainda não discerni").
9. Voto: cada nome precisa de mais da metade dos votos dados; o presidente define quantas pessoas a função pede, e o voto aceita até esse número.
10. Tudo num sábado como alternativa para distritos grandes; a organizadora trabalha durante a lição da Escola Sabatina, não durante o sermão.
11. Agenda do pastor: segunda-feira protegida; reuniões preferencialmente sábado ou domingo à tarde.

## Fontes primárias usadas
- "Diretrizes de Deus nos Escritos de Ellen White para a Escolha de Oficiais Locais na IASD" (compilação de 15 páginas do pastor).
- "Escolha de Oficiais e Estrutura Organizacional Adventista" (estudo completo, ~500 mil caracteres) — base para as próximas etapas (delegados, Associação, União).
- Manual da Igreja, cap. 10 (nomeações) — trechos enviados pelo pastor.
- Volumes I e II da v1 (substituídos pelo livro).

## Pendências
- **Bug relatado (membros presos em "Você entrou")** — corrigido nesta versão sem poder testar com o Firebase real: conexão por long polling, conferência no servidor a cada 8 s, botão "Atualizar", aviso de "sem conexão" e mensagem clara quando o servidor recusa uma mudança. Ao testar: se aparecer "recusou a mudança (permission-denied)", as regras não foram publicadas; se aparecer "sem conexão", é internet ou configuração.
- Testar o livro no Pages (fonte Georgia).
- Confirmar no egwwritings as 8 referências marcadas "a confirmar" e a tradução oficial de Ms 8, 1863 e Lt 97, 1892.
- Conferir a lista de funções com o Manual (Notas, nº 1).
- Próximas etapas: delegados à Associação; esferas superiores.

## Melhorias aplicadas nesta rodada
- Conexão mais robusta e visível (ver o bug acima).
- Quantas pessoas por função, com limite no voto (até N nomes; só um quando a função é de uma pessoa).
- Presidente pode conduzir pelo Mac.
- O aparelho do presidente lembra a igreja, o nome e a última lista de membros.

## Sugestões registradas (ainda não aplicadas), por ordem de valor
1. **Modo telão** no Mac/TV: mostra o passo e os textos em letra grande para a sala, útil para quem não tem celular.
2. **Presença real**: botão "Saí da reunião", para o "x de N" não travar quando alguém vai embora; e conferência do quórum votado.
3. **Fichas impressas geradas pelo site**: ficha de observações já com os nomes da lista e folha de cada classe em PDF.
4. **Relatório em PDF** e envio direto ao WhatsApp do secretário.
5. **Apagar sozinho** os dados do servidor depois de 60 dias (regra de validade do Firestore) e proteção contra uso indevido (App Check).
6. **Tamanho de letra ajustável** para irmãos mais velhos.
7. **Cronômetro discreto** de 1–2 minutos por nome nas observações rápidas.
8. **Modo treino** para anciãos: reunião simulada guiada, para formar presidentes onde o pastor não alcança.
9. **Calendário .ics por igreja** gerado a partir do sábado escolhido de cada congregação.
10. **Fontes num repositório** (GitHub) para o Claude Code na nuvem editar e testar direto.
