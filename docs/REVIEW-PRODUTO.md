# Review de Produto — SIGE v1.27.0

> Análise de produto do SIGE: para quem é, o que resolve, onde se diferencia e por que é um
> bom negócio. Escrito para investidores, parceiros e recrutadores — sem código-fonte.

---

## 1. Resumo executivo

O SIGE é um **SaaS de gestão para pequenas e médias empresas brasileiras** que unifica
ERP, PDV, cardápio digital, delivery e financeiro em uma plataforma só. Diferente de
soluções que começam pelo web e "consertam depois", o SIGE foi desenhado ao redor da
**frente de caixa real** — loja com internet instável, operador sem treinamento, caixa que
precisa fechar certo.

- **Estágio:** produção, cliente pagante ativo (plano Enterprise — sob consulta).
- **Versão:** 1.27.0 (nov/2026) — suíte fiscal completa + controle de acesso fail-closed.
- **Time:** desenvolvimento solo full-stack + infraestrutura + QA.

---

## 2. Público-alvo

| Perfil | Dor principal | Como o SIGE resolve |
|---|---|---|
| **Bares e restaurantes** | Comissão de marketplaces, erro de caixa, cardápio desatualizado | Cardápio próprio + delivery sem comissão + PDV com cupom |
| **Varejo de bairro** | Estoque fura, sem visão de lucro | Estoque integrado ao PDV + relatórios |
| **Redes pequenas (2–5 lojas)** | Dados espalhados por ferramentas | Multi-loja com dado único |
| **Operações com equipe** | Quem pode o quê? | Matriz de permissões granular por papel |

**ICP (perfil ideal):** negócio com 1–5 pontos de venda, 2–30 funcionários, faturamento
R$ 30k–500k/mês, que já sentiu dor de estoque/caixa e paga hoje por 2+ ferramentas.

---

## 3. Proposta de valor

> **"O sistema da sua empresa inteira — caixa, estoque, delivery e financeiro — num lugar só,
> sem comissão de marketplace e sem depender de internet para vender."**

Três pilares verificáveis:

1. **Offline-first na operação** — o PDV vende sem internet e sincroniza depois. Para o
   varejo brasileiro, isso não é feature: é requisito.
2. **Dado único** — a venda no PDV baixa estoque, movimenta financeiro e alimenta o
   dashboard no mesmo instante. Zero re-lançamento.
3. **Segurança por desenho** — permissões com padrão negar (fail-closed), isolamento
   multi-tenant no próprio banco e trilha de auditoria. Pensado para vender a quem
   se preocupa com compliance — e para não virar manchete.

---

## 4. Diferenciais competitivos

| Dimensão | SIGE | ERP tradicional (ex.: genéricos) | "Só PDV" | Marketplaces + apps soltos |
|---|---|---|---|---|
| PDV desktop offline-first | ✅ nativo (Rust/Tauri) | ⚠️ web, depende de internet | ✅ | ❌ |
| Delivery sem comissão | ✅ cardápio próprio | ❌ | ❌ | ❌ (cobra 15–30%) |
| Estoque + financeiro integrados | ✅ | ✅ | ⚠️ parcial | ❌ |
| Fiscal completo (NFS-e, SPED) | ✅ | ✅ | ❌ | ❌ |
| Multi-tenant / multi-loja | ✅ nativo | ⚠️ caro | ❌ | ❌ |
| Onboarding guiado (implantação) | ✅ wizard mede o que falta | ❌ consultoria cara | ❌ | ❌ |
| Custo de entrada | mensalidade única | licença + implantação | mensalidade | comissão por venda |

**Fosso técnico difícil de copiar:** a arquitetura multi-tenant via FDW + PDV offline com
sync + matriz de permissões fail-closed são coerentes entre si desde a fundação. Concorrentes
acrescentam "modo multi-empresa" sobre monólitos; aqui é a própria fundação.

---

## 5. Modelo de negócio

| Plano | Público | Módulos | Preço |
|---|---|---|---|
| **Free** | 1 usuário, 5 GB — testes e negócios que estão começando | 27 módulos | **R$ 0,00/mês** grátis para sempre |
| **Profissional** | 10 usuários, 20 GB — pequenos negócios em operação | 63 módulos | **R$ 49,90/mês** ou R$ 497,00/ano (economize 17%) |
| **Enterprise** | usuários ilimitados, 100 GB, gerente de conta dedicado, API personalizada | 67 módulos | **Sob consulta** — valor personalizado |

