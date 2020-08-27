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
```
n <- nrow(model.matrix(regwagecsquares)); p <- ncol(model.matrix(regwagecsquares)) lev_scores <- hatvalues(regwagecsquares) #can also use influence(regwagecsquares)$hat plot(lev_scores,col=ifelse(lev_scores > (2*p/n), 'red2', 'navy'),type="h", ylab="Leverage score",xlab="Index",main="Leverage Scores for all observations") text(x=c(1:n)[lev_scores > (2*p/n)]+c(rep(2,4),-2,2),y=lev_scores[lev_scores > (2*p/n)], labels=c(1:n)[lev_scores > (2*p/n)])
```

#### Cook Distance
```
plot(regwagecsquares,which=4,col=c("blue4"))
```
#### Residule
```
plot(regwagecsquares,which=5,col=c("blue4"))
```
