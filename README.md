# Pressão hospitalar no SNS: ocupação e urgência (2022-2026)

Análise de dados abertos do Serviço Nacional de Saúde de Portugal para perceber **como evoluem a ocupação do internamento e a procura nas urgências**, e onde existem sinais de pressão, com um olhar especial sobre a região do Algarve.

> Projeto de portfólio em ciência de dados. Estado: análise em Python concluída; consultas em SQL e painel em Power BI em desenvolvimento.

## Pergunta do projeto

Como evoluíram a ocupação do internamento e a procura nas urgências dos hospitais do SNS entre 2022 e 2026, e quais instituições e regiões mostram sinais persistentes de sobrecarga?

Perguntas de análise:
1. A taxa de ocupação mudou entre 2022 e 2026 e tem padrão sazonal?
2. Que proporção dos atendimentos na urgência é de baixa gravidade (pulseiras verde e azul)?
3. As instituições com mais atendimentos na urgência são também as mais ocupadas?

Caso de estudo: ULS do Algarve.

## Dados

- **Fonte:** [Portal da Transparência do SNS](https://transparencia.sns.gov.pt) (dados abertos).
- **Conjuntos:** *Taxa de Ocupação Hospitalar* e *Atendimentos em Urgência Hospitalar por Triagem de Manchester*.
- **Período:** janeiro de 2022 a julho de 2026 (55 meses), por instituição e por mês. O portal tem dados desde 2012, mas foi escolhido o período recente para reduzir a influência dos anos mais críticos da pandemia.
- **Tamanho:** 2.464 linhas na ocupação e 2.200 na urgência; tabela final com 2.471 linhas e 48 instituições.

Os dados são agregados por instituição e mês, **não por doente**. A análise mostra padrões, não causas.

## Método

1. **Compreensão dos dados** (`01_compreensao_dados.ipynb`): dicionário de dados, tipos, valores ausentes, período e instituições. Descobriu-se que os dias de internamento e os atendimentos são **valores acumulados desde janeiro**.
2. **Limpeza** (`02_limpeza_dados.ipynb`):
   - Valores acumulados convertidos em valores de cada mês (acumulado menos o do mês anterior; em janeiro, o próprio valor).
   - Em janeiro de 2024 várias instituições mudaram de nome (centros hospitalares e hospitais passaram a Unidades Locais de Saúde). Os nomes antigos foram ligados aos atuais e a ligação foi validada comparando o número de camas antes e depois (43 instituições, nenhuma com variação superior a 30%).
   - Valores incompatíveis com um acumulado (repetidos ou negativos) foram marcados como ausentes. Nenhum valor foi estimado.
   - Taxa de ocupação mensal recalculada: dias de internamento do mês ÷ (camas × dias do mês) × 100. Linhas com taxa fora de 40%-130% ficam marcadas como pouco confiáveis, sem serem removidas.
3. **Análise** (`03_analise.ipynb`): três perguntas e caso de estudo do Algarve.

## Principais resultados

- **Ocupação:** a taxa mediana entre instituições oscila entre cerca de 81% e 93%, sem tendência entre 2022 e 2026, e com sazonalidade clara: máxima em fevereiro (89,7%) e mínima em agosto (82,5%).
- **Urgência:** 38,0% dos atendimentos triados têm pulseira verde ou azul (pouco ou não urgentes). Esta proporção desce de 42,9% em 2022 para 33,3% em 2025 e 32,3% em 2026 (janeiro a julho), e a descida mantém-se ao comparar apenas os mesmos meses de cada ano.
- **Relação entre as duas:** a correlação entre urgência e ocupação por instituição é fraca (Spearman 0,28, com 40 instituições) e pode dever-se ao acaso.
- **Algarve:** nos meses de janeiro a julho, os dias de internamento passam de 167.896 (2022) para 183.107 (2024) e ficam estáveis depois (180.520 em 2026). Os atendimentos triados sobem até 2024 e descem em 2025 e 2026. A taxa de 2026 não é confiável (ver limitações).

## Limitações

- Os dados são agregados e mostram padrões, não causas.
- O portal não descreve as colunas. Os significados foram deduzidos dos nomes, da triagem de Manchester e de verificações nos dados.
- **Pulseira branca:** não faz parte do sistema de Manchester. Segundo fontes jornalísticas, é atribuída por razões administrativas ou casos clínicos específicos, e por isso foi analisada à parte. Falta confirmação em fonte oficial.
- **Algarve em 2026:** a lotação registada passa de cerca de 920 para 329 camas em janeiro de 2026, sem que os dias de internamento mudem. Por isso a taxa de ocupação de 2026 não foi usada, e o Algarve foi analisado pelos dias de internamento.
- A correspondência de nomes de 2024 foi validada pelos dados, e não por fonte oficial. O teste não distingue instituições de dimensão semelhante, e as novas unidades podem ter um âmbito maior que os hospitais antigos.
- Os valores em falta da urgência (principalmente "sem triagem") não foram tratados, porque não se sabe se vazio significa zero ou dado não enviado. Alguns meses do Hospital de Loures estão em falta e foram mantidos vazios.
- As coordenadas do portal estão arredondadas em parte das linhas e não foram usadas.
- 2026 tem apenas sete meses, e as comparações anuais foram feitas com os mesmos meses de cada ano.

## Estrutura do repositório

```
├── 01_compreensao_dados.ipynb
├── 02_limpeza_dados.ipynb
├── 03_analise.ipynb
└── README.md
```

## Como reproduzir

1. Descarregar do Portal da Transparência os dois conjuntos de dados em CSV, com o filtro de período de janeiro de 2022 em diante.
2. Guardá-los numa pasta de dados no Google Drive.
3. Executar os notebooks por ordem no Google Colab (Ambiente de execução, Executar tudo). Ferramentas: Python e pandas.

## Próximos passos

- Consultas em SQL com os mesmos indicadores.
- Painel em Power BI com filtros por região e instituição.
- Versão em inglês deste documento.

## Autoria

[Victor Emannuel Araújo Leal] · [linkedin.com/in/victor-leal-a0b4a1133] · [victuleal16@gmail.com]
