## Idioms For Statistics

Sometimes we have a statistical collection of samples-
* Many samples from a set of experiments
* many rows in a spreadsheet<br>

Each such sample may stand alone or be an entire time series.


Sometimes the values are inherently statistical-
* values that come with estimates and variances
* or parameter sets, each defining a statistical distribution


We can always plot the value with error bars, but that may not be adequate.
If possible we want to give more information, up to showing the entire
distribution.



## Anscombe's quartet

If someone hands us a bunch of x-y samples,
natural laziness pushes us in the direction of just doing the regression,
presenting the regression parameters, and declaring victory.

But sometimes this misses critical details.


Anscombe, F. J. (1973). "Graphs in Statistical Analysis".
American Statistician. 27 (1): 17&ndash;21. doi:10.1080/00031305.1973.10478966.
JSTOR 2682899.

There is a copy in the data directory for the course.


These are four datasets with identical (to 2 decimal places or better):
* mean of x and y
* variance of x and y
* correlation
* linear regression line
* $ R^2 $ of the linear regression


But they are very different.<br>
<span class='smalltext'>Anscombe.svg: Schutz, CC BY-SA 3.0, via Wikimedia Commons</span><br>
<span class='image60'>![](https://upload.wikimedia.org/wikipedia/commons/e/ec/Anscombe%27s_quartet_3.svg)</span>


See also the *Datasaurus Dozen*
<span class='image60'>[![Datasaurus Dozen](https://damassets.autodesk.net/content/dam/autodesk/research/publications-assets/images/AllDinosGrey_1.png)](https://www.autodesk.com/research/publications/same-stats-different-graphs)</span>


This sort of situation is what makes visualization such a critical tool for
scientists and data analysts.  Fortunately there are tools available that
will quickly generate very revealing graphics for this sort of data.  Seaborn
is one such tool.



## Let's get familiar with Seaborn

[Seaborn](https://seaborn.pydata.org/)
is another python plot library built on top of matplotlib.
It has *layered grammar of graphics* concepts in a Pythonic interface,
and some very nice features for statistical data.


```python
import pandas as pd
import seaborn as sns
anscombes_df = pd.read_csv('/path/to/data/anscombes_quartet.csv')
sns.set_theme()  # default theme
sns.lmplot(data=anscombes_df, x='x', y='y',
           col='dataset')
```
![abscomb datasets drawn with seaborn](images/abscomb_by_seaborn.png)


Seaborn's *lmplot* plots linear models.  In this case we see the regression
line and the standard deviation of the regression line- and that is a deep
enough anlysis that the differences between the datasets become visible.


Why is it "sns"?  It turns out to be an old joke, a reference to
[Sam Seaborn](https://pbs.twimg.com/media/C3C6q1ZUYAALXX0.jpg), a
character played by Rob Lowe on _The West Wing_ .



## Seaborn's [Tutorial](https://seaborn.pydata.org/tutorial.html) is *Excellent*
We are going to look at several notebooks that demonstrate some of
the material there, but you should read through the tutorials.


Do a 'git pull' of your clone of the class GitHub repo, and follow as
we look at:
* **seaborn_mechanics.ipynb** to straighten out graph display on your system
* **seaborn_themes.ipynb** to introduce Seaborn's styling interface
* **review_categorical_data.ipynb** deals with handling categorical data in pandas
