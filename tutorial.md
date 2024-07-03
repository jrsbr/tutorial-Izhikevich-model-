## Julia Basics and how to model a single neuron

This initial tutorial will introduce Julia's basic structures and how to model the LIF and the Izhikevich models of neuronal spiking.

## Julia's basic structures

In Julia, we assign a variable simply by calling it by its name and assigning a value to it using the `=` symbol:

```julia
my_variable = 42
```

For basic math operations, we can just use the following operators:

```julia
sum = 3 + 2
product = 3 * 2
difference = 3 - 2
quotient = 3 / 2
power = 3 ^ 2 
modulus = 3 % 2
```

To define a function in Julia, we first write `function` followed by the function's name, its parameters, the commands and `end`:

```julia
function tutorial(parameter)
    # Your code goes here
    println("The input was $parameter.")
end
```

And we can simply call it by its name:
 
```julia
tutorial(Neuroblox)
#This would print "The input was Neuroblox."
```

## Plotting graphs

We can plot graphs in Julia using the `Plots.jl` package. To add this package we use the terminal:

```julia
using Pkg
Pkg.add("Plots")
# This will use the package manager to explicitely add the Plots.jl package

using Plots
# This will load the package
```

Inside this package, there are several built-in functions. First, we have to tell Plots.jl which backend we will be using for rendering plots.

In this tutorial we will be using `GR backend`, but there are several others and you can find them here: https://docs.juliaplots.org/latest/backends/

To call GR, we simply call it as the parameterless function `gr()`. After that, we need an interval and a data set to generate a graph. In general, we'll have something like this:

```julia
using Plots

# Set GR as the backend for Plots.jl
gr()

# Create some data (an interval and a random 10-element vector)
x = 1:10
y = rand(10)

# Plot the data
plot(x, y, label="Random Data", xlabel="X-axis", ylabel="Y-axis", title="Example")
```

You can try running the code above in your Julia terminal using your own data to see how it works.

## Solving differential equations

Another essential package we'll be using is DifferentialEquations, a library designed to solve various types of differential equations, such as ODEs, SDEs and PDEs.

To use the DifferentialEquations library, we need to define a differential equation as a function, solve it and then plot it (we know how to do the last part).

Let's solve the ODE $\frac{du}{dt}=-2u$  using this package. First we define a function to define the problem:

```julia
function f(du, u, p, t)
    du .= -2 * u
end
```
Here the variable `du` is used to store the result of the differential equation, `u` is the current value of the solution, `p` is for parameters (not used in this simple example), and `t` is the current time.

Then, we need to define the initial conditions of our ODE. We can do this simply by defining the variable `u0` and `tspan`:

```julia
u0 = [1.0]          # Initial condition 
tspan =(0.0, 5.0) # Time span from 0 to 5
```

Using our library, we can finally set up the problem and solve it using `ODEProblem` and `solve` functions:

```julia
using DifferentialEquations

# Sets up the problem
prob = ODEProblem(f, u0, tspan)

# Computes the ODE's solution
sol = solve(prob)
```

Finally, we can graph the solution using what we've already learned:

```julia
using Plots

plot(sol, xlabel="Time", ylabel="u(t)", title="Solution to du/dt=-2u")
```

## The Leaky Integrate-and-Fire model

The Leaky Integrate-and-Fire model is described by a single differential equation:

$$
\frac{du}{dt} = \frac{-gL \cdot (u - EL) + I}{C}.
$$

Our objetictive is to model this in Julia with the help of Neuroblox.


## The Izhikevich model

The Izhikevich model is described by the following two-dimensional system of ordinary differential equations:

$$
\begin{align}
\frac{dv}{dt} &= 0.04 v^2 + 5 v + 140 - u + I \\
\frac{du}{dt} &= a(bv - u)
\end{align}
$$

with the auxiliary after-spike resetting

$$
\begin{align}
\text{if }v \geq 30\text{ mV, then }\begin{cases}v\leftarrow c \\ u\leftarrow u + d.\end{cases}
\end{align}
$$
