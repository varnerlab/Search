## Direct search methods

### Introduction
Direct search methods (DSM) maximize (or minimize) an objective function by without using gradient information.
Direct search methods are often slower to converge than gradient based methods, however, they are global in scope and are less likely to get stuck in local extrema.
In this package, we have encoded a generic direct search run loop that can be customized by the user to solve constrained and unconstrained
parameter search problems.

### Installation
``Search`` is a Julia package that can be installed into your Julia installation using the ``Pkg.clone`` command:

    Pkg.clone("https://github.com/varnerlab/Search.git")

Once the package has been installed, you can use the functions in the ``Search`` package by issuing the command:

    $ using Search

from the REPL prompt or by adding this line to the top of any code that is calling ``Search`` package functions.

### Contents
In the ``Search`` package,  we encode a generic run loop for a DSM. We allow the user to generate their own DSM by writing the run loop in terms of the user-defined functions.
The main function of the ``Search`` package is ``estimate_model_parameters``:

    (objective_archive,parameter_archive) = estimate_model_parameters(objective_function::Function,
      generation_function::Function,
      acceptance_function::Function,
      constraints_function::Function,
      initial_parameter_guess::Array{Float64,1};
      maximum_number_of_iterations=20,
      show_trace=true)

The ``estimate_model_parameters`` function has the input arguments:

Argument | Type | Description
--- | --- | ---
objective_function | [Function](http://docs.julialang.org/en/stable/manual/functions/) | Objective function
generation_function | [Function](http://docs.julialang.org/en/stable/manual/functions/) | Generates new parameter guess
acceptance_function | [Function](http://docs.julialang.org/en/stable/manual/functions/) | Accepts or rejects parameter guess
constraints_function | [Function](http://docs.julialang.org/en/stable/manual/functions/) | Evaluates the parameter constraints
initial_parameter_guess | [Array](http://docs.julialang.org/en/stable/manual/arrays/) | Initial guess for the ``P x 1`` array of unknown parameters
maximum_number_of_iterations | Int | Maximum number of iterations (default value 20)
show_trace | boolean | if true, prints out debug information

and produces the values:

Argument | Type | Description
--- | --- | ---
objective_archive | [Array](http://docs.julialang.org/en/stable/manual/arrays/) | ``maximum_number_of_iterations x 1`` array of objective function values found during the run
parameter_archive | [Array](http://docs.julialang.org/en/stable/manual/arrays/) | ``maximum_number_of_iterations x 1`` array of ``ParameterArray`` wrappers (defined in Types.jl)

### How I use the ``Search`` package?
To use the ``Search`` package, you need to provide implementations for the input functions. These functions are expected to have the following signatures and return types:

Function | inputs | returns
--- | --- | ---
objective_function | `` P x 1`` current parameter guess | objective value for the parameter guess
generation_function | ``P x 1`` previous parameter guess, constraints_function | new parameter guess that satisfies the constraints_function
acceptance_function | current objective function value, objective_archive::Array{Float64,1} | ``{true | false}``, ``true`` if accept this error, otherwise ``false``
constraints_function | current parameter guess | corrected parameter guess
