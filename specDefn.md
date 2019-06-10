### Outline of spec:

BP : this is just for readability, consider enforcing line limits in your markdown,
mostly the `` sections, because it's hard to read on github when the lines are really
long


Note the graphs should be stored in csv format with columns defined as: "node source" "node target" and "weight". Multi-graphs are stored in one csv, with the multiple weight columns referring to the multiple edges from the different graphs.

BP : explain that each row of this csv represents an edge


BP : misleading to call a csv itself sparse, there will be no "missing entries" in the 
csv. But yes, it is storing the graph in a sparse format which is worth mentioning.

The csv will be sparse! 
And for each graph, there is a json defining its metadata.

### How to define attributes:
Attributes are broken into three levels, and are formatted as a dictionary:

The top level of the json contains a **list of keys** for each attribute for every level, followed by the individual attributes formatted as dictionary:

BP : I don't understand what you mean by the above

```
{
"graphAttributes": [list of keys contained in the json that define global graph-level attributes], ex: ["multi-graph", "weughted", etc]

"nodeAttributes": [list of keys contained in the json that define node-level attributes], ex: ["name", etc].
Please specifically use the term "node" to refer to these attributes instead of "vertex" or other synonyms.


"edgeAttributes": [list of keys contained in the json that define edge-level attributes], ex: ["type", etc]  # BP: I know we went back and forth about edge attributes, do these correspond to 
the columns of the edgelist? I'm a little confused what these would be in practice.
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




### MUST HAVE ATTRIBUTES:
```
{ "graph": {

# BP: always make something boolean if you can
"multi-graph": defines if the graph is a multi-graph, options: "yes" or "no" 
# BP: True/False
# Is this not just read off from the number of columns in the csv? 

"directed/undirected": defines if the graph is directed or not, options: "directed" or "undirected",
# BP: "directed": True/False


"weighted": defines if the graph is weighted or not, options: "yes" or "no",
# BP: True/False
# BP: on second thought, can be calculated from the graph if you specify how to store 
# an unweighted graph.
# i.e. the spec says "unweighted graph should not have a column for edge weights in 
# the csv, just source and target." Though we'd have to decide what to do for multigraph
# not sure about this one but we should ahve this conversation

"hollow": "yes" or "no" to indicate the absence or presence, respectively, of self-loops
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
Other options that the user can define. 

BP : I don't know what the below means

Please err on the side of well defined attributes.  
