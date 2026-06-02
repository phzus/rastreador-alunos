# PROGRESSO

## 2026-06-02 — Arranque: pesquisa, validação e apresentações

Sessão de fundação do projeto (pré-venda).

**Pesquisa e validação**
- Validados custos reais de canais (WhatsApp per-message jul/2025, FCM grátis, Evolution com risco de ban, SMS).
- Validada viabilidade técnica (Traccar, integração com plataformas fechadas BR, geofence, infra, push). Achado central: tempo real exige hardware próprio.
- Pesquisada estrutura de pitch B2B/B2G e analisado o padrão de documentação dos outros projetos do Paulo (CLAUDE.md + docs/ + adr/).

**Pivôs de escopo (decisões do Paulo ao longo da sessão)**
- Público: de crianças (escolar) → **universitários**.
- Comprador: de prefeitura → **empresa de transporte (operadora)**; venda B2B. A prefeitura é quem paga a operadora.
- Canal: **app-first** (push) em vez de WhatsApp como principal.
- Ângulo de venda: **negócio (contrato/margem/nome)**, não emoção. Família = prova. Não afirmar "telefone tocando" (sem dado — regra No Invention).
- Escopo: **dois níveis** — avisos (MVP) e mapa ao vivo (fase 2).

**Entregáveis produzidos**
- [x] Deck de venda `apresentacao/apresentacao-saquarema.html` (16:9, cor, ângulo de negócio, fonte Hanken, copy sem clichê). Render validado em PDF.
- [x] Proposta técnica `apresentacao/proposta-tecnica-saquarema.html` (ofício retrato, P&B, validada). Render validado (12 págs).
- [x] Motor de copy instalado em `copy/` (essência do MSE Funnel Labs V2) + BRIEFING-COPY/AVATAR/COMUNICACAO do projeto.
- [x] Documentação base em `docs/` (PROJETO, SOLUCAO-TECNICA, ADRs 0001–0004, este log, OPEN-QUESTIONS).

### Ajuste de posicionamento (mesmo dia)
- **Deck virou 100% venda B2B privada** — removida toda menção a licitação/prefeitura/edital/voto (decisão Paulo + sócio). Valor: menos reclamação no administrativo, mais segurança, diferencial de serviço.
- **Adicionada seção de fluxo do usuário** (slides 6–9): acesso por link, login **celular + CPF**, escolher ponto de referência, acompanhar. Foco em facilidade (cidade com pouca familiaridade tech).
- **Slide de custo refeito** com tarifas do WhatsApp por conversa (Utilidade R$0,06, Marketing R$0,58, 1.000 grátis/mês) + exemplo. App push = R$0. Atualizado também em SOLUCAO-TECNICA.
- Deck passou de 13 → **15 slides**. Render revalidado.

### Repositório
- Projeto versionado em git e vinculado a **https://github.com/phzus/rastreador-alunos** (branch `main`). Commit inicial com deck, proposta técnica, PDFs, motor de copy e docs. Screenshots de verificação ignorados via `.gitignore`.

**Pendente**
- Preencher placeholders (nome da empresa, telefone).
- Decidir se a Proposta Técnica também é limpa de licitação/prefeitura (ver OPEN-QUESTIONS).
- Confirmar abordagem de login (celular + CPF) e segurança aceitável.
- Diagnóstico técnico da frota (após fechamento).
