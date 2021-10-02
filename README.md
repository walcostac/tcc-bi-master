# Otimização para Dimensionamento de Equipe de TI

#### Aluno: Walter da Costa Carvalho(https://github.com/walcostac)
#### Orientador: Felipe Borges(https://github.com/FelipeBorgesC)

---

Trabalho apresentado ao curso [BI MASTER](https://ica.puc-rio.ai/bi-master) como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".

---

### 1. Introdução

Uma estatal planeja contratar uma empresa, via leilão reverso, para atender demandas da área de TI. Como trata-se de uma contratação de serviço, o vencedor do certame fica responsável pelo dimensionamento da equipe.

As demandas de TI, por serem bastante diversificadas, foram divididas em 5 áreas (Banco de Dados; Sistemas Operacionais; Atendimento ao Cliente; Desenvolvimento e Arquitetura). Dessa forma, a composição da equipe deverá contemplar analistas especializados em cada um dos nichos. Além disso, sabendo que 100% das demandas são encaminhadas por uma plataforma de atendimento que precisa funcionar de segunda a sexta (das 7h até 22h), a empresa vencedora deverá atender os seguintes requisitos:

- Atendimento em 3 turnos (7h-11h; 10h-19h; 13h-22h);
- Para cada uma das 5 áreas, a equipe deverá ter, pelo menos, 2 analistas seniores com 15 anos de experiência na respectiva área. Os outros integrantes deverão ter, no mínimo, 8 anos de experiência (analistas plenos);
- Para cada nicho de atuação deverá existir uma mesa de atendimento para receber as demandas encaminhas pelos clientes;
- Um dos diversos parâmetros de volumetria sinalizados no edital diz respeito ao tempo total de atendimento das demandas na plataforma. Baseado no histórico de atendimento, a estatal informou que, em média, as contratadas utilizam 255.395 minutos mensalmente com atendimento de demandas das 5 áreas de atuação. Sendo assim, a vencedora do certame tem que ser capaz de suportar tal carga mantendo os níveis de qualidade (a qualidade do serviço é medida mensalmente utilizando diversos indicadores sinalizados no edital).

Como a estatal percebeu muitos problemas com dimensionamento de equipe nos contratos anteriores (geralmente as contratadas subdimensionam o time para aumentar o lucro), uma das etapas do processo seletivo será justamente a verificação quanto à qualidade e quantidade de analistas da equipe para a prestação do serviço. Sendo assim, além de verificar o currículo dos analistas, a estatal utilizará algoritmos genéticos para estabelecer um contingente mínimo.

Outro ponto importante diz respeito ao número de integrantes qualificados na equipe: a satisfação da estatal com o serviço prestado, em geral, é diretamente proporcional ao número de integrantes qualificados da equipe contratada. Dessa forma, a estatal não informará o número mínimo aceitável de analistas na equipe que prestará o serviço e desclassificará participantes que não conseguirem provar matematicamente que o dimensionamento da equipe atende ao principal requisito do edital quanto à volumetria de serviço.

Dada a explanação do problema, o TCC tem o objetivo de apresentar uma solução, utilizando algoritmo genético, para determinar o contingente mínimo (tamanho de equipe), baseado no histórico mensal de tempo total médio de atendimento apresentado no edital (pouco mais de 255 mil minutos).

Este trabalho utilizou o "solver", com o método "evolutionary". Como toda e qualquer empresa que participa de leilões visa aumentar o lucro e diminuir gastos, o cálculo da função objetivo baseou-se no custo mínimo para a prestação de serviço, atendendo à restrição que motivou esse trabalho: garantir que o dimensionamento da equipe seja capaz de suportar tal carga de atendimento ininterruptamente.

A tabela abaixo (*TABELA I*) detalha um pouco mais o problema que será resolvido via o tal suplemento do Microsoft Excel:

- Coluna “Cargo” sinaliza quais são os cargos dos funcionários da empresa que prestará o serviço;
- Coluna “Variaveis” – Está dividida em 3 turnos de trabalho, contém efetivamente as variáveis do problema (quantidade de integrantes da equipe);
- Coluna “Nº Funcionarios” - Apenas auxilia na visualização das variáveis do problema concentrando o total de empregados nos 3 turnos, para cada cargo;
- Coluna “Salario Unitario” - É autoexplicativa. Seus valores foram determinados com base no guia salarial da empresa de recrutamento e seleção Robert Half;
- Coluna “Custo Mensal” - Os custos estão representados por um fator multiplicador (1,8) o qual já é de conhecimento da estatal e leva em consideração os diversos itens de formação do custo total para a prestação do serviço (RH; impostos; alimentação; plano de saúde; etc);
- Coluna “Disponibilidade ...” – Representa a disponibilidade teórica do funcionário na mesa de atendimento;
- Coluna “Indisponibilidade ...” – Representa toda e qualquer atividade não relacionada com as demandas da mesa de atendimento (ex: reuniões semanais; descanso por questões de SMS; etc). Note que o preposto não está disponível para atendimento na mesa porque sua atividade principal é fazer a gestão da equipe. Além disso, veja que a indisponibilidade dos analistas mais experientes é maior porque os mesmos participam de reuniões de alinhamento com mais frequência;
- Coluna “Coeficiente ...” – Fator multiplicador que leva em consideração a senioridade do funcionário.  Como as demandas são bastantes similares se comparadas mensalmente, tal coeficiente não influencia na resolução desse problema;
- Coluna “Tempo Efetivo ...” – Informa o tempo efetivo de cada cargo disponível para atendimento das demandas;
- Coluna “Total HH ...” – Contempla o HH efetivo e o somatório de todas as linhas dessa coluna é utilizado como a principal restrição para buscar a solução ótima desse problema específico.


*TABELA I* 
| Cargo	                    | 1º Turno | 2º Turno | 3º Turno | Nº Funcionarios | Salario Unitario (A) | Custo  Mensal 1,8*(A) | Disponibilidade Diária(h) | Indisponibilidade Mesa(h) | Coeficiente | Tempo Efetivo(h) | Total HH |
| :------------------------ | -------- | -------- | -------- | --------------- | -------------------- | --------------------- | ------------------------- | ------------------------- | ----------- | ---------------  | -------: | 
| Preposto                  |     ?    |     ?    |     ?    |         0       |      R$ 9.000,00     |           0           |              8            |             8             |       1     |         0        |     0    |
| Analista Sênior 1(Área 1) |     ?    |     ?    |     ?    |         0       |      R$ 10.300,00    |           0           |              8            |             2,4           |       1     |         5,6      |     0    |
| Analista Pleno 1(Área 1)  |     ?    |     ?    |     ?    |         0       |      R$ 9.300,00     |           0           |              8            |             1,4           |       1     |         6,6      |     0    |
| Analista Sênior 2(Área 2) |     ?    |     ?    |     ?    |         0       |      R$ 10.000,00    |           0           |              8            |             2,4           |       1     |         5,6      |     0    |
| Analista Pleno 2(Área 2)  |     ?    |     ?    |     ?    |         0       |      R$ 9.000,00     |           0           |              8            |             1,4           |       1     |         6,6      |     0    |
| Analista Sênior 3(Área 3) |     ?    |     ?    |     ?    |         0       |      R$ 7.800,00     |           0           |              8            |             2,4           |       1     |         5,6      |     0    |
| Analista Pleno 3(Área 3)  |     ?    |     ?    |     ?    |         0       |      R$ 6.800,00     |           0           |              8            |             1,4           |       1     |         6,6      |     0    |
| Analista Sênior 4(Área 4) |     ?    |     ?    |     ?    |         0       |      R$ 7.650,00     |           0           |              8            |             2,4           |       1     |         5,6      |     0    |
| Analista Pleno 4(Área 4)  |     ?    |     ?    |     ?    |         0       |      R$ 6.650,00     |           0           |              8            |             1,4           |       1     |         6,6      |     0    |
| Analista Sênior 5(Área 5) |     ?    |     ?    |     ?    |         0       |      R$ 7.5500,00    |           0           |              8            |             2,4           |       1     |         5,6      |     0    |
| Analista Pleno 5(Área 5)  |     ?    |     ?    |     ?    |         0       |      R$ 6.550,00     |           0           |              8            |             1,4           |       1     |         6,6      |     0    |


### 2. Modelagem

2.1 – Identificação dos Parâmetros

- Problema: Dimensionar equipe mínima para suportar carga mensal de trabalho;
- Restrições: A equipe deve ser capaz de suportar uma carga mensal de demandas equivalente à 255.395 minutos de atendimento ininterrupto. Apesar de não ser o foco do trabalho, foram aplicadas outras restrições para uma melhor distribuição dos empregados nos 3 turnos:
	- Total Analista - Turno 1  <= 4
	- Total Analista - Turno 1  >  0
	- Total Analista - Turno 3  >  Total Analista - Turno 1
	- Total Analista - Turno 3  <= 6
	- Preposto >= 1
	- Preposto <  4
	- Total Analista Sênior 1   >= 2
	- Total Analista Sênior 2   >= 2
	- Total Analista Sênior 3   >= 2
	- Total Analista Sênior 4   >= 2
	- Total Analista Sênior 5   >= 2
	- Total Analista Pleno 1 > Total Analista Senior 1
	- Total Analista Pleno 2 > Total Analista Senior 2
	- Total Analista Pleno 3 > Total Analista Senior 3
	- Total Analista Pleno 4 > Total Analista Senior 4
	- Total Analista Pleno 5 > Total Analista Senior 5
	- Total Analista Senior no 1º Turno = 0
	- Tempo total para execução das atividades (minutos) >= 255.395
- Espaço de Busca: Limitado a 100 para cada solução do problema;
- Variáveis: Conforme sinalizado na tabela *TABELA I*, corresponde ao número de empregados para cada cargo, distribuídos entre os turnos de trabalho;
- Função Objetivo: Menor Custo para formação da Equipe.

2.2 – Alimentação do otimizador  e execução do solver 

- O otimizador foi efetivamente alimentado com os parâmetros identificados no passo anterior;
- O solver foi executado algumas dezenas de vezes, utilizando o método evolutionary com a seguinte configuração:
	- Convergência: 0,0002
	- Taxa de Mutação: 0,07
	- Tamanho da População: 100
	- Propagação Aleatória: 0
	- Tempo máximo sem aperfeiçoamento: 30

### 3. Resultados

-  Mesmo sabendo que para conseguir uma rápida convergência, o ideal é começar com valores de variáveis atendendo as restrições, todas as variáveis foram iniciadas com valor igual a zero. Ainda assim, houve uma convergência rápida para a maioria das variáveis. Com 3 execuções, apenas as variáveis “Analista Pleno 4” e “Analista Pleno 5” tinham espaço para obter indivíduos melhores, fazendo com que o custo total, com uma equipe de 32 integrantes, chegasse a R$ 450.540,00. A solução otimizada obtida na 3ª execução foi a seguinte:

| Cargo	                    | 1º Turno | 2º Turno | 3º Turno | Nº Funcionarios | Salario Unitario (A) | Custo  Mensal 1,8*(A) | 
| :------------------------ | -------- | -------- | -------- | --------------- | -------------------- | --------------------: | 
| Preposto                  |     0    |     0    |     1    |         1       |      R$ 9.000,00     |      R$ 16.200,00     | 
| Analista Sênior 1(Área 1) |     0    |     2    |     0    |         2       |      R$ 10.300,00    |      R$ 37.080,00     |
| Analista Pleno 1(Área 1)  |     0    |     3    |     0    |         3       |      R$ 9.300,00     |      R$ 50.220,00     |
| Analista Sênior 2(Área 2) |     0    |     2    |     0    |         2       |      R$ 10.000,00    |      R$ 36.000,00     | 
| Analista Pleno 2(Área 2)  |     0    |     2    |     1    |         3       |      R$ 9.000,00     |      R$ 48.600,00     | 
| Analista Sênior 3(Área 3) |     0    |     2    |     0    |         2       |      R$ 7.800,00     |      R$ 28.080,00     |
| Analista Pleno 3(Área 3)  |     3    |     0    |     0    |         3       |      R$ 6.800,00     |      R$ 36.720,00     |
| Analista Sênior 4(Área 4) |     0    |     0    |     2    |         2       |      R$ 7.650,00     |      R$ 27.540,00     |
| Analista Pleno 4(Área 4)  |     0    |     3    |     0    |         3       |      R$ 6.650,00     |      R$ 35.910,00     |
| Analista Sênior 5(Área 5) |     0    |     2    |     0    |         2       |      R$ 7.5500,00    |      R$ 27.180,00     |
| Analista Pleno 5(Área 5)  |     0    |     7    |     2    |         9       |      R$ 6.550,00     |      R$ 106.110,00    |

- A partir da 4ª rodada, a otimização obtida ocorria apenas utilizando as variáveis “Analista Pleno 4” e “Analista Pleno 5”;
- Na 7ª rodada foi alcançado o custo de R$ 450.360 ( Analista Pleno 4 = 7 e Analista Pleno 5 = 5);
- Na 8ª rodada foi alcançado o custo de R$ 450.180 ( Analista Pleno 4 = 6 e Analista Pleno 5 = 6;
- Nas rodadas seguintes, houve evolução apenas na 13ª, 14ª e 17ª rodadas. Posteriormente ainda foram executados mais 13 ciclos de otimização (fechando em 30 execuções realizadas ininterruptamente sem nenhum modificação manual) sem qualquer evolução do resultado ao comparar com a 17ª rodada;
- Apenas para entender o comportamento do solver após ter alcançado aparente solução global, o valor unitário do salário da variável *Analista Pleno 4* foi alterado de tal forma a ficar menor que o salarial unitário da variável * Analista Pleno 5* (o salário do profissional passou a ser R$ 6.350,00). Em seguida, o 31º ciclo de otimização foi iniciado e nada de especial aconteceu.  Porém, o solver conseguiu perceber tal mudança na 33ª rodada, aumentando a quantidade de analistas Pleno 4  para 4 e diminuindo a quantidade de analistas Pleno 5 para 8 integrantes (Custo Total = R$ 447.660 e 32 integrantes). Na rodada seguinte, o otimizador encontrou uma solução mais otimizada ainda alterando as mesmas variáveis na mesma proporção. As 16 rodadas seguintes não mostraram nenhuma evolução fazendo com que o 34º ciclo passasse a ser uma possível solução global(Custo Total = R$ 447.300 e 32 integrantes).

### 4. Conclusões

Como a finalidade é encontrar o tamanho mínimo de equipe para atender uma carga mensal de trabalho, e as soluções obtidas nos 50 ciclos de otimização sempre formavam equipes com 32 integrantes, é possível que o resultado com a melhor solução tenha sido alcançado. O resultado final (com a obtenção de uma solução ótima e aparentemente global e sem considerar as rodadas após o 30º ciclo) foi o seguinte: Custo Mensal mínimo de R$ 449.640,00 com a formação de uma equipe com 32 integrantes.

Vale ressaltar que esse trabalho tem espaço para avançar. O trabalho de gestão realizado pelo preposto é bastante desgastante porque o mesmo participa de muitas reuniões e/ou salas de guerra e, muitas das vezes, não consegue dar a atenção necessária para as demandas que chegam na mesas. Dessa forma, uma melhoria plausível para alcançar o tamanho ótimo da equipe poderia ser listar mais restrições para o preposto pois dependendo da quantidade de demandas que chegam e do tamanho da equipe, pode ser necessário que haja mais que um preposto.

Outro ponto que pode ser explorado futuramente é o aproveitamento desse trabalho para outras finalidades. Um exemplo claro é se as empresas candidatas à prestação de serviço quisessem verificar a viabilidade do leilão ao determinar o lance mínimo. O mesmo seria o valor limite a partir do qual deve-se desistir do leilão devido a inviabilidade econômica.

---

Matrícula: 192.671.121

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação *Business Intelligence Master*

