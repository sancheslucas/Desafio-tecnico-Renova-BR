# Desafio-tecnico-Renova-BR-Lucas
Este repositório contém as bases de dados em formato Excel, um README em formato pdf com todos os passo a passos de como desenvolvi o desafio e quais ferramentas utilizei e os resultados encontrados.

Este é mais um README como foi solicitado, a única diferença é que não possui os resultados em si (apenas o pdf possui), apenas meus pensamentos e códigos.

# Uso do Excel
O primeiro passo foi escolher a ferramenta para tratar os dados. Escolhi o Excel e o decidi eliminar/deletar colunas que não seriam interessantes para a análise.
A maioria das colunas deletadas se deve por serem "duplicadas", como por exemplo o código (CD) que representa alguma descrição (DS) de outra coluna, foi preferível excluir a coluna de código e manter a descrição, uma vez que facilitaria a visualização.
As outras variáveis que não eram desse tipo, acabaram sendo excluídas por serem redundantes, ou nocivas para uma possível análise futura (maiores explicações no README em pdf).

Ainda tratando as bases, foi necessário formatar todas as células de cada coluna separadamente, garantindo que o valor contido na célula tenha esse mesmo formato. Por exemplo, se o valor é uma variável quantitativa discreta (como votos) a coluna toda deve ser colocada em formato de número.
Se o valor da coluna representa uma variável qualitativa nominal (como nome dos candidatos), a coluna deve ser formatada para texto. E assim sucessivamente.
O desafio propôs a análise do estado de São Paulo e seus municípios, então na base do perfil do eleitorado utilizei a filtragem do Excel na coluna SG_UF para manter apenas os dados de interesse.
Percebi que a coluna SG_PARTIDO possuia valores com caracteres especiais (#NULO#), tive que mudá-los para não prejudicar o código de programação e melhorar a visualização da base.

Para isso selecionei a coluna e fiz uso da ferramenta localizar e substituir, para alterar todas as células de uma vez para "NULO". Já com essa ferramenta podemos observar o somatório de votos NULOS.
Fiz o mesmo para a variável NM_VOTAVEL, substituí "Branco" para "BRANCO" a fim de padronizar toda a base.
Tais colunas como NM_VOTAVEL possuem esse voto em branco, porém não julguei sendo uma variável que devemos excluir a linha correspondente, pois ela pode trazer insights futuros.

Mantive a coluna NR_ZONA pois poderia ser interessante uma análise espacial usando geoestatística, para verificar através de um gráfico de mapa como ficou a quantidade de votos por região. Porém para isso, iriamos precisar da localidade que cada zona representa.

Através do comando "ctrl+shift+space" selecionei todos os dados, na aba "inserir" selecionei "tabela" a fim de solucionar o segundo tópico proposto. Tais tabelas com os dados tratados estão disponíveis no repositório em formato xlsx.

# Uso de programação (R e Python)
A partir do tópico pedido (um JOIN), comecei a realizar as análises dentro do software estatístico R ao invés de Python, isso porque as linguagens são extremamente parecidas. O R é focado em análise de dados e métodos estatísticos, essencial para dados eleitorais e análise de dados, com uma única desvantagem de tempo que leva para rodar alguns códigos ou bases. Porém ainda sim, forneci o respectivo código em linguagem Python no final deste README e do pdf.

Vou explicar agora meus pensamentos para o resto da análise e em seguida colocarei os códigos comentados para reprodução.
Através do JOIN fiz duas tabelas, uma que representa qual candidato foi mais votado em cada município, e outra qual município o candidato X foi mais votado.
Sobre qual perfil do eleitorado votou em cada candidato eu optei por não fazer, uma vez que estatísticamente o resultado final do JOIN poderia ser tendencioso e incorreto, mais explicações no arquivo pdf.
Talvez a parte de montagem da base de dados e coleta de informações poderia ter sido planejada e executada de forma mais eficiente.

Para não finalizarmos aqui, decidi seguir a sugestão do desafio e tentar trazer novas ideias e insights, utilizando de uma análise exploratória de dados e métodos estatísticos.
Através do RStudio decidi melhorar ainda mais a tabela da base do perfil, separando por municípios, faixas de cada variável e suas respectivas contagens. Desta forma podemos entender ainda melhor as respectivas quantidades dos eleitores em cada faixa no estado de São Paulo.

Fiz uma análise geral sobre todos os candidatos de todos os municípios, a fim de encontrar suas colocações. Representei através de um gráfico de dispersão e depois um gráfico de linhas suavizado para melhor visualização.

Agora calculando a porcentagem de votos em cada partido, decidi representar os valores através de um histograma e verificar a disparidade entre partidos.

Penso que geralmente estaremos interessados em um município específico, por exemplo Campinas, São Paulo, etc. Então separei o município de interesse (São Paulo) como exemplo, e fiz uma análise dentro dele, selecionando em uma tabela os 3 prefeitos mais votados e outra com os 3 vereadores mais votados, ambas com uma coluna adicional contendo o total de votos de cada um naquele município.
Para melhor visualizção realizei um gráfico de barras, e um de pizza (apesar de nunca ser recomendado o  uso deste tipo de gráfico) para as duas situações.

Seguindo a mesma lógica, criei agora as mesmas colunas porém relacionado a todos os candidatos da cidade de interesse, novamente separado entre prefeito e vereador, plotei as tabelas obtidas com o somatório dos votos de cada candidato e para visualização optei por um gráfico de dispersão e depois de radar.
O gráfico de radar e sua interpretação foi melhor explicado no pdf, decidi o usar pois podemos verificar qual candidato se destaca mais na área de interesse.

Por fim analisei a base de perfil de eleitorado, a fim de plotar e verificar a distribuição de cada característica dos eleitores, por exemplo, verificar a distribuição do gênero e do estado civil. Para isso tive que separar cada coluna de variável e plotar em um histograma com todas as faixas de interesse.
Consegui observar que os dados aparentemente seguem uma ditribuição normal ou algo muito aproximado, seria interessante em uma análise futura verificar a normalidade dos dados usando testes estatísticos que descrevi no pdf (como Jarque-Bera entre outros), e se comprovarmos tal fato através do p-valor, podemos utilizar metodologias mais profundas ou partir para análises não paramétricas se a normalidade não se verificar, criando modelos de previsão para o segundo turno, modelando resultados e realizando testes de hipóteses.
Outra sugestão seria verificar a correlação entre variáveis(lembrando que não implicam em causalidade), e verificar a assimetria e curtose das distribuições encontradas.

# Códigos comentados para replicação (R e Python)
## R

#Bibliotecas que iremos utilizar, todas devem ser instaladas usando o comando install.packages("")

library(dplyr)
library(readxl)
library(ggplot2)

#Insereindo os dados tratados no excel

perfil_eleitorado_reduzido_sp_final <- read_excel("C:/Users/romba/OneDrive/Área de Trabalho/perfil eleitorado reduzido sp final.xlsx")

View(perfil_eleitorado_reduzido_sp_final)

SP_turno_1_reduzido_final <- read_excel("C:/Users/romba/OneDrive/Área de Trabalho/SP_turno_1-reduzido final.xlsx")

View(SP_turno_1_reduzido_final)


#--------------------------------------------------------------------------------------------------

#Qual candidato foi mais votado em cada município

#Filtrando os cargos 

dados_prefeito <- SP_turno_1_reduzido_final %>%
  filter(DS_CARGO_PERGUNTA == "Prefeito")

dados_vereador <- SP_turno_1_reduzido_final %>%
  filter(DS_CARGO_PERGUNTA == "Vereador") 

#Obtendo os candidatos mais votados em cada município

pref_mais_vot <- dados_prefeito %>%
  group_by(NM_MUNICIPIO) %>%
  summarize(Prefeito_mais_votado = NM_VOTAVEL[which.max(QT_VOTOS)])

vere_mais_vot <- dados_vereador %>%
  group_by(NM_MUNICIPIO) %>%
  summarize(Vereador_mais_votado = NM_VOTAVEL[which.max(QT_VOTOS)])

#Join dos dados

dados_juntados <- left_join(dados_prefeito, pref_mais_vot, by = "NM_MUNICIPIO")
dados_juntados <- left_join(dados_juntados, vere_mais_vot, by = "NM_MUNICIPIO")

View(dados_juntados)

resultado_final <- dados_juntados %>%
group_by(NM_MUNICIPIO) %>%
summarize(Prefeito_mais_votado = first(Prefeito_mais_votado),
          Vereador_mais_votado = first(Vereador_mais_votado))


#Mostrando a tabela

print(resultado_final,n=nrow(resultado_final))

View(resultado_final)

#--------------------------------------------------------------------------------------------

#Qual município o candidato foi mais votado

#Separando infos dos candidatos

cand_mun_mais_voto <- bind_rows(dados_prefeito, dados_vereador) %>%
group_by(NM_VOTAVEL) %>%
summarize(Cargo = first(DS_CARGO_PERGUNTA), 
            Cidade_Mais_Votado = NM_MUNICIPIO[which.max(QT_VOTOS)],
            Qt_De_Votos = max(QT_VOTOS))

#Mostrando o resultado

print(cand_mun_mais_voto)

View(cand_mun_mais_voto)

#---------------------------------------------------------------------------------------------

#Melhor visualização dos perfis

base1_consolidada <- SP_turno_1_reduzido_final %>%
  distinct(NM_MUNICIPIO, .keep_all = TRUE)
  
View(base1_consolidada)

base2_consolidada <- perfil_eleitorado_reduzido_sp_final %>%
  group_by(NM_MUNICIPIO, DS_FAIXA_ETARIA) %>%
  summarize(Soma_QT_ELEITORES_PERFIL = sum(QT_ELEITORES_PERFIL),
            Soma_QT_ELEITORES_DEFICIENCIA = sum(QT_ELEITORES_DEFICIENCIA),
            Estados_Civis_Unicos = paste(unique(DS_ESTADO_CIVIL), collapse = ", "),
            Contagem_Estados_Civis = paste(table(DS_ESTADO_CIVIL), collapse = ", "),
            Generos_Unicos = paste(unique(DS_GENERO), collapse = ", "),
            Contagem_Generos = paste(table(DS_GENERO), collapse = ", "),
            Soma_QT_ELEITORES_INC_NM_SOCIAL = sum(QT_ELEITORES_INC_NM_SOCIAL))
            
View(base2_consolidada)

print(base2_consolidada )

#--------------------------------------------------------------------------------------

#Análises gerais

descricao_geral <- cand_mun_mais_voto %>%
  mutate(Qt_De_Votos = as.numeric(Qt_De_Votos)) %>%  
  arrange(desc(Qt_De_Votos))

#Criando uma coluna para a posição dos candidatos

descricao_geral <- descricao_geral %>%
  mutate(Posicao = seq_along(NM_VOTAVEL))

#Gráfico de Dispersão

grafico_dispersao <- ggplot(descricao_geral, aes(x = Posicao, y = Qt_De_Votos, label = NM_VOTAVEL)) +
  geom_point(color = "blue") +
  geom_text(hjust = 1.2, vjust = 0) +
  labs(title = "Relação entre número de votos e posição do candidato",
       x = "Posição do candidato",
       y = "Total de votos") +
  theme_minimal()

#Mostrando o gráfico de dispersão

print(grafico_dispersao)

#A visualização não ficou legal, iremos tranformar em gráfico de linhas

#Gráfico de Linhas

grafico_linhas <- ggplot(descricao_geral, aes(x = Posicao, y = Qt_De_Votos)) +
  geom_line(color = "blue") +
  labs(title = "Variação dos votos ao longo da contagem de candidatos",
       x = "Contagem de candidatos",
       y = "Total de votos") +
  theme_minimal()

#Mostrando o gráfico de linhas

print(grafico_linhas)

#---------------------------------------------------------------------------------------------

#Análise dos partidos mais votados

partidos_mais_votados <- SP_turno_1_reduzido_final %>%
  group_by(SG_PARTIDO) %>%
  summarize(Total_Votos = sum(QT_VOTOS)) %>%
  arrange(desc(Total_Votos)) %>%
  mutate(Porcentagem_Votos = (Total_Votos / sum(Total_Votos)) * 100)

#Mostrando os partidos mais votados em ordem decrescente

print(partidos_mais_votados,n = nrow(partidos_mais_votados) )

#Criando um histograma para os partidos

hist_partidos <- ggplot(partidos_mais_votados, aes(x = SG_PARTIDO, y = Porcentagem_Votos)) +
  geom_bar(stat = "identity", fill = "blue") + 
  labs(title = "Partidos mais votados", x = "Partido", y = "Porcentagem de votos") +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1),
        plot.title = element_text(hjust = 0.5),  
        panel.grid.major = element_blank(),  
        panel.grid.minor = element_blank(), 
        axis.line = element_line(color = "black"),  
        axis.title = element_text(size = 10, face = "bold"),  
        axis.text = element_text(size = 10),  
        legend.title = element_blank(),  
        legend.text = element_text(size = 10))  


