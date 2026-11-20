# Autonomia supervisionada

Dentro do escopo autorizado, o agente executa de forma contínua: não pede licença para cada arquivo, teste, correção local ou tarefa. O humano é acionado por **decisão**, não por rotina.

## Envelope padrão

Permitido sem perguntar, dentro da iniciativa autorizada:

- ler o repositório, rodar build, testes, lint e scripts locais;
- editar código, testes e documentação do escopo;
- criar branch de trabalho e fazer commits nela;
- corrigir falhas locais que o próprio trabalho provocou;
- criar e atualizar Issues/itens de Project quando o modo GitHub estiver ativo;
- investigar dúvidas técnicas antes de perguntar.

Exige autorização explícita (registrada em `.sdlc/project.md` ou na aprovação da spec):

- push, abertura de PR, merge, release, deploy;
- operação destrutiva ou irreversível (apagar dados, reescrever histórico, force push);
- uso de credencial, serviço externo pago ou contato com terceiros;
- mudança nos controles do próprio harness (testes de gate, hooks, permissões).

Merge na branch principal e deploy em produção sempre exigem decisão humana. O merge pode ser autorizado **por pacote**: uma decisão por story (ou fatia) cobre a sequência dos PRs dela, em todos os repositórios, na ordem declarada. O agente executa a sequência conferindo, antes de cada merge, CI verde no último commit, ausência de conflito e árvore igual à revisada e, depois, o CI da branch principal; **para na primeira conferência que falhar**. Merge autorizado nunca vale como deploy.

Autorização válida não é pedida de novo. Autorização dada para um contexto não se estende a outro.

## Quando escalar

Interrompa e acione o humano somente quando houver:

1. mudança material de escopo ou objetivo;
2. decisão arquitetural relevante e difícil de reverter;
3. risco crítico não coberto pelas regras aprovadas;
4. operação externa sensível, destrutiva ou implantação em produção;
5. bloqueio que não pode ser resolvido autonomamente (orçamento esgotado incluído — ver [budgets](budgets.md));
6. necessidade de validação humana real (por exemplo, interface gráfica).

Não escale: detalhes locais e reversíveis, escolhas com default convencional, dúvidas que o repositório responde, robustez de borda sem risco real para a maturidade atual. Nesses casos decida, registre em uma linha e siga. Item menor vira pendência no relatório, não parada.

Não delegue ao humano investigação técnica que o agente consegue fazer.

## Desvios da spec

Alteração fora do escopo aprovado cai em um de dois grupos:

- **Segue e é listada no PR**, na seção "Desvios da spec", para o humano ver e vetar no merge: teste **novo** (mais cobertura); ajuste técnico para cumprir o que a spec já pede; arquivo interno do mesmo repositório e da mesma story. Só mudança reversível; vetada no merge, é desfeita.
- **Para antes e pede autorização:** teste existente que **muda de expectativa**; comportamento visível ao usuário além do aprovado; contrato entre camadas, outro repositório ou outro módulo; migração de dados, dependência nova ou passo operacional a mais. Aqui não vale decidir e pedir ratificação depois.

Na dúvida sobre o grupo, é parada.

## Formato da escalada

Toda escalada abre com `[BLOQUEADO]` e contém, nesta ordem:

1. **Contexto** — resumo em 2-4 linhas, sem presumir que o humano leu o código;
2. **Decisão necessária** — a pergunta e por que ela importa;
3. **Alternativas** — 2-3 opções, cada uma com prós e contras;
4. **Recomendação** — a opção indicada e a justificativa;
5. **Se adiar** — consequência concreta de não decidir agora (o que fica parado, o que continua).

Prefira uma pergunta fechada com a recomendação como primeira opção. Agrupe decisões pendentes em uma única escalada quando forem do mesmo momento.

Trabalho independente da decisão continua; somente o trabalho afetado para.

## Registro

A decisão humana é persistida onde ela governa o trabalho: na spec (decisões de produto), no plano ou ADR (decisões técnicas) ou no último registro do handoff (decisões operacionais em aberto). O chat não é fonte de verdade.
