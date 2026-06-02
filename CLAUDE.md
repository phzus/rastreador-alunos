# Rastreador de Alunos — App de Transporte Universitário

App (white-label) de acompanhamento de transporte universitário + rastreamento GPS. **Venda B2B:** o comprador é uma **empresa de transporte (operadora)** que atende/quer atender contrato de transporte universitário na região de **Saquarema-RJ**. Quem paga a operadora é a **prefeitura** (contexto de licitação). Desenvolvido por **Lone Mídia**.

> Status: **Pré-venda.** Duas apresentações + motor de copy + validação técnica prontos. O desenvolvimento começa após o fechamento, aqui mesmo neste projeto. Última atualização: **2026-06-02**.
>
> **Repositório:** https://github.com/phzus/rastreador-alunos (branch `main`).

---

## O que é (escopo atual — leia antes de tudo)

Um aplicativo com a **marca da transportadora** onde a família (pai, mãe, namorado/a — quem o estudante autorizar) acompanha as viagens do universitário. Mais painel de gestão e relatórios para a operação e para a prefeitura.

**Ângulo de venda (a "malandragem"):** o operador não compra "saber onde está o aluno". Compra **contrato renovado, margem e nome**. A prefeitura é quem manda no bolso dele; o app é a ferramenta que o torna difícil de substituir. Segurança da família = prova, não o discurso. Detalhe em [copy/BRIEFING-COPY.md](copy/BRIEFING-COPY.md).

**Dois níveis de escopo** (ver ADR-0004):
- **Nível 1 (MVP, recomendado):** avisos por evento — ônibus saiu · passou no ponto que a família escolhe · chegou.
- **Nível 2 (fase 2):** mapa ao vivo, tempo real.

---

## Entregáveis desta fase

| Arquivo | O que é |
|---|---|
| [apresentacao/apresentacao-saquarema.html](apresentacao/apresentacao-saquarema.html) | Deck de venda 16:9 (colorido), ângulo de negócio. Exporta PDF. |
| [apresentacao/proposta-tecnica-saquarema.html](apresentacao/proposta-tecnica-saquarema.html) | Proposta técnica ofício retrato, P&B, validada. Companheiro técnico do deck. |
| [copy/](copy/) | Motor de copy: BRIEFING-COPY, AVATAR, COMUNICACAO + referência (anti-IA, frameworks). |
| [docs/](docs/) | Este segundo cérebro: escopo, solução técnica, decisões, progresso, pendências. |

PDFs gerados: `apresentacao/preview.pdf` (deck) e `apresentacao/proposta-tecnica.pdf`. Gere frescos antes de apresentar (instruções no rodapé de cada HTML).

---

## Decisões-chave (detalhe em docs/adr/)

- **Canal de notificação:** app + push (FCM) como principal (≈R$0/aviso); WhatsApp/SMS como reforço; Evolution API (não-oficial) **descartada** por risco de ban. → ADR-0001
- **Rastreamento:** tempo real confiável exige **hardware próprio** (chip M2M nosso → nosso servidor); integração com a plataforma existente só onde ela permitir. → ADR-0002
- **Backend:** serviço **Node.js + fila**, não n8n single-thread, para o caminho crítico de notificação. → ADR-0003
- **Escopo em 2 níveis:** avisos (MVP) e mapa ao vivo (fase 2). → ADR-0004

## Stack (a confirmar após diagnóstico)

| Camada | Tecnologia |
|---|---|
| Rastreamento | Traccar (Apache 2.0) + PostgreSQL |
| Hardware GPS | Teltonika / Suntech + SIM M2M (cenário recomendado p/ tempo real) |
| Backend crítico | Node.js (Fastify) + fila (Redis/BullMQ) |
| App família | PWA / Capacitor + push (FCM) |
| Painel | Next.js |
| Infra | VPS Linux + Docker + Nginx/SSL |

---

## Estrutura de pastas

```
rastreador-alunos/
├── CLAUDE.md                       ← este arquivo (índice + contexto)
├── apresentacao/
│   ├── apresentacao-saquarema.html      ← deck de venda (16:9, cor)
│   ├── proposta-tecnica-saquarema.html  ← proposta técnica (ofício, P&B)
│   └── *.pdf                            ← exports
├── copy/                           ← motor de copy (briefing, avatar, voz, referência)
└── docs/
    ├── PROJETO.md                  ← escopo, personas, oferta, métricas
    ├── SOLUCAO-TECNICA.md          ← arquitetura validada, custos, riscos, fontes
    ├── PROGRESSO.md                ← log cronológico do projeto
    ├── OPEN-QUESTIONS.md           ← pendências e o que confirmar
    └── adr/                        ← decisões arquiteturais numeradas
```

---

## Convenções de documentação (docs vivos)

- Decisão técnica/produto → novo ADR em `docs/adr/`.
- Trabalho concluído → entrada datada em `docs/PROGRESSO.md`.
- Dúvida/pendência → `docs/OPEN-QUESTIONS.md`.
- Mudança de escopo → `docs/PROJETO.md`. Mudança de copy/posicionamento → `copy/BRIEFING-COPY.md`.
- Toda copy passa pelo filtro `copy/referencia/anti-ia-blacklist.yaml`.
- Datas `YYYY-MM-DD`; `**Status:**` em negrito; sem emojis; português do Brasil.

> Se um trabalho significativo for concluído e os docs não forem atualizados, é bug.

---

## Stakeholders
- **Lone Mídia** — fornecedor (dev + comercial). lonemidiamkt@gmail.com
- **Transportadora (a definir)** — comprador / cliente da proposta.
- **Prefeitura de Saquarema** — contratante do transporte universitário (paga a operadora).
- **Sócio do Paulo** — ponte comercial (intermediou a reunião).
- **Usuários finais** — famílias (recebem avisos no app) e operação (usa o painel).
