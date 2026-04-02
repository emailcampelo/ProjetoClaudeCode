# Design Spec — Planilha de Viabilidade Econômica
**Data:** 2026-03-29
**Projeto:** Incorporadora Imobiliária
**Pasta:** 07_FINANCEIRO
**Arquivo de saída:** `Viabilidade_Economica_Dashboard.xlsx`

---

## 1. Contexto e Objetivo

Criar uma planilha semi-automatizada de viabilidade econômica para **1 empreendimento**, com horizonte de **18 a 36 meses**, que consolide todos os KPIs financeiros e operacionais em um único dashboard executivo. O usuário preenche abas de entrada; o dashboard se atualiza automaticamente via fórmulas (sem macros/VBA).

---

## 2. Estrutura de Abas

| Aba | Tipo | Descrição |
|-----|------|-----------|
| `DASHBOARD` | Saída | Visão executiva com todos os KPIs em 1 tela |
| `CONFIG` | Entrada | Dados fixos do empreendimento e metas |
| `RECEITAS` | Entrada | Unidades, preços e cronograma de recebimentos |
| `CUSTOS` | Entrada | Orçado vs. realizado por categoria |
| `CRONOGRAMA` | Entrada | Datas planejadas vs. reais por fase |
| `FORNECEDORES` | Entrada | Tabela com score automático de risco |
| `DESEMBOLSO` | Misto | Curva mensal de desembolso (entrada + gráfico) |

---

## 3. Dashboard — Blocos de KPI

### Bloco 1 — Margem do Empreendimento
- Receita Total (VGV) − Custo Total = Lucro Bruto
- Exibido em R$ e % sobre VGV
- Semáforo: verde (>20%), amarelo (10–20%), vermelho (<10%)

### Bloco 2 — ROI
- Lucro Líquido ÷ Capital Próprio Investido
- Exibido em % total e % ao ano (anualizado pelo prazo real)
- Meta configurável em `CONFIG`

### Bloco 3 — Custo Realizado vs. Previsto
- Categorias: Terreno | Projetos e Aprovações | Obra Civil | Instalações | Marketing | Comercialização | Financeiro | Outros
- Colunas: Orçado R$ | Realizado R$ | Desvio R$ | Desvio %
- Linha de totais destacada; formatação condicional por desvio

### Bloco 4 — Curva de Desembolso Mensal
- Gráfico combinado: barras (mensal) + linha (acumulado)
- Eixo X: Mês 1 a Mês 36
- Mês atual destacado automaticamente

### Bloco 5 — Prazo Real vs. Planejado
- 8 fases: Aquisição Terreno | Projeto Arq. | Aprovações | Lançamento | Obra Início | Obra Conclusão | Habite-se | Entrega de Chaves
- Colunas: Data Planejada | Data Real | Desvio (dias) | Status (no prazo / atrasado)

### Bloco 6 — Ranking de Fornecedores (Top 10)
- Score de Risco = (Desvio Custo % × 0,5) + (Desvio Prazo normalizado × 0,5)
- Ranking automático do maior para o menor risco
- Formatação condicional: vermelho (alto risco), amarelo (médio), verde (baixo)

---

## 4. Abas de Entrada — Especificação

### CONFIG
- Nome do empreendimento, endereço, data de início
- Prazo previsto (meses), VGV total, custo total orçado
- Capital próprio investido, meta de margem (%), meta de ROI (%)

### RECEITAS
- Tabela de unidades: tipo, qtd, preço unitário, % vendido, valor recebido
- Cronograma de recebimentos: sinal, parcelas de obra, chaves, financiamento

### CUSTOS
- 8 categorias fixas
- Campos: Orçado R$ | Realizado R$ | % executado
- Desvio calculado automaticamente

### CRONOGRAMA
- 8 fases com: Data Planejada | Data Real
- Desvio em dias calculado automaticamente

### FORNECEDORES
- Campos: Nome | Categoria | Valor Orçado | Valor Realizado | Prazo Previsto (dias) | Prazo Real (dias)
- Cálculos automáticos: Desvio Custo % | Desvio Prazo dias | Score de Risco | Ranking

### DESEMBOLSO
- 36 colunas (Mês 1 a Mês 36)
- Linhas por categoria de custo
- Totais mensais e acumulados automáticos

---

## 5. Regras de Negócio

- Semáforos via formatação condicional (sem VBA)
- Score de fornecedor normalizado no intervalo 0–100
- Mês atual detectado via função `HOJE()` comparada ao mês de início em `CONFIG`
- Todas as fórmulas usam referências nomeadas para facilitar manutenção
- Valores em R$ formatados com separador de milhar e 2 casas decimais

---

## 6. Decisões de Design

- **Sem macros/VBA:** garante compatibilidade total com Excel e Google Sheets
- **Dados de entrada em abas separadas:** evita corrupção acidental de fórmulas
- **Score ponderado 50/50:** custo e prazo têm peso igual no ranking de fornecedores (ajustável em `CONFIG`)
- **Horizonte fixo de 36 colunas em DESEMBOLSO:** cobre o prazo máximo declarado; meses futuros ficam em branco até preenchimento
