This is a markdown defining what the spec should be like:

Note the graphs should be stored in csv format with columns defined as: "node source" "node target" and "weight". Multi-graphs stored in one csv, with the multiple weight columns referring to the multiple edges from the multiple graphs.
The csv will be sparse!
And for each graph, there is a json defining its metadata.

### How to define attributes:
Attributes are broken down into three levels, and are formatted as a dictionary:
graphAttributes: define global graph-level attributes
nodeAttributes: define node-level attributes. Please specifically use the term "node" to refer to these attributes instead of "vertex" or other synonyms. Nodes are referred to them by their integers IDs.
edgeAttributes: defines edge-level attributes. Please specifically use the the term "edge" to refer to these attributes. Edges are referred to by their "node source" and "node target". In the spec, please refrain from adding the "weights" of the edges, since the csv contains this information.

When defining the attributes, if the attribute is binary use "yes" or "no" as options in the spec.
If there are attributes that are common among the nodes/edges, for example metadata contained in atlases, please create a separate json for them instead of subsuming it into json for each of the graphs.
### MUST HAVE ATTRIBUTES:
```
{ graphAttributes: {


"multi-graph": defines if the graph is a multi-graph, options: "yes" or "no"
"directed/undirected": defines if the graph is directed or not, options: "directed" or "undirected",
"weighted": defines if the graph is weighted or not, options: "yes" or "no",
"modality": defines the experimental modality used to obtain the data, options: string to specify the modality. Use full form of the modality: (i.e. "functional magnetic resonance imaging" instead of "fmri", etc)

},

}

```




### OPTIONAL ATTRIBUTES: These attributes will be concatenated with the must have attributes to have a common dictionary
```
{
  graphAttributes: {
    "region": string, defines the region of the anatomy the graph is extracted from. Use biologically relevant terms
    "sex": string defining the sex of the subject
    "species": string for species name

    "atlas": name of the atlas the graph is registered to
  },

  nodeAttributes:{

    "name": string defines the name of the node, if any.
    "location": list of float marking the location of the node.
    "size": tuple defining the size/volume of the node with units: (size, units)
    "cell type": string to define the cell. If there is a hierarchal division, refer to them sequentially as "cell type1", "cell type2", etc


},






}
```

### USER DEFINED ATTRIBUTES:
Other options that the user can define. Please err on the side of well defined attributes.  
