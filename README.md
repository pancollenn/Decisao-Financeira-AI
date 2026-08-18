# MC906 - Decisão Financeira (Trabalho 2)
Universidade Estadual de Campinas (UNICAMP)
Instituto de Computação
Disciplina: MC906 - Introdução à Inteligência Artificial (Abril 2026)

## 👥 Equipe
- Bruno Jambeiro Mesquita (RA: 260382)
- Lucas Rodrigues de Mendonça (RA: 236800)
- Fernando Rodrigues da Silva (RA: 247409)
- Thiago Augusto de Tulio Nascimento (RA: 252937)
- Victor Itiro Ogitsu (RA: 244075)

## 📈 Sobre o Projeto
Este projeto tem como objetivo desenvolver um agente inteligente modelado via **Processo de Decisão de Markov (MDP)** para a tomada de decisão em um ambiente financeiro simulado. O agente visa otimizar o retorno de investimentos ao longo do tempo (lucro) atuando sobre séries temporais sintéticas (senoide com ruído) e dados reais do mercado extraídos via Yahoo Finance (ex: PETR4.SA).

O relatório completo pode ser encontrado no arquivo [report.pdf](report.pdf), contendo detalhes sobre a modelagem do problema, implementação dos algoritmos, resultados obtidos e conclusões.

## 🏗️ Arquitetura do Projeto
O projeto é modularizado para separar a lógica de ambiente, agentes, utilitários e scripts de execução:

### 1. Agentes e Lógica de Decisão
- `agent/q_learning.py`: Implementação do agente de Q-Learning com suporte a estratégias de exploração ($\epsilon$-greedy).
- `agent/policy_evaluation.py`: Implementação da classe que constrói o modelo empírico de transição e resolve as Equações de Bellman.
- `env/market_env.py`: Módulo contendo a lógica da simulação do mercado e do portfólio.

### 2. Scripts de Treinamento e Execução
- `train.py` (ou `run_q_learning.py`): Script principal para treinar o agente de Q-Learning em múltiplos estados e gerar gráficos de performance.
- `train_value_iteration.py` (ou `run_value_iteration.py`): Executa o planejamento via Value Iteration para encontrar a política ótima teórica e gerar mapas de calor (`heatmap`) de valores.
- `comparar_algoritmos.py`: Realiza o *benchmark* direto entre o desempenho do Q-Learning e o Value Iteration (Bellman).

### 3. Análise e Otimização
- `otimizar_hiperparametros.py`: Script de *Grid Search* que testa diversas combinações de $\alpha$ (taxa de aprendizado) e $\gamma$ (fator de desconto).
- `testar_exploracao.py`: Compara estratégias de exploração, especificamente o decaimento de epsilon versus exploração fixa.

## ⚙️ Modelagem do Problema
O ambiente financeiro foi formalizado como um MDP $(S, A, T, R)$ da seguinte maneira:
- **Espaço de Ações ($A$):** Discreto. `0` (Manter), `1` (Comprar), `2` (Vender).
- **Espaço de Estados ($S$):** O estado informa se o portfólio está Líquido (`0`) ou Comprado (`1`) e adota uma de três abstrações de mercado:
  - *Simples (1 dia):* Indica apenas se o preço subiu ou desceu em relação ao dia anterior.
  - *Janela de Tendência (3 dias):* O histórico de alta/baixa dos últimos 3 dias.
  - *Médias Móveis:* Cruzamento de uma média móvel rápida (5 períodos) com uma lenta (20 períodos).
- **Recompensa ($R$):** O lucro ou prejuízo absoluto e imediato, calculado pela variação do valor total do portfólio após cada transição.
- **Convergência:** O Value Iteration utiliza o limiar $\theta=10^{-5}$ para garantir a estabilidade teórica da função de valor $V(s)$.

## 🧠 Algoritmos Desenvolvidos e Avaliados
1. **Planejamento via Value Iteration (Equações de Bellman):** Uma abordagem baseada em modelo (*model-based*) que mapeia o ambiente empiricamente. Após isso, aplica a Equação de Otimalidade de Bellman para convergir para uma política determinística, ditando matematicamente a ação de maior lucro a longo prazo.
2. **Aprendizado via Q-learning:** Uma abordagem livre de modelo (*model-free*) que aprende a função de valor de ação iterativamente por tentativa e erro. Utiliza a estratégia de exploração $\epsilon$-greedy com taxa de decaimento iterativa.

## 📊 Resultados Principais
- **Model-Based vs Model-Free:** O modelo empírico resolvido via Value Iteration encontrou o teto absoluto teórico de lucro (\$9421.95). Validando o framework, o Q-learning aproximou essa eficiência ativamente, alcançando um desempenho prático equiparável (\$9231.78).
- **Efeito do Contexto:** Modelagens de janela curta sofrem de miopia financeira. Incorporar contexto (Janela de 3 dias ou Médias Móveis) evitou falsas correlações e gerou bases muito mais confiáveis para a inteligência artificial.
- **Otimização de Hiperparâmetros:** A análise de *Grid Search* confirmou que baixos valores de $\alpha$ são essenciais para filtrar o ruído do mercado simulado, enquanto valores elevados de $\gamma$ incentivam a priorização de ganhos consolidados a longo prazo.

## 🚀 Como Executar o Projeto

### Pré-requisitos e Dependências
O código foi concebido para Python 3. Certifique-se de instalar as dependências gráficas, numéricas e financeiras necessárias:
```bash
pip install numpy matplotlib seaborn yfinance
```

### Instruções de Execução
Com o ambiente preparado, você pode treinar os agentes executando os scripts principais na raiz do projeto:

**1. Simulação com Value Iteration:**
```bash
python train_value_iteration.py
```

**2. Treinamento com Q-Learning:**
```bash
python train.py
```

**3. Comparar Algoritmos:**
```bash
python comparar_algoritmos.py
```