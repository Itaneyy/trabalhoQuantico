# Trabalho Prático — Busca Clássica vs. Algoritmo de Grover (N = 4)

**Entrega:** Código (Notebook)

## Sumário

1. Objetivo
2. Cenário
3. Parte 1 — Busca clássica
4. Parte 2 — Busca quântica (Grover, 2 qubits)
5. Parte 3 — Análise comparativa
6. Entrega

---

# 1. Objetivo

Comparar, de forma quantitativa, o custo de buscar um item específico em uma lista não ordenada de **4 elementos** usando duas abordagens:

1. Busca clássica (busca linear);
2. Algoritmo de Grover sobre **2 qubits**.

A comparação deve ser feita **pelo número de consultas à função de busca**, e **não pelo tempo de execução**. Entender por que essa é a métrica correta faz parte do trabalho.

---

# 2. Cenário

Considere uma lista de **4 posições**, indexadas pelos estados de **2 bits**:

| Índice | Estado |
|--------:|:------:|
| 0 | `00` |
| 1 | `01` |
| 2 | `10` |
| 3 | `11` |

Existe exatamente **um item marcado** (o elemento procurado).

A regra do jogo, nas duas abordagens, é a mesma:

Você só descobre se uma posição é a procurada perguntando à função de busca `f(x)`, que devolve:

- `1`, se `x` é o item marcado;
- `0`, caso contrário.

Cada chamada a `f` conta como **uma consulta**.

> Escolha um índice marcado (por exemplo, `11`) e mantenha o mesmo nas duas partes.

---

# 3. Parte 1 — Busca clássica

Implemente uma **busca linear** que percorre a lista de `0` a `3` e para ao encontrar o item marcado.

## Requisitos

- A função deve contar e retornar o número de comparações (consultas a `f`) feitas até encontrar o item.
- Execute a busca para cada uma das **4 posições possíveis** do item marcado e registre o número de consultas em cada caso.
- A partir desses 4 valores, calcule:
  - o número médio de consultas;
  - o número de consultas no pior caso.

### Resultado esperado

- Média:

\[
\frac{1+2+3+4}{4}=2,5
\]

- Pior caso:

```
4 consultas
```

---

# 4. Parte 2 — Busca quântica (Grover, 2 qubits)

Implemente, em **Qiskit**, o algoritmo de Grover para encontrar o mesmo item marcado da Parte 1.

Como **N = 4** e existe **1 item marcado**, **uma única iteração de Grover** é suficiente e o item correto é medido com probabilidade **1**.

O circuito possui três blocos.

## (a) Inicialização — Superposição uniforme

Aplique uma porta **H** em cada qubit.

Isso coloca os quatro índices em superposição com amplitudes iguais.

---

## (b) Oráculo

Marque o item procurado invertendo o sinal (fase) da sua amplitude.

Para o item `11`, isso é simplesmente um **CZ** entre os dois qubits.

Se você escolheu outro índice, ajuste com portas **X** antes e depois do **CZ** para mover a marcação para o estado desejado.

---

## (c) Operador de difusão (inversão sobre a média)

Utilize a sequência:

```
H ⊗ H
↓
X ⊗ X
↓
CZ
↓
X ⊗ X
↓
H ⊗ H
```

Esse bloco amplifica a amplitude do estado marcado (a que o oráculo deixou negativa) e reduz as demais.

### Requisitos

- Meça os 2 qubits ao final.
- Execute no simulador (`AerSimulator` ou `qasm_simulator`) com, por exemplo, **1024 shots**.
- Apresente o histograma das medições mostrando que o item marcado domina o resultado.
- Indique quantas vezes o oráculo foi consultado.

> Na construção deste circuito: **1 consulta**, pois há apenas **1 iteração**.

---

# 5. Parte 3 — Análise comparativa

Compare as duas abordagens pelo número de consultas, preenchendo uma tabela como a seguinte:

| Abordagem | Consultas (N = 4) |
|-----------|------------------:|
| Clássica — caso médio | 2,5 |
| Clássica — pior caso | 4 |
| Grover | 1 |

---

# 6. Entrega

Entregar um **Notebook (`.ipynb`)** contendo:

- Código da Parte 1 (busca clássica);
- Código da Parte 2 (algoritmo de Grover em Qiskit);
- Histograma das medições;
- Respostas e análise da Parte 3.
