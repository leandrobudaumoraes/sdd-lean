# SDD e LeanSpec

Leitura pessoal. 14 de outubro de 2026.

Este texto é estudo. Não é regra do Instituto Espiritual Metatron, não entra no git do projeto e não descreve o que o código faz hoje. O Instituto aparece só como cenário, porque o método fica concreto quando o exemplo é um fluxo que você já conhece: sessão, pagamento, Oráculo.

O retrato das ferramentas é de 2025–2026. Nomes e adoção mudam rápido. Os princípios mudam menos.

## 1. A ideia em uma frase

Spec-Driven Development é o hábito de fechar um contrato antes de pedir código, quando errar custa caro.

LeanSpec é esse mesmo hábito com um teto de tamanho: a spec tem de caber na sua atenção e no contexto útil do modelo. Não é um método rival. É a ponta leve do mesmo espectro.

## 2. A analogia que segura o resto

Pense numa obra.

A conversa no canteiro resolve o azulejo do banheiro. Você aponta, a pessoa assente, o trabalho sai. Escrever planta para isso atrasa.

A planta existe para a viga, a hidráulica e a ligação com a rede. Ali um “deixa que eu entendi” vira retrabalho, vazamento ou conta errada. A planta não substitui o pedreiro. Ela diz o que não pode ser inventado no meio da parede.

Há ainda um terceiro grau, raro hoje: a planta vira o único arquivo que o dono edita, e a casa é gerada a partir dela. Isso existe como experimento. Não é o jeito comum de construir.

SDD é a planta. LeanSpec é a planta de uma folha, legível em dez minutos. O terceiro grau, a casa gerada só da planta, aparece mais adiante com o nome spec-as-source.

## 3. O problema

Com um agente de código, a conversa solta funciona bem na exploração. Você testa uma frase de copy, mexe num espaçamento, pergunta como um arquivo está organizado. O custo de errar é baixo e o retorno da velocidade é alto. Esse modo tem nome na prática: vibe coding. Não é defeito. É a fase em que a intenção ainda está mole.

O problema começa quando a intenção continua mole e o agente grava código. Ele preenche o silêncio. Se você não disse quem pode ver a carta, ele escolhe. Se você não disse o que acontece quando o webhook chega duas vezes, ele escolhe. A escolha pode compilar, passar num teste local e mesmo assim cobrar a pessoa errada ou misturar a leitura de dois usuários.

Ambiguidade com agente tem três efeitos recorrentes:

- Ele implementa o que você não pediu, porque o texto soava incompleto.
- Ele trata um exemplo como regra.
- Ele “melhora” um fluxo vizinho e o diff deixa de ser revisável.

A spec existe para cortar esse silêncio antes da implementação. Ela não torna o agente mais inteligente. Ela reduz o espaço em que ele precisa adivinhar.

Conversa basta quando o trabalho é local, reversível e sem regra de domínio. Spec passa a valer quando um chute vaza dado, cobra errado, altera schema ou mistura sessões.

## 4. O que é uma spec

Uma spec é o contrato entre quem decide e quem executa. O executor pode ser você daqui a dois dias, outro agente ou você relendo o diff.

Ela torna explícito, antes do código:

- o objetivo, em uma frase que dá para testar
- o que fica de fora
- as regras que não se negociam no meio da implementação
- o fluxo feliz e os fluxos de erro que importam
- o critério de aceite, no formato passou ou falhou
- a restrição técnica que, se ignorada, quebra o sistema

Ela não é ata, não é manual do produto inteiro e não é prompt enorme. Texto longo sem decisão não fecha contrato. Decisão é uma frase que muda o que o agente pode fazer.

Uma spec boa cabe numa revisão. Você lê, aponta o buraco, corrige o contrato e só então pede código. Se a implementação desviar, o desvio é comparável com o texto. Sem esse texto, a revisão vira gosto.

O ciclo útil, em qualquer ponto do espectro, é o mesmo:

1. Explorar, se a intenção ainda estiver mole.
2. Escrever o contrato.
3. Criticar o contrato: ambiguidade, buraco, risco.
4. Planejar sem gravar código.
5. Implementar só o que está escrito. Se faltar fato, parar e perguntar.
6. Validar contra o aceite.
7. Atualizar o contrato com o que a implementação ensinou.