#Mostrando o histograma

print(hist_partidos)

#------------------------------------------------------------------------------------------

#Análises de principais cidades de interesse

#Comparando os resultados dos 3 mais votados de sp

dados_vereador_sp <- dados_vereador %>%
  filter(NM_MUNICIPIO == "SÃO PAULO") %>%
  filter(NM_VOTAVEL != "NULO" & NM_VOTAVEL != "BRANCO") #Retirando branco e nulo pois são os que mais votaram


#Obtendo os candidatos mais votados em São Paulo para prefeito

pref_mais_vot_sp <- dados_prefeito_sp %>%
  group_by(NM_VOTAVEL) %>%
  summarize(Total_Votos = sum(QT_VOTOS)) %>%
  top_n(3, Total_Votos) %>%
  arrange(desc(Total_Votos))

#Obtendo os candidatos mais votados em São Paulo para vereador

vere_mais_vot_sp <- dados_vereador_sp %>%
  group_by(NM_VOTAVEL) %>%
  summarize(Total_Votos = sum(QT_VOTOS)) %>%
  top_n(3, Total_Votos) %>%
  arrange(desc(Total_Votos))

#Visualizando a tabela de São Paulo para candidatos

print(pref_mais_vot_sp)

print(vere_mais_vot_sp)

#Tornando a variável numérica

