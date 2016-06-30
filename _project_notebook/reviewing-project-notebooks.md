
# Reviewing Project Notebooks

## Existing Open Project Notebooks

- [John Stanton-Geddes][js1] ([source][js3])
- [Madsen Lab][ml1] ([source][ml2])
- [Carl Boettiger][cb1] ([source][cb4])

Carl Boettiger could be considered the pioneer in electronic lab notebooks and
is by far the most popular (as seen by various google searches). I recommend
reading his [introduction to his lab notebook][cb3]. I particularly like his
disclaimer:

> Welcome to my open lab notebook. This is the active, permanent record of my scientific research, standing in place of the traditional paper bound lab notebook. The notebook is primarily a tool for me to do science, not communicate it. I write my entries with the hope that they are intelligible to my future self; and maybe my collaborators and experts in my field. Only the occasional entry will be written for a more general audience. […] In these pages you will find not only thoughts and ideas, but references to the literature I read, the codes or manuscripts I write, derivations I scribble and graphs I create and mistakes I make. 

Similarly John Staton-Geddes also has a very benefitial [first post][js2].
Marin Fenner [discusses][mf1] how to include *.docx* and other common file type in a
git repository (his website is also using jekyll, [source][mf2]).

## Scripts or Tools used to develop notebook

Most of the project notebooks reviewed use [Jekyll][jek1] to develop the pages
for their notebooks. Mr. Boettiger provides an introduction on how to start a
notebook with a static website provided by jekyll ([Learning Jekyll][cb2]). He
recommends another [introduction][jek2] for details about jekyll.

Madsen used scripts "newlabnote.sh" and "newblogpost.sh" to create new files into
the right location with the right name, and contain the base template.

## Conclusion

It would seem that I should create a jekyll static website to aid in the navigation
of my notebook. I will refer to a [github repository][fs1] that is designed to be a
template for a project notebook ([template is hosted][fs2]). I am choosing not
to simply fork the repository because I want to start simple and build it from
the foundation up with a good understanding of what everything does.



[jek1]: https://github.com/jekyll/jekyll
[jek2]: http://jekyllbootstrap.com/lessons/jekyll-introduction.html

[js1]: http://johnstantongeddes.org/labnotebook.html
[js2]: http://johnstantongeddes.org/open%20science/2013/03/13/first-post.html
[js3]: https://github.com/johnstantongeddes/johnstantongeddes.org

[cb1]: http://www.carlboettiger.info/lab-notebook.html
[cb2]: http://www.carlboettiger.info/2012/12/30/learning-jekyll.html
[cb3]: http://www.carlboettiger.info/2012/09/28/Welcome-to-my-lab-notebook.html
[cb4]: https://github.com/cboettig/cboettig.github.io

[ml1]: http://notebook.madsenlab.org/labnotebook.html
[ml2]: https://github.com/mmadsen/lnraw

[mf1]: http://blog.martinfenner.org/2014/08/25/using-microsoft-word-with-git/#
[mf2]: https://github.com/mfenner/mfenner.github.io

[fs1]: https://github.com/fdschneider/jekyll-lablog
[fs2]: https://fdschneider.github.io/jekyll-lablog/index.html