A ordem importa. Pedir código antes de fechar a ambiguidade crítica é o ponto em que o método colapsa e volta a ser conversa com diff grande.

## 5. Spec-Driven Development

SDD é o nome da prática: a spec vem antes do código e serve de fonte da intenção para a pessoa e para o agente. A definição ainda se move, porque o termo foi adotado por produtos diferentes em 2025. O núcleo estável é este: documentação de intenção primeiro, código depois, aceite verificável.

Birgitta Böckeler, no artigo do Martin Fowler sobre Kiro, Spec Kit e Tessl, separa três graus. Eles não são três produtos. São três relações entre a spec e o código.

**Spec-first.** A spec é escrita antes e conduz aquela tarefa. Terminada a tarefa, a spec pode envelhecer. O valor foi dirigir a mudança.

**Spec-anchored.** A spec permanece depois da entrega. A próxima mudança daquela feature relê e atualiza o mesmo contrato. A spec vira memória da decisão, não só roteiro da semana.

**Spec-as-source.** A spec é o arquivo que a pessoa edita. O código é gerado a partir dela. Em alguns experimentos o arquivo gerado abre com um aviso de que não se edita à mão. A pessoa muda a intenção; a ferramenta regenera a implementação.

Hoje, quase todo uso sério está no primeiro grau, com pitadas do segundo. O terceiro é experimento. Tratar os três como a mesma coisa gera expectativa errada: ninguém no fluxo comum deixa de tocar o código.

### O que uma spec SDD costuma carregar

Para uma feature de risco médio ou alto, o contrato completo responde:

- objetivo
- quem é afetado
- fora de escopo
- regras e invariantes
- fluxo feliz
- fluxos de erro
- critérios de aceite
- restrições técnicas
- como validar
- dúvidas ainda abertas

Feature grande ganha uma segunda camada: plano de implementação e lista de tarefas. A spec diz o quê e por quê. O plano diz por onde cortar o trabalho. A lista diz o que cada passo entrega, para um agente não engolir a feature inteira num diff só.

Esse corte é o que as ferramentas formais automatizam.

### O polo formal

Quatro nomes bastam para ler o mercado sem se perder no catálogo.

**GitHub Spec Kit.** O fluxo canônico é specify, plan, tasks, implement. Há uma constituição do repositório, uma spec por mudança, um plano e uma lista de tarefas. Cada spec tende a viver numa branch. O desenho assume que a mudança é uma unidade com começo, revisão e fim. Funciona quando várias pessoas, ou vários agentes, precisam do mesmo contrato auditável. O custo é cerimônia: muitos arquivos, muito contexto, e a tentação de preencher seção vazia para o template ficar “completo”.

**Kiro.** O IDE pede requirements, design e tasks, mais arquivos de steering que orientam o agente o tempo todo. É SDD embutido na ferramenta. A vantagem é o trilho. A desvantagem é o mesmo trilho em tarefa pequena: o formulário existe mesmo quando a decisão cabe em dez linhas.

**OpenSpec.** Fica no meio. Há pasta de mudança, proposta e arquivo. A spec acumula com o projeto. Vale quando a memória das decisões começa a se perder e um `spec.md` solto já não dá conta. Ainda há processo. Menos que o Spec Kit, mais que um arquivo único no editor.

**Tessl.** É o nome associado ao grau spec-as-source: a spec mapeada para código gerado, com a pessoa mantendo a spec. O produto público ainda experimenta o formato. Serve como horizonte, não como padrão para adotar amanhã.

O polo formal ganha quando o custo de duas pessoas implementarem intenções diferentes é maior que o custo de escrever e manter a planta. Pagamento, schema de produção, autorização e qualquer fluxo com auditoria caem aí com frequência. Time, compliance e vários agentes no mesmo contrato também.

O polo formal perde quando a planta fica maior que a obra. A spec vira novela, o agente lê o meio e esquece a regra do início, e ninguém reabre o arquivo na próxima mudança. Cerimônia sem releitura é teatro.

## 6. LeanSpec

LeanSpec é uma metodologia de SDD desenhada em cima de três limites que não se negociam com ferramenta:

- **Física do modelo.** A janela de contexto é grande, e a qualidade cai bem antes do limite. Contexto longo aumenta erro e o efeito de perder o que está no meio.
- **Biologia da atenção.** Uma pessoa segura poucos conceitos de uma vez. Leitura focada de uma spec dura algo como cinco a dez minutos. O gargalo é atenção, não disco.
- **Economia.** Token e hora de engenharia custam. Spec grande custa para escrever, para o modelo ler e para manter verdadeira.

Desses limites saem cinco princípios. A ordem é a regra de desempate. Quando dois conselhos brigam, o de cima vence.

### 1. Context economy

A spec inteira tem de caber na cabeça e no contexto útil do modelo.

O alvo prático publicado pelo LeanSpec é menos de 2.000 tokens por arquivo. Entre 2.000 e 3.500 tokens, o arquivo ainda serve e pede vigilância. Perto de 3.500, parte. Uma spec “completa” de centenas de linhas falha aqui mesmo que cada frase seja boa. Nada do que vem abaixo compensa um texto que ninguém segura.

O teste: dá para ler e entender em cinco a dez minutos, e repetir a estrutura sem olhar?

### 2. Signal-to-noise

Cada frase informa uma decisão ou sai.

O teste: que decisão esta frase muda? Se a resposta é “nenhuma” ou “talvez no futuro”, a frase é ruído. Ruído gasta atenção e empurra a regra real para fora da janela útil.

Detalhe entra quando explica um trade-off, uma restrição, um aceite ou um exemplo que desambigua. Detalhe sai quando é óbvio para quem vai implementar, quando vive em outro arquivo, ou quando vai mudar antes do código existir.

### 3. Intent over implementation

O contrato guarda o porquê e o quê. O como fica livre, salvo quando o como é a restrição.

Intenção é estável. Biblioteca, nome de função e formato interno mudam. O agente decide melhor com o motivo do que com uma receita que envelhece na primeira hora de código.

O teste: outra pessoa consegue escolher uma implementação decente sem você na sala?

Exemplo curto. “Usar Redis com 1 GB e LRU” é receita. “A leitura do painel precisa responder em menos de 100 ms para a mesma consulta repetida; cache com descarte LRU é o meio; a complexidade extra só se justifica por essa latência” é intenção com a restrição que importa.

### 4. Bridge the gap

O mesmo texto serve para a pessoa e para o agente.

A pessoa precisa de contexto e motivo. O agente precisa de estrutura, regra e aceite no formato passou ou falhou. Linguagem natural cumpre os dois quando o aceite é verificável. Parágrafo inspirador sem critério deixa o agente preencher o critério.

### 5. Progressive disclosure

Começa mínimo. Campo, subprocesso e arquivo extra entram quando a ausência dói.

No primeiro dia bastam status e o contrato. Etiqueta, prioridade, responsável e ligação com ticket entram quando procurar a spec certa virou o problema. Reescrever tudo a cada fase é desperdício. Acrescentar um campo é o movimento previsto.

O teste: estamos sentindo dor sem isto? Se não, não adiciona.

### O loop

Discover, Plan, Implement, Validate, Update.

Discover é procurar spec existente antes de abrir outra. Plan é criar ou corrigir a spec até a intenção ficar inequívoca. Implement é pessoa e agente executando o que está escrito. Validate é teste mais uma checagem de que a spec ainda está densa. Update é marcar status, lição e o que ficou para depois.

A spec é documento vivo durante a vida da decisão. Não é papelada de abertura de obra.

Três papéis, que podem ser a mesma pessoa: quem esclarece o problema, quem implementa, quem confere o aceite antes de mudar o status.

### O artefato

O caso normal é um arquivo. Frontmatter leve, se fizer falta: status, tags, prioridade. Seções opcionais. As que o template padrão sugere são Overview, Design, Plan, Test e Notes. Seção vazia não fica no arquivo para “completar o modelo”.

Um miolo que costuma bastar:

- objetivo
- fora de escopo
- regras que decidem
- aceite
- como validar

Quando o arquivo passa do teto, ou mistura assuntos que se leem em momentos diferentes, a spec vira pasta:

```text
specs/NNN-nome/
├── README.md
├── DESIGN.md
├── PLAN.md
└── TEST.md
```

