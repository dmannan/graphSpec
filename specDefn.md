### Outline of spec:

BP : this is just for readability, consider enforcing line limits in your markdown,
mostly the `` sections, because it's hard to read on github when the lines are really
long

``
Note the graphs should be stored in csv format, with each row representing an edge. Columns defined as: "node source" "node target" and "weight". Multi-graphs are stored in one csv, with the multiple weight columns referring to the multiple edges from the different graphs. **Edge attributes** also **stored under separate cloumns in the csv** (see exampleEdgelist.csv). 

BP : explain that each row of this csv represents an edge


BP : misleading to call a csv itself sparse, there will be no "missing entries" in the 
csv. But yes, it is storing the graph in a sparse format which is worth mentioning.

The graphs are sparse, hence, non-existent edges between nodes have no entry in the csv.
And for each graph, there is a json defining its metadata.
``
### How to define attributes:

##### General Guidelines:
1. Use attribute names that reflect their semantic meaning  
2. Attribute names should not be long, if it can be helped
3. Use biologically relevant terms where possible
4. Do not use capital letter for attributes 
5. Attributes with null value can be omitted.
6. Use "node" instead of "vertex" or other synonyms. 
7. Use "edge" instead of other synonyms.
8. Nodes are referred by their integer keys, which can be non-consecutive
9. For Boolean-type attributes, use True/False for their value 


``
Attributes are broken into three levels, and their key/value pairs are formatted as a dictionary:

The top level of the json contains a **list of attributes** for graph and node level, followed by the individual key/value pairs formatted as dictionary. Note, edge attributes DO NOT have a list of keys because that information is contained in the headers of the edgelist csv. In the json, the edge attributes will only be explained and WILL NOT contain key/value pairs:



```
{
"graphAttributes": [list of keys contained in the json that define global graph-level attributes], ex: ["multi-graph", "weighted", etc]

"nodeAttributes": [list of keys contained in the json that define node-level attributes], ex: ["name", etc].




}
```
Following the list of keys, the attributes themselves are defined as dictionaries, with the above mentioned keys and their corresponding values. Note: Nodes are referred to them by their **integers IDs**. Edge attributes is a list of dictionaries, where the edges are referred to by their **"node source" and "node target"**. In the spec, please refrain from adding the "weights" of the edges, since the csv contains this information.:
```
# BP: we should consider calling these graphs, nodes, cause they may be multiple 
# BP: in the multigraph case is graph a list of dicts? 
# BP: I realize I stored the nodes as below for the c. elegans example I sent you
# but now I'm wondering if a list would make more sense. A dict of integer keys seems
# clunky when you could just use a list... unless you let the integer keys be non-
# consecutive.
{"graph": {"key": "value"},
"node": {"0":{"key": "value"}, "1": {"key": "value"}, etc},
"edge": [{"node source": "value", "node target": "value", "attribute": "value"}, etc]

}


```

Hence, the **overall spec** would look as follows:
```
{
  "graphAttributes": [keys],
  "nodeAttributes": [keys],
  "edgeAttributes": [keys],
  "graph": {"key": value},
  "node": {node ID:{"key": value}},
  "edge": [{"node source": value, "node target": value, "key": value}]

}




```

When defining the attributes, if the attribute is binary use "yes" or "no" as options in the spec.
If there are attributes that are common among the nodes/edges, for example metadata contained in atlases, please create a separate json for them instead of subsuming it into json for each of the graphs.



``
### MUST HAVE ATTRIBUTES:
```
{ "graph": {


"multi-graph": defines if the graph is a multi-graph, options: "True/False"

# BP: Is this not just read off from the number of columns in the csv? LOOK INTO THIS 

"directed": defines if the graph is directed or not, options: "True/False",



"weighted": defines if the graph is weighted or not, options: "True/False",
# BP: True/False
# BP: on second thought, can be calculated from the graph if you specify how to store 
# an unweighted graph.
# i.e. the spec says "unweighted graph should not have a column for edge weights in 
# the csv, just source and target." Though we'd have to decide what to do for multigraph
# not sure about this one but we should ahve this conversation

"hollow": "True/False" to indicate the absence or presence, respectively, of self-loops
# BP: Can be calculated from the graph



},

}

```




### OPTIONAL ATTRIBUTES: These attributes will be concatenated with the must have attributes to have a common dictionary

 
```
{
  # BP: again, I don't think we need to tell them anything about what these should be
  # the below are all great examples, but literally anything is an "optional attribute"
  "graph": {
    "region": string, defines the region of the anatomy the graph is extracted from. Use biologically relevant terms
    "sex": string defining the sex of the subject
    "species": string for species name

    "atlas": name of the atlas the graph is registered to
    "subject ID": integers or string or both identifying the subject
    "session number": integers
    "modality": defines the experimental modality used to obtain the data, options: string to specify the modality. Use full form of the modality: (i.e. "functional magnetic resonance imaging" instead of "fmri", etc)
  },

  "node":{

    "name": string defines the name of the node, if any.
    "location": list of float marking the location of the node.
    "size": tuple defining the size/volume of the node with units: (size, units)
    "cell type": string to define the cell. If there is a hierarchal division, refer to them sequentially as "cell type1", "cell type2", etc


},






}
```

### USER DEFINED ATTRIBUTES:
Other options that the user can define, following above guidelines. 

 
