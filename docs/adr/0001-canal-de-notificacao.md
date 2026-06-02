# 0001 — Canal de notificação

**Data:** 2026-06-02
**Status:** Aceita

## Contexto
Notificar famílias em escala (milhares de avisos/dia). O rascunho original usava WhatsApp como canal principal. O WhatsApp oficial passou a cobrar por mensagem (jul/2025): utilidade ~R$0,04–0,07. A ~4 avisos/dia × 200 dias, isso dá ~R$50 mil/ano por 1.000 alunos. O WhatsApp não-oficial (Evolution API) é gratuito mas tem risco real de banimento do número.

## Decisão
- **Canal principal: push do app (FCM)** — gratuito e ilimitado.
- **Reforço/fallback: WhatsApp oficial (utilidade) e SMS** para o aviso crítico e para quem não tem o app.
- **Evolution API descartada** em produção (risco de outage total por ban).

## Consequências
- (+) Custo marginal de aviso ≈ R$0; cresce nº de alunos sem crescer a conta.
- (+) Argumento comercial forte (custo previsível, margem).
- (−) Depende de o usuário instalar o app e permitir notificação → exige bom onboarding.
- (−) Push pode falhar (iOS/Android) → daí o reforço WhatsApp/SMS no aviso crítico (ver ADR e SOLUCAO-TECNICA, risco 3).
