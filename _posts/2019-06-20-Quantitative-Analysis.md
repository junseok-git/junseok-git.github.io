---
title: "Quantitative Analysis"
Publish Date: 2019-06-20 09:00:00
categories: codes
---
# 계량분석 과목의 프로젝트 시 작성한 코드입니다.

___


# 1. 단순선형회귀

## 1.1 data reading

```
daily <- read.csv("E:/TermProject/seoul bike_daily_201806_3.csv", header = T)
summary(daily)
str(daily)
bike <- daily[, -c(3, 5, 8, 9)]    # 필요없는 대여소이름, 성별, 운동량, 탄소량 변수 제거

sum(is.na(bike))
bike_1 <- na.omit(bike)
knitr::opts_chunk$set(echo = TRUE)
```

## 1.2 training / test
```
set.seed(1201)
flag <- sample(c("tr", "te"), size = nrow(bike_1), c(8, 2), replace = T) # 8:2로 training / test set
train <- bike_1[which(flag == "tr"), ]
test <- bike_1[which(flag == "te"), ]
knitr::opts_chunk$set(echo = TRUE)
```

## 1.3 Preprocessing
```
boxplot(train)
b1 <- boxplot(train$이동거리)
out1 <- which(train$이동거리 > b1$stats[5]) # 이동거리 outlier index
train <- train[-out1, ] # 이동거리 outlier 제거
boxplot(train$이동거리)
boxplot(train)
knitr::opts_chunk$set(echo = TRUE)
```

## 1.4 Correlation analysis
```
install.packages("corrplot")
library(corrplot)

c <- cor(train) 
corrplot(c, method = "circle", order = "hclust", addrect = 5)
knitr::opts_chunk$set(echo = TRUE)
```


## 1. 5 Simple linear regression
```
m1 <- lm(이용건수~이동거리, data = train) # y변수: , x변수: sqft_living
summary(m1)

plot(train$이동거리, train$이용건수, pch = 19, cex = 0.5, xlab = "이동거리", ylab = "이용건수") 
abline(m1, col = "red", lwd = 2) # 예측 회귀선
knitr::opts_chunk$set(echo = TRUE)
```


## 1.6 Prediction 
```
pred <- predict(m1, test) # test dataset을 이용하여 예측
knitr::opts_chunk$set(echo = TRUE)
```

## 1.7 결과 plotting
```
plot(train$이용건수, m1$fitted.values, cex = 0.5, pch = 19, xlim = c(0, 60), ylim = c(0, 60), xlab = "실제 이용건수", ylab = "예측된 이용건수")
points(test$이용건수, pred, pch = 3, cex = 0.05, col = 3)
abline(1,1, lty = 2, col = "blue", lwd = 2)
legend("topright", legend = c("training", "test", "실제 이용건수"), pch = c(19, 3, NA), col = c("black", "green", "blue"), lty = c(NA, NA, 1))
knitr::opts_chunk$set(echo = TRUE)
```

# 2. 다중선형회귀

## 2.1 Multiple Linear Regression

```
# 다중 회귀모형 학습
m2 <- lm(이용건수~., data = train)
knitr::opts_chunk$set(echo = TRUE)
```

## 2.2 다중공선성 확인
```
library(car)

# 분산 팽창 지수 계산 (VIF)
vif(m2)
summary(m2)
knitr::opts_chunk$set(echo = TRUE)
```

## 2.3  stepwise regression
```
# AIC 값이 작을수록 적합한 모형

full_m <- lm(이용건수~., data = train) # 모든 변수를 이용한 full model
null_m <- lm(이용건수~1, data = train) # 변수를 한 개도 이용하지 않은 null model

# stepwise regression 학습
forw_m <- step(null_m, direction = "forward", trace = 1, scope = list(lower = null_m, upper = full_m))
summary(forw_m)   # R^2=0.55, AIC=220171.5 (모든 변수 포함인 경우)

back_m <- step(full_m, direction = "backward", trace = 1, scope = list(lower = null_m, upper = full_m))
summary(back_m)   # R^2=0.55, AIC=220171.5 (")

both_m <- step(null_m, direction = "both", trace = 1, scope = list(lower = null_m, upper = full_m))
summary(both_m)   # R^2=0.55, AIC=220171.5 (")
knitr::opts_chunk$set(echo = TRUE)
```

## 2.4 test dataset prediction
```
# test dataset의 이용건수 예측 (점)
pre <- predict(full_m, newdata = test)
head(pre)

# test dataset의 이용건수 예측 (구간)
pred_step <- predict(full_m, newdata = test, interval = "predict")
pred_step <- as.data.frame(pred_step)
head(pred_step)


# test dataset의 이용건수 예측 성공률 확인
pred_step_1 <- pred_step
tf <- NA
pred_step_1 <- cbind(pred_step_1, test$이용건수)
pred_step_1 <- cbind(pred_step_1, tf)
pred_step_1$tf[pred_step_1$`test$이용건수`>= pred_step_1$lwr & pred_step_1$`test$이용건수` <= pred_step_1$upr] <- T
pred_step_1$tf[is.na(pred_step_1$tf)] <- F
head(pred_step_1)

sum(pred_step_1$tf=="TRUE")/dim(pred_step_1)[1]     # 예측 성공률 89%
knitr::opts_chunk$set(echo = TRUE)
```

## 2.5 결과 plotting
```
plot(train$이용건수, m2$fitted.values, cex = 0.5, pch = 19, xlim = c(0, 60), ylim = c(0, 60), xlab = "실제 이용건수", ylab = "예측된 이용건수")
points(test$이용건수, pre, pch = 3, cex = 0.05, col = 3)
lines(train$이용건수, train$이용건수, lty = 2, col = "blue")
legend("topright", legend = c("training", "test", "실제 이용건수"), pch = c(19, 3, NA), col = c("black", "green", "blue"), lty = c(NA, NA, 1))
knitr::opts_chunk$set(echo = TRUE)
```

## 2.6 Stepwise regression performance evaluation
```
MSE <- function(y, y_hat){ return(mean((y - y_hat)^2)) }
MAPE <- function(y, y_hat){ return(mean(abs((y - y_hat) / y)) * 100) }


# training MSE
MSE_tr2 <- mean((train$이용건수 - full_m$fitted.values)^2)
MSE_tr2

# test MSE
MSE_te2 <- mean((test$이용건수 - pre)^2)
MSE_te2

# traing MAPE
MAPE_tr2 <- mean(abs((train$이용건수 - full_m$fitted.values)/train$이용건수)*100)
MAPE_tr2

# test MAPE
MAPE_te2 <- mean(abs((test$이용건수 - pre)/test$이용건수))*100
MAPE_te2


par(mfrow=c(2,2))
plot(full_m)
par(mfrow=c(1,1))
knitr::opts_chunk$set(echo = TRUE)
```

## 2.7 변수의 중요도 파악
```
install.packages("relaimpo")
library(relaimpo)
calc.relimp(forw_m, rela = T)
knitr::opts_chunk$set(echo = TRUE)
```
