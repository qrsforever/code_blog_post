---

title: Graphviz绘图

date: 2019-07-24 14:00:24
tags: [How]
categories: [Tutorial]

---

[Graph Visualization Software](https://graphviz.gitlab.io/ "高效而简洁的绘图工具graphviz")

# Requirements

```shell
sudo apt install pandoc
sudo apt install graphviz
sudo apt install dot2tex
sudo pip3 install pygraphviz pandocfilters
```

# graphviz.py

```python
#!/usr/bin/python3

import os
import sys

import pygraphviz

from pandocfilters import toJSONFilter, Para, Image, get_filename4code
from pandocfilters import get_caption, get_extension, get_value

top_path = os.path.abspath(os.path.dirname(__file__))
dir_name = 'graph-image'
git_path = 'https://raw.githubusercontent.com/qrsforever/assets_blog_post/master/'

def graphviz(key, value, format, meta):
    if key == 'CodeBlock':
        [[ident, classes, keyvals], code] = value
        if "graph" in classes:
            caption, typef, keyvals = get_caption(keyvals)
            prog, keyvals = get_value(keyvals, u"prog", u"dot")
            filetype = get_extension(format, "svg", html="svg", latex="pdf")
            # dest = get_filename4code(graph, code, filetype)
            filename, _ = get_value(keyvals, u"filename")
            if filename is None:
                sys.stderr.write('not set filename in {}\n')
                exit(-1)
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
                    remotepath = os.path.join(git_path, filepath)
                dir = os.path.dirname(localpath)
                if not os.path.isdir(dir):
                    os.makedirs(dir)
            except Exception as e:
                sys.stderr.write('{}: not found datapath in meta, please patch/run.sh\n'.format(e))
                exit(-1)

            g = pygraphviz.AGraph(string=code)
            g.layout()
            g.draw(localpath, prog=prog)
            sys.stderr.write('Local Path [' + localpath + ']\n')
            sys.stderr.write('Remote Path [' + remotepath + ']\n')

            image = Image([ident, classes, keyvals],
                          caption,
                          [remotepath, typef])

            return Para([image])

if __name__ == "__main__":

    toJSONFilter(graphviz)
```

# Demo

```{.graph .center caption="caption1" filename='test'}

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

# References

1. http://www.nrstickley.com/pandoc-markdown/