descricao_vereador_sp <- vere_mais_vot_sp %>%
  mutate(Total_Votos = as.numeric(Total_Votos)) %>%  
  arrange(desc(Total_Votos))

descricao_prefeito_sp <- pref_mais_vot_sp %>%
  mutate(Total_Votos = as.numeric(Total_Votos)) %>%  
  arrange(desc(Total_Votos))

#Tema dos gráficos, padronizando os gráficos a seguir

tema_personalizado <- theme_minimal() +
  theme(axis.title.x = element_blank(),
        axis.title.y = element_blank(),
        plot.title = element_text(size = 16, face = "bold"),
        legend.position = "right")

#Gráfico de barras vereadores

grafico_barras_vere <- ggplot(descricao_vereador_sp, aes(x = reorder(NM_VOTAVEL, Total_Votos), y = Total_Votos)) +
  geom_bar(stat = "identity", fill = "blue") +
  coord_flip() +
  labs(title = "Vereadores mais votados em São Paulo",
       x = "Vereador",
       y = "Total de votos") +
  tema_personalizado

#Gráfico de pizza vereadores

grafico_pizza_vere <- ggplot(descricao_vereador_sp, aes(x = "", y = Total_Votos, fill = NM_VOTAVEL)) +
  geom_bar(stat = "identity", width = 1) +
  coord_polar("y", start = 0) +
  labs(title = "TOP 3 vereadores mais votados") +
  tema_personalizado

