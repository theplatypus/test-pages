# Scott 

Scott is a software able to compute, for any colored (edge and node) graph, a tree representative of its isomorphism class, that we can derive to a canonical trace (string) or adjacency matrix.

Written and developed by : 

- Nicolas BLOYET **([See-d](https://www.see-d.fr/), [IRISA Expression](https://www-expression.irisa.fr/fr/), [LMBA](http://web.univ-ubs.fr/lmba/))**
- Pierre-François MARTEAU **([IRISA Expression](https://www-expression.irisa.fr/fr/))**
- Emmanuel FRÉNOD **([See-d](https://www.see-d.fr/), [LMBA](http://web.univ-ubs.fr/lmba/))**

![logos](https://raw.githubusercontent.com/theplatypus/test-pages/master/docs/logos/logos.png "Institutions")

# Table of contents

- [Overview](#overview)
  * [Graph Isomorphism](#graph-isomorphism)
  * [Graph Canonization](#graph-canonization)
  * [State of Art](#state-of-art)
  * [Key Idea](#key-idea)
- [Getting started](#getting-started)
  * [From source code](#from-source-code)
  * [From Pypi](#from-pypi)
  * [From Docker](#from-docker)
- [Usage](#usage)
  * [Import Graphs](#import-graphs)
  * [Canonical traces](#from-source-code)
  * [Canonical Adjacency Matrices](#canonical-adjacency-matrices)
- [Citation](#citation)
- [Licence](#licence)
- [References](#references)


## Overview

A graph G is a graph, defined by a set of vertices V and a set of edges E, which are pairs of vertices. In some cases, both edges and vertices can carry a label (or color) as additional local information.

We present below the example of the cafeine molecule, as a graph in which vertices represent atoms, labelled by chemical element (C, O, N, H, etc.), and the edges represent covalent bonds, labelled following the bond modality (simple, double, triple, etc.).

![cafeine](https://raw.githubusercontent.com/theplatypus/test-pages/master/docs/img/cafeine.svg?sanitize=true)

### Graph Isomorphism

As long as we describe a graph by an enumeration of its elements, there are several possible descriptions of the same structure.

Let G and H be two graphs. They are said to be isomorphics if there exists a bijection between their respective vertices sets which preserves edges. Below the [Wikipedia](https://en.wikipedia.org/wiki/Graph_isomorphism) example, where you will find more informations about the [problem](https://en.wikipedia.org/wiki/Graph_isomorphism_problem).

<!-- G ≃ H ⟺ ∃ f : V<sub>G</sub> ⟶ V<sub>H</sub>, ∀ (v<sub>1</sub>, v<sub>2</sub>) ∈ E<sub>G</sub>, (f(v<sub>1</sub>), f(v<sub>2</sub>)) ∈ E<sub>H</sub> -->

![wikipedia example](https://raw.githubusercontent.com/theplatypus/test-pages/master/docs/img/isomorphism.png)

Isomorphism is an equivalence relation, and naturally leads to the definition of *isomorphism class*, a set of graphs sharing an isomorphism with each other, and so all representing the same structure.

While determining (or not) if two graphs are isomophics seems trivial for small graphs, it is actually a problem which remains unresolved in polynomial time in the general case (polynomial heuristics do exist for restricted class of graphs). There is still uncertaincy about this problem theoretical complexity, despite some very recent works [[x](https://arxiv.org/abs/1512.03547)] seem to prove its appartenance to P (Polynomial).

There are a lot of practical applications of this problem [[x](https://en.wikipedia.org/wiki/Graph_isomorphism_problem#Applications), [x](https://math.stackexchange.com/a/120482/606995)], as in many graph related problem, we want to consider graphs belonging to the same isomorphism class as equals.

### Graph Canonization

The [graph canonization](https://en.wikipedia.org/wiki/Graph_canonization) is a related problem, consisting in finding for a graph a *canonical representant*, unique for its isomorphism class. Two graphs are isomorphics if and only if their canonical forms are equal.

This problem is at least as difficult as graph isomorphism, as it answers to it explicitly. Actually, it is very often less efficient to find a canonical representative than testing isomorphism between two graphs, as there are some shortcuts leading to an early decision in the second case (e.g. not the same number of edges/vertices, not the same degrees, etc.). 

However, once the canonical representant of a graph is computed, it can be stored and re-used, making this method of resolution suitable in (sub-)graph matchings : given a population of graphs for which we previously computed their canonical form, we can tell if a new graph is already present in the collection without testing the candidate against every known graph, considering it is trivial to compare two canonical representants.

### State of Art

Several algorithms already exist, the most used being `nauty`, `bliss`, `traces` for canonization or `conauto`, `saucy` for isomorphism testing. These algorithms are highly efficient, but unfortunately, among algorithms able to give a canonical form, none of them is able to natively deal with labelled edges otherwise than rewriting the graph in an edge-unlabelled way, increasing the problem size. Moreover, those algorithms are sequential, and do not take advantage of multi-threaded hardware.

Another approach is `gspan`, which can handle both edge and vertice labelling, but as it is based on finding a lexicographic minimal description of a graph among the enumeration of them, it is not suitable for whole graphs as long as their size grow, explaining why it is mainly used for (small) subgraphs mining.

> We adress through `Scott` the problem of **canonizing** each **edge-labelled**, **vertice-labelled** graph.

### Key idea

We propose here an algorithm based on graph rewriting. Scott execution follows three main steps, illustrated below.

 1. Levelling of vertices, according to an elected root
 2. Re-writing of cycles without information loss
 3. Canonical encoding of the tree obtained

![Scott example](https://raw.githubusercontent.com/theplatypus/test-pages/master/docs/img/steps.svg?sanitize=true)

The root identity can be obvious is the best case (combination of label, degree, degree of neighboorhood-<1:n>, etc.), but in the worst case where there are several candidates, they are computed, the minimal trace obtained being unique for an isomorphism class. 

It can be proved that the following set of three rewritings is sufficient to transform a levelled-graph into a unique tree.

![Rewritings](https://raw.githubusercontent.com/theplatypus/test-pages/master/docs/img/bounds.svg?sanitize=true)

By applying successive re-writings, aiming to avoid all form of cycle without any loss of information, the graph converges to a tree. Those graphs editions are applied following an order derived on the graph itself, ensuring the tree obtained is a canonical representant of the isomorphism class of the graph.

![Substeps](https://raw.githubusercontent.com/theplatypus/test-pages/master/docs/img/substeps.svg?sanitize=true)

As it is possible to recursively define an order relation on a tree (known property), we can use this canonical tree to obtain some compacts isomorphism-invariants representation of the graph, such as trace (string) or standardized adjacency matrix.

For more details about the algorithm, please refer to the paper or the Python implementation we propose.

---

## Getting started 

### From source code

Simply clone the repo in a local repertory

```bash
# get the code
git clone https://github.com/theplatypus/scott.git
cd ./scott

# (optionnal) install using setuptools
python3 setup.py install
```

Then you should be able to import `scott` package from python :

```python
import scott as st
```

### From Pypi

```bash
pip install <todo>
```

### From Docker

To get `scott` in an environment with additional dependencies installed (chemical librabries, jupyter notebokks,etc.), a Docker container is available :

```bash
# Build the image containing all the stuff for a simple standalone install
docker build -t scott .
# or,
docker pull <todo>

# run an interactive shell, where you can import scott in python default interpreter
docker run --rm -it scott

# run a jupyter notebook including scott
docker run -it -p 8888:8888 scott /bin/bash -c "jupyter notebook --notebook-dir=/opt/notebooks --ip='*' --port=8888 --no-browser --allow-root"
```

## Usage

### Import Graphs

```python
import scott as st

g = ... todo
```

### Canonical traces

A canonical trace is a string representation of the tree representative of an isomorphism class. 

If you only need a [hash function](https://en.wikipedia.org/wiki/Hash_function) for graphs, you can apply a string hash function (`md5`, `sha`, etc.) to the trace obtained.

```python
g = st.parse.from_dot(file_path="./data/isotest/cfi-rigid-t2-dot/cfi-rigid-t2-0020-02-2.dot")[0]
h = st.parse.from_dot(file_path="./data/isotest/cfi-rigid-t2-dot/cfi-rigid-t2-0020-02-1.dot")[0]

c = st.canonize.to_cgraph(g, candidate_rule="$degree > $label")
c2 = st.canonize.to_cgraph(h, candidate_rule="$degree > $label")

str(c) == str(c2)
```

### Canonical Adjacency Matrices

On a graph `G` of `N` vertices, an adjacency matrix `A` is a `N*N` array describing the link between two vertices.
If `G` is non-directed, then `A` is symetric. If edges are not labelled, then `A` is binary.

```python
import numpy as np

np.array(g.adjacency_matrix())
# array([
#       [0, 1, 0, 0, 1, 1, 1, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 0, 0, 1],
#       [1, 0, 1, 1, 0, 1, 0, 0, 1, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0, 1],
#       [0, 1, 0, 1, 1, 0, 0, 0, 1, 1, 0, 1, 1, 0, 0, 1, 1, 1, 0, 1],
#       [0, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 0, 0, 1, 0, 0, 0],
#       [1, 0, 1, 1, 0, 0, 0, 1, 1, 0, 0, 1, 0, 1, 1, 1, 0, 1, 1, 1],
#       [1, 1, 0, 1, 0, 0, 1, 0, 1, 1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 0],
#       [1, 0, 0, 1, 0, 1, 0, 1, 0, 0, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0],
#       [1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 1, 1, 1, 0, 1, 0, 1, 1, 1, 0],
#       [1, 1, 1, 1, 1, 1, 0, 0, 0, 1, 0, 0, 0, 1, 1, 1, 0, 0, 0, 1],
#       [1, 1, 1, 1, 0, 1, 0, 0, 1, 0, 1, 1, 1, 0, 1, 1, 0, 1, 1, 0],
#       [0, 0, 0, 1, 0, 1, 1, 1, 0, 1, 0, 0, 0, 1, 0, 1, 1, 1, 1, 0],
#       [0, 1, 1, 1, 1, 0, 0, 1, 0, 1, 0, 0, 1, 1, 1, 0, 0, 1, 1, 1],
#       [1, 1, 1, 1, 0, 0, 1, 1, 0, 1, 0, 1, 0, 0, 1, 1, 1, 1, 0, 1],
#       [1, 1, 0, 1, 1, 1, 1, 0, 1, 0, 1, 1, 0, 0, 0, 1, 1, 0, 0, 1],
#       [1, 1, 0, 0, 1, 1, 1, 1, 1, 1, 0, 1, 1, 0, 0, 0, 1, 1, 1, 0],
#       [1, 0, 1, 0, 1, 1, 0, 0, 1, 1, 1, 0, 1, 1, 0, 0, 1, 1, 1, 1],
#       [1, 0, 1, 1, 0, 1, 1, 1, 0, 0, 1, 0, 1, 1, 1, 1, 0, 1, 0, 1],
#       [0, 0, 1, 0, 1, 1, 0, 1, 0, 1, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1],
#       [0, 0, 0, 0, 1, 1, 1, 1, 0, 1, 1, 1, 0, 0, 1, 1, 0, 1, 0, 0],
#       [1, 1, 1, 0, 1, 0, 0, 0, 1, 0, 0, 1, 1, 1, 0, 1, 1, 1, 0, 0]])
```

We typically use as input in a Graph Neural Network the 3-tuple :
   - `A`, the binary adjacency matrix
   - `X`, a `V*D1` matrix mapping each vertice with a vector of `D1` elements (color, etc.)
   - `E`, a `V*V*D2` matrix mapping each edge with a vector of `D2` elements
   
We can merge `A` and `E` to get an *adjacency tensor* of shape `N*N*D2`, depending of the GNN implementation used. Note that if edges labels are qualitative, you should always use a one-hot encoding, and so a `E` matrix. The only case where a "flat" `A` is acceptable is when edges labels are purely quantitative (or inexistent). 

In any case, `Scott` can help to get a standardized adjacency matrix, such as isomophic graph will have the exact same adjacency matrices, which can help learning process by bringing the "same elements towards the same neurons".

```python
# Let g and h be two isomorphic graphs
g = st.parse.from_dot(file_path="./data/isotest/cfi-rigid-t2-dot/cfi-rigid-t2-0020-02-2.dot")[0]
h = st.parse.from_dot(file_path="./data/isotest/cfi-rigid-t2-dot/cfi-rigid-t2-0020-02-1.dot")[0]

# if we compare their adjacency matrix, it is very unlikely to get the two exact same matrices,
# as there is no order on vertices
g.adjacency_matrix() == h.adjacency_matrix()
# False

# but if we induce an order based on the representant tree given by scott,
# there is only one canonical adjacecny matrix
g.adjacency_matrix(canonic = True) == h.adjacency_matrix(canonic = True)
# True
```



## Citation

If you use or fork `scott` in further works, please cite the following :

```
@inproceedings{bloyet:hal-02314658,
  TITLE = {{Scott : A method for representing graphs asrooted trees for graph canonization}},
  AUTHOR = {Bloyet, Nicolas and Marteau, Pierre-Fran{\c c}ois and Frenod, Emmanuel},
  URL = {https://hal.archives-ouvertes.fr/hal-02314658},
  BOOKTITLE = {{Complex Networks}},
  ADDRESS = {Lisbon, Portugal},
  PUBLISHER = {{Springer}},
  SERIES = {Studies in Computational Intelligence Series},
  YEAR = {2019},
  MONTH = Dec,
  KEYWORDS = {graph canonization ; graph isomorphism ; graph rewriting ; labeled graph},
  HAL_ID = {hal-02314658},
  HAL_VERSION = {v1},
}
```

## Licence

The Python source code we provide is available on the [GitHub repo](https://github.com/theplatypus/scott), under the MIT public licence. 

Feel free to improve it.

## References

