# 0003 — Backend próprio vs n8n

**Data:** 2026-06-02
**Status:** Aceita

## Contexto
O rascunho usava n8n para receber o webhook do Traccar, consultar o banco e disparar a notificação. Em instância única, o n8n é single-thread: sob pico (vários ônibus cruzando geofences no mesmo horário), webhooks dão timeout e eventos se perdem. Para "o aviso não chegou" ser inaceitável, isso é frágil. A alternativa robusta do n8n (queue mode + Redis + workers) tem complexidade equivalente a escrever o serviço.

## Decisão
Caminho crítico (evento → consulta → disparo) feito por **serviço próprio em Node.js (Fastify) + fila (Redis/BullMQ)**, com idempotência e reprocessamento. n8n, se usado, fica só para automações não-críticas.

## Consequências
- (+) Confiabilidade: nenhum aviso perdido por pico ou falha momentânea.
- (+) Controle total do código crítico e do monitoramento.
- (−) Não é editável visualmente por não-programador (não é requisito aqui).
- (−) Um componente a manter (mas mais simples de depurar que n8n em queue mode).