#Gráfico de barras prefeitos

grafico_barras_pref <- ggplot(descricao_prefeito_sp, aes(x = reorder(NM_VOTAVEL, Total_Votos), y = Total_Votos)) +
  geom_bar(stat = "identity", fill = "blue") +
  coord_flip() +
  labs(title = "Prefeitos mais votados em São Paulo",
       x = "Prefeito",
       y = "Total de votos") +
  tema_personalizado

#Gráfico de pizza para prefeitos

grafico_pizza_pref <- ggplot(descricao_prefeito_sp, aes(x = "", y = Total_Votos, fill = NM_VOTAVEL)) +
  geom_bar(stat = "identity", width = 1) +
  coord_polar("y", start = 0) +
  labs(title = "TOP 3 prefeitos mais votados") +
  tema_personalizado

#Mostrando os gráficos

plot(grafico_barras_vere)

plot(grafico_pizza_vere)

plot(grafico_barras_pref)

plot(grafico_pizza_pref) 

#Análise geral do estado de sp

pref_geral<-dados_prefeito_sp %>%
  group_by(NM_VOTAVEL) %>%
  summarize(Total_Votos = sum(QT_VOTOS))

vere_geral<-dados_vereador_sp %>%
  group_by(NM_VOTAVEL) %>%
  summarize(Total_Votos = sum(QT_VOTOS))


