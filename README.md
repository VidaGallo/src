# Symbolic regression

  The project was done by Gabriele Pirilli and Vida Gallo. 

<br>
<br>

## Introduction
  Each problem (1-8) has its own set of HYPERPARAMETERS defined at the beginning of the code. To prevent overfitting, we used only the training set (80% of all the samples) to build the formula, while the test set was used to evaluate the performance on unseen data. We implemented a Symbolic Regression using the steady-state approach. We also used an ISLAND MODEL, consisting of three islands: migrants moved from Pop1 to Pop2, from Pop2 to Pop3, and from Pop3 back to Pop1. <br>
  
  ![image](https://github.com/user-attachments/assets/a7c218c9-32cb-4bcf-b1ba-74b8b873004c)

<br>
<br>



## Results
We achieved satisfactory results considering the high computational cost of the algorithm. Despite the absence of parallelization, we managed to achieve low MSE values even for functions in higher dimensions, with the exception of a couple of particularly challenging cases. The results could be improved by considering a larger population, more generations and further hyperparameter tuning. <br>
Using three distinct subpopulations streamlined the tuning of exploitation and exploration parameters by assigning these roles to different groups. Population 3, which operated with a delay relative to Population 2, effectively reduced the risk of premature convergence. Population 2 concentrated on determining an optimal size by working with larger trees, while Population 1 emphasized exploration. The average depth of the preceding subpopulation served as the foundation for defining the optimal size in each group, resulting in an "adaptive" approach to size determination.
<br><br>
In particular the algorithm performed:
1. very well with the Problems 1, 3, 4, 5, 6
2. well with Problems 7, 8
3. not that well with Problem 2 (it could not converge in time to a reasonable result)
<br>
<br>



## Algorithm Overview
Each individual in the population is represented by the class **IndividualTree**, which is made up of nodes of the **Node** class. The tree has a pointer to its root node, and each node has pointers to its parent, left child, and right child. Possible operations can be unary (considered only left child) or binary (considered both children). 

The algorithm has the following characteristics:
1. **A decreasing temperature**, where higher temperature encourages more exploration.
2. **Tournament selection** for the parent selection for the generation, with occasional selection of random parents.
3. **Different types of mutations and crossovers**, each performed with a certain probability.
4. **Extinction** is performed occasionally, removing deeper trees with a certain probability proportional to the tree's depth or removing improper individuals (e.g., trees containing only constants).
5. **Deterministic survival selection**, with a small probability to pick random individuals too.
6. **An exploitation phase** performed occasionally (more often in Pop3), which aims to improve some randomly selected trees.
7. **Insertion of random individuals** (only in Pop1) at each iteration.

<br>
<br>

The considered classes are:
```python
class Node:
    def __init__(self, value = None, dim = None, operation = None, left=None, right=None, parent = None):
class IndividualTree:
    def __init__(self, root=None, num_dimensions = 1):
```
each own with its own methods and fields.

<br>
<br>

The following function:
```python
def available_un_func(max_grade, flag_un_ele):
```
is a function that just checks the max_grade that we want to use and if we want to use elementary functions as sin(x) and log(x) or not. 

<br>
<br>

The following functions:
```python
def assemble_tree(num_dimensions, depth, max_grade=2, flag_un_ele = False, parent=None):
def assemble_tree_with_vars(operation=None, num_dim=1, ele1=None, ele2=None):
```
build 2 different types of trees, the first ones are random, while the second creates costumized trees.

<br>
<br>

To create and evaluate the population we considered:
```python
def BasicLinearPopulation(num_dim):
def create_pop(degree = 3, depth = 2, num_dimensions =1, n_pop = 50, flag_un_ele = False):
def evaluate_population(population, X, y_true):
```

<br>
<br>

The consdiered the following mutations and recombinations:
```python
def SubtreeMutation(tree, depth_max, degree = 3, flag_un_ele = False):
def PointMutation(tree, degree=3, flag_un_ele = False):
def LRMutations(tree):
def HoistMutation(tree, max_attempts = 23):
def Expansion(tree, expansion_factor = 1, degree=3, flag_un_ele = False):
def HalfRecombination(tree1, tree2):
def Recombination(tree1, tree2):
def ApplyMutation(individual, max_depth, degree, flag_un_ele):
```

<br>
<br>

The populations evolve thanks to the following function:
```python
def EvolvePopulation(X, y_true, population, n_pop, fitness_scores, recombination_rate, max_depth, degree, 
                     current_t, tournament_size, extintion_coeff, flag_un_ele = False):
def TournamentSelection(population, fitnesses, tournament_size, k_ind = 1, temperature = 1):
def ApplyExtinction(X, population, max_depth, extinction_factor=0.2, max_ext_n=10):
```

<br>
<br>

The exploitation instead is composed of:
```python
def Exploitation(population, X, y_true):
def CoefImprovement(tree, X, y):
def OptimalPruning(tree,X,y):
```
<br>
<br>

Each population has its own function:
```python
def Pop1(...):
def Pop2(...):
def Pop3(...):
```
and everything is handled by:
```python
def SymbolicRegressionMULTIPOP(xx, yy, population_size=50, max_generations=100, recombination_rate=0.7, max_depth=100, 
                       degree=3,  flag_un_ele_beginning = False, flag_un_ele_in_between = False, t_coeff = 0.1, 
                       tournament_size_coeff = 0.1, extintion_coeff = 0.1):
```


<br>
<br>








