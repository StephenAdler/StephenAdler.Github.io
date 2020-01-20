---
layout: post
title: Introduction to Julia
---

Basic Julia Syntax that we'll use:

{% highlight julia %}
```Julia
using Distributions
using LinearAlgebra
using Optim

N=100
K=2
x=MvNormal(zeros(K),I)  #define distribution type-multivariate normal
X=rand(x,N)
X=[ones(N) X']
epsilon=randn(N)
truebeta=[0.2,0.3,0.4]
Y=X*truebeta+epsilon

#=loglikelihood function
l=sum log phi(y-x beta/sigma)=#

##define loglikehood function
function loglike(beta)
   r = Y - X * beta
   prob = logpdf.(Normal(0, 1), r)
   # pdf(distribution,x) cdf, quantile,
   loglikelihood = -sum(prob)    #remember to add negative sign
   return loglikelihood
end

theta0=[0.2,0.2,0.2]  #intial value

opt=optimize(loglike,theta0)
#optimize(function,initial value, Newton() #method)
MLE=opt.minimizer


lower=[0.0,0.0,0.0]
upper=[1.0,1.0,1.0]
##Optimization with box constraint
opt = optimize(loglike,lower,upper,theta0)
#one dimiension optimization no initial value    method:brent or goldsection
MLE = opt.minimizer
```
{% endhighlight %}
