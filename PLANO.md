# Mira — Plano do produto (SaaS de voz com IA)

> Marca: **Mira** (placeholder; alt: Nora/Ava/Remi/Penny). Mercado: **EUA/inglês/USD**.
> Recepcionista de IA por assinatura: atende ligação/WhatsApp/SMS, agenda, passa preços.
> Multi-categoria, self-serve, 15 dias grátis, cobra no 15º dia.
> Preços placeholder: Starter $249 / Pro $499 / Enterprise custom (a confirmar).

## Idiomas (i18n)
- **Fase 1 (agora):** EN — inglês, serviços/preços EUA. Foco único.
- **Fase 2:** ES (espanhol) e PT (português) — mesmas telas, textos traduzidos.
- Site já tem seletor EN/ES/PT na barra (ES/PT marcados "soon"); estrutura reserva o lugar.
- No app real: textos em arquivo de tradução (i18n), 1 fonte por idioma.

## Status
- [x] Pesquisa de concorrentes (modelo a copiar: **Synthflow**; ref: Retell, Vapi)
- [x] Landing page de teste — `index.html`
- [x] Wizard de onboarding clicável (6 passos) — `onboarding.html`
- [x] Dashboard logado (KPIs, ligações + transcrições, agenda, resumo) — `dashboard.html`
- [x] Demo de ligação que fala no navegador (Web Speech) + logo SVG + depoimentos
- [ ] Backend real (auth, banco, billing, motor de voz)

## Arquitetura (build vs buy)
| Peça | Ferramenta | Nota |
|---|---|---|
| Motor de voz | **Vapi** (1 assistente/cliente via API) | já na memória + Twilio |
| Telefone | Twilio (provisão automática) | |
| WhatsApp / SMS | Twilio / Meta Cloud API | |
| Calendário | Cal.com (open-source) + Google Calendar OAuth | agenda via API |
| Cobrança | **Stripe** Checkout — `trial_period_days=15`, cartão na hora | cobra dia 15 |
| Frontend | HTML/Tailwind agora → Next.js depois | |
| Auth + Banco | Supabase | multi-tenant |

## Onboarding (6 passos) — "criança de 3 anos faz"
1. Categoria (cards) → 2. Dados+preços → 3. Voz+script (template editável) →
4. Conectar agenda (1 clique) → 5. Canais (tel/WhatsApp/SMS) → 6. Testar (IA liga) + Pagar

## Como o multi-tenant funciona (por baixo)
Cliente termina o wizard → backend cria via API:
1. Número Twilio (ou conecta o do cliente)
2. Assistente Vapi com o system prompt = dados do negócio + script + lista de serviços/preços
3. Webhook de agendamento → Cal.com/Google
4. Assinatura Stripe com trial de 15 dias
Cada cliente = 1 registro no Supabase ligando esses IDs.

## Backlog de features (prioridade)
**P0 (MVP cobrável)**
- Login/cadastro (Supabase)
- Wizard salva de verdade no banco
- Criação automática do assistente Vapi + número Twilio
- Stripe trial 15 dias + cobrança automática
- Botão "ligar pra mim" real (test call via Vapi)

**P1 (retenção)**
- Dashboard: gravação + transcrição + resumo por ligação
- Resumo diário no WhatsApp do dono
- Agendamentos sincronizados (Google/Cal.com)
- Transferência para humano (fallback)
- Horário de funcionamento / after-hours

**P2 (escala)**
- Multi-idioma (PT-BR/EN/ES)
- Multi-unidade / vários números
- Treino de FAQ por upload (PDF/site)
- Métricas (ligações atendidas, agendamentos, conversão)
- White-label / revenda para agências

## Compliance (inegociável)
- Aviso de gravação na abertura da ligação
- Opt-out em SMS/WhatsApp (TCPA/LGPD)
- Segredos só no .env (Vapi/Twilio/Stripe/Supabase keys)
- Teto de gasto por cliente (limite de minutos do plano)