O README navega. Cada arquivo continua abaixo do teto. O agente carrega o arquivo da tarefa, não a pasta inteira. Isso é o que o LeanSpec chama de context engineering: partir, compactar e isolar para o contexto da vez ficar pequeno.

Não há pasta de change obrigatória, proposta formal nem passo de arquivar. A spec edita como código: muda, commita, segue. O histórico fica no git.

A ferramenta de linha de comando (`lean-spec`, validação, MCP) é opcional. Os princípios funcionam num markdown comum. Adotar o CLI sem adotar o teto de tamanho não produz LeanSpec.

### Quando não escrever spec

A própria metodologia manda pular a spec quando o trabalho é protótipo local que não vai entrar, ou bug pequeno de escopo óbvio e sem borda. Forçar arquivo nesses casos é o teatro que o método tenta evitar.

Spec curta entra na feature de uma pessoa, com complexidade média. Spec partida em pasta entra quando há mais de um assunto grande. Processo de time e trilha de auditoria pedem um vizinho mais formal, não um arquivo de duzentas linhas fingindo ser governança.

## 7. O mesmo problema, dois formatos

Cenário de estudo, inventado para comparação. Não é a spec real do webhook do Instituto.

Situação: a pessoa pagou a leitura de astrologia ocidental. O aviso de pagamento do Mercado Pago chega ao servidor. Só então a leitura daquela pessoa pode seguir no ciclo já conhecido do produto, de pago para processamento e, no sucesso, para concluído. Falha recuperável volta para a tentativa. A carta de outra pessoa não pode aparecer. O mesmo aviso, se chegar de novo, não pode gerar segunda leitura nem segundo crédito.

### Versão no polo formal

Esta é a forma SDD quando o risco pede contrato completo, plano e tarefas. Num Spec Kit isto viraria branch, `spec.md`, `plan.md` e `tasks.md`. Aqui está condensado num texto só para você ler a anatomia. Numa ferramenta formal, cada bloco abaixo seria um arquivo.

```text
# Confirmar pagamento e liberar a leitura de astrologia ocidental
Status: to-be
Constituição: vigente

## Objetivo
O aviso autêntico de pagamento aprovado libera uma única geração
da leitura de astrologia ocidental para o usuário dono daquele pagamento.

## Quem é afetado
A pessoa que pagou. O servidor que recebe o aviso. Ninguém mais vê a carta.

## Fora de escopo
Astrologia védica. Reembolso. Troca de preço. Tela de checkout.
Geração do texto da carta. E-mail de confirmação.

## Regras
- O usuário e o serviço vêm da sessão e do registro do pagamento no banco.
  Nada disso se aceita do corpo do aviso como autoridade.
- Um pagamento aprovado move o status daquela leitura de pago para
  processamento, e no sucesso para concluído.
- Falha recuperável da geração marca tentativa de novo, sem perder o pagamento.
- O mesmo aviso, repetido, não cria segunda leitura nem altera outro usuário.
- Preço, slug do serviço e papel não são lidos do cliente.

## Fluxo feliz
1. O provedor envia o aviso de pagamento aprovado.
2. O servidor confere autenticidade do aviso.
3. O servidor localiza o pagamento e o dono no banco.
4. Se o status ainda é pago, passa a processamento e dispara a geração.
5. No sucesso, o status fica concluído e a pessoa abre a própria carta.

## Fluxos de erro
- Aviso sem autenticidade: ignora, responde o que o provedor exige para
  não reenviar à toa, não muda status.
- Pagamento desconhecido: não cria leitura.
- Status já concluído: não gera de novo.
- Geração falha de modo recuperável: marca tentativa de novo.
- Geração falha de modo definitivo: permanece visível para suporte,
  sem cobrar outra vez.

## Critérios de aceite
- [ ] Aviso válido de pagamento aprovado libera uma geração para o dono.
- [ ] O mesmo aviso, enviado de novo, não gera segunda carta.
- [ ] Aviso de outro pagamento não altera esta leitura.
- [ ] Pessoa autenticada como outro usuário não lê esta carta.
- [ ] Falha recuperável não devolve o status para “não pago”.

## Restrições técnicas
Segredo do provedor só no servidor. Sem log de corpo do aviso,
token ou conteúdo da carta. Schema novo, se existir, sai como SQL
aditivo para o painel, sem aplicar daqui em produção.

## Validação
Teste do aviso duplicado. Teste de usuário cruzado.
Teste de falha recuperável. Lint do módulo tocado.

## Dúvidas abertas
Nenhuma que bloqueie. Reembolso fica para outra spec.

## Plano
1. Travar a transição de status no servidor, com o dono vindo do banco.
2. Tratar aviso repetido como sucesso idempotente.
3. Cobrir os três testes do aceite.

## Tarefas
- [ ] Transição pago → processamento → concluído, só com aviso válido.
- [ ] Idempotência do aviso repetido.
- [ ] Isolamento: outra sessão não lê o resultado.
- [ ] Testes dos três casos.
```

