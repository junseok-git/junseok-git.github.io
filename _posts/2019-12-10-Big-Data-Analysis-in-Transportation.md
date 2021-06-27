# 트위터 분석 패키지
#install.packages(c("twitteR", "ROAuth", "base64enc"))
library("twitteR")
library("ROAuth")
library("base64enc")

# 트위터 분석 계정 API
consumerKey <- "VN5FbM6GzIRCZuXpTQuEfHrhu"
consumerSecret <- "2vzuIums0cd4a8AWW0Tv2rE7ekhLctbIHDIRn1SVrXUwgEmZLl"
accessToken <- "165321919-BI4EwuNRCyj84pEQ6dfb6V9gm2Q11OEml1BNvZar"
accessTokenSecret <- "jSXIiq35lXoq0QvwD4KR7xrt6deNxF6qkQy3kMy9gMEJT"

setup_twitter_oauth(consumerKey, consumerSecret, accessToken, accessTokenSecret)

# 조사할 키워드
keyword <- '강남+버스'
keyword <- iconv(keyword, 'CP949', 'UTF8')

system.time(key <- searchTwitter(keyword, n=1000, lang="ko"))

key.df <- twListToDF(key)
text <- unique(key.df$text)
text.df <- as.data.frame(text)
write.table(text, "b2.txt", row.names = F)

b2.txt<-readLines("b2.txt")

positive <- readLines("positive.txt", encoding = "UTF-8")
positive=positive[-1]

negative <- readLines("negative.txt", encoding = "UTF-8")
negative=negative[-1]

sentimental = function(sentences, positive, negative){
  
  scores = laply(sentences, function(sentence, positive, negative) {
    
    sentence = gsub('[[:punct:]]', '', sentence) # 문장부호 제거
    sentence = gsub('[[:cntrl:]]', '', sentence) # 특수문자 제거
    sentence = gsub('\\d+', '', sentence)        # 숫자 제거
    
    word.list = str_split(sentence, '\\s+')      # 공백 기준으로 단어 생성 -> \\s+ : 공백 정규식, +(1개 이상)
    words = unlist(word.list)                    # unlist() : list를 vector 객체로 구조변경
    
    pos.matches = match(words, positive)           # words의 단어를 positive에서 matching
    neg.matches = match(words, negative)
    
    pos.matches = !is.na(pos.matches)            # NA 제거, 위치(숫자)만 추출
    neg.matches = !is.na(neg.matches)
    
    score = sum(pos.matches) - sum(neg.matches)  # 긍정 - 부정   
    return(score)
  }, positive, negative)
  
  scores.df = data.frame(score=scores, text=sentences)
  return(scores.df)
}

result<-sentimental(b2.txt, positive, negative)

result$remark[result$score >=1] = "긍정"

result$remark[result$score ==0] = "중립"

result$remark[result$score < 0] = "부정"

sentiment_result<- table(result$remark)

result_new <- data.frame(table(result$remark))
result_new





########## 토지가격 구하기 ##########
#install.packages("data.tabel")     # 대용량 파일 읽기용
library(data.table)
land<-fread("E:/Google 드라이브/학교 공부용/2학년/빅데이터/텀프로젝트/data/소득/AL_11_D151_20191126/AL_11_D151_20191126.csv")
install.packages('bit64') #오류해결
str(land)
head(land)

library(dplyr)
land_1<-land %>% select(법정동코드, 법정동명, 지번, 기준년도, 공시지가)
year <- land_1 %>% filter(기준년도 == 1990)          # 지번이 계속 추가되기때문에 1990년부터 2019년까지
year2 <- land_1 %>% filter(기준년도 == 2019)          # 계속 남아있는 동, 지번만 뽑아내기 위함

주소<-paste(land_1$법정동명, land_1$지번)
before<-cbind(land_1, 주소)
before<-before[,-c(2,3)]
주소<-paste(year$법정동명, year$지번)
after<-cbind(year, 주소)
after<-after[,-c(2,3)]
주소<-paste(year2$법정동명, year2$지번)
after2<-cbind(year2, 주소)
after2<-after2[,-c(2,3)]

land_price<-merge(before,after,by='주소',all=FALSE)
land_price<-land_price[-c(1:4743),-c(5:7)]
names(land_price)<-c("주소","법정동코드","기준년도","공시지가")
land_price2<-merge(land_price,after2,by='주소')
land_price2<-land_price2[,-c(5:7)]

#---- 이 위까지 1990-2019 모두 있는 공시지가 뽑아내기 ----