#Dados dos prefeitos, geral

pref_geral <- dados_prefeito_sp %>%
  group_by(NM_VOTAVEL) %>%
  summarize(Total_Votos = sum(QT_VOTOS)) %>%
  arrange(desc(Total_Votos))

#Dados dos vereadores, geral

vere_geral <- dados_vereador_sp %>%
  filter(NM_VOTAVEL != "NULO" & NM_VOTAVEL != "BRANCO") %>%
  group_by(NM_VOTAVEL) %>%
  summarize(Total_Votos = sum(QT_VOTOS)) %>%
  arrange(desc(Total_Votos))


#Gráfico de Dispersão para Prefeito

grafico_dispersao_pref <- ggplot(pref_geral, aes(x = seq_along(NM_VOTAVEL), y = Total_Votos)) +
  geom_point(color = "green", alpha = 0.6) +
  geom_text(aes(label = ifelse(Total_Votos > max(Total_Votos) * 0.02, NM_VOTAVEL, "")), 
            hjust = 1.2, vjust = 0.2, size = 3) +
  labs(title = "Relação entre número de votos e posição do candidato para prefeito",
       x = "Posição do candidato",
       y = "Total de votos") +
  theme_minimal() +
  theme(legend.position = "bottom") +
  guides(color = guide_legend(title = "Candidatos"))

#Gráfico de Dispersão para Vereador

grafico_dispersao_vere <- ggplot(vere_geral, aes(x = seq_along(NM_VOTAVEL), y = Total_Votos)) +
  geom_point(color = "magenta", alpha = 0.6) +
  geom_text(aes(label = ifelse(Total_Votos > max(Total_Votos) * 0.02, NM_VOTAVEL, "")), 
            hjust = 1.2, vjust = 0.2, size = 3) +
  labs(title = "Relação entre número de votos e posição do candidato para vereador",
       x = "Posição do candidato",
       y = "Total de votos") +
  theme_minimal() +
  theme(legend.position = "bottom") +
  guides(color = guide_legend(title = "Candidatos"))

#Mostrando os gráficos

print(grafico_dispersao_pref)
print(grafico_dispersao_vere)

#Gráfico de Linhas para Prefeito

