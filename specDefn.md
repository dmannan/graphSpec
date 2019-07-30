### Outline of spec:


Note the graphs should be stored in csv format, with each row representing an edge. Columns defined as: "node source" "node target" and "weight"(if the graph is weighted). If the graph is unweighted, then a row of "node source" and "node target" exists if there is an edge between the. Multi-graphs are stored in one csv, with the multiple weight columns referring to the multiple edges from the different graphs.  

BP : explain that each row of this csv represents an edge


BP : misleading to call a csv itself sparse, there will be no "missing entries" in the 
csv. But yes, it is storing the graph in a sparse format which is worth mentioning.

The graphs are sparse, hence, non-existent edges between nodes have no entry in the csv.
And for each graph, there is a json defining its metadata. Multi-graph metadata is contained in one json. 

### How to define attributes:

##### General Guidelines:
1. Use attribute names that reflect their semantic meaning
2. If there are attributes that can be calculated from the graph, do not specify them  
3. Attribute names should not be long, if it can be helped
4. Use biologically relevant terms where possible
5. Do not use capital letter for attributes 
6. Attributes with null value can be omitted.
7. Use "node" instead of "vertex" or other synonyms. 
8. Use "edge" instead of other synonyms.
9. Nodes are referred by their integer keys, which can be non-consecutive
10. For Boolean-type attributes, use True/False for their value
11. Use plural form for attributes that are plural 


```
Attributes are broken into three levels, and their key/value pairs are formatted as a dictionary:
Note: Nodes are referred to them by their **integers IDs**. Edges are reffered to by the row of the csv. The node and edge attributes are subsumed with the graph dictionary to keep the node and edge data that corresponds to the graph. Multigraphs are stored as a list of dictionaries and individual graphs are refferenced by the column of the csv file:
```
## should the nodes/edges be a list of dicts or dict of dicts
```
{"graph": {"key for graph": "value for the graph",
"nodes": ["0":{"key": "value"}, "1": {"key": "value"}, etc],
"edges": ["0": {"key": "value"}, etc]

}
}


```

Hence, the **overall spec** would look as follows:
```
  {
  "graph": {"key": value,
  
  "nodes": [list of dictionaries refferenced by nodeID],
  "edges": [list of dictionary referrenced by edgeID(row of the csv file)]
  }
  }


```
### Multigraphs are stored as follows:

```


{
"graphs":[
  
   {
  "graph 1": {"key": value,
  
  "nodes": [list of dictionaries refferenced by nodeID],
  "edges": [list of dictionary referrenced by edgeID(row of the csv file)]
  }
  },

  {
  "graph 2": {"key": value,
  
  "nodes": [list of dictionaries refferenced by nodeID],
  "edges": [list of dictionary referrenced by edgeID(row of the csv file)]
  }
  }
]

}

```

If there are attributes that are common among the nodes/edges, for example metadata contained in atlases, please create a separate json for them instead of subsuming it into json for each of the graphs.



``




### MUST HAVE ATTRIBUTES:
```
{ "graph": {


"multi-graph": defines if the graph is a multi-graph, options: "True/False" ### I'll take this off 
##since the next point made by BP
# BP: Is this not just read off from the number of columns in the csv? 

"directed": defines if the graph is directed or not, options: "True/False",



"weighted": defines if the graph is weighted or not, options: "True/False",
# BP: True/False
# BP: on second thought, can be calculated from the graph if you specify how to store 
# an unweighted graph.
# i.e. the spec says "unweighted graph should not have a column for edge weights in 
# the csv, just source and target." Though we'd have to decide what to do for multigraph
# not sure about this one but we should ahve this conversation
##DM: for an unweighted multigraph, we'll just have 0 in the column where no edge 
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

 