O que este formato compra: um revisor consegue discutir o fluxo de erro sem abrir o código. Um segundo agente pode pegar uma tarefa sem reler a conversa. Daqui a um mês, a decisão do aviso duplicado ainda está escrita.

O que este formato custa: é longo para uma mudança cujo miolo é uma frase, “um pagamento, uma geração, o dono vem do banco”. Se você for o único leitor e a regra já está na constituição, metade das seções repete contexto que você já carrega.

### Versão LeanSpec

Mesma decisão, no tamanho que o princípio pede. Abaixo de uma leitura de dez minutos. Sem plano separado, porque o corte do trabalho cabe no aceite.

```text
---
status: planned
tags: [pagamento, oraculo]
---

# Liberar a leitura depois do pagamento

## Objetivo
Um aviso autêntico de pagamento aprovado libera uma única geração
da astrologia ocidental para o dono daquele pagamento.

## Fora de escopo
Védica, reembolso, checkout, texto da carta, e-mail.

## Regras
- Dono e serviço vêm do banco, ligados ao pagamento. O aviso não é autoridade
  sobre usuário, preço ou slug.
- Transição: pago → processamento → concluído.
- Falha recuperável: tentativa de novo, pagamento permanece válido.
- Aviso repetido ou status já concluído: não gera outra carta.

## Aceite
- [ ] Aviso válido gera uma leitura para o dono.
- [ ] Repetir o aviso não gera a segunda.
- [ ] Outra sessão não lê esta carta.
- [ ] Falha recuperável não volta o status para não pago.

## Validação
Testes de aviso duplicado, usuário cruzado e falha recuperável.

## Restrição
Segredo e conteúdo da carta ficam no servidor e fora de log.
```

A intenção é a mesma. Sumiram o plano, a lista de tarefas, o fluxo numerado e o bloco “quem é afetado”. Eles somem porque, neste tamanho, não mudam decisão: o aceite já diz o que tem de passar, e o fora de escopo já diz quem não entra.

Se na implementação aparecer uma borda que o aceite não cobre, por exemplo aviso de pagamento ainda pendente, a spec lean ganha uma linha na hora. Ela não nasce com todas as bordas hipotéticas. Essa é a diferença prática do progressive disclosure.

### O que comparar nos dois textos

Os dois proíbem a mesma coisa: segunda carta, usuário cruzado, confiar no corpo do aviso para saber quem pagou.

O formal acrescenta sequência, plano e tarefas. Isso vale quando outra pessoa vai executar um pedaço, quando a revisão acontece antes do código, ou quando o fluxo de erro tem mais de um comportamento que o aceite curto apagaria. Aqui, “ignorar aviso sem autenticidade” e “pagamento desconhecido não cria leitura” estão mais protegidos na versão formal. Se essas bordas já são regra estável do sistema, repetir na spec é ruído. Se ainda não estão, a versão curta está fina demais e a linha que falta deve entrar nela, não o plano inteiro.

O movimento certo, quando a spec curta doi, é acrescentar a regra que faltou. O movimento errado é trocar para o template completo por ansiedade.

## 8. Diferença, vantagem e custo

