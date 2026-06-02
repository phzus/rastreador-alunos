# PROJETO — App de Transporte Universitário

## Visão geral

| Campo | Valor |
|---|---|
| Produto | App (white-label) de acompanhamento de transporte universitário + rastreamento GPS |
| Comprador | Empresa de transporte (operadora) — região de Saquarema-RJ |
| Quem paga a operadora | Prefeitura (contrato/licitação de transporte universitário) |
| Usuário final | Família do universitário (quem o estudante autorizar) |
| Fornecedor | Lone Mídia |
| Fase | Pré-venda (apresentações + validação técnica prontas) |
| Início | 2026-06-02 |

## Objetivo

Tornar visível e comprovável um serviço que hoje acontece sem registro: a família acompanha as viagens do estudante por um app com a marca da transportadora, e a operação ganha dados e relatórios para gerir e para comprovar o serviço à prefeitura. Isso vira diferencial de contrato para a operadora.

## Escopo

**Dentro (fase inicial)**
- Captação de GPS da frota + eventos de rota (geofence).
- App da família (white-label), Nível 1 (avisos) com opção de Nível 2 (mapa ao vivo).
- Notificação por push (principal); WhatsApp/SMS como reforço.
- Painel de gestão + relatórios (rota cumprida, frequência).
- Infra, monitoramento, SLA.
- Diagnóstico técnico da frota (etapa inicial obrigatória).

**Fora (fase inicial)**
- Bilhetagem, catraca, controle de presença por leitor.
- Integração com sistemas acadêmicos.
- Substituição da frota.

## Personas

| Persona | Papel | O que quer |
|---|---|---|
| Dono/diretor da transportadora | **Comprador** | Ganhar/renovar contrato, margem, nome. Não decide por emoção. |
| Prefeitura | Contratante (paga a operadora) | Imagem/voto, comprovação do serviço, menos reclamação. |
| Família (pai/mãe/etc.) | **Usuário** do app | Acompanhar a viagem do estudante. É a prova/vitrine, não o alvo da venda. |
| Estudante universitário | Rastreado (via ônibus) | Tem a viagem acompanhada pela família; em geral viaja à noite, intermunicipal. |
| Motorista | Operacional | Nada de trabalho extra (sistema automático). |

## Oferta — dois níveis (ver ADR-0004)

| | Nível 1 — Avisos (MVP) | Nível 2 — Mapa ao vivo |
|---|---|---|
| Experiência | Saiu · passou no ponto que a família escolhe · chegou | Avisos + ônibus no mapa em tempo real |
| GPS | 30–60 s | 10–15 s |
| Hardware próprio | Não necessariamente | Em geral, sim |
| Custo de operação | Baixo | Médio |
| Indicação | Começo / piloto / maioria | Contrato maior / fase 2 |

## Eventos de notificação (Nível 1)

| Evento | Gatilho | Mensagem |
|---|---|---|
| Saiu | Saída da origem/faculdade | "O ônibus saiu." |
| Passou no ponto | Ponto de referência escolhido pela família | "Passou no seu ponto. Chegada em ~X min." |
| Chegou | Destino/ponto final | "Chegou ao destino." |

## Componentes do sistema

1. Captação GPS (rastreador → servidor Traccar).
2. Motor de eventos (geofence + proximidade no backend).
3. Backend de notificação (Node + fila): "qual ônibus → quem avisar".
4. Canais: push (principal); WhatsApp/SMS (reforço).
5. App da família (white-label).
6. Painel da operação + relatórios.

## Métricas de sucesso

- Latência evento → aviso entregue: < 60 s (Nível 1).
- Entrega do aviso crítico (com fallback): > 99%.
- Disponibilidade: 99,5%/mês.
- Adoção: % de famílias com app ativo por rota.
- Comercial: piloto aprovado → expansão da frota.
