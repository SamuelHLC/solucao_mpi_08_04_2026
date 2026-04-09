## Relatório da Atividade: Avaliação de Similaridade de Perguntas com MPI
**Disciplina:** Programação Paralela e Concorrente  
**Aluno:** Samuel Henrique Lima Cançado 
**Turma:** Sistema da Informa~ção - Noturno - 5º Semestre  
**Professor:** Rafael  
**Data:** 08/04/2026  

---

# 1. Descrição do Problema
O programa resolve o problema de comparação de similaridade textual entre pares de perguntas de um dataset real.

* **Objetivo:** Identificar o nível de similaridade entre diferentes textos para detectar duplicatas ou perguntas correlatas.
* **Algoritmo:** Similaridade de Jaccard (calcula a razão entre a interseção e a união dos conjuntos de palavras de dois textos).
* **Entrada:** Primeiras **3.000 perguntas** do dataset "Quora Question Pairs", resultando em **4.498.500 comparações** únicas.
* **Complexidade:** $O(n^2)$ - quadrática, o que torna o processamento sequencial custoso conforme o volume de dados aumenta.

---

# 2. Ambiente Experimental

| Item | Descrição |
| :--- | :--- |
| **Processador** | AMD Ryzen 5 5500 (3.60 GHz) |
| **Número de núcleos** | 6 núcleos físicos / 12 threads |
| **Memória RAM** | 16 GB |
| **Sistema Operacional** | Windows 11 (64 bits) |
| **Linguagem utilizada** | Python 3.14 |
| **Biblioteca de paralelização** | mpi4py (MPI) |
| **Compilador / Versão** | MS-MPI v10.1 |

---

# 3. Metodologia de Testes
* **Medição de tempo:** Tempo total de execução (wall time) medido internamente pelo script.
* **Execuções:** Testes realizados com 1, 2, 4, 8 e 12 processos.
* **Carga:** Divisão estática de tarefas baseada nos índices das perguntas.
* **Condições:** Execução em máquina local, minimizando processos em segundo plano para maior precisão.

---

# 4. Resultados Experimentais

| Nº Threads/Processos | Tempo de Execução (s) |
| :--- | :--- |
| 1 (Serial) | **9.60** |
| 2 | **5.05** |
| 4 | **2.82** |
| 8 | **1.71** |
| 12 | **1.65** |

---

# 6. Tabela de Resultados (Speedup e Eficiência)

| Threads/Processos | Tempo (s) | Speedup | Eficiência |
| :--- | :--- | :--- | :--- |
| 1 | 9.60 | 1.00 | 1.00 |
| 2 | 5.05 | 1.90 | 0.95 |
| 4 | 2.82 | 3.40 | 0.85 |
| 8 | 1.71 | 5.61 | 0.70 |
| 12 | 1.65 | 5.81 | 0.48 |

---

# 10. Análise dos Resultados
* **Speedup e Escalabilidade:** O sistema apresentou escalabilidade positiva. O speedup máximo de **5.81x** aproximou-se do limite físico de núcleos (6 núcleos) do hardware utilizado.
* **Ponto de Queda de Eficiência:** A eficiência caiu drasticamente ao utilizar 12 processos (0.48). Isso ocorre porque, embora o processador possua 12 threads, ele possui apenas 6 núcleos físicos compartilhando recursos de execução (SMT).
* **Overhead:** Houve overhead perceptível na carga do dataset via MPI, uma vez que cada processo precisa carregar os dados na memória individualmente.
* **Distribuição de Carga:** O loop de comparação triangular ($j > i$) gera um desbalanceamento natural, onde os primeiros processos executam mais iterações que os últimos, impedindo um speedup perfeitamente linear.

---

# 11. Conclusão
O experimento demonstrou a eficácia do uso de computação distribuída com MPI para problemas de alta complexidade computacional. A redução do tempo de execução de **9.60s para 1.65s** valida a paralelização como solução viável. Para o hardware testado, a configuração com **6 a 8 processos** ofereceu o melhor equilíbrio entre desempenho e eficiência de recursos.
