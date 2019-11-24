# Scott 

Scott is a software able to compute, for any colored (edge and node) graph, a tree representative of its isomorphism class, that we can derive to a canonical trace (string) or adjacency matrix.

Written and developed by : 

- Nicolas BLOYET **([See-d](https://www.see-d.fr/), [IRISA Expression](https://www-expression.irisa.fr/fr/), [LMBA](http://web.univ-ubs.fr/lmba/))**
- Pierre-François MARTEAU **([IRISA Expression](https://www-expression.irisa.fr/fr/))**
- Emmanuel FRÉNOD **([See-d](https://www.see-d.fr/), [LMBA](http://web.univ-ubs.fr/lmba/))**

![logos](https://raw.githubusercontent.com/theplatypus/test-pages/master/docs/logos/logos.png "Institutions")

## Overview

A graph G is a graph, defined by a set of vertices V and a set of edges E, which are pairs of vertices. In some cases, both edges and vertices can carry a label (or color) as additional local information.

We present below the example of the cafeine molecule, as a graph in which vertices represent atoms, labelled by chemical element (C, O, N, H, etc.), and the edges represent covalent bonds, labelled following the bond modality (simple, double, triple, etc.).

![cafeine](https://raw.githubusercontent.com/theplatypus/test-pages/master/docs/img/cafeine.png)

### Graph Isomorphism

As long as we describe a graph by an enumeration of its elements, there are several possible descriptions of the same structure. 

### Graph Canonization

The graph canonization is a related problem, consisting in finding for a graph a *canonical representant*, unique for its isomorphism class. Two graphs are isomorphs if and only if their canonical forms are equal.

This problem is at least as difficult as graph isomorphism, as it answers to it explicitly. Actually, it is very often less efficient to find a canonical representative than testing isomorphism between two graphs, as there are some shortcuts leading to an early decision (e.g. not the same number of vertices, not the same degrees, etc.). 

However, once the canonical representant of a graph is computed, it can be stored and re-used, making this method of resolution suitable in (sub-)graph matchings : given a population of graphs for which we previously computed their canonical form, we can tell if a new graph is already present in the collection without testing the candidate against every known graph, considering it is trivial to compare two canonical representants.

### Key idea

## Getting started 

### Python installation

Simply clone the repo in a local repertory

```bash
# get the code
git clone https://github.com/theplatypus/scott.git
cd ./scott

# install using setuptools
python3 setup.py install
```
### Pypi package

```bash
pip install <todo>
```

### Docker

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

## Getting started 

### Canonical traces

```python
g = st.parse.from_dot(file_path="./data/isotest/cfi-rigid-t2-dot/cfi-rigid-t2-0020-02-2.dot")[0]
h = st.parse.from_dot(file_path="./data/isotest/cfi-rigid-t2-dot/cfi-rigid-t2-0020-02-1.dot")[0]

c = st.canonize.to_cgraph(g, candidate_rule="$degree > $label")
c2 = st.canonize.to_cgraph(h, candidate_rule="$degree > $label")

str(c) == str(c2)
```

### Canonical Adjacency Matrices

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

If you use or fork `scott` in further works, please consider citing the following :

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

