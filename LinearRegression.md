#  LinearRegression

### Multi regression
```
model <- lm(y ~ u + v + w)
```

### Check summary
```
summary(model)
```


### Confidence Interval
```
confint(model, level = 0.95)
```


### Identify High Leverage Point

#### Leverage Score 

![image](https://note.youdao.com/yws/public/resource/0e29b13ca4589bcb98e3c7e0e7314081/xmlnote/F1ACB33FCBEA4F7AA2D5F2A18DC6F6CD/59302)

The equation can abbreviate to 

![image](https://latex.codecogs.com/gif.latex?%5Cwidehat%7By%7D%20%3D%20Hy)

Each observation's leaverage is in the diagonal of matrix H 
these elements consist 

![image](https://latex.codecogs.com/gif.latex?%5CLARGE%20h_%7Bii%7D)

![image](https://note.youdao.com/yws/public/resource/0e29b13ca4589bcb98e3c7e0e7314081/xmlnote/0E35D88EAD754ED5B9210F417F4E06D0/59304)

#### Threshold

Threshold of leverage score  : 2(p + 1) / n

- n : Observation size
- p : predict variable size

```
n <- nrow(beer)
p <- 3
lev_scores <- 2 * ( p + 1) / n

```

#### Caculate each data's threshold score 
```
lev_scores <- hatvalues(model = modle)
```


#### Plot 
```
plot(lev_scores,col=ifelse(lev_scores > (2*p/n), 'red2', 'navy'),type="h", ylab="Leverage score",xlab="Index",main="Leverage Scores for all observations") 
text(x=c(1:n)[lev_scores > (2*p/n)]+c(rep(2,4),-2,2),y=lev_scores[lev_scores > (2*p/n)], labels=c(1:n)[lev_scores > (2*p/n)])
```
![image](https://note.youdao.com/yws/public/resource/0e29b13ca4589bcb98e3c7e0e7314081/xmlnote/30AA4E789ECC462390FC36537AC6E58C/59310)

* Explain *

The red line mean data's levarage score is higher than thresholdh. 
However, if their value is not near 1, it's OK 


#### Cook Distance
```
plot(model,which=4,col=c("blue4"))
```
![image](https://note.youdao.com/yws/public/resource/0e29b13ca4589bcb98e3c7e0e7314081/xmlnote/5050725C5D354E34B97E419A272FABB0/59308)

* Explain *

Di - Cook Distance for data point i

The consensus seems to be that Di > 1 indicates an observation is an influential value, but we generally pay attention to observations with  Di > 0.5.


#### Residule
```
plot(model,which=5,col=c("blue4"))
```
![image](https://note.youdao.com/yws/public/resource/0e29b13ca4589bcb98e3c7e0e7314081/xmlnote/B48F81B3436D4990B97C7C9DB4FB6842/59306)

* Explain *

If a data point's standardized residuals > 2, it's a outlier 

If it's cook distance > 1, then it has high effect on fit model. 

So, an outlier is not necessary have high effect on model. Then it's a good news. 


#### Validation and MES Evaluation


```
set.seed(123) 
train_index <- sample(nrow(wages),round(0.7*nrow(wages)),replace=F) 
train <- wages[train_index,] 
test <- wages[-train_index,] 
regwagecsquares_train <- lm(bsal~sex+seniorc+agec+agec2+educc+experc+experc2,data=train) 
y_test_pred <- predict(regwagecsquares_train,test) 

temp <- cbind(test$bsal,y_test_pred); 
colnames(temp) <- c("Truth","Predicted"); 
temp[1:5,]

# MES 

testMSE <- mean((test$bsal - y_test_pred)^2); 
testMSE

# Square MES 

sqrt(testMSE)
```








