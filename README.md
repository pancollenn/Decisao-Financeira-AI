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

O relatório completo pode ser encontrado no arquivo [report.pdf](https://github.com/pancollenn/Decisao-Financeira-AI/blob/main/report.pdf), contendo detalhes sobre a modelagem do problema, implementação dos algoritmos, resultados obtidos e conclusões.

## ⚙️ Modelagem do Problema
O ambiente financeiro foi formalizado como um MDP da seguinte maneira:
- **Espaço de Ações ($A$):** Discreto. `0` (Manter), `1` (Comprar), `2` (Vender).
- **Espaço de Estados ($S$):** O estado informa se o portfólio está Líquido (`0`) ou Comprado (`1`) e adota uma de três abstrações de mercado:
  - *Simples (1 dia):* Indica apenas se o preço subiu ou desceu em relação ao dia anterior.
  - *Janela de Tendência (3 dias):* O histórico de alta/baixa dos últimos 3 dias.
  - *Médias Móveis:* Cruzamento de uma média móvel rápida (5 períodos) com uma lenta (20 períodos).
- **Recompensa ($R$):** O lucro ou prejuízo absoluto e imediato, calculado pela variação do valor total do portfólio após cada transição.

## 🧠 Algoritmos Desenvolvidos e Avaliados
O trabalho implementa, avalia e compara duas estratégias clássicas da Inteligência Artificial:
1. **Planejamento via Value Iteration (Equações de Bellman):** Uma abordagem baseada em modelo (*model-based*) que mapeia o ambiente empiricamente construindo matrizes de transição e recompensa após episódios exploratórios. Após isso, aplica a Equação de Otimalidade de Bellman para convergir para uma política determinística, ditando matematicamente a ação de maior lucro a longo prazo.
2. **Aprendizado via Q-learning:** Uma abordagem livre de modelo (*model-free*) que aprende a função de valor de ação iterativamente por tentativa e erro no simulador. Utiliza a estratégia de exploração $\epsilon$-greedy com taxa de decaimento iterativa para garantir uma transição suave de exploração inicial para explotação (lucro). 

## 📊 Resultados Principais
- **Teto Teórico vs Aprendizado Prático:** O modelo empírico gerado e resolvido via Value Iteration encontrou o teto absoluto teórico de lucro. Validando o framework, a abordagem model-free de Q-learning foi capaz de iteragir e aproximar sua eficiência ativamente até chegar em rendimentos praticamente equivalentes.
- **Efeito do Contexto:** Modelagens de janela curta (1 dia) sofrem de miopia financeira. Incorporar contexto (Janela de 3 dias ou Médias Móveis) proporcionou ao agente bases mais confiáveis para evitar falsas correlações e sustentar uma curva de aprendizado próspera.

## 🚀 Como Executar o Projeto

### Pré-requisitos e Dependências
O código foi concebido para Python 3. Certifique-se de instalar as dependências gráficas, numéricas e financeiras necessárias.
```bash
pip install numpy matplotlib seaborn yfinance
```

### Estrutura do Repositório
- `env/market_env.py`: Módulo contendo a lógica da simulação do mercado e do portfólio.
- `agent/q_learning.py`: Lógica principal do agente de aprendizado Q-Learning.
- `agent/policy_evaluation.py`: Rotina para mapear o ambiente e rodar o iterador de Bellman.
- `utils/`: Scripts auxiliares (plotagem com `matplotlib`/`seaborn`, módulo de download `yfinance`).

### Instruções de Execução
Com o ambiente preparado, você pode treinar os agentes executando os scripts principais na raiz do projeto:

**1. Simulação com Value Iteration:**
```bash
python run_value_iteration.py
```
*(Extrai a matriz de transição e recompensa ótima, gerando os Heatmaps visuais da Função V(s) em `plots/`)*

**2. Treinamento com Q-Learning:**
```bash
python run_q_learning.py
```
*(Inicia episódios simulados de tentativa/erro. Os resultados da convergência e gráficos do trading ao longo do tempo serão salvos em `plots/`)*
