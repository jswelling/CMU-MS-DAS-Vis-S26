## Graphs - The kind with Nodes and Edges ##

Graphs are ubiquitous in computational work and data science.


I have used graphs for:
* software component descriptions
* analysis workflow descriptions
* patient flow
* transport and logistics
* describing the logistics and provenance of datasets
* constructing an ontology

You too will need to draw complex graphs.



### Types of graphs

A *graph* is any collection of nodes and edges.  Not all nodes must
have edges, but every edge must connect two nodes.<br>
![Simple example graph](images/example_graph_1.svg)


In a *directed graph* or *digraph*, every edge has a beginning and an end.<br>
![directed graph](images/example_graph_2.svg)


An *acyclic graph* has no closed loops.  We can make the last graph
acyclic by reversing one edge.<br>
![acyclic graph](images/example_graph_3.svg)


A *tree* always branches out, so that every node has at most one
incoming edge.  One node has zero incoming edges- it is the root of
the tree.<br>
![this graph is a tree](images/example_graph_4.svg)


The mathematics of graphs is beautiful and fascinating.  We could talk
about graph centrality and connectedness for an entire course.

But that would not be this course.



### Layout methods

Graph layout is a tricky business, because the display area is 2D.  How to
minimize overlap and crossing of graph edges?

[Gephi](https://gephi.org/) is a wonderful
<span class="smalltext">(but slow)</span> stand-alone
interactive graph tool.  The [Networkx](https://networkx.org/) and
[PyVis](https://pyvis.readthedocs.io/en/latest/) packages can provide
force-directed layout in a Jupyter notebook.


We will also be working with [Graphviz](https://graphviz.org/) , an old but
very stable and capable *non-interactive* package.  Because it can
produce such lovely graphs and has a wide selection of *layout engines*,
a lot of the static graphs you see on the web were made with Graphviz.

Someday, you will want to put a really lovely graph in a paper or report.
Graphviz is the way to make that graph.


Javascript tools to do really lovely interactive layout also exist, many
using the D3 library.  Let's look at an
[example from D3](https://observablehq.com/@d3/force-directed-graph) .


Interactive layout is generally force-directed.

Graphical elements are made to repel each other, in this case via
javascript interactions.  Note that you may still have to rearrange
elements to get a clear view.


Other interactive layout methods exist, for example grid-based layout.
If the graph is actually a tree, layout becomes a lot simpler.



## Is GraphViz Working?
You have hopefully installed GraphViz.  We're going to make sure your installation
is working now, and do some in-class debugging if it is not.

If Graphviz is correctly installed, the following command should work:
```
$ dot -V
dot - graphviz version 2.43.0 (0)
```
That is, the `dot` command should be able to execute and print its version number.


Try the following command line:
```
$ echo 'digraph { a -> b }' | dot -Tpng -otest.png
```
This should create a png file named `test.png` which looks like:
![sample png file](images/graphviz_test.png)


A working GraphViz is all you need to do the homework.  We're going to talk about
a convenient interactive app wrapper for GraphViz next.



### Using Our Demo App

We are going to talk about web servers soon, and as an example we will use a little app
that wraps GraphViz.  It looks like this:
<span class="image50">![graphserver pic](images/graphserver_pic.png)</span>


_If_ you have GraphViz working, please try to get this server working too. You'll save time
later.  The source code is in the [CMU-MS-DAS-Vis-Streamlit github](https://github.com/jswelling/CMU-MS-DAS-Vis-Streamlit) repo.


Clone the repo, create a virtual environment with Python 3.11 or higher in it, and use
```
pip install -r requirements.txt
```
to add the necessary modules.


Start up the Streamlit server, which will call Graphviz.
```
$ cd src/webserver  # if you are not already there
$ conda activate streamlitEnv  # or whatever you called your environment
$ dot -V  # to make sure GraphViz is working in this shell
$ streamlit run graphserver.py
```


The web [http://localhost:8501/](http://localhost:8501/) should now be
available on your local machine!
![Image of the Graphviz Streamlit page](images/streamlit_graphviz_app.png)


If things are working, try copying the following simple graph into the "Dot Language
To Render" box:
```
digraph { A -> B -> C -> A }
```
An image of the graph appears!


You can change the layout engine with the selector, but it won't make a huge
difference with this simple graph.  You can also read larger graphs from a file.



The goal here is to make sure you have GraphViz properly installed, and if possible to run it from
within the Streamlit web tool we built last class.  You will need at least GraphViz to do the
homework assignment.


If you can't get the Streamlit app to work, you can still try experiments (and do
the homework) by putting your Dot language examples in a file and using the
Graphviz command line to convert them to graph images.



### More Structure, and Attributes

Note the difference between:
```
digraph { A -> B -> C -> A }
```

and
```
graph { A -- B -- C -- A }
```

It's an error to try to mix the two.


#### There's a lot more syntax

```
digraph {
  "/some/long/name" [label="name" color="red" style="filled" fillcolor="lightgray"];
  "/a/different/name" [label="thing" color="blue"];
  "/some/long/name" -> "/a/different/name"
}
```


#### Edges can have attributes too
```
digraph {
  A;
  B [label="node B"];
  A -> B [color="blue"];
}
```


#### Defaults can be set for the whole graph
```
digraph {
  node [shape=diamond];
  A -> B -> C -> A;
}
```


#### Full list of attributes...

See [the attributes page at graphviz.org](https://graphviz.org/doc/info/attrs.html) .
There are a lot of them.

Also see the excellent set of examples at
[Rene Nyffenegger's page](https://renenyffenegger.ch/notes/tools/Graphviz/examples/index) .



### There are python wrappers for Graphviz...

But they break, or are abandoned.  *gv* was closely tied to the internals
of the Graphviz library, but it is very difficult to find or build now.

It is very sad to write code, then come back in a couple of years and find
that you cannot run it.  Writing simple python wrappers that generate Dot
is almost as easy as using these interfaces, and much more future-proof.



### Layout and rendering are actually separate steps

Try a simple graph, but select *Output DOT with coordinates*.  When you click
*Submit*, what you get back is Dot- but with coordinate info added.  The layout
engine has figured out where to put everything.

Copy the Dot with coordinates back into the input window, select *render to
SVG only (do no layout)*, and click submit.  Presto! The actual image is
rendered.


The *extended layout* option works the same way, but with even more info.

From the command line, you can get layout-only results using the `-Tdot` or
`-Txdot` option.  `neato -n2 -Tsvg` will render the layout into an svg image.


If you specify node coordinates and edges without coordinates, **neato**
will generate edges with nice arcs.  I've used this to generate arc curves
for transit lines to overlay on a map.


There is a javascript engine that can render fully-laid-out Dot.  This is
a nice design pattern- the server runs the layout engine but doesn't have
to generate the SVG or image.  That work gets delegated to the browser.



### There are some large sample graphs

In the examples subdirectory of the CMU-MS-DAS-Vis-Streamlit repo, you will
find:
* *bridges_network.dot* is the internal network for the old Bridges
  supercomputer.
* *hermes_classes.dot* is a set of class relationships for some code.
* *hospital_transfers.dot* shows patient transfers in a small hospital
  network.
* tinytest_shipping_network.dot is a commodity shipping simulation result.
  Note how nodes get scaled to keep this graph compact.


Try them with different layout engines.  **Note** that some may take a long
time to do the layout, and you will have to zoom out to see the whole graph.



## The Homework

There is an assignment to try this out.  The skeleton code for the assignment
is
```
graphviz_assignment_skeleton.ipynb
```
it provides some helpful tools for drawing GraphViz graphs in Jupyter notebooks.


Let's pause and look at the assignment and skeleton code now.




## Tableau Can Draw Force-Directed Graphs...

...with a little help.  There are plug-ins that can teach it how.

We're going to download the plug-in
[Network by LaDataViz](https://exchange.tableau.com/products/998)
from the Tableau Data Exchange.  Please open that page and download
the `.trex` (Tableau Extension) file now.


Once you have the `.trex` file and a network dataset (like the sample from
the class data directory!), you can load the data into Tableau and draw
a graph:


1. Download the Tableau extension as a `.trex` file.
2. In Tableau, load your data and create a Sheet. In the Marks area,
   use the dropdown to select Viz Extensions -> Add Extension.
3. In the resulting pop-up, click "Access Local Viz Extensions".
   Find and select the .trex file you downloaded.
4. Now it's just like any other Tableau interface, where you drag
   variables onto dimensions of the visualization. 


Choose to use an extension here...
<span class="image30">
![Tableau dialogs to use an extension module](images/tableau_use_extension.png)
</span>


And then choose to use your downloaded extension here.
<span class="image30">
![Tableau dialog to use the downloaded extension](images/tableau_use_extension_2.png)
</span>


