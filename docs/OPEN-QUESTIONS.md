# OPEN QUESTIONS — pendências e o que confirmar

## Para preencher antes de apresentar
- [ ] **Nome da empresa de ônibus** — substitui `[NOME DA EMPRESA]` no deck e `[SUA TRANSPORTADORA]` na proposta técnica (capa + telas do app).
- [ ] **Telefone de contato** — slide final do deck (`(00) 00000-0000`).
- [ ] **Marca que assina** — confirmar se é "Lone Mídia" ou outra entidade/logo.

## Decisões de produto/copy
- [ ] **Alinhar a Proposta Técnica ao mesmo posicionamento?** O deck ficou 100% privado (sem licitação/prefeitura). A `proposta-tecnica-saquarema.html` ainda cita prefeitura/comprovação. Confirmar se ela também deve ser limpa (provável, se for entregue ao cliente) ou se fica como doc interno/técnico.
- [ ] **Login:** celular + CPF como senha (simples p/ público pouco tech). No build, avaliar se o CPF basta ou se entra código por SMS/PIN (CPF é conhecível → tradeoff de segurança).
- [ ] **Acesso/consentimento:** NÃO é "o aluno autoriza quem acompanha" (corrigido no deck). Modelo real: acesso via cadastro da empresa (celular+CPF); consentimento no cadastro. Aplicar a mesma correção na proposta técnica (§7 LGPD) quando for alinhada.
- [x] Slide de custo: mostra a calc do WhatsApp por conversa (Utilidade R$0,06, Marketing R$0,58, 1.000 grátis) + app R$0.
- [x] Removidas todas as menções a licitação/prefeitura/voto no deck — 100% B2B privado.

## A confirmar com o cliente / no diagnóstico
- [ ] Quem é exatamente o comprador na reunião (a operadora) e qual o porte da frota (nº de ônibus/alunos)?
- [ ] Qual rastreador/plataforma a frota usa hoje? É própria ou comodato? Tem API/tempo real? (define cenário A vs B — ver SOLUCAO-TECNICA).
- [ ] O contrato de transporte universitário é com a Prefeitura de Saquarema? Há edital/termo de referência a atender?
- [ ] As rotas são intermunicipais/noturnas? (afeta cobertura 4G e cálculo de chegada).
- [ ] O cliente quer começar no Nível 1 (avisos) ou já mira Nível 2 (mapa ao vivo)?

## Resolvidas
- [x] Público = universitários (não crianças).
- [x] Comprador = operadora de transporte (B2B), não a prefeitura.
- [x] Canal principal = app/push; WhatsApp/SMS reforço; Evolution descartada.
- [x] Tamanho da proposta técnica = ofício retrato, P&B (pode trocar p/ A4 no CSS).
