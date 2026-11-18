<div align="center">

<img src="assets/screenshots/hero-banner.png" alt="SIGE — Sistema Integrado de Gestão Empresarial" width="100%"/>

# SIGE — Sistema Integrado de Gestão Empresarial

**Um SaaS completo para pequenas e médias empresas: ERP, PDV, cardápio digital, delivery e gestão financeira em uma única plataforma multi-tenant.**

![Versão](https://img.shields.io/badge/versão-1.27.0-1976d2)
![Status](https://img.shields.io/badge/status-produção-2e7d32)
![PHP](https://img.shields.io/badge/PHP-8.4-777BB4)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-4169E1)
![Tauri](https://img.shields.io/badge/Tauri-2.0(Rust)-FFC131)
![Multi-tenant](https://img.shields.io/badge/arquitetura-multi--tenant-6a1b9a)

*Projeto pessoal · Desenvolvido do zero, do banco de dados ao instalador desktop*

</div>

---

## 🎯 O problema

Pequenos negócios (bares, restaurantes, lojas, delivery) operam com **3 a 5 ferramentas desconectadas**: um ERP caro, um sistema de PDV à parte, um app de delivery com comissões abusivas, planilhas de estoque e um caderno para o financeiro. O resultado é **dado duplicado, erro de caixa, fura de estoque e zero visão do negócio**.

## 💡 A solução

O SIGE integra **tudo em uma só plataforma**, com um dado de verdade único:

| Você precisa de... | No SIGE é... |
|---|---|
| Frente de caixa rápida e confiável | **PDV Desktop** (Rust/Tauri) com modo offline e sincronização automática |
| Vender online sem comissão | **Cardápio digital + delivery** próprios, com pedidos direto no caixa |
| Controlar estoque e compras | **Estoque** com movimentações, inventário e política de venda negativa configurável |
| Saber se está dando lucro | **Financeiro** (contas a pagar/receber), relatórios e dashboard |
| Gerenciar equipe e permissões | **Matriz de permissões granular** por papel, módulo e operação |
| Crescer para várias lojas | Arquitetura **multi-tenant e multi-loja** pronta desde a fundação |

---

## 📸 O produto

<div align="center">

### ERP Web
<img src="assets/screenshots/dashboard-erp.png" alt="Dashboard do ERP" width="82%"/>

### PDV Desktop (Tauri 2.0)
<img src="assets/screenshots/pdv-desktop.png" alt="PDV Desktop" width="82%"/>

### Cardápio Digital
<img src="assets/screenshots/cardapio-digital.png" alt="Cardápio digital" width="82%"/>

### Gestor SaaS — visão multi-tenant
<img src="assets/screenshots/gestor-saas.png" alt="Gestor SaaS" width="82%"/>

### Onboarding guiado de novos clientes
<img src="assets/screenshots/wizard-onboarding.png" alt="Wizard de onboarding" width="82%"/>

### Controle de acesso fail-closed
<img src="assets/screenshots/matriz-permissoes.png" alt="Matriz de permissões" width="82%"/>

</div>

---

## 🧩 Os módulos

| Módulo | O que resolve |
|---|---|
| 🛒 **PDV Desktop** | Venda offline-first com sync automático, impressão de cupom, controle de caixa por turno/terminal e auto-update assinado |
| 🍔 **Cardápio Digital + Delivery** | Pedidos online com slug próprio por tenant, mesa, retirada e delivery — direto no caixa, sem intermediário |
| 📦 **Estoque** | Inventário inicial, movimentações, fornecedores e política de estoque negativo por produto |
| 💰 **Financeiro** | Contas a receber/pagar, formas de pagamento com taxas, comissões de vendedores e estorno seguro |
| 👥 **Clientes & Fidelidade** | Cadastro, pontos, cashback e histórico de compras |
| 📄 **Fiscal** | NFS-e, impostos por produto, SPED e manifestação (o cliente pode operar sem NF — módulo oculto pelo plano) |
| 🔐 **Permissões** | Papéis granulares (leitura/escrita) com padrão **negar**: ação sem permissão explícita = bloqueada |
| 🧭 **Onboarding de implantação** | Wizard que mede o que falta para o cliente operar (empresa, usuários, caixa, estoque, cardápio) |
| 📧 **E-mail transacional** | Templates e fila de e-mails com SMTP por tenant |
| 📊 **Relatórios & Exportação** | Vendas, estoque e financeiro exportáveis |

---

## 🏗️ Arquitetura — em uma imagem

```mermaid
flowchart LR
    subgraph Nuvem["SaaS multi-tenant"]
        W[ERP Web PHP 8.4] --> DB[(PostgreSQL\nERP por tenant + FDW)]
        G[Gestor SaaS] --> SAAS[(Banco SaaS\nplanos · assinaturas · tenants)]
        A[API REST JWT/Session] --> DB
        C[Cardápio Digital] --> DB
        DB -. "foreign tables" .-> SAAS
    end
    subgraph Desktop["Na loja do cliente"]
        P[PDV Tauri/Rust] -->|"sync + auto-update"| A
    end
    E[Delivery / Clientes] --> C
```

**Decisões que sustentam o produto:**

- **Multi-tenancy via FDW** — isolamento lógico com custo zero de infraestrutura por cliente, e caminho de migração para banco-a-banco quando necessário.
- **PDV em Rust/Tauri** — binário leve (vs Electron), auto-update assinado criptograficamente e operação offline para lojas com internet instável.
- **Session + JWT lado a lado** — sessão revogável no ERP web, tokens para PDV/mobile/API, com 2FA TOTP.
- **PHP puro + PostgreSQL** — sem frameworks pesados: manutenção simples, deploy rápido e stack que qualquer equipe consegue manter.

---

## 🔒 Segurança tratada a sério (desde o primeiro commit)

- **Permissões fail-closed**: 258 ações de negócio mapeadas a módulo + operação; ação fora do mapa é **negada por padrão** — e o gate de regressão **falha** se alguém adicionar uma ação sem mapeá-la.
- **Multi-tenant**: nenhum cliente enxerga dados de outro (isolamento no banco + contexto de tenant em toda query).
- **2FA TOTP**, proteção contra brute-force (Nginx + aplicação), CSRF, CSP por host, TLS Full Strict via Cloudflare.
- **Trilha de auditoria**: eventos de segurança registrados, backups automáticos do banco, firewall e fail2ban na infraestrutura.

---

## 📈 Números do projeto

| | |
|---:|---|
| **155** migrations idempotentes sequenciais | **258** ações de negócio sob controle de permissão |
| **145** tabelas (ERP + SaaS) | **67** módulos contratáveis por plano |
| **28** verificações automatizadas no gate de QA | **8** subdomínios (ERP, Gestor, API, Cardápio, Portal, Landing, PDV, Docs) |
| **v1.27.0** em produção | **100%** do sistema construído por uma pessoa |

---

## 🗺️ Roadmap

- ✅ **v1.26** — Suite fiscal completa (NFS-e, impostos, SPED)
- ✅ **v1.27** — Trava anti-fraude fail-closed + onboarding guiado de clientes
- 🔜 **v1.28+** — PIX/boleto/CNAB, multi-loja operacional, reativação da suíte E2E de venda
- 🔭 **Horizonte** — marketplaces, BI avançado, app do cliente, parcerias com contadores

Detalhes na [review de produto](docs/REVIEW-PRODUTO.md) e no [roadmap](docs/ROADMAP.md).

---

## 🧑‍💼 Sobre este projeto

O SIGE é um produto **real, em operação com cliente pagante**, construído por mim — Leonardo Ribeiro ([@leonardosanrn](https://github.com/leonardosanrn)) — de ponta a ponta: modelagem de banco, backend, frontend, PDV desktop em Rust, infraestrutura, deploy e o processo de QA automatizado.

> ⚠️ **Este repositório é a vitrine pública do produto.** O código-fonte é proprietário e vive em repositórios privados — disponível para demonstração em processos seletivos e conversas com investidores e parceiros.

**Quer ver o sistema rodando?** Fale comigo: [linkedin](https://linkedin.com/in/SEU-LINKEDIN) · e-mail · [seusige.com](https://seusige.com)

---

<div align="center"><sub>© 2026 SIGE — Sistema Integrado de Gestão Empresarial. Todos os direitos reservados.</sub></div>
