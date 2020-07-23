# R_Sentiment_Analysis
Sentiment analysis in R on the Donald Trump's tweets.

## Number of tweets per year and median of retweets
<img src=img/number_tweets_retweets.JPG>

Code: 
```javascript
    ggplot(lbc) +
    geom_bar(aes(x=factor(year), y=id_str, fill = "Nombre\n de tweets"), stat="identity") + 
    geom_line(aes(x=factor(year), y=0.5*retweet_count, group = 1, color = "Mediane\n de retweets")) +
    scale_color_manual(NULL, values = "darkblue") + 
    geom_point(aes(x = factor(year), y = 0.5*retweet_count, group = 1, color = "Mediane\n de retweets")) +
    scale_y_continuous(sec.axis = sec_axis(~./0.5, name = "Médiane du nombre des retweets")) +
    labs(title = "Nombre de tweets vs médiane des retweets par année",
       x = "Année",
       y = "Nombre de tweets",
       fill = " ") +
    scale_fill_brewer(palette="Accent") 
```

## Proportion of negative and positive tweets before and during presidency
<img src=img/sentiment_proportion.JPG>

Code: 
```javascript
ggplot(df3ans, aes(x=2, y = id_str, fill=sentiment)) +
  geom_bar(stat = "identity", position = "fill", alpha = 0.8) +
  xlim(0.5, 2.5) +
  facet_grid(.~ periode) +
  coord_polar(theta = "y") + 
  labs(title = "Proportion des sentiments des tweets avant et pendant la présidence",
       fill = "Sentiment")
```
## Perceptual map for 5 analysed entities
<img src=img/positioning_map.JPG>

Code: 
```javascript
# Préparation des données
percept<- merge(aggregate(id_str ~ entites_q3, data = q3df[q3df$periode %in% c("2017-2019"),], FUN = "count.unique"),
                aggregate(score ~ entites_q3, data = q3df[q3df$periode %in% c("2017-2019"),], FUN = "mean"),
                by = "entites_q3")
percept

# Le graphique
ggplot(percept, aes(x=id_str, y=score)) +
  geom_point(aes(color = entites_q3), alpha = 0.7, shape = 18, size = 5) +
  annotate(geom = "text", x = percept$id_str, y = percept$score, label = percept$entites_q3, vjust = 3.4,
           xmin = 0, xmax = 800, ymin = -0.35, ymax = 0.05)+
  geom_point(aes(color = entites_q3), alpha = 0.2, shape = 18, size = 23) +
  labs(title = "Carte perceptuelle, 2017-2019",
       x = "Nombre de tweets",
       y = "Score moyen",
       color = "Entités") +
  theme_custom +
  theme_custom + 
  theme(legend.position = "none")
```

## Distribution of favorite tweets 
<img src=img/favorite_tweets.JPG>

Code: 
```javascript
q3df2017_2019<- q3df[(q3df$year %in% c("2017", "2018", "2019")),]

ggplot(q3df2017_2019, 
       aes(x=entites_q3, y=favorite_count, fill = entites_q3)) + 
  geom_boxplot(alpha = 0.6) + 
  geom_jitter(alpha = 0.15) +
  coord_flip() +
  labs(title = "Distribution de tweets favoris par entité: 2017 - 2019",
       x = "Entité",
       y = "Nombre de favorites",
       fill = "Entité")
```

## Number of tweets per weekday 
<img src=img/weekdays.JPG>

Code: 
```javascript
# Préparation des données
jsem_2017_2019 <- aggregate(id_str ~ joursemaine + entites_q3 + sent3, data = q3df[q3df$periode %in% c("2017-2019"),], 
                            FUN = length)

# Le graphique
ggplot(jsem_2017_2019[jsem_2017_2019$entites_q3 != "Putin",], aes(x=factor(joursemaine), y=id_str, fill = entites_q3)) +
  geom_bar(stat = "identity", alpha = 0.9) + 
  facet_wrap(~ entites_q3) +
  labs(title = "Nombre de tweets par jour de la semaine (2017-2019)*,
       x = "Jour de la semaine",
       y = "Nombre de tweets",
       fill = "Entités")
```
