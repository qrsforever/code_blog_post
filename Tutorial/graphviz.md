---

title: (原创)Graphviz绘图

date: 2019-07-24 14:00:24
tags: [How, DrawIt]
categories: [Tutorial]

---


<!-- vim-markdown-toc GFM -->

* [Requirements](#requirements)
* [Blog](#blog)
    * [pandoc config](#pandoc-config)
    * [filter graphviz.py](#filter-graphvizpy)
    * [ladot](#ladot)
* [Demo](#demo)
    * [docs](#docs)
    * [office example](#office-example)
    * [latex graph](#latex-graph)
    * [neural network](#neural-network)
        * [simplest](#simplest)
        * [complicated case](#complicated-case)
    * [others](#others)
        * [matrix](#matrix)
* [References](#references)

<!-- vim-markdown-toc -->

<!-- more -->

[Graph Visualization Software](https://graphviz.gitlab.io/ "高效而简洁的绘图工具graphviz")

# Requirements

```shell
sudo apt install pandoc
sudo apt install graphviz graphviz-dev
sudo apt install dot2tex latex2html dvipng
sudo pip3 install pygraphviz pandocfilters
```

if need show latex equation in graphviz, also install [latex](https://brighten.bigw.org/projects/ladot/).

# Blog

## pandoc config

[My Blog Config](https://github.com/qrsforever/git-blog-setting/blob/master/_config.yml)

```{.yaml .numberLines startFrom="1}

pandoc:
    extra:
        - standalone:
        - highlight-style: haddock
        - number-offset: 0
        - columns: 200
        - css: /css/pandoc.css
        - filter: graphviz.py
    mathEngine: mathjax

```

## filter graphviz.py

```{.python .numberLines startFrom="1"}
#!/usr/bin/python3

import os
import sys
import hashlib

import pygraphviz

from pandocfilters import toJSONFilter, Para, Image
from pandocfilters import get_caption, get_extension, get_value

tmp_path = '/tmp/gitblog'
top_path = os.path.abspath(os.path.dirname(__file__))
dir_name = 'graph-image'
git_path = 'https://raw.githubusercontent.com/qrsforever/assets_blog_post/master/'
git_post = '?sanitize=true'

doc_tmpl = r"""\documentclass[12pt,border=5pt,varwidth=true]{standalone}
\usepackage{graphicx}
\usepackage[utf8]{inputenc}
\usepackage[T1]{fontenc}
\usepackage{psfrag}
\begin{document}
    \input{%s}
    \includegraphics{%s}
\end{document} """

def graphviz(key, value, format, meta):
    if key == 'CodeBlock':
        [[ident, classes, keyvals], code] = value
        if "graph" in classes:
            caption, typef, keyvals = get_caption(keyvals)
            prog, keyvals = get_value(keyvals, u"prog", u"dot")
            filetype = get_extension(format, "svg", html="svg", latex="pdf")
            md5 = hashlib.sha1(code.encode(sys.getfilesystemencoding())).hexdigest()
            basename, _ = get_value(keyvals, u"fileName")
            if basename is None:
                sys.stderr.write('not set filename in {}\n')
                exit(-1)

            latex, _ = get_value(keyvals, u"latex")
            if latex is not None:
                filetype = 'png'

            filename = basename + '.' + filetype
            while True:
                try:
                    datapath = meta['datapath']['c']
                    drafts_idx = datapath.find('_drafts')
                    if drafts_idx > 0:
                        prefix = datapath[drafts_idx+8:-3]
                    else:
                        prefix = datapath[datapath.find('_posts')+7:-3]
                    filepath = os.path.join(prefix, dir_name, filename)
                    if drafts_idx > 0:
                        localpath = os.path.join(top_path, "source/assets", filepath)
                        remotepath = os.path.join("/assets", filepath)
                    else:
                        localpath = os.path.join(top_path, "source/_assets", filepath)
                        remotepath = os.path.join(git_path, filepath) + git_post
                    if os.path.exists(localpath):
                        if os.path.exists(localpath + '.' + md5):
                            break
                        else:
                            os.system('rm -f %s*' % localpath)

                    dir = os.path.dirname(localpath)
                    if not os.path.isdir(dir):
                        os.makedirs(dir)

                    if latex is None:
                        g = pygraphviz.AGraph(string=code)
                        g.layout()
                        g.draw(localpath, prog=prog)
                    else:
                        ladot_file = os.path.join(tmp_path, '{}.ladot'.format(basename))
                        latex_file = os.path.join(tmp_path, '{}.latex'.format(basename))

                        with open(ladot_file, 'w') as f:
                            f.write(code)

                        with open(latex_file, 'w') as f:
                            f.write(doc_tmpl % ('{}.tex'.format(basename), '{}.ps'.format(basename)))

                        resolution, _ = get_value(keyvals, u"resolution")
                        if resolution is None:
                            resolution = "1200"
                        density, _ = get_value(keyvals, u"density")
                        if density is None:
                            density = "200"

                        os.system('{}/ladot {} {} {} {}'.format(top_path, ladot_file, tmp_path, resolution, density))
                        if os.path.exists('%s.png' % os.path.join(tmp_path, basename)):
                            os.system('cp %s.png %s' % (os.path.join(tmp_path, basename), localpath))

                    sys.stderr.write('Local Path [' + localpath + ']\n')
                    sys.stderr.write('Remote Path [' + remotepath + ']\n')
                    os.system('touch %s.%s' % (localpath, md5))
                except Exception as e:
                    sys.stderr.write('{}: not found datapath in meta, please patch/run.sh\n'.format(e))
                    exit(-1)
                finally:
                    break

            image = Image([ident, classes, keyvals],
                          caption,
                          [remotepath, typef])

            return Para([image])

if __name__ == "__main__":
    if not os.path.exists(tmp_path):
        os.mkdir(tmp_path)
    toJSONFilter(graphviz)
```

## ladot

```{.perl .numberLines startFrom="1"}
#!/usr/bin/perl

%paststubs = ();

if (@ARGV != 4) {
    die "Usage: $0 [infile] [workspace dir] [resolution] [density]";
    }

# switch to workspace dir
chdir($ARGV[1]) or die "$!";
$resolution = $ARGV[2]
$density = $ARGV[3]

# Open input and output files
open(INFILE, "<$ARGV[0]");
$ARGV[0] =~ /(.*)\.ladot$/;
$basename = $1;

if ($basename eq "") {
    $basename = $ARGV[0];
    }
open(DOTOUT, ">$basename.dot");
open(TEXOUT, ">$basename.tex");

while ($line = <INFILE>)
    {
    while ($line =~ /(\$.*?\$)(\((\d+)\))?/) {
        # print "LINE: $line";
        $sizehint = $3;
        # print "SIZEHINT: $sizehint\n";
        $tex = $1;
        $stub = make_stub($tex, $sizehint);
        $line =~ s/(\$.*?\$)(\((\d+)\))?/$stub/;
        print TEXOUT "\\psfrag{$stub}[cc][cc]{$tex}\n";
        }
    print DOTOUT $line;
    }

close(DOTOUT);
close(TEXOUT);

# Generate postscript of the graph, with stubs
system("dot -Tps $basename.dot > $basename.ps");

# LaTeX
if (-e "$basename.latex") {
    system("latex $basename.latex > $basename.latex.log");
    if (-e "$basename.dvi") {
        system("dvips $basename.dvi -D $resolution -o $basename.dvips > $basename.dvips.log");
        if (-e "$basename.dvips") {
            system("pstoimg $basename.dvips -density $density -out $basename.png > $basename.pstoimg.log");
        }
    }
}

# Graphviz 2.2 and later outputs postscript in which psfrag won't replace tags.
# This is psfrag's fault, but our problem.  Here's a fix.
system("sed -ibackup \"s/xshow/pop show/g\" $basename.ps");
# There is a way to get sed to modify in place with no backup file, but
# it seems the syntax of this option is slightly different in different
# versions of sed.  So we just remove the backup ourselves.
# unlink("$basename.psbackup");

# unlink("$basename.dot");

sub make_stub($$)
    {
    # Make a placeholder (stub) for the TeX which will be substituted for the
    # real formatted TeX later.  Arguments are TeX code and an optional
    # "hint" about the desired width of the box.
    
    # This is tricky because the length of the stub
    # that we choose affects how Dot formats the PostScript.  We use the
    # heuristic that the length of the LaTeX code is correlated with the
    # amount of space needed to render the LaTeX code.

    if ($paststubs{$_[0]}) {
        return $paststubs{$_[0]};
        }

    my $length = int($_[1]);
    if ($length == 0) {
        # no sizehint given
        $length = length($_[0]) - 1;
        }
    # print "LENGTH of $_[0]: $length\n";
    # The first character of an unquoted string in Dot has to be one of these.
    $first_charset="_abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
    # Subsequent characters can be any of these.
    $subseq_charset="1234567890$first_charset";
    my $stub = "";
    for (my $i = 1; $i <= $length; $i++) {
        if ($i == 1) {
            $charset = $first_charset;
            }
        else {
            $charset = $subseq_charset;
            }
        $stub .= substr($charset, int(rand(length($charset))), 1);
        }
    #print "NEW STUB for $_[0]: $stub\n";
    $paststubs{$_[0]} = $stub;
    return $stub;
    }
```

# Demo

## docs

- [attributes](https://graphviz.gitlab.io/_pages/doc/info/attrs.html)

- [nodes shape](https://graphviz.gitlab.io/_pages/doc/info/shapes.html)

- [dot guide](https://graphviz.gitlab.io/_pages/pdf/dotguide.pdf)

## office example

```{.numberLines startFrom="1"}
digraph G {

  bgcolor="#ffffff00"

  subgraph cluster_0 {
    style="filled, rounded";
    color="#E6EAF2"
    node [style=filled,color=white];
    a0 -> a1 -> a2 -> a3;
    a3 -> a1 [label = " -10" color=red fontcolor=red];
    label = "System A";
  }

  subgraph cluster_1 {
    node [style=filled color="#E6EAF2"];
    b0 -> b1 -> b2 -> b3;
    b0 -> b2 [label = " +12" color=green fontcolor=green];
    label = "System B";
    style="dashed, rounded"
    color=blue
  }

  start -> a0;
  start -> b0;
  a1 -> b3;
  a3 -> end;
  b3 -> end;

  start [label="load" shape=folder];
  end [label="store" shape=box3d];
}
```

output:

```{.graph .center caption="Demo" fileName="test"}

digraph G {

  bgcolor="#ffffff00"

  subgraph cluster_0 {
    style="filled, rounded";
    color="#E6EAF2"
    node [style=filled,color=white];
    a0 -> a1 -> a2 -> a3;
    a3 -> a1 [label = " -10" color=red fontcolor=red];
    label = "System A";
  }

  subgraph cluster_1 {
    node [style=filled color="#E6EAF2"];
    b0 -> b1 -> b2 -> b3;
    b0 -> b2 [label = " +12" color=green fontcolor=green];
    label = "System B";
    style="dashed, rounded"
    color=blue
  }

  start -> a0;
  start -> b0;
  a1 -> b3;
  a3 -> end;
  b3 -> end;

  start [label="load" shape=folder];
  end [label="store" shape=box3d];
}
```

## latex graph

```{.numberLines startFrom="1"}

{.graph .center caption="Latex Graph Demo" fileName="ladotdemo" latex="true" resolution="1080" desity="100"}

digraph mydot {
    rankdir=LR
    node [shape=plaintext, width=0]
    $v_1$(2)
    $v_1$ -> $v_2$(2) [label=$\sqrt{2}+\frac{1}{x+5}$(9)]
    $v_2$ -> $v_3$(2) [label=$S \subseteq \{1,2,3\}^4$(6)]
    $v_3$ -> $v_1$ [label=$(f \circ g)^{-1}$(7)]
}
```

output:

```{.graph .center caption="Latex_Graph_Demo" fileName="ladotdemo" latex="true" resolution="1080" desity="100"}
digraph mydot {
    rankdir=LR
    node [shape=plaintext, width=0]
    $v_1$(2)
    $v_1$ -> $v_2$(2) [label=$\sqrt{2}+\frac{1}{x+5}$(9)]
    $v_2$ -> $v_3$(2) [label=$S \subseteq \{1,2,3\}^4$(6)]
    $v_3$ -> $v_1$ [label=$(f \circ g)^{-1}$(7)]
}
```

## neural network

### simplest

```{.numberLines startFrom="1"}

{.graph .center caption="simplest" fileName="graphviz_simplest"}

digraph G {

    rankdir=LR;   /* makes the directed graphs drawn from left to right */
	splines=line; /* controls how the edges are represented */
    nodesep=.05;  /* forces edges to become invisible */
    
    node [label=""];   /* [ ] sets the default node property */
        
    subgraph cluster_0 {
		color=white;
        node [style=solid,color=blue4, shape=circle];
		x1 x2 x3;
		label = "layer 1";
	}

	subgraph cluster_1 {
		color=white;
		node [style=solid,color=red2, shape=circle];
		a12 a22 a32 a42 a52;
		label = "layer 2";
	}

	subgraph cluster_2 {
		color=white;
		node [style=solid,color=red2, shape=circle];
		a13 a23 a33 a43 a53;
		label = "layer 3";
	}

	subgraph cluster_3 {
		color=white;
		node [style=solid,color=seagreen2, shape=circle];
		O1 O2 O3 O4;
		label="layer 4";
	}

    x1 -> a12;
    x1 -> a22;
    x1 -> a32;
    x1 -> a42;
    x1 -> a52;

    x2 -> a12;
    x2 -> a22;
    x2 -> a32;
    x2 -> a42;
    x2 -> a52;

    x3 -> a12;
    x3 -> a22;
    x3 -> a32;
    x3 -> a42;
    x3 -> a52;

    a12 -> a13
    a22 -> a13
    a32 -> a13
    a42 -> a13
    a52 -> a13

    a12 -> a23
    a22 -> a23
    a32 -> a23
    a42 -> a23
    a52 -> a23

    a12 -> a33
    a22 -> a33
    a32 -> a33
    a42 -> a33
    a52 -> a33

    a12 -> a43
    a22 -> a43
    a32 -> a43
    a42 -> a43
    a52 -> a43

    a12 -> a53
    a22 -> a53
    a32 -> a53
    a42 -> a53
    a52 -> a53

    a13 -> O1
    a23 -> O1
    a33 -> O1
    a43 -> O1
    a53 -> O1

    a13 -> O2
    a23 -> O2
    a33 -> O2
    a43 -> O2
    a53 -> O2

    a13 -> O3
    a23 -> O3
    a33 -> O3
    a43 -> O3
    a53 -> O3

    a13 -> O4
    a23 -> O4
    a33 -> O4
    a43 -> O4
    a53 -> O4
}
```

output:

```{.graph .center caption="simplest" fileName="graphviz_simplest"}

digraph G {

    rankdir=LR
	splines=line
    nodesep=.05;
    
    node [label=""];
        
    subgraph cluster_0 {
		color=white;
        node [style=solid,color=blue4, shape=circle];
		x1 x2 x3;
		label = "layer 1";
	}

	subgraph cluster_1 {
		color=white;
		node [style=solid,color=red2, shape=circle];
		a12 a22 a32 a42 a52;
		label = "layer 2";
	}

	subgraph cluster_2 {
		color=white;
		node [style=solid,color=red2, shape=circle];
		a13 a23 a33 a43 a53;
		label = "layer 3";
	}

	subgraph cluster_3 {
		color=white;
		node [style=solid,color=seagreen2, shape=circle];
		O1 O2 O3 O4;
		label="layer 4";
	}

    x1 -> a12;
    x1 -> a22;
    x1 -> a32;
    x1 -> a42;
    x1 -> a52;

    x2 -> a12;
    x2 -> a22;
    x2 -> a32;
    x2 -> a42;
    x2 -> a52;

    x3 -> a12;
    x3 -> a22;
    x3 -> a32;
    x3 -> a42;
    x3 -> a52;

    a12 -> a13
    a22 -> a13
    a32 -> a13
    a42 -> a13
    a52 -> a13

    a12 -> a23
    a22 -> a23
    a32 -> a23
    a42 -> a23
    a52 -> a23

    a12 -> a33
    a22 -> a33
    a32 -> a33
    a42 -> a33
    a52 -> a33

    a12 -> a43
    a22 -> a43
    a32 -> a43
    a42 -> a43
    a52 -> a43

    a12 -> a53
    a22 -> a53
    a32 -> a53
    a42 -> a53
    a52 -> a53

    a13 -> O1
    a23 -> O1
    a33 -> O1
    a43 -> O1
    a53 -> O1

    a13 -> O2
    a23 -> O2
    a33 -> O2
    a43 -> O2
    a53 -> O2

    a13 -> O3
    a23 -> O3
    a33 -> O3
    a43 -> O3
    a53 -> O3

    a13 -> O4
    a23 -> O4
    a33 -> O4
    a43 -> O4
    a53 -> O4
}
```

### complicated case

```{.numberLines startFrom="1"}

digraph G {
    rankdir = LR;
    splines=false;
    edge[style=invis];  /* hide the edges */
    ranksep= 1.4;

    /* {...} specifies the scope of the node property */
    {
        node [shape=circle, color=yellow, style=filled, fillcolor=yellow];
        x0 [label=<x<sub>0</sub>>]; 
        a02 [label=<a<sub>0</sub><sup>(2)</sup>>]; 
        a03 [label=<a<sub>0</sub><sup>(3)</sup>>];
    }
    {
        node [shape=circle, color=chartreuse, style=filled, fillcolor=chartreuse];
        x1 [label=<x<sub>1</sub>>];
        x2 [label=<x<sub>2</sub>>]; 
        x3 [label=<x<sub>3</sub>>];
    }
    {
        node [shape=circle, color=dodgerblue, style=filled, fillcolor=dodgerblue];
        a12 [label=<a<sub>1</sub><sup>(2)</sup>>];
        a22 [label=<a<sub>2</sub><sup>(2)</sup>>];
        a32 [label=<a<sub>3</sub><sup>(2)</sup>>];
        a42 [label=<a<sub>4</sub><sup>(2)</sup>>];
        a52 [label=<a<sub>5</sub><sup>(2)</sup>>];
        a13 [label=<a<sub>1</sub><sup>(3)</sup>>];
        a23 [label=<a<sub>2</sub><sup>(3)</sup>>];
        a33 [label=<a<sub>3</sub><sup>(3)</sup>>];
        a43 [label=<a<sub>4</sub><sup>(3)</sup>>];
        a53 [label=<a<sub>5</sub><sup>(3)</sup>>];
    }
    {
        node [shape=circle, color=coral1, style=filled, fillcolor=coral1];
        O1 [label=<a<sub>1</sub><sup>(4)</sup>>];
        O2 [label=<a<sub>2</sub><sup>(4)</sup>>]; 
        O3 [label=<a<sub>3</sub><sup>(4)</sup>>]; 
        O4 [label=<a<sub>4</sub><sup>(4)</sup>>];
    }
    {
        rank=same;       /* Rank constraints on the nodes in a subgraph. */
        x0->x1->x2->x3;  /* specifies the relative position of the four nodes */
    }
    {
        rank=same;
        a02->a12->a22->a32->a42->a52;
    }
    {
        rank=same;
        a03->a13->a23->a33->a43->a53;
    }
    {
        rank=same;
        O1->O2->O3->O4;
    }
    a02->a03;  // prevent tilting
    l0 [shape=plaintext, label="layer 1 (input layer)"];
    l0->x0;
    {rank=same; l0;x0};
    l1 [shape=plaintext, label="layer 2 (hidden layer)"];
    l1->a02;
    {rank=same; l1;a02};
    l2 [shape=plaintext, label="layer 3 (hidden layer)"];
    l2->a03;
    {rank=same; l2;a03};
    l3 [shape=plaintext, label="layer 4 (output layer)"];
    l3->O1;
    {rank=same; l3;O1};
    edge[style=solid, tailport=e, headport=w];  /* let all the edges point to the same position. */
    {x0; x1; x2; x3} -> {a12;a22;a32;a42;a52};
    {a02;a12;a22;a32;a42;a52} -> {a13;a23;a33;a43;a53};
    {a03;a13;a23;a33;a43;a53} -> {O1,O2,O3,O4};
}
```

output:

```{.graph .center caption="complicated" fileName="graphviz_complicated"}

digraph G {
    rankdir = LR;
    splines=false;
    edge[style=invis];
    ranksep= 1.4;
    {
        node [shape=circle, color=yellow, style=filled, fillcolor=yellow];
        x0 [label=<x<sub>0</sub>>]; 
        a02 [label=<a<sub>0</sub><sup>(2)</sup>>]; 
        a03 [label=<a<sub>0</sub><sup>(3)</sup>>];
    }
    {
        node [shape=circle, color=chartreuse, style=filled, fillcolor=chartreuse];
        x1 [label=<x<sub>1</sub>>];
        x2 [label=<x<sub>2</sub>>]; 
        x3 [label=<x<sub>3</sub>>];
    }
    {
        node [shape=circle, color=dodgerblue, style=filled, fillcolor=dodgerblue];
        a12 [label=<a<sub>1</sub><sup>(2)</sup>>];
        a22 [label=<a<sub>2</sub><sup>(2)</sup>>];
        a32 [label=<a<sub>3</sub><sup>(2)</sup>>];
        a42 [label=<a<sub>4</sub><sup>(2)</sup>>];
        a52 [label=<a<sub>5</sub><sup>(2)</sup>>];
        a13 [label=<a<sub>1</sub><sup>(3)</sup>>];
        a23 [label=<a<sub>2</sub><sup>(3)</sup>>];
        a33 [label=<a<sub>3</sub><sup>(3)</sup>>];
        a43 [label=<a<sub>4</sub><sup>(3)</sup>>];
        a53 [label=<a<sub>5</sub><sup>(3)</sup>>];
    }
    {
        node [shape=circle, color=coral1, style=filled, fillcolor=coral1];
        O1 [label=<a<sub>1</sub><sup>(4)</sup>>];
        O2 [label=<a<sub>2</sub><sup>(4)</sup>>]; 
        O3 [label=<a<sub>3</sub><sup>(4)</sup>>]; 
        O4 [label=<a<sub>4</sub><sup>(4)</sup>>];
    }
    {
        rank=same;
        x0->x1->x2->x3;
    }
    {
        rank=same;
        a02->a12->a22->a32->a42->a52;
    }
    {
        rank=same;
        a03->a13->a23->a33->a43->a53;
    }
    {
        rank=same;
        O1->O2->O3->O4;
    }
    a02->a03;  // prevent tilting
    l0 [shape=plaintext, label="layer 1 (input layer)"];
    l0->x0;
    {rank=same; l0;x0};
    l1 [shape=plaintext, label="layer 2 (hidden layer)"];
    l1->a02;
    {rank=same; l1;a02};
    l2 [shape=plaintext, label="layer 3 (hidden layer)"];
    l2->a03;
    {rank=same; l2;a03};
    l3 [shape=plaintext, label="layer 4 (output layer)"];
    l3->O1;
    {rank=same; l3;O1};
    edge[style=solid, tailport=e, headport=w];
    {x0; x1; x2; x3} -> {a12;a22;a32;a42;a52};
    {a02;a12;a22;a32;a42;a52} -> {a13;a23;a33;a43;a53};
    {a03;a13;a23;a33;a43;a53} -> {O1,O2,O3,O4};
}

```

## others

### matrix

```{ caption="graphviz_matrix_code" .numberLines startFrom="1"}

digraph matrix {
    723->722
    505->732
    729->732
    731->730->729
    726->729
    730->726
    726->810->725
    729->810->725
    729->733->792->793
    722->731
    732->737->736->733
    733->810->725
    729->505
    736->506
    505->506
    179->759
    759->725
    759->737
    759->769->768->778
    768->303
    737->739->736->778
    736->769
    778->303
    506->303
    769->506
    769->780
    778->779
    736->773->774->779->780
    779->303
    780->303
    506->780
    505->724
}

```

output:

```{.graph .center caption="graphviz matrix" .numberLines startFrom="1" fileName="graphviz_matrix"}

digraph matrix {
    723->722
    505->732
    729->732
    731->730->729
    726->729
    730->726
    726->810->725
    729->810->725
    729->733->792->793
    722->731
    732->737->736->733
    733->810->725
    729->505
    736->506
    505->506
    179->759
    759->725
    759->737
    759->769->768->778
    768->303
    737->739->736->778
    736->769
    778->303
    506->303
    769->506
    769->780
    778->779
    736->773->774->779->780
    779->303
    780->303
    506->780
    505->724
}
```

# References

#. http://www.nrstickley.com/pandoc-markdown

#. http://github.com/qrsforever/git-blog-setting

#. https://zhu45.org/posts/2017/May/25/draw-a-neural-network-through-graphviz
