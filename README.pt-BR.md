# ✈️ Flight Deal Finder

🇺🇸 [English](README.md) | 🇧🇷 **Português**

Bot em Python que busca, compara e monitora preços de passagens aéreas, com histórico de preços em banco de dados e alertas pelo Telegram.

**Versão atual:** V1
**Fonte de dados:** [Travelpayouts / Aviasales Data API](https://travelpayouts.github.io/slate/)

## O que ele faz

- **Busca de voos por período flexível:** consulta a API mês a mês e junta todos os resultados dentro de um intervalo de datas.
- **Aeroportos alternativos:** compara vários destinos de uma vez (ex.: Milão por MXP, LIN e BGY).
- **Ranking de custo-benefício:** dá uma nota a cada voo combinando critérios com pesos ajustáveis:

  | Critério | Peso padrão |
  |---|---|
  | Preço | 50% |
  | Duração | 25% |
  | Escalas | 15% |
  | Horário de partida (8h–20h é o ideal) | 10% |

  Dá para trocar os pesos por perfil, por exemplo um perfil "conforto" que prioriza duração e menos escalas.
- **Histórico de preços em SQLite:** cada busca é salva num banco local (tabela `voos`), criando uma base histórica por rota. Os alertas de preço ficam numa tabela própria (`alertas`).
- **Classificação de preço:** compara o preço atual com a média histórica da rota:
  - 🟢 **Oferta excelente:** 15% ou mais abaixo da média
  - 🟡 **Bom preço:** de 5% a 15% abaixo da média
  - ⚪ **Preço normal:** entre 5% abaixo e 10% acima da média
  - 🔴 **Preço alto:** mais de 10% acima da média
- **Detecção de ofertas:** só dispara quando a rota já tem pelo menos 3 registros, para não confiar em médias sem base.

## Bot no Telegram (RECON-1)

| Comando | Função |
|---|---|
| `/start` | Apresenta o bot e mostra como usar |
| `/missao ORIGEM, DESTINO, DD/MM/AAAA, DD/MM/AAAA` | Busca voos no período, envia um GIF de "radar" com a varredura e devolve os 3 melhores do ranking, com link |
| `/alerta ORIGEM, DESTINO, max=VALOR` | Cria um alerta de preço máximo, conferido automaticamente a cada 30 minutos |

Origem e destino aceitam nome de cidade ou código IATA (ex.: `São Paulo` ou `GRU`).

Exemplo:
```
/missao São Paulo, Milão, 15/09/2026, 30/09/2026
```

## Tecnologias

- **Python**
- **Requests** para consumir a API REST
- **Pandas** para tratamento e análise dos dados
- **SQLite** para o histórico de preços (consultas em SQL)
- **Matplotlib** para a animação de radar
- **python-telegram-bot** para o bot e os alertas agendados
- **Google Colab** como ambiente de execução

## Como rodar

1. Abra o notebook no Google Colab.
2. Crie dois tokens:
   - **Travelpayouts:** cadastro gratuito em travelpayouts.com
   - **Telegram:** crie um bot pelo [@BotFather](https://t.me/BotFather)
3. No Colab, adicione os tokens em **Secrets** (ícone de chave na barra lateral) com os nomes:
   - `TRAVELPAYOUTS_TOKEN`
   - `TELEGRAM_TOKEN`
4. Execute as células em ordem. O banco `flight_deal_finder.db` é criado automaticamente na pasta `FlightDealFinder` do seu Google Drive.

> Os tokens nunca ficam no código: são lidos pelos Secrets do Colab (`userdata.get`).

## Próximos passos

- Rodar o bot fora do Colab, em um servidor, para ficar sempre online
- Gráficos de evolução de preço por rota ao longo do tempo
- Ida e volta combinadas e múltiplos passageiros
- Previsão de tendência de preço com base no histórico

## Aviso

Projeto pessoal, com fins de estudo e portfólio. Os preços vêm da API da Travelpayouts/Aviasales e podem mudar até a compra.
