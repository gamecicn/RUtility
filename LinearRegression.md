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



#### Threshold

Threshold of leverage score  ![equation](https://latex.codecogs.com/gif.latex?2%28p%20&plus;%201%29/n)

- n : Observation size
- p : predict variable size

```
n <- nrow(model.matrix(regwagecsquares)); 
p <- ncol(model.matrix(regwagecsquares)) 
lev_scores <- hatvalues(regwagecsquares) 
#can also use influence(regwagecsquares)$hat 

plot(lev_scores,col=ifelse(lev_scores > (2*p/n), 'red2', 'navy'),type="h", ylab="Leverage score",xlab="Index",main="Leverage Scores for all observations") 
text(x=c(1:n)[lev_scores > (2*p/n)]+c(rep(2,4),-2,2),y=lev_scores[lev_scores > (2*p/n)], labels=c(1:n)[lev_scores > (2*p/n)])
```

#### Cook Distance
```
plot(regwagecsquares,which=4,col=c("blue4"))
```
#### Residule
```
plot(regwagecsquares,which=5,col=c("blue4"))
```


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








