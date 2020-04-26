.. -*- mode: rst -*-

BeeColPy
============

BeeColPy is a module for function optimization through artificial bee colony
algorithm, a method developed by Karaboga (2005), a variant of classical
particle swarm optimization. It's distributed under the 3-Clause BSD license.

Website: https://github.com/renard162/BeeColPy/

Installation
------------

Dependencies
~~~~~~~~~~~~

BeeColPy requires:

- Python (>= 3.5)
- NumPy (>= 1.1.0)

**BeeColPy do not to support Python 2.7.**
BeeColPy needs Python 3.5 or newer.

User installation
~~~~~~~~~~~~~~~~~

If you already have a working installation of numpy,
the easiest way to install BeeColPy is using ``pip``   ::

    pip install git+git://github.com/renard162/BeeColPy.git

Usage Instructions
~~~~~~~~~~~~~~~~~

Load data and options:
abc_obj = abc(function, boundaries, colony_size=40, scouts=10, iterations=50, min_max='min')

Execute algorithm: 
solution = abc_obj.fit()

Get solution after execute fit() without execute it again:
solution = abc_obj.get_solution()

Get execution status:
(executed_iterations, scout_event) = abc_obj.get_status()

Get iterations historic:
agents = abc_obj.get_agents()

Parameters
----------
function : Name
	A name of a function to minimize/maximize.
	Example: if the function is:
		def my_func(x): return x[0]**2 + x[1]**2 + 5*x[1]
		Put "my_func" as parameter.

boundaries : List of Tuples
	A list of tuples containing the lower and upper boundaries of each
	dimension of function domain.
	Obs.: The number of boundaries determines the dimension of function.
	Example: [(-5,5), (-20,20)]

[colony_size] : Int --optional-- (default: 40)
	A value that determines the number of bees in algorithm. Half of this
	amount determines the number of points analyzed (food sources).
	According articles, half of this number determines the amount of
	Employed bees and other half is Onlooker bees.

[scouts] : Float --optional-- (default: 0)
	Determines the limit of tries for scout bee discard a food source and
	replace for a new one.
	If scouts = 0 : Scout_limit = colony_size * dimension
	If scouts = (0 to 1) : Scout_limit = colony_size * dimension * scouts
		Obs.: scout = 0.5 is used in [3] as benchmark.
	If scout = (1 to iterations) : Scout_limit = scout
	If scout >= iterations: Scout event never occurs
	Obs.: Scout_limit is rounded down in all cases.

[iterations] : Int --optional-- (default: 50)
	The number of iterations executed by algorithm.

[min_max] : String --optional-- (default: 'min')
	Determines if algorithm will minimize or maximize the function.
	If min_max = 'min' : Try to localize the minimum of function.
	If min_max = 'max' : Try to localize the maximum of function.


Methods
----------
fit()
	Execute the algorithm with defined parameters.
	Obs.: Returns a list with values found as minimum/maximum coordinate.

get_solution()
	Returns the value obtained after fit() the method.
	Obs.: If fit() is not executed, return the position of
		  best initial condition.

get_status()
	Returns a tuple with:
		- Number of iterations executed
		- Number of scout events during iterations

get_agents()
	Returns a list with the position os each food source during
	each iteration.

Example
~~~~~~~~~~~~~~~~~
"""
To find the minimum  of sphere function on interval (-10 to 10) with
2 dimensions in domain:
"""

from beecolpy import abc

def sphere(x):
	total = 0
	for i in range(len(x)):
		total += x[i]**2
	return total
	
abc_obj = abc(sphere, [(-10,10), (-10,10)]) #Load data
solution = abc_obj.fit() #Execute the algorithm

#If you want to get the obtained solution after execute the fit() method:
solution2 = abc_obj.get_solution()

#If you want to get the number of iterations executed and number of times that
#scout event occur:
iterations = abc_obj.get_status()[0]
scout = abc_obj.get_status()[1]

#If you want to get a list with position of all points (food sources) used in each iteration:
food_sources = abc_obj.get_agents()
