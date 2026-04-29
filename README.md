# contaski2

> Fork e extensão do [CONTASKI](https://github.com/yanuehara/contaski).

---

## Origem

Este repositório é baseado na implementação original do **CONTASKI** (_Cooperative Task Assignment based on Relational Consensus for IIoT Networks_), desenvolvida por Yan Uehara e disponível em:

- **Repositório original:** [github.com/yanuehara/contaski](https://github.com/yanuehara/contaski)
- **Artigo de referência:** [WGRS 2020 — "CONTASKI: Cooperative Task Assignment based on Relational Consensus for IIoT Networks"](https://sol.sbc.org.br/index.php/wgrs/article/view/12461)

O CONTASKI define um mecanismo de agrupamento cooperativo para redes IIoT baseado em similaridade de capacidades sensoriais entre nós, com eleição distribuída de líderes e despacho de tarefas com verificação de quorum.

---

## O que este repositório adiciona

| Aspecto | Original (yanuehara/contaski) | Este repositório |
|---|---|---|
| Função de similaridade | `capabilitiesSimilarityUFD` | `capabilitiesSimilarity` (Jaccard — Eq. 1 do artigo) |
| Eleição de líder | Por vizinhança | Por vizinhança + capacidades + IP (3 critérios) |

A principal correção em relação ao original é o uso da **função de similaridade correta** (`capabilitiesSimilarity`, coeficiente de Jaccard), conforme a Equação 1 do artigo, em substituição à função `capabilitiesSimilarityUFD` utilizada na implementação original.

---

## Branch Principal

### `contaski-ns3.29`

Implementação do CONTASKI na versão NS-3 3.29, com as correções aplicadas em relação ao repositório original. Esta é a branch de referência histórica.

**Fluxo de simulação:**

| Instante | Evento |
|---|---|
| `t = 0–60 s` | Beaconing: nós constroem lista de vizinhos |
| `t = 60–90 s` | Disseminação de capacidades entre vizinhos |
| `t = 90,5 s` | Cálculo de similaridade, formação de clusters e eleição de líder |
| `t ≈ 91 s` | Líderes registram-se no AP |
| `t = 150 s` | AP inicia despacho de tarefas |

**Similaridade de capacidades:**

Utiliza o coeficiente de Jaccard:

```
sim(A, B) = |A ∩ B| / |A ∪ B|
```

Nós com `sim ≥ 0,95` são agrupados no mesmo cluster. O líder é eleito em cascata por: (1) maior número de vizinhos, (2) maior número de capacidades, (3) menor endereço IPv6.

---

## Relação com Projetos Derivados

O trabalho iniciado neste repositório evoluiu para dois projetos subsequentes:

| Projeto | Descrição |
|---|---|
| [sectional](https://github.com/vitor-fagundes/sectional) | Extensão com Q-Learning para recuperação de nós órfãos após falhas de líderes |
| [synapt](https://github.com/vitor-fagundes/synapt) | Sistema com aprendizado intuitivo dual (S1/S2), detecção de anomalias e ciclo de decisão contínuo |

---

## Como Compilar e Executar

```bash
# Na raiz do ns-3-dev (versão 3.29)
./waf build

# Executar com N nós, rodada R
./waf --run "contaski --nNodes=200 --run=1"
```

---

## Créditos

CONTASKI original desenvolvido por **Yan Uehara** (UFPR).
Extensões e adaptações por **Vítor Fagundes** (UFMG), sob orientação de Prof. Aldri Santos (UFMG) e Prof. Carlos Pedroso (UFPR).
