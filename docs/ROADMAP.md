# Roadmap do Produto — SIGE

> Visão pública de produto. Marcos concluídos e direção futura — sem detalhe de implementação.
> Versão oficial e interativa: **[seusige.com/roadmap.php](https://seusige.com/roadmap.php)**

---

## ✅ Entregue

| Versão | Marco | O que mudou para o cliente |
|---|---|---|
| **v1.25** | Fundação de produção | Ambiente endurecido, migrations versionadas, deploy com sync de banco |
| **v1.26** | **Suíte fiscal completa** | NFS-e, impostos por produto, SPED, manifestação — o cliente que emite nota opera tudo dentro do SIGE |
| **v1.27** | **Segurança + implantação** | Controle de acesso fail-closed (258 ações mapeadas), matriz de papéis para 7 perfis, wizard de onboarding que mostra o que falta para o cliente operar |

---

## 🔜 Próximo (v1.28)

| Marco | Valor para o cliente |
|---|---|
| **PIX, boleto e CNAB** | Recebimento e conciliação bancária sem sair do sistema |
| **Multi-loja operacional** | Várias unidades com caixa/estoque separados e visão consolidada |
| **Suíte E2E de venda reativada** | Cada release valida o fluxo completo de venda antes de chegar ao cliente |

## 🗓️ Horizonte (v1.29+)

| Marco | Valor |
|---|---|
| **Marketplaces** | Vender em canais externos com estoque sincronizado |
| **BI e indicadores** | Margem por produto, curva ABC, ticket médio por período |
| **App do cliente** | Fidelidade, cashback e pedidos no celular |
| **Canal de parceiros** | Implantação via contadores e revendas — escala sem equipe de vendas própria |

---

## Princípios que guiam o roadmap

1. **A operação primeiro** — caixa aberto e estoque certo vêm antes de qualquer dashboard bonito.
2. **Nada de feature fantasma** — o cliente vê só o que seu plano e seu negócio usam (ex.: bar sem nota fiscal não paga por menus de fiscal).
3. **Segurança não é item de roadmap** — é requisito de entrada de qualquer feature.
4. **Release só com gate verde** — QA automatizado (lint, migrations, permissões, regressão) antes de qualquer deploy.

---

<div align="center"><sub>Atualizado em nov/2026 · SIGE v1.27.0</sub></div>