| | SDD no polo formal | LeanSpec |
|---|---|---|
| O que é | A prática, levada com constituição, spec, plano e tarefas | A mesma prática, com teto de atenção |
| Unidade | Mudança com branch ou pasta e vários arquivos | Um arquivo; pasta só quando não cabe |
| Tamanho | O template pede completude | Alvo abaixo de 2.000 tokens; parte perto de 3.500 |
| Onde o “como” mora | Plano e tarefas, às vezes cedo | Só se o como for restrição |
| Memória depois da entrega | Forte no spec-anchored e no OpenSpec | A spec que ainda informa decisão; o git guarda o resto |
| Governança | Trilha, revisão, vários agentes no mesmo contrato | Fraca de propósito |
| Ferramenta | Spec Kit, Kiro, ou OpenSpec quando a memória acumula | Markdown basta; CLI é opcional |
| Falha típica | Teatro: spec longa que ninguém relê | Spec magra demais num fluxo que tinha borda perigosa |

Vantagem do polo formal: duas pessoas, ou dois agentes, não implementam intenções diferentes. A revisão olha o contrato antes do diff. Pagamento, schema, autorização e auditoria ficam explícitos nas bordas, não só no caminho feliz. Daqui a meses, a decisão ainda tem endereço.

Custo do polo formal: tempo de escrita, tokens, e o risco de a spec existir para parecer organizada. Spec escrita depois do código, para documentar o que já saiu, não dirigiu nada. Spec vaga (“deixar robusto”, “melhorar a experiência”) também não. O agente preenche o adjetivo.

Vantagem do LeanSpec: a spec é relida, porque é curta. O modelo recebe sinal, não contexto inchado. Você escreve no ritmo da tarefa. O método cresce quando dói, em vez de nascer com o organograma de um time que você não tem.

Custo do LeanSpec: borda omitida vira bug, porque não havia seção obrigatória de erro. Falta trilha quando mais de uma pessoa precisa provar o que foi combinado. Um arquivo único que cresce escondido viola o próprio teto e vira o pior dos dois mundos: longo e sem plano.

O erro estável não é escolher um nome. É usar um formato para toda tarefa.

## 9. Como escolher

Use o custo de errar, não a preferência estética.

**Conversa, sem arquivo.** Copy, ajuste local de layout, pergunta de leitura, protótipo que não entra. Explorar aqui é o certo.

**LeanSpec, um arquivo.** Feature de uma pessoa, regra já estável em volta, aceite que cabe numa lista curta. Bug com borda real, quando o risco cabe em cinco linhas. A maior parte do trabalho solitário de produto fica aqui.

**SDD formal, spec mais plano e tarefas.** Feature nova em que o fluxo de erro tem vários comportamentos, mais de um agente vai executar, ou a revisão do contrato acontece antes da primeira linha. Pagamento, webhook, crédito, schema, autorização, isolamento de leitura e dado sensível puxam para cá mesmo quando você trabalha sozinho. O motivo não é o tamanho do time. É o estrago de um chute.

**OpenSpec ou equivalente com pasta de mudança.** Quando as specs começam a se acumular e você perde qual decisão ainda vale. A dor é de arquivo, não de implementação.

**Spec-as-source.** Só como experimento consciente, num recorte em que você aceita não editar o código gerado. Não é o fluxo do dia a dia.

Um atalho que cabe no Metatron, como ilustração e não como regra nova: se um chute vaza dado ou cobra errado, o contrato existe antes do código. Se a tarefa é pequena e a regra em volta já está fechada, o contrato é a folha curta. Se a feature é nova, o contrato ganha fluxo de erro. Texto longo sem decisão continua sem ser spec, em qualquer um dos graus.

## 10. Como isso já aparece no Instituto

A constituição do repositório já é um híbrido, escrito antes deste texto. Vale a pena reconhecer o mapa, para não instalar um segundo método em cima do que já decide.

A conversa cobre copy, layout local e dúvida de leitura.

A spec passa a ser obrigatória antes de gravar código quando a mudança toca autorização, sessão, papel, convite, promoção, admin, preço, checkout, webhook, crédito, capacidade de evento, schema, Oráculo, CPF, anamnese, conteúdo gerado, HTML rico, rota pública ou qualquer fluxo em que um chute vaza dado ou cobra errado.

A spec curta tem cinco campos: objetivo, fora de escopo, regras, aceite, validação. Isso é LeanSpec com outro nome. O teto não está numerado em tokens, mas a frase “texto longo sem decisão não é spec” é o princípio de signal-to-noise.

