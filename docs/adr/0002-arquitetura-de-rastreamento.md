# 0002 — Arquitetura de rastreamento

**Data:** 2026-06-02
**Status:** Aceita

## Contexto
O rascunho original prometia integração "não-invasiva" com o rastreador já instalado, sem hardware novo. A pesquisa mostrou que plataformas fechadas comuns no Brasil (ex.: Sascar, Omnilink) só liberam dados por polling com latência de ~15 min e 1 requisição por integradora, dependendo de autorização do cliente. Chip/APN travado e comodato impedem redirecionar o sinal bruto. Ou seja: não há tempo real confiável por terceiro nesse cenário.

## Decisão
- **Tempo real (Nível 2) exige hardware próprio:** rastreador nosso (Teltonika/Suntech, que aceitam destino duplo) com chip M2M nosso → nosso Traccar, ping 10–15 s.
- **Integração com a plataforma existente** só onde ela permitir e quando tempo real não for exigido (Nível 1 best-effort).
- **Diagnóstico da frota é etapa obrigatória** antes de qualquer promessa (define cenário A — integração — ou B — hardware próprio).
- Servidor: **Traccar + PostgreSQL** (nunca H2); geofence em polígono + proximidade no backend.

## Consequências
- (+) Arquitetura sólida e em tempo real quando controlamos o hardware.
- (+) Honestidade no diagnóstico vira credibilidade na venda.
- (−) Vira "hardware + software": CAPEX de equipamento/instalação + custo M2M por veículo.
- (−) Prazo/custo só fecham após o diagnóstico.