*Valores conforme o [site oficial](https://seusige.com/#planos). Todos os planos incluem 30 dias de teste grátis, sem cartão de crédito, e cancelamento sem multas.*

- **Módulos contratáveis por plano** (67 hoje) — upsell natural sem forçar upgrade de produto.
- **Receita recorrente (MRR)** com churn baixo: ERP + PDV são críticos para o negócio rodar.
- **Margem:** infraestrutura multi-tenant compartilhada (custo ~zero por tenant adicional).
- **Expansão:** cada novo módulo (marketplace, BI, app cliente) abre nova camada de receita.

---

## 6. Maturidade técnica (por que confiar)

| Área | Estado |
|---|---|
| **QA automatizado** | Gate de regressão com 28 verificações: lint, reaplicação de 155 migrations, testes de permissão e fluxo |
| **Segurança** | Enforcement fail-closed nas 258 ações do ERP; suíte de teste prova que papel sem permissão recebe 403 |
| **Fiscal** | NFS-e, impostos por produto, SPED e manifestação — homologáveis |
| **Deploy** | Pipeline com migrations sincronizadas; VPS endurecida (Cloudflare, TLS Full Strict, fail2ban, firewall) |
| **PDV desktop** | Auto-update assinado criptograficamente via GitHub Actions (Windows: NSIS + MSI) |
| **Dados** | 145 tabelas (ERP + SaaS), 102+ relações de negócio, backups automáticos |
| **Processo** | Sprints versionadas com critérios de aceite, changelog e política de versionamento semântico |

---

## 7. Pontos fortes (honestamente)

1. **Produto de verdade, não protótipo** — cliente pagante operando caixa, estoque e cardápio.
2. **Vertical completo** — do banco PostgreSQL ao instalador Windows assinado, sem caixa-preta de framework.
3. **Cultura de segurança e QA** — o gate de regressão quebra o build se uma ação nova nascer sem permissão. Isso é processo de time maduro, escrito por uma pessoa.
4. **Documentação viva** — ADRs (decisões de arquitetura), specs por domínio, sprints com critérios de aceite.
5. **Economia de produto** — feature que não serve ao cliente (ex.: fiscal para quem não emite nota) é **oculta por configuração**, não vendida como "pacote mínimo".

## 8. Riscos e limitações (transparência)

| Risco | Mitigação em curso |
|---|---|
| Time solo (bus factor) | Documentação extensa, processos automatizados, arquitetura sem exotismos |
| Suíte E2E de venda desativada temporariamente | Reativação priorizada no roadmap (v1.28) |
| Competidores maiores com mais caixa | Foco no nicho (PDV-first, offline, sem comissão) e no atendimento de implantação |
| Aprovação fiscal depende de homologação por estado | Arquitetura já pronta; escalonamento via parceria com contadores |
| Base de clientes ainda pequena | Piloto de implantação assistida comprova o guia de onboarding |

---

## 9. Tração e próximos passos

- **Hoje:** 1 cliente Enterprise pagante em operação real (bar, 710 produtos, cardápio digital
  ativo), go-live acompanhado com onboarding assistido.
- **Curto prazo (v1.28):** PIX/boleto/CNAB, multi-loja operacional, reativação da suíte E2E.
- **Médio prazo:** marketplaces, BI, app do cliente, canal de parceiros (contadores e
  revendas de tecnologia para o varejo).

---

## 10. O que estou buscando

| Público | Conversa |
|---|---|
| **Investidores** | Validação de mercado, MRR e roadmap de expansão — aberto a apresentar números e demo |
| **Parceiros** | Contadores, revendas de PDV, integradores de delivery |
| **Recrutadores** | Este produto é meu portfólio vivo: full-stack, DevOps, segurança e produto — [LinkedIn](https://www.linkedin.com/in/leonardosantosrn/) · [GitHub](https://github.com/leonardosanrn) |

---

<div align="center"><sub>SIGE v1.27.0 · documento público de produto · nov/2026</sub></div>