A spec de feature acrescenta contexto, fluxo feliz, fluxos de erro, restrições técnicas e dúvidas abertas. Isso é o grau spec-first completo, ainda sem plano e sem lista de tarefas. Plano entra na conversa com o agente, quando o corte do trabalho precisa ser explícito.

O código em `main` vence o texto. Isso impede o grau spec-as-source por decisão de projeto: a spec não é o sistema. A spec dirige a mudança. O sistema é o que está em execução. Dívida conhecida se declara na spec. Não se documenta como se já fosse a regra.

Você não precisa de um CLI novo para praticar. Precisa escolher o tamanho do contrato pelo risco, e recusar spec que não muda decisão.

## 11. A tendência

Retrato de outubro de 2026. Serve para orientar leitura. Não é previsão.

Em 2025, “spec-driven” virou o nome da categoria. Kiro, Spec Kit e Tessl popularizaram a ideia de que agente sem contrato implementa a intenção de quem calhou de escrever o prompt. O artigo do Fowler fixou o vocabulário dos três graus e mostrou que as ferramentas não fazem a mesma coisa, embora usem a mesma palavra.

Em seguida o mercado se abriu num espectro, não numa troca de campeão. Num extremo, o prompt cru. No meio, um `spec.md` ou uma spec no estilo LeanSpec. Ao lado, OpenSpec, quando a memória do projeto precisa de ciclo de vida. No outro extremo, Spec Kit e Kiro, com constituição, plano e tarefas. Tessl permanece no experimento em que a spec é a fonte e o código é gerado.

O movimento visível não é “LeanSpec substitui SDD” nem “todo repositório vai ter Spec Kit”. SDD ficou. A cerimônia passou a ser calibrada pelo custo de errar. Quem adotou o template completo em todo bug pequeno sentiu o teatro e enxugou. Quem ficou só no prompt em fluxo de pagamento sentiu o retrabalho e passou a escrever contrato. Os dois recuos apontam para o mesmo lugar: formalidade proporcional ao risco.

A adoção de CLI do LeanSpec é menor que a do Spec Kit e a do OpenSpec. A disciplina que ele nomeia, spec curta que cabe na atenção, é o que está se espalhando, muitas vezes sem o nome e sem a ferramenta. O princípio sobrevive ao produto.

Spec-as-source continua experimental. A limitação é prática: intenção estável não cobre todo o como, e código gerado que ninguém pode tocar exige uma spec tão precisa quanto o programa. Isso se paga em recortes pequenos e muito estáveis. Não se paga como modo padrão de um produto com domínio, pagamento e exceção.

O que tende a permanecer, independente da marca:

- contrato antes do código quando errar é caro
- aceite no formato passou ou falhou
- fora de escopo explícito, porque é isso que impede o agente de “aproveitar e fazer”
- spec relida, portanto curta o bastante para ser relida
- código como o que o sistema faz; spec como o que a mudança prometeu

O que tende a sair de moda é o processo único obrigatório. Template cheio em toda tarefa, e também a recusa de qualquer spec em nome da velocidade.

## 12. Um hábito

Na próxima tarefa que toque pagamento, sessão ou Oráculo, escreva a folha curta antes de pedir código. Cinco blocos: objetivo, fora de escopo, regras, aceite, validação. Peça ao agente para apontar buraco, não para implementar. Se ele achar uma borda que muda comportamento, acrescente uma linha. Se ele sugerir seção que não muda decisão, corte.

Se a folha passar de uma leitura de dez minutos, ou se outro agente for executar um pedaço, aí sim abra fluxo de erro, plano e tarefas. Até lá, a folha é o método.

## Fontes usadas neste retrato

- Birgitta Böckeler, “Understanding Spec-Driven Development: Kiro, spec-kit, and Tessl”, martinfowler.com, para os três graus.
- Documentação do LeanSpec (codervisor), princípios e teto de tokens: context economy, signal-to-noise, intent over implementation, bridge the gap, progressive disclosure.
- Espectro de ferramentas descrito em materiais de Intent Engineering por volta de 2026: prompt, spec curta, LeanSpec, OpenSpec, Spec Kit.
- Constituição do Instituto, só como ilustração do híbrido que você já tem: spec curta e spec de feature. O código do repositório continua sendo a autoridade sobre o produto.
