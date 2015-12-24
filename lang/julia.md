# Julia

Basics
directories
```julia
$ homedir()
$ cd("./Desktop")
$ pwd()
```
read file
```julia
$ data = readcsv("gasoline.csv");
```
slice data
```julia
$ data[:,6];
$ data[6,:];

$ data[:Colname]

# remove missing values (NA)
$ data[!isna(data[:Colanme]),:]
```
packages
```julia
$ Pkg.add("DataFrames")
$ using DataFrames
```
running IJulia
```julia
$ ipython notebook --profile julia
```

### Gradient descent

```sh
$ julia gradient_descent.jl .001 .0001 100
```

```julia
# gradient_descent.jl

using Gadfly

data = readcsv("spam.csv")
Y = data[1:end, 1]
X = data[1:end, 2:end]

X_stand = (X.-mean(X, 1))./std(X, 1)
X_log = log(X .+ .01)
X_binary = int(X .> 0)

function mu_fun(X, beta)
  1./(1+exp(X*transpose(-beta)))
end

function gradient(X, Y, beta, mu, lmda)
  2*lmda*beta-transpose(Y-mu)*X
end

function neg_log_like(Y, beta, lmda, mu)
  lmda*norm(beta)^2-(transpose(Y)*log(mu)+transpose(1-Y)*log(1-mu))
end

function gradient_descent(X, Y, iter, lmda, step_size)
  nrow, ncol = size(X)
  beta = zeros(1, ncol)
  nll_array = Float64[]
  while iter > 0
    mu = mu_fun(X, beta)
    grad = gradient(X, Y, beta, mu, lmda)
    beta = beta-step_size*grad
    nll = neg_log_like(Y, beta, lmda, mu)
    if isnan(nll[1])
      break
    else
      push!(nll_array, nll[1])
      iter -= 1
    end
  end
  return nll_array
end

lmda = float(ARGS[1])
step_size = float(ARGS[2])
iter = int(ARGS[3])

gd_stand = gradient_descent(X_stand, Y, iter, lmda, step_size)
gd_log = gradient_descent(X_log, Y, iter, lmda, step_size)
gd_binary = gradient_descent(X_binary, Y, iter, lmda, step_size)

myplot = plot(layer(x=1:iter, y=gd_stand, Theme(default_color=color("red")), Geom.line),
              layer(x=1:iter, y=gd_log, Theme(default_color=color("blue")), Geom.line),
              layer(x=1:iter, y=gd_binary, Theme(default_color=color("green")), Geom.line),
              Guide.title("Gradient Descent"),
              Guide.xlabel("Iterations"),
              Guide.ylabel("NLL"),
              Guide.yticks(ticks=[0:500:3000]),
              #Guide.xticks(ticks=[0:1000:iter]),
              Scale.y_continuous(minvalue=0, maxvalue=3000))
draw(PNG("plot.png", 6inch, 5inch), myplot)
```