grafico_linhas_pref <- ggplot(pref_geral, aes(x = seq_along(NM_VOTAVEL), y = Total_Votos)) +
  geom_line(color = "green") +
  labs(title = "Variação dos votos ao longo da contagem de candidatos para prefeito",
       x = "Contagem de candidatos",
       y = "Total de votos") +
  theme_minimal()

#Gráfico de Linhas para Vereador

grafico_linhas_vere <- ggplot(vere_geral, aes(x = seq_along(NM_VOTAVEL), y = Total_Votos)) +
  geom_line(color = "magenta") +
  labs(title = "Variação dos votos ao longo da contagem de candidatos para vereador",
       x = "Contagem de candidatos",
       y = "Total de votos") +
  theme_minimal()

#Mostrando os gráficos

print(grafico_linhas_pref)

print(grafico_linhas_vere)

#Gráfico de Radar para Prefeito

grafico_radar_pref <- ggplot(pref_geral, aes(x = NM_VOTAVEL, y = Total_Votos)) +
  geom_polygon(aes(group = 1), fill = "skyblue", alpha = 0.5) +
  geom_line(aes(group = 1), color = "blue") +
  coord_polar(start = 0) +
  labs(title = "Características dos candidatos para prefeito",
       y = "Total de votos") +
  theme_minimal() +
  theme(axis.title.x = element_blank(),
        axis.text.x = element_blank(),
        axis.ticks.x = element_blank())

#Gráfico de Radar para Vereador

grafico_radar_vere <- ggplot(vere_geral, aes(x = NM_VOTAVEL, y = Total_Votos)) +
  geom_polygon(aes(group = 1), fill = "green", alpha = 0.5) +
  geom_line(aes(group = 1), color = "magenta") +
  coord_polar(start = 0) +
  labs(title = "Características dos candidatos para vereador",
       y = "Total de votos") +
  theme_minimal() +
  theme(axis.title.x = element_blank(),
        axis.text.x = element_blank(),
        axis.ticks.x = element_blank())

#Mostrando os gráficos

print(grafico_radar_pref)

print(grafico_radar_vere)

#-----------------------------------------------------------------------------------------

#Sugestões de análises do banco de perfil do eleitorado

#Filtrando o estado de São Paulo

base_sp <- base2_consolidada %>%
  filter(NM_MUNICIPIO == "SÃO PAULO")

#Porcentagens

base_sp_percentual$Porcentagem <- base_sp %>%
  mutate(Porcentagem = Soma_QT_ELEITORES_PERFIL / sum(Soma_QT_ELEITORES_PERFIL) * 100)

#Tabela com porcentagens

View(base_sp_percentual)

print(base_sp_percentual, n = nrow(base_sp_percentual) )

#Gráfico de barras para Estado Civil

grafico_estado_civil <- ggplot(base_sp_percentual, aes(x = DS_FAIXA_ETARIA, y = Porcentagem, fill = Contagem_Estados_Civis)) +
  geom_bar(stat = "identity") +
  labs(title = "Porcentagem de eleitores por estado civil em São Paulo",
       x = "Faixa etária",
       y = "Porcentagem (%)") +
  theme_minimal() +
  theme(legend.position = "top")

#Gráfico de barras para Gênero

grafico_genero <- ggplot(base_sp_percentual, aes(x = DS_FAIXA_ETARIA, y = Porcentagem, fill = Contagem_Generos)) +
  geom_bar(stat = "identity") +
  labs(title = "Porcentagem de eleitores por gênero em São Paulo",
       x = "Faixa etária",
       y = "Porcentagem (%)") +
  theme_minimal() +
  theme(legend.position = "top")

#Se for do interesse, podemos fazer o mesmo para verificar os eleitores com inclusão de nome social

#Mostrando os gráficos

print(grafico_estado_civil)

print(grafico_genero)

#Os dados aparentemente seguem aparentemente uma distribuição normal

#Seria possível utilizar testes estatísticos para essa comprovação, tais como Shapiro-Wilk, Kolmogorov-Smirnov, Jarque-Bera.

#Se comprovarmos aa normalidade dos dados, outros testes estatísticos podem ser realizados para uma análise mais profunda.

