# SOLUÇÃO TÉCNICA — validação e arquitetura

> Cérebro técnico do projeto. Base para o desenvolvimento. Resultado da pesquisa de viabilidade (2026-06-02). A proposta cliente-facing é `apresentacao/proposta-tecnica-saquarema.html`; aqui ficam o raciocínio, os números e as fontes.

## Veredito

**Viável, com uma ressalva que define o modelo de negócio:** o stack (Traccar + backend + app/push) é sólido e barato. O projeto não morre por tecnologia — morre se depender de rastreador de plataforma fechada para tempo real. Caminho recomendado: **Nível 1 (avisos) + hardware próprio onde necessário**.

## Arquitetura

```
Rastreador GPS → Traccar (geofence, event forwarding) → Backend Node+fila
   → push (FCM) [principal] · WhatsApp/SMS [reforço]  +  Painel (Next.js)
```

- **Traccar:** open-source Apache 2.0, +2.000 dispositivos, geofenceEnter/Exit, event forwarding por webhook, REST + WebSocket. Em produção: **PostgreSQL** (o H2 padrão corrompe). Geofencing tem bugs conhecidos → usar **polígono**, vincular ao device (não a grupo), raio generoso e **confirmar por proximidade no backend** com debounce.
- **Backend:** Node.js (Fastify) + fila (Redis/BullMQ) para o caminho crítico. n8n em instância única é single-thread e perde webhook sob pico — só serve para automações não-críticas.
- **Infra:** VPS Docker; ~150 veículos a 30–60 s = poucos eventos/s. 4 GB RAM / 2–4 vCPU com folga. Custo fixo baixo (VPS classe Hetzner/Contabo ~US$5–20/mês; avaliar VPS BR por latência do painel).

## Decisão 1 — Rastreamento (ADR-0002)

Integrar com rastreador **já instalado** de plataforma fechada **não dá tempo real**:
- Plataformas tipo Sascar/Omnilink expõem só **polling SOAP**, latência ~**15 min**, **1 requisição por integradora**, dados em lote (D0/D1), e exigem autorização do cliente caso a caso.
- SIM/APN travado + comodato impedem redirecionar o sinal bruto.
- **Saída:** fornecer rastreador próprio (Teltonika FMB/Suntech aceitam **dois destinos**) com chip M2M nosso → nosso Traccar, ping 10–15 s. Vira "hardware + software", com CAPEX de instalação.
- Onde a frota for própria/aberta e tempo real não for exigido (Nível 1), integração é possível como best-effort.

## Decisão 2 — Canais e custo (ADR-0001)

Modelo de cobrança do WhatsApp adotado no projeto (números informados pelo cliente, BR, 2026): **cobrança por conversa de 24h**, com as **1.000 primeiras conversas iniciadas pelo cliente grátis/mês**.

| Canal | Custo | Papel |
|---|---|---|
| App push (FCM) | gratuito / ilimitado | **Principal** |
| WhatsApp — Utilidade (nossos avisos) | **R$0,06 por conversa de 24h** | Reforço / possibilidade |
| WhatsApp — Marketing | R$0,58 por conversa | Não usar para avisos |
| SMS | ~R$0,08–0,33 | Último recurso |
| WhatsApp não-oficial (Evolution) | sem tarifa, mas risco de ban | Descartado em produção |

**Cálculo (Utilidade):** vários avisos do mesmo dia ao mesmo responsável cabem em 1 conversa de 24h. Ex.: 500 alunos × ~22 dias úteis = 11.000 conversas/mês − 1.000 grátis = 10.000 × R$0,06 = **R$600/mês**. Pelo app (push), o mesmo aviso custa **R$0**.

> Houve mudança de modelo no WhatsApp em 2025 (per-message × per-conversation). Para a apresentação, usar os números do cliente (conversa: Utilidade R$0,06, Marketing R$0,58, 1.000 grátis/mês) e confirmar contra a fatura do BSP no fechamento.

## Decisão 3 — Escopo em 2 níveis (ADR-0004)

- **Nível 1 (avisos):** geofence + push, ping 30–60 s. Matemática: a 60 km/h o ônibus anda ~1 km/min → raio generoso + proximidade no backend. Robusto e barato. **MVP.**
- **Nível 2 (mapa ao vivo):** ping 10–15 s + WebSocket; exige hardware próprio. Fase 2.

## Riscos e mitigações

1. **Integração com plataforma fechada** → diagnóstico primeiro; hardware próprio onde preciso.
2. **Evento de geofence perdido / entrega falha** → polígono + proximidade + fila idempotente + monitor de "veículo sem sinal".
3. **Push não chega** (iOS PWA exige instalar; Android encerra app) → app Capacitor/nativo + reforço WhatsApp/SMS no aviso crítico.
4. **Sinal 4G fraco em rota intermunicipal** → buffer no rastreador + tolerância no cálculo de chegada.

## Fontes (verificadas em 2026-06-02)

- Traccar: traccar.org/events, /geofences, /forward, /traccar-api; licença Apache 2.0.
- WhatsApp pricing: developers.facebook.com (per-message, jul/2025); EngageLab/SleekFlow/MessageCentral (faixas BR utilidade ~US$0,007–0,008); Twilio +US$0,005 markup.
- FCM gratuito: firebase.google.com/pricing.
- Evolution API (Baileys, risco de ban): github.com/EvolutionAPI/evolution-api + relatos de comunidade.
- Integração frotas BR: manual SasIntegra v2.05 (Michelin/Sascar) — polling, 1 req, D0/D1; Omnilink WSTT/REST.
- Push reliability: OneSignal/CleverTap (OEM/force-stop); iOS 16.4 + add-to-home-screen para web push.

> Confirmar números/contratos no diagnóstico e contra fatura real do BSP — preços mudam mês a mês.
