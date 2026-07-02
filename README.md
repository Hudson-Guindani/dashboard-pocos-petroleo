# Dashboard de Análise de Bacias e Poços de Petróleo (Brasil)

Este repositório contém um dashboard interativo desenvolvido no **Power BI Desktop** para análise, monitoramento e mapeamento do cenário operacional de poços de petróleo e gás. O objetivo do projeto é consolidar dados volumosos do setor para apoiar tomadas de decisão sobre eficiência operacional, status de produção e distribuição geográfica/geológica dos ativos.

## 📊 Visões e Insights Gerados pelo Projeto

Com base no modelo de dados estruturado, o relatório foi dividido em 4 visões principais para responder a diferentes perguntas de negócio:

1. **Visão Geral (Bacias e Poços):** 
   * **O que traz:** Um panorama completo contendo o ecossistema total de ativos (37 Bacias, 550 Campos e mais de 30 mil Poços mapeados).
   * **Insights:** Identificação rápida de que a grande maioria dos poços está concentrada em **Terra (77,36%)** em comparação com o Mar (22,64%), e que a **Petrobras** detém a liderança esmagadora na operação desses poços (mais de 23 mil ativos sob sua titularidade).

2. **Visão de Ativos Ativos (Poços Produzindo):**
   * **O que traz:** Uma visão filtrada dinamicamente focada apenas nos poços que estão gerando receita (Status: PRODUZINDO).
   * **Insights:** Permite enxergar que, embora a maioria dos poços totais esteja em terra, o comportamento muda dependendo do foco operacional. Consolida dados de operadoras privadas menores que ganham relevância regional na produção em bacias maduras (como na Bacia Potiguar e Recôncavo).

3. **Visão de Passivos e Desativação (Poços Abandonados):**
   * **O que traz:** Foco exclusivo nos ativos que já encerraram seu ciclo produtivo (Status: ABANDONADO).
   * **Insights:** Monitoramento de passivos ambientais e operacionais. Mostra graficamente quais bacias possuem o maior volume de poços desativados (Recôncavo e Potiguar liderando) e o perfil desses poços por categoria (maioria classificada como poços de *Desenvolvimento*).

4. **Análise de Causa Raiz (Árvore Hierárquica):**
   * **O que traz:** Um visual de inteligência artificial (Decomposition Tree) que permite ao usuário quebrar a métrica de `Total Poços` de forma granular.
   * **Insights:** Permite rastrear instantaneamente o caminho lógico do dado: escolhendo uma **Bacia** (ex: Potiguar) $\rightarrow$ visualiza-se a distribuição por **Estado** (RN) $\rightarrow$ descobrindo qual **Operador** atua ali (Petrobras) $\rightarrow$ chegando até o **Campo** específico (Estreito). Perfeito para auditorias rápidas de portfólio.

---

## 🛠️ Estrutura do Modelo de Dados

O projeto foi construído seguindo as melhores práticas de modelagem (Star Schema), separando tabelas de busca/dimensões da tabela de fatos e centralizando as regras de negócio em uma tabela exclusiva de medidas.

### Tabela Fato Principal (`fato_pocos`)
* **`POCO`**: Código identificador único do poço (Chave primária).
* **`OPERADOR` / `TITULARIDADE`**: Empresa responsável pela operação (ex: Petrobras, Trident, Potiguar E&P) e regime de concessão (Público/Privado).
* **`ESTADO` / `BACIA` / `CAMPO`**: Atributos geográficos e geológicos para fatiamento espacial.
* **`TERRA_MAR`**: Classificação do ambiente de perfuração (Terra / Mar).
* **`CATEGORIA`**: Classificação técnica do poço (Desenvolvimento, Pioneiro, Extensão, Injeção).
* **`SITUACAO`**: Status atual do ativo (PRODUZINDO, INJETANDO, ABANDONADO).
* **`CONCLUSAO`**: Data histórica em que a perfuração/obra do poço foi concluída.

### Medidas DAX Utilizadas
As principais métricas foram calculadas dinamicamente utilizando a linguagem DAX:
* `Total Poços`: Contagem do volume total de ativos mapeados.
* `Total Bacias` / `Total Campos`: Contagens distintas (`DISTINCTCOUNT`) para mensurar a capilaridade geológica.
* `Poços Produzindo` / `Injetando` / `Abandonados`: Métricas calculadas utilizando a função `CALCULATE` combinada com filtros específicos sobre a coluna `SITUACAO`.
* `Índice de Eficiência` e `% Integridade`: Indicadores de performance para avaliar a proporção de poços ativos versus o total de passivos.

---

## 🚀 Como executar este projeto

1. Faça o download ou clone este repositório:
   ```bash