#Se existir alguma tabela feita que interesse estar em formato excel, é possível usar comandos para gerá-las, para isso basta usar a biblioteca library(openxlsx).

#--------------------------------------------------------------------------------------------------------------------------------------------------------

## Python

#Importando o que vai ser necessário

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

#Lendo os dados tratados do Excel

perfil_eleitorado_reduzido_sp_final = pd.read_excel("C:/Users/romba/OneDrive/Área de Trabalho/perfil eleitorado reduzido sp final.xlsx")

SP_turno_1_reduzido_final = pd.read_excel("C:/Users/romba/OneDrive/Área de Trabalho/SP_turno_1-reduzido final.xlsx")

#-----------------------------------------------------------------------------------------------------------------------------

#Qual candidato foi mais votado em cada município

dados_prefeito = SP_turno_1_reduzido_final[SP_turno_1_reduzido_final['DS_CARGO_PERGUNTA'] == "Prefeito"]

dados_vereador = SP_turno_1_reduzido_final[SP_turno_1_reduzido_final['DS_CARGO_PERGUNTA'] == "Vereador"]

pref_mais_vot = dados_prefeito.groupby('NM_MUNICIPIO')['NM_VOTAVEL'].apply(lambda x: x[x.index[x['QT_VOTOS'] == x['QT_VOTOS'].max()][0]])

vere_mais_vot = dados_vereador.groupby('NM_MUNICIPIO')['NM_VOTAVEL'].apply(lambda x: x[x.index[x['QT_VOTOS'] == x['QT_VOTOS'].max()][0]])

dados_juntados = pd.merge(dados_prefeito, pref_mais_vot, on='NM_MUNICIPIO')

dados_juntados = pd.merge(dados_juntados, vere_mais_vot, on='NM_MUNICIPIO')

resultado_final = dados_juntados.groupby('NM_MUNICIPIO').agg(Prefeito_mais_votado=('NM_VOTAVEL_x', 'first'),
                                                              Vereador_mais_votado=('NM_VOTAVEL_y', 'first'))

print(resultado_final)
print("Número de municípios:", resultado_final.shape[0])

#--------------------------------------------------------------------------------------------------------------------------------

#Qual município o candidato foi mais votado

cand_mun_mais_voto = pd.concat([dados_prefeito, dados_vereador], ignore_index=True)

cand_mun_mais_voto = cand_mun_mais_voto.groupby('NM_VOTAVEL').agg(Cargo=('DS_CARGO_PERGUNTA', 'first'),
                                                                  Cidade_Mais_Votado=('NM_MUNICIPIO', lambda x: x[x.index[x['QT_VOTOS'] == x['QT_VOTOS'].max()][0]]),
                                                                  Qt_De_Votos=('QT_VOTOS', 'max'))

print(cand_mun_mais_voto)
print("Número de candidatos:", cand_mun_mais_voto.shape[0])

#---------------------------------------------------------------------------------------------------------------------------------

#Melhor visualização dos perfis

base1_consolidada = SP_turno_1_reduzido_final.drop_duplicates(subset='NM_MUNICIPIO', keep='first')

base2_consolidada = perfil_eleitorado_reduzido_sp_final.groupby(['NM_MUNICIPIO', 'DS_FAIXA_ETARIA']).agg(
    Soma_QT_ELEITORES_PERFIL=('QT_ELEITORES_PERFIL', 'sum'),
    Soma_QT_ELEITORES_DEFICIENCIA=('QT_ELEITORES_DEFICIENCIA', 'sum'),
    Estados_Civis_Unicos=('DS_ESTADO_CIVIL', lambda x: ', '.join(np.unique(x))),
    Contagem_Estados_Civis=('DS_ESTADO_CIVIL', lambda x: ', '.join(map(str, x.value_counts().tolist()))),
    Generos_Unicos=('DS_GENERO', lambda x: ', '.join(np.unique(x))),
    Contagem_Generos=('DS_GENERO', lambda x: ', '.join(map(str, x.value_counts().tolist()))),
    Soma_QT_ELEITORES_INC_NM_SOCIAL=('QT_ELEITORES_INC_NM_SOCIAL', 'sum')).reset_index()

