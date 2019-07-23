### Comparison of different graph spec methods:

1. Json Graph Specification: https://github.com/jsongraph/json-graph-specification
Pros:
- all in one file
- in json so can use in built Py func/netwrokx func to use it 
- very well laid out how to use it

Cons: 
- edge/node/graph get their own objects.
- edge object seems redundant with attributes such as "relation", and each edge might have repeatitive attributes ("directed"), 
- don't know where the weights for the edges go (check to make sure in json)
- seems verbose so gets large and maybe hard to handle


2. Graphml (Read: http://www2.sta.uwi.edu/~mbernard/research_files/fileformats.pdf):
Pros:
- all in one file
- the top of the file lists the keys and defines them, with the type of value it is

Cons:
- gets large and hard to read
- can't readily extract the graph from file and need additional func/packages to handle (check this)
- some of the above arguments apply (with edges are separate objects so "node source/target" gets repetitive)


Article describing file formats: http://www.maths.adelaide.edu.au/matthew.roughan/papers/hitch_hikers_guide.pdf
https://gephi.org/users/supported-graph-formats/
https://arxiv.org/abs/1503.02781
- from these articles, I gather that having an edgelist would make it easy to change data if mistakes are made- so it is easy to use. We don't need extra software engineering effort to use our spec. Don't need additional software to use a csv file and load a json (there are packages in to read a json in R and Py). 


Our spec: we can easily get the graph, and metadata is separate.. above methods don't have this (the graph is with the metadata). Openml format matches ours

The above formats (even ours) is portable? 