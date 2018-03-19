---
title: Jupyter Lab
tag: Jupyter
header: 
    image: /assets/images/jupyter.png
    caption: "Credit: Project Jupyter"
classes: wide
---

Jupyter Lab got it's [Beta release](https://blog.jupyter.org/jupyterlab-is-ready-for-users-5a6f039b8906?gi=b36b7db07c5c)
a couple weeks back. I've been playing with it and can't ever go back to plain
old notebooks again. While it's technically still in Beta, I've replaced
Jupyter Notebook with JupyterLab as my main programming environment, it is that
stable. 

Imho it makes Jupyter a full featured development environment. I found
notebooks a little clunky for typical use cases- check cpu/gpu usage, error
logs, view data files, use the terminal for whatever reason. So I defaulted to
using vim and tmux. With Lab, I get all of that from the same app + other goodies.

A few of the said goodies:
- Reconfigurable panes for notebooks, editors, consoles, terminals and "views"
- "Views" of files that update in real time. The files in question can be csvs,
  scripts, what have you. The framework doesn't care!
- Bind a text editor to the console that sort of emulates my
  vim+tmux+[vim-slime](https://github.com/jpalardy/vim-slime) setup
- Fullscreen mode for any pane
- More powerful extention system- there's a
  [plugin](https://github.com/jupyterlab/jupyterlab-google-drive) that allows
  real-time collaboration via Google Drive!

## Screenshots

<figure class="align-center" style="width:1200px">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/jupyterlab/jupyterlab_sc1.png" alt="">
  <figcaption>Multiple panes- file browser, notebook, terminal</figcaption>
</figure> 

<figure class="align-center" style="width:1200px">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/jupyterlab/jupyterlab_sc2.png" alt="">
  <figcaption>Multiple views of the same notebook, one with code and the other with results</figcaption>
</figure> 


<figure class="align-center" style= "width:1200px">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/jupyterlab/jupyterlab_sc4.png" alt="">
  <figcaption>Tie an editor to a console window. And view large csvs, no sweat!</figcaption>
</figure> 

<figure class="align-center" style= "width:600px">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/jupyterlab/jupyterlab_sc3.png" alt="">
  <figcaption>Autosuggest now provides useful type information</figcaption>
</figure> 


## Conclusion

Read more about it
[here](https://blog.jupyter.org/jupyterlab-is-ready-for-users-5a6f039b8906?gi=b36b7db07c5c)
and if you have more time, [this video](https://www.youtube.com/watch?v=w7jq4XgwLJQ) by the 
devs is a great walk-through of the nifty new features.

**Try it live with [Binder](https://mybinder.org/v2/gh/jupyterlab/jupyterlab-demo/18a9793b58ba86660b5ab964e1aeaf7324d667c8?urlpath=lab%2Ftree%2Fdemo%2FLorenz.ipynb)**

*Update:* If you're planning on setting up a Juypter server for remote
access, look at my newer [post]({{ site.baseurl }}{% post_url 2018-03-07-jupyterserver %})