print(base2_consolidada)
print("Número de municípios:", base2_consolidada.shape[0])

#-------------------------------------------------------------------------------------------------------------------------------

#Análises gerais

descricao_geral = cand_mun_mais_voto.copy()
descricao_geral['Qt_De_Votos'] = pd.to_numeric(descricao_geral['Qt_De_Votos'])
descricao_geral = descricao_geral.sort_values(by='Qt_De_Votos', ascending=False)
descricao_geral['Posicao'] = np.arange(1, descricao_geral.shape[0] + 1)

#Gráfico de Dispersão

plt.figure(figsize=(10, 6))
sns.scatterplot(data=descricao_geral, x='Posicao', y='Qt_De_Votos', label='NM_VOTAVEL', color='blue')
plt.title("Relação entre número de votos e posição do candidato")
plt.xlabel("Posição do candidato")
plt.ylabel("Total de votos")
plt.legend()
plt.show()

#Gráfico de Linhas

plt.figure(figsize=(10, 6))
sns.lineplot(data=descricao_geral, x='Posicao', y='Qt_De_Votos', color='blue')
plt.title("Variação dos votos ao longo da contagem de candidatos")
plt.xlabel("Contagem de candidatos")
plt.ylabel("Total de votos")
plt.show()

#----------------------------------------------------------------------------------------------------------------------------

#Análise dos partidos mais votados

partidos_mais_votados = SP_turno_1_reduzido_final.groupby('SG_PARTIDO')['QT_VOTOS'].sum().reset_index()
partidos_mais_votados = partidos_mais_votados.sort_values(by='QT_VOTOS', ascending=False)
partidos_mais_votados['Porcentagem_Votos'] = (partidos_mais_votados['QT_VOTOS'] / partidos_mais_votados['QT_VOTOS'].sum()) * 100

print(partidos_mais_votados)
print("Número de partidos:", partidos_mais_votados.shape[0])

plt.figure(figsize=(10, 6))
sns.barplot(data=partidos_mais_votados, x='SG_PARTIDO', y='Porcentagem_Votos', color='blue')
plt.title("Partidos mais votados")
plt.xlabel("Partido")
plt.ylabel("Porcentagem de votos (%)")
plt.xticks(rotation=45, ha='right')
plt.show()

#------------------------------------------------------------------------------------------------------------------------------

#Sugestões de análises do banco de perfil do eleitorado

base_sp = base2_consolidada[base2_consolidada['NM_MUNICIPIO'] == "SÃO PAULO"]
base_sp_percentual = base_sp.copy()
base_sp_percentual['Porcentagem'] = (base_sp_percentual['Soma_QT_ELEITORES_PERFIL'] / base_sp_percentual['Soma_QT_ELEITORES_PERFIL'].sum()) * 100

plt.figure(figsize=(10, 6))
sns.barplot(data=base_sp_percentual, x='DS_FAIXA_ETARIA', y='Porcentagem', hue='Contagem_Estados_Civis')
plt.title("Porcentagem de eleitores por estado civil em São Paulo")
plt.xlabel("Faixa etária")
plt.ylabel("Porcentagem (%)")
plt.legend(title="Estado Civil")
plt.xticks(rotation=45, ha='right')
plt.show()

plt.figure(figsize=(10, 6))
sns.barplot(data=base_sp_percentual, x='DS_FAIXA_ETARIA', y='Porcentagem', hue='Contagem_Generos')
plt.title("Porcentagem de eleitores por gênero em São Paulo")
plt.xlabel("Faixa etária")
plt.ylabel("Porcentagem (%)")
plt.legend(title="Gênero")
plt.xticks(rotation=45, ha='right')
plt.show()


