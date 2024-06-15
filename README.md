# Sentimental-Analysis-in-R-Mental-Health-Tweets-2023
install.packages("syuzhet")
library(syuzhet)
install.packages("lubridate")
library(lubridate)
install.packages("scales")
library(scales)
install.packages("reshape2")
library(reshape2)
#parshub
library(ggplot)
library(dplyr)

tweets<-read.csv(file.choose(),header = T)
tweets<-iconv(tweets$tweet,to="utf-8")
#obtain sentimnets table
#most common nrc dictionary
s<-get_nrc_sentiment(tweets)
tweets[5]
get_nrc_sentiment("guilty")
get_nrc_sentiment("depressed")
#subtract positive -negative
# toget on scale positive - neg
s %>% 
  mutate(sentiment1=positive-negative,
         sentiment2=(positive-negative)/(positive+negative)) %>% head()
#or
head(get_sentiment(tweets,method="nrc"))
get_sentiment(tweets[5],method="syuzhet")
get_sentiment(tweets[5],method="bing")
#valence

#barplot
barplot(colSums(s),
        las=2,
        col=rainbow(10),
        ylab ="Count",
        main="Sentiment count for Mental Health Tweets in 2023")

#score
t<-get_sentiment(tweets,method="syuzhet")
data<-data.frame(Tweet=tweets,Sentiment_Score=t)
ggplot(data,aes(x=Sentiment_Score))+
         geom_histogram(binwidth = 0.5,fill="skyblue",color="black")+
  labs(title="Distrinutioon of Sentimental Score",x="Sentiment_Score",y="Frequency")

#Package
install.packages("sentimentr")
library(sentimentr)
sentimentr::sentiment_by(tweets[5])

a<-get_sentences(tweets)
b<-sentimentr::sentiment(a) 
c<-sentimentr::sentiment_by(a)
