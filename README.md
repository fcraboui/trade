# trade

## TODO

1. [ ] Base de données locale pour stocker les quotes?
    * Pour commencer, faire avec un CSV ou fetcher les données à chaque fois

1. [ ] Récupérer les données Yahoo Finances

    * How?

1. [ ] Check if uptrend:

    * <https://medium.com/analytics-vidhya/super-performance-stocks-how-to-make-more-than-1-125-on-a-single-small-cap-stock-with-python-5ea3ae393791>

1. [ ] Auto Envelope?
    * ou alors Bandes de Bollinger?

1. [ ] Sortir Prix achat, SL, TP

1. [ ] Construire tableau récap sur la semaine et le mois

1. [ ] Panel statistics

## Datamodel

```plantuml
@startuml
    skinparam backgroundColor #EEEBDC
    skinparam handwritten true

    !define table(x) class x << (T,#FFAAAA) >>
    !define primary_key(x) <b><u>x</u></b>
    !define foreign_key(x) <u>x</u>
    hide methods
    hide stereotypes

    table(Ticker) {
        + primary_key(id)
        + name
    }

    table(Quotes) {
        + primary_key(id)
        + foreign_key(ticker_id)
        + time::timestamp
        + open
        + close
        + low
        + high
        + volume
    }

    table(Position) {
        + primary_key(id)
        + foreign_key(ticker_id)
        + foreign_key(buy_deal_id)
        + foreign_key(sell_deal_id)
        + pnl
        + tp
        + sl
    }

    table(Order) {
        + primary_key(id)
        + foreign_key(position_id)
        + foreign_key(ticker_id)
        + time::timestamp
        + price
        + order_type (Buy, Sell)
        + volume
        + total_price
        # Prévoir job pour conserver tous les changements SL, TP, etc.
    }

    table(Statistics) {
        + daily_pnl
        + weekly_pnl
        + monthly_pnl
    }

    Quotes::ticker_id --> Ticker::id
    Position::ticker_id --> Ticker::id
    Position::buy_deal_id --> Order::id : Buy
    Position::sell_deal_id --> Order::id : Sell
    Order::position_id --> Position::id
    Order::ticker_id --> Ticker::id
@enduml
```
