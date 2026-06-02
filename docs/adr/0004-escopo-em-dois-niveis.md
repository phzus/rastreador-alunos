# 0004 — Escopo em dois níveis

**Data:** 2026-06-02
**Status:** Aceita

## Contexto
Acompanhamento ao vivo passo a passo parece complexo e caro, e nem sempre é o que o cliente precisa. O Paulo pediu para oferecer duas possibilidades em vez de um app único e completo. Tecnicamente, avisar por evento (geofence) é muito mais simples e robusto que streaming em tempo real.

## Decisão
Oferecer dois níveis:
- **Nível 1 — Avisos por evento (MVP, recomendado):** ônibus saiu · passou no ponto que a **família escolhe** no app · chegou. Geofence + push, ping 30–60 s.
- **Nível 2 — Mapa ao vivo (fase 2):** ônibus em tempo real no mapa. Ping 10–15 s + WebSocket; exige hardware próprio (ver ADR-0002).

Vender começando pelo Nível 1; Nível 2 é upsell.

## Consequências
- (+) Entrega rápida e barata do que importa; reduz risco da venda.
- (+) Caminho de evolução claro (upsell) e alinhado ao orçamento do cliente.
- (+) "Ponto escolhido pela família" é diferencial concreto e simples de implementar (geofence configurável).
- (−) Gerir duas configurações de produto; deixar claro o que cada nível entrega.