ad<-data.frame(do.call('rbind',strsplit(as.character(land_price2$주소),split=' ',fixed=TRUE)))   #주소 띄어쓰기로 분할
land_ad<-cbind(ad,land_price2)
land_ad<-land_ad[,-c(1,3:6)]
names(land_ad)<-c("구","기준년도","공시지가")
land_p<-land_ad %>% group_by(구, 기준년도) %>% summarise(mean(공시지가))
write.csv(land_p, file="2019 구별 평균토지가격.csv", row.names=F)


############ 구별 평당 평균 주택가격 #############
install.packages("data.tabel")
library(data.table)
house<-fread("E:/Google 드라이브/학교 공부용/2학년/빅데이터/텀프로젝트/data/소득/2019년 공동주택 공시가격 정보/2019년 공동주택 공시가격 정보.csv")
install.packages('bit64') #오류해결
str(house)
head(house)

library(dplyr)
seoul <- house %>%  filter(시도 == "서울특별시")   # 서울만 추출
seoul <- seoul %>% mutate(평당가격 = 공시가격 / 전용면적 * 3.3)   # 평당가격 계산
avg <- seoul %>% select(시군구, 평당가격)   # 구, 평당가격 칼럼만 추출
avg <- avg %>% group_by(시군구) %>% summarise(평균가격 = mean(평당가격))  # 구별 평당평균가격 계산
write.csv(avg, file="2019 구별 평균주택가격.csv", row.names=F)



### 공시지가 예측 분석 ###

land<-read.csv("E:/Google 드라이브/학교 공부용/2학년/빅데이터/텀프로젝트/result/연도별 구별 평균토지가격 정리.csv")
house<-read.csv("E:/Google 드라이브/학교 공부용/2학년/빅데이터/텀프로젝트/result/2019 구별 평균주택가격.csv")
result <- data.frame(house[c(-2)]) # 결과정리용 구 이름정리.

# 정규화 식 넣은 함수
normalize <- function(x){
  return ((x-min(x))/(max(x)-min(x)))
}

# 전체 데이터 프레임에 정규화 적용
land_n <- as.data.frame(lapply(land[,2:26],normalize))

library(keras)

######## LSTM #########

G = 25  # 구 이름 순서대로 번호. (쉽게하려고)
N = 30
n = seq(1:N)
step = 2
a = land_n[1:30,G]
a = c(a,replicate(step,tail(a,1)))
x = NULL
y = NULL

# LSTM 양식에 맞게 매트릭스 배열맞추기
for(i in 1:N){
  s = i-1+step
  x = rbind(x,a[i:s])
  y = rbind(y,a[s+1])
}

X = array(x, dim=c(N,step,1))
dim(X)

# LSTM 모델 생성
model = keras_model_sequential()
model %>% 
  layer_lstm(units=128, input_shape=c(step, 1), activation="relu") %>%  
  layer_dense(units=64, activation = "relu") %>% 
  layer_dense(units=32) %>% 
  layer_dense(units=1, activation = "linear")

model %>% compile(loss = 'mse',
                  optimizer = 'adam',
                  metrics = list("mean_absolute_error")
)

model %>% summary()

history <- model %>% fit(X,y, epochs=1, batch_size=1, shuffle = FALSE, verbose=0)
plot(history)

# 예측 수행
y_pred  =  model %>% predict(X)
scores  =  model %>% evaluate(X, y, verbose = 0)
print(scores)

# 시각화 (결과 표출)
year = 1990:2019
plot(year, y, type="l", col="red", lwd=2)
lines(year, y_pred, col="blue",lwd=2)
legend("topleft", legend=c("price-original", "price-predicted"),
       col=c("red", "blue"), lty=1,cex=0.8)

# 예측 데이터 기록
y_pred
result[G,2] <- y_pred[30,1]*land[30,G+1]

names(result) <- c("구", "지가")
write.csv(result, "2020 구별 평균공시지가.csv", row.names=F)
############## Clustring #############

data <- read.csv("E:/Google 드라이브/학교 공부용/2학년/빅데이터/텀프로젝트/data/result.csv", fileEncoding = "euc-kr")
data2 <- data[-c(1)]

library(cluster)

# 클러스터 분석
pam.result <- pam(data2, 5)          # 클러스터 개수 5개
plot(pam.result)

cluster <- pam.result$clustering
medoids <- pam.result$medoids
data3 <- cbind(data, cluster)

write.csv(data3, file="cluster.csv", row.names = F)
