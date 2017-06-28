
# Topic Words in Context (TWiC)

## Table of Contents


* [Overview](#overview) 
* [Data Shapes](#datashapes)
  * [Introduction](#datashapes_introduction)
  * [Topic Bullseyes](#datashapes_topicbullseyes)
  * [Topic Rectangles](#datashapes_topicrectangles)
  * [Topic Words](#datashapes_topicwords)
* [Panels](#panels)
  * [Introduction](#panels_introduction)
  * [Topic Bar](#panels_topicbar)
  * [Data Bar](#panels_databar)
  * [Corpus View](#panels_corpusview)
  * [Corpus Cluster View](#panels_corpusclusterview)
  * [Text Cluster View](#panels_textclusterview) 
  * [Text View](#panels_textview)
  * [Publication View](#panels_publicationview)
* [How to Use](#howtouse)
  * [Server-side - Python Script](#howtouse_python)
  * [Client-side - D3 Visualization](#howtouse_visualization)
  * [Client-Side - Alternate D3 Visualization for Publication Arrangements](#howtouse_publicationview)
  * [Custom Levels](#howtouse_customlevels)
* [The Emily Dickinson Corpus Example](#dickinson) 

<br/>
["overview"]
## Overview

  Thanks for trying out **Topic Words in Context**, a.k.a. **TWiC**.
  
  TWiC is a data visualization created by [Jonathan Armoza](https://www.twitter.com/jonathangrams) that provides hierarchical, top-down and bottom-up views of LDA topic models generated by the popular topic modeler, [MALLET](http://mallet.cs.umass.edu). It was developed under the advisement and support of [Stéfan Sinclair](https://www.mcgill.ca/langlitcultures/faculty/full-time-faculty-0/stefan-sinclair) and [Peter Gibian](https://www.mcgill.ca/english/staff/peter-gibian) at [McGill University](http://www.mcgill.edu).

  TWiC was born out of the need for digital humanities researchers to understand how the topic word list outputs from topic modelers like MALLET relate to the texts they attempt to model.
  
  The visualization presents users with multiple related views in the form of reorganizable and resizable [panels](#panels), each of which portray how topics are distributed throughout the collection of texts (a.k.a. "corpus") being modeled. These views look from all the way above the model – the "big data" view – and dive downward to where topic words are situated in their original contexts: the model's texts themselves. Geometric icons, known in TWiC as [data shapes](#datashapes) represent the model data (topic distributions, clusters of texts, texts, and topic words) of each view. The primary view of topic model data in TWiC – what is termed a "level" in TWiC – shows a topic model from the perspective of the model data, that is, how the model situates texts by topic. However, TWiC also has the ability to reinsert these data shapes into original publication arrangements. This alternate level shares a few panels with TWiC's primary level, but its focus is a panel called the [Publication View](#panels_publicationview).
  
  TWiC has "server"- and "client"-side components. It consists of custom Python scripts (server-side) which run MALLET and re-interpret its outputs, and a web browser visualization (client-side) which uses [D3](https://d3js.org/), [jQuery](https://jquery.com/), and [Packery](http://packery.metafizzy.co/) as well as a custom JavaScript library that organizes and controls TWiC's data shapes and panels. 
  
  The basic workflow for viewing a text collection with TWiC involves:
  
  1. `Inserting your text collection into TWiC's input text folder`
  2. `Running the main Python script`
  3. `Starting a local server in TWiC's root directory`
  4. `Viewing "twic.html" in a web browser (via that server)`
  
After TWiC's main Python script has been run once, there is generally no need to run it again unless you wish to change the topic model. For more detailed instructions on how to use TWiC, see the [How to Use](#howtouse) section below.

<br/>
["datashapes"]
## Data Shapes

["datashapes_introduction"]
### Introduction

In order to aid the exploration of the data of a topic model, TWiC uses colored shapes and words in a visually suggestive ways to help users understand the relationships between words, texts, and topics. These **data shapes** are described below.

["datashapes_topicbullseyes"]
### Topic Bullseyes
<br/>
![](/docs/images/twic_datashapes_bullseye_medium.png)

The shape above is one of two primary means by which topic distribution is visualized in TWiC: a geometric abstraction of the top N topics of the texts being represented. In TWiC this is called a **topic bullseye**. The one above would represent the top ten topics of the average topic distribution of a cluster of texts or of the entire corpus. Each ring represents a topic and is given the unique color assigned to each topic of the topic model (as seen in the [Topic Bar](#panels_topicbar). Moving inward from the outside of the shape toward the center, higher weighted topics are represented, until one arrives at the center circle, or, the top topic of the topic distribution.

Mousing over each ring/center circle lowers the shade of the other rings of the bullseye, and triggers highlighting of shapes and words of that topic it represents throughout the rest of the open panels in TWiC. Clicking on a ring/center circle freezes highlighting for the topic it represents. To highlight another topic in the bullseye when highlighting is frozen, another ring can be clicked. To stop highlighting all together, just click in the panel outside of the shape entirely.

["datashapes_topicrectangles"]
### Topic Rectangles
<br/>
![](/docs/images/twic_datashapes_rectangle.png)

The other primary data shape in TWiC is the **"topic rectangle"**. It utilizes the same paradigm as the topic bullseye, where outer rings (in this case, outer rectangular rings) represent the top N topics of a topic distribution. In the case of a topic rectangle though, it represents the topic distribution for an individual text. This data shape is links topics with the texts they model. The rectangle above would represent the top five topics of one text. At the center of the topic rectangle is a miniature, programmatically-derived view of the first ten lines of the text itself. Its topic words are represented by small colored rectangles in the order in which they would appear in the actual text. Uncolored rectangles (in a light goldenrod) represent the stopwords which have not been considered for the topic model by MALLET.

Mouse interaction with topic rectangles functions the same way as it does for [topic bullseyes](#datashapes_topicbullseyes).

["datashapes_topicwords"]
### Topic Words

TWiC gives a unique color to each of the topics of a topic model. Each of those topics are given a weight per each of the texts of a collection being modeled. Topics themselves consist of words that are also assigned different weights – or in other words, a ranking of their importance to a particular topic. In the actual implementation of topic modeling, all words of the text collection being modeled are in every topic. However, words are more frequently featured together in those texts are given higher weights. (Other words that are not actually used in that topic are assigned an evenly split portion of a dummy weight, so as not to assign them a weight of zero.)

![](/docs/images/twic_datashapes_topicwords_topic.png)

Topic words are featured in several places in TWiC. The top words of a topic appear in the topic word lists of the [Topic Bar](#panels_topicbar). This is the standard view in which prominent topic words are often seen when viewing the data outputs of a topic model. In the Topic Bar, topics can be clicked on, triggering and freezing highlighting of that topic throughout TWiC's panels. Clicking on the topic again unhighlights all.

![](/docs/images/twic_datashapes_topicwords_textview.png)

In TWiC however, topic words are seen in two other meaningful perspectives. They appear in their original textual contexts in the [Text View](#panels_textview). The point of this vantage, the eponymous view of the visualization, is to understand human conceptual models mesh with the more collection-comprehensive topic judgments made by the topic modeler. 

Mousing over topic words will lower the color shade of the rest of the topic words not in that particular topic (as well as the stop words), highlighting all instances of words of topic in that text as well as data shapes of that topic in the other panels. Clicking on those words will freeze topic highlighting, and clicking on them again or outside of the text's words will unfreeze highlighting. Like topic bullseyes and rectangles, clicking on other topic words while highlighting is frozen will just switch the highlighting to the topic of that other word.

![](/docs/images/twic_datashapes_topicwords_databar.png)

The other place where topic words are featured in TWiC is in its [Data Bar](#panels_databar). When a topic word is clicked on in that individual text view, the top 100 words of a topic appears in the "Data Bar." This takes users to a microscopic view of the model, and one less typically featured, especially in respect to the topic distribution of texts and the in-context author placements of those topic words. In essence, this other vantage of topic words allows users to understand how important those words are to the topics themselves. (Mouse interactions with topic word weights in the Data Bar are currently under development.)

<br/>
[panels]
## Panels

["panels_introduction"]
### Introduction
![](/docs/images/twic_panels_initialview_medium.png)

<br/>
  TWiC's initial view begins with its highest level panel, the [Corpus View](#panels_corpusview), alongside two information-oriented panels, the [Topic Bar](#panels_topicbar) and [Data Bar](#panels_databar). Double clicking shapes like the ["bullseye"](#datashapes_topicbullseyes) above will open new views until you arrive at TWiC's "lowest" view, the [Text View](#panels_textview) where topic words are seen as embedded in individual texts. As each new view is revealed, the current panels will be repositioned and resized as part of an initial set of animations meant to represent a sort of "drilling-down" into the topic model. Any panel however can always be repositioned and resized as you see fit. Each successive view is then considered part of the larger whole of the previous view, or to put it another way, a subset of texts "underneath" the previous view. Below is a view of TWiC with all panels open for viewing the topic model from its own perspective (see the [Publication View](#panels_publicationview) for more on TWiC's alternate perspective on topic models).
  
<br/>
![](/docs/images/twic_panels_allpanelsopen_medium.png)  

<br/>
  TWiC also uses mouseover of its shapes to enact highlighting of topic-related objects across panels. Clicking on the highlighted portion of a shape or topic word list freezes highlighting so the topic relations can be viewed indefinitely. To "unclick" and resume mouseover highlighting, click in an open space outside of TWiC's data shapes.
  
<br/>
![](/docs/images/twic_panels_initialview_highlighted_medium.png)


![](/docs/images/twic_panels_allpanelsopen_highlighted_medium.png)

<br/>
  At the top left of each panel is an Mac OSX-like stoplight window control. These can be used to resize or hide panels. The red button hides and unhides panels, the yellow reduces panels to their smallest possible dimensions, and the green increases the window to the maximum size available in your web browser window. In addition, TWiC panels can be resized just like operating system windows from their edges and corners.
  TWiC panels may be freely dragged around the browser window. A JavaScript library called [Packery](http://packery.metafizzy.co/) reorganizes the other panels as this dragging action occurs to try and fill the available space with panels as best as possible. (**NOTE:** Dragging a panel off the screen and releasing the mouse will reorganize the panels if Packery's algorithm has set their positions too awry.)

<br/>
![](/docs/images/twic_panels_stoplight.png)

<br/>
  Panels are divided into two types: "graphical" and "informational." Before taking a look at TWiC's different graphical panels, let's take a look at its two informational panels.

<br/>
["panels_topicbar"]
### Topic Bar

![](/docs/images/twic_panels_topicbar_medium.png)

The best place to understand why a tool like TWiC became necessary is to examine its **Topic Bar** panel. The Topic Bar simply lists what the majority of topic model users expect to see at the end of a modelling run: lists of correlated topic words known as "topics." Even with the known weightings for each topic in a collection of texts, the reason for topic word correlation can still be quite opaque, especially if one comes to a collections of texts with little advanced knowledge of it. 

TWiC uses the unique numeric IDs assigned to topics by MALLET (0 through the number of topics in the model minus 1). Each topic is also assigned a unique color by TWiC to help visually distinguish different topics. These unique colors for each topic are also used by data shapes and topic words in the other panels to facilitate visual topic correlation across panels.  ([User provided color palettes coming soon](https://github.com/jarmoza/twic/issues/72).)

Though mouseovers are disabled in this view to facilitate scrolling through the topic list, each topic can be clicked and unclicked to freeze and unfreeze highlighting throughout the visualization, respectively. 

The Topic Bar does not feature the OSX-like resize controls, but it can be resized via its edges and corners.

<br/>
["panels_databar"]
### Data Bar

![](/docs/images/twic_panels_databar_small.png)

The **Data Bar** panel is the first, new and integral integral concept TWiC introduces to its users, though it might not appear so at first the most visually distinguished of TWiC's panels. The Data Bar is a context sensitive view of the currently selected data shape. Clicking on [data shapes](#datashapes) will reveal new metadata in the Data Bar. It reveals the relative prevalence of the characteristics of data shapes in each panel.

Listed by panel/data shape, the Data Bar will reveal the following:

* **[Corpus View](#panels_corpusview)/[Topic Bullseye](#datashapes_topicbullseyes):** 
  * Number of texts in the corpus
  * The average topic distribution of the corpus (the topic bullseye at panel center)
* **[Corpus Cluster View](#panels_corpusclusterview)/[Topic Bullseyes](#datashapes_topicbullseyes):**
  * Number of texts in a corpus-level cluster of texts (a topic bullseye)
  * The similarity (calculated distance) of the topics of a corpus-level cluster of texts to the corpus's average topic distribution
  * The average topic distribution of clusters of related texts (topic bullseyes in the panel)
  * The average topic distribution of the corpus (the topic bullseye at panel center)
* **[Text Cluster View](#panels_textclusterview)/[Topic Rectangles](#datashapes_topicrectangles):**
  * Number of texts in the current text cluster
  * The similarity (calculated distance) of the topics of a text to the current text cluster's average topic distribution
  * The topic distribution of an individual text (topic rectangles in the panel)
  * The average topic distribution of the text cluster (the topic bullseye at panel center)
* **[Text View](#panels_textview)/[Topic Words](#datashapes_topicwords):**
  * The topic-level weights of the words of a topic
  * The topic-level weight of the clicked word
* **[Publication View](#panels_publicationview)/[Topic Rectangles](#datashapes_topicrectangles):**
  * The same data as the Text Cluster View (albeit text rectangles arranged via "publication arrangment" from a separate, custom JSON file)

For more on these panels and data shapes, click the links above or read on.

["panels_corpusview"]
### Corpus View

<br/>
![](/docs/images/twic_panels_corpusview_small.png)

The Corpus View is one of the simplest panels in TWiC, but it also represents the topic model weighting that is most readily available when one runs MALLET from the command line: the topic weights situated alongside the topics in the output topic keys file. 

The [topic bullseye](#datashapes_topicbullseyes) at its center represents the average topic distribution of all of the texts in the modeled corpus. Therefore, as one mouses over the bullseye towards its center, users are observing higher-weighted topics in the model – on average. Double clicking on the bullseye will reveal clusters of texts "underneath" this average topic distribution in the next panel, the [Corpus Cluster View](#panels_corpusclusterview). For more details on mouse interactions with bullseyes, see the [data shapes section](#datashapes).

If you look at the contents of the [Data Bar](#panels_databar) when the panel's bullseye has been clicked, you will note a fairly even distribution in the topic weight percentages. The Data Bar will also show you the number of texts that have been modeled as well.

["panels_corpusclusterview"]
### Corpus Cluster View

<br/>
![](/docs/images/twic_panels_corpusclusterview_medium.png)

The Corpus Cluster View is where TWiC starts to separate pieces of the model so users can see a topic model from an entirely new perspective. Though this is not the only way to split up the texts of the model, TWiC currently clusters texts by their top weighted topics. 

In the panel, most topic bullseyes represent these clusters and the top N topics of the average topic distribution of those texts. The numeric topic ID assigned by MALLET floats beneath each bullseye. This number indicates the top topic each cluster represents. At the center of the panel is the same topic bullseye from the [Corpus View](#panels_corpusview). 

The rest of the topic bullseyes are set at a distance from that center/corpus average bullseye like graph nodes. A graph edge connects each to the central bullseye. This is the ["Jensen-Shannon"](https://en.wikipedia.org/wiki/Jensen%E2%80%93Shannon_divergence) distance between the cluster's average topic distribution and the corpus's. Currently due to D3's force directed graph code and TWiC's collision detection code, not all bullseye's will be perfectly spaced, but clicking on a cluster bullseye will show the exact distance in the [Data Bar](#panels_databar). 

When clicked, a cluster bullseye's average topic distribution will also be shown in the Data Bar as well. You will note that the average topic distributions shown in the Data Bar is expectedly skewed toward the topic the cluster bullseye represents. 

<br/>
![](/docs/images/twic_panels_corpusclusterview_highlighting_medium.png)

And when mousing/clicking over the bullseyes you will notice that other rings representing the same topic in other bullseyes in the panel will become highlighted along with their graph edges (with the color of the top topic of each cluster bullseye). Double clicking on any of these cluster bullseyes (sans the center one) will reveal the cluster of texts each represents in the [Text Cluster View](#panels_textclusterview).

["panels_textclusterview"]
### Text Cluster View

<br/>
![](/docs/images/twic_panels_textclusterview_medium.png)

The primary TWiC level takes the perspective of the topic model, but the Text Cluster View is the first panel which explicitly addresses the link between the topic model and its individual texts. Each text is represented as a graph node set at a distance from the average topic distribution of this group of texts. 

In the previous panel, the [Corpus Cluster View](#panels_corpusclusterview), these texts were represented by a [topic bullseye](#datashapes_topicbullseyes). That bullseye is now at the center of this panel's graph, much like the corpus average topic distribution bullseye of the [Corpus View](#panels_corpusview) was at the center of the Corpus Cluster View panel. 

<br/>
![](/docs/images/twic_panels_textclusterview_highlighting_medium.png)


The connections visualized by TWiC's highlighting however, now explicitly shows where the model feels texts are connected by their prevalent (or highly-weighted) topics.

This panel contain a slightly different data shape, the ["topic rectangle"](#datashapes_topicrectangles). Topic rings are now rectangular and the rectangles contain miniaturized visuals of the first ten lines of each text. Each line contains stopwords and topic words, represented by small rectangles, each colored according to the colors assigned to topics in all other panels.

These shapes and the panel utilize the same data-spatial paradigms of the "Corpus Cluster View," with mouseover highlighting and clicking. Clicking on each topic rectangle will reveal the topic distribution of that text as well as its distance from the text cluster's average topic distribution in the [Data Bar](#panels_databar). Double clicking any of the topic rectangles will reveal the text itself in the panel "underneath" this one: the [Text View](#panels_textview). 

["panels_textview"]
### Text View

<br/>
![](/docs/images/twic_panels_textview.png)

The Text View is TWiC's eponymous panel, and is one of its most important panels, particularly for those interested in the authorial contexts of topic words. This panel shows topic words and stopwords in the original order of the texts being modeled. Before one would sit with the topic word lists as seen in the [Topic Bar](#panels_topicbar) and try to determine a reasonable, general semantic association for these words. The Text View aids such a process, and really expands the view of such a experiential, human-conceptual model by showing where those topic words actually live.

Mousing over topic words extends highlighting throughout all of TWiC's panels, but also highlights words of the same topic within a text. (Words not in that topic are lowered a shade to produce this highlighting effect.) Clicking functions similarly to other panels, freezing topic highlighting. However, clicking on each topic word will reveal the topic wordweight distribution in the [Data Bar](#panels_databar), revealing the relative importance (or weighting) for each word to the rest of the top-weighted words of topic.

["panels_publicationview"]
### Publication View

![](/docs/images/twic_panels_publicationview_medium.png)

The Publication View panel (seen in the top left of the image above) exists in TWiC's alternate level. It utilizes a custom JSON file format built by users to re-situate topic model data into the original publication arrangements – or really, any arrangement the user sees fit. (See the ["How to Use" section](#howtouse_publicationview) for details on how to implement and access this view.)

This alternate level contains four panels: the "Publication View," "Text View," "Data Bar," and "Topic Bar." The Publication View contains ["topic rectangles"](#datashapes_topicrectangles) that each represent an individual text and the top topics of its topic distribution.

Mousing over these rectangles highlights the common topic rings surrounding each rectangle, and thus shared prevalent topics of the texts. Clicking them freezes/unfreezes highlighting as they function in the [Text View](#panels_textview), and double clicking them will produce the actual words of the text in the Text View panel. The [Data Bar](#panels_databar) and [Topic Bar](#panels_topicbar) function as they do with the panels in TWiC's primary, topic model based level described above.


<br/>
["howtouse"]
## How to Use

<br/>
Though the TWiC visualization primarily operates through the web browser, it must first be set up by running a Python script that topic models the user's text collection and then re-interprets the model into  intermediate JSON data files usable by TWiC.

Don't worry though. This setup step just requires a familiarity with how to access and use the terminal/command line on your computer (and an installation of [Python](https://www.python.org/downloads/) – preferably version 2.7, which comes installed by default on OSX). Any advanced script options will be included after the basic steps for running TWiC.

["howtouse_python"]
### Server-side Python

#### Basic Steps

Steps **1-3** below only need to be done when first setting up TWiC. Steps **4-5** however will need to be run whenever you want to rerun the modeler or adjust its parameters.

1. **Download** TWiC from github.com/jarmoza/twic.

2. **Unzip** the download to where you want the TWiC folder. (This will unzip as a folder called `twic-master`. You can rename or place it wherever you would like. Just recall that whenever I refer to a folder named `twic` below, this is the folder I'm referring to.)

3. **Make MALLET** 

    i. At the terminal/command line navigate to where you placed the `twic` folder.
  
    ii. Change directory (`cd`) to the `lib/mallet` folder.
    
    iii. Run `make` in that folder (If you have issues building MALLET with `make`, see the [Notes section](#howtouse_python_notes).

4. **Edit** the file `twic_config.yaml` with a text editor.
	
	i. _(Required)_ Set the path for the text collection field (ex. `user_source_path: /Users/MyUserName/MyCorpusFolder/`).
	
	ii. _(Optional but recommended)_ Set the number of topics (ex. `num_topics: 100`) and number of intervals for the topic modeler (ex. `num_intervals: 1000`).
	
	iii. _(Optional but recommended)_ You can also set TWiC's text chunk size (ex. `text_chunk_size_words: 5000`). This will split any large texts in your collection every 5000 words or at the end of the most recent paragraph. Splitting texts into chunks is part of recommended best practices when modeling large texts.
	
	iv. _(Optional)_ Set the full and short corpus names. The full name is how your text will be referred to in the visualization (ex. `corpus_full_name: My Collection Name`. The short name is just an internal identifier for TWiC and MALLET (ex. `corpus_short_name: mycollection`). This will be the name of your MALLET output files, located in `twic/data/output/mallet`.
	
5. **Run** TWiC's main Python script `twic_corpus2vis.py`.

	i. At the terminal/command line navigate to where you placed the `twic` folder.
	
	ii. Change directory (`cd`) to the `code/py/general` folder.
	
	iii. Run `python twic_corpus2vis.py gmi`. All options for this script are described [below](#howtouse_python_options), but _g_ tells the script to copy texts from the YAML-supplied source folder, and _m_ and _i_ tell the script to 'run _M_ALLET' and '_i_nterpret' its output files, respectively.
  
	
**With that done, you are ready to launch TWiC in your web browser!** 

["howtouse_python_notes"]
#### Notes

Sometimes there are issues building MALLET via the `make` command. Build errors are usually attributable to the version of Java on your system. You may need to download the [Java Development Kit](http://www.oracle.com/technetwork/java/javase/downloads/index.html). You can, however, generally ignore "warnings" generated from building MALLET.
 
Running `twic_corpus2vis.py` can take a considerable amount of time, depending on the size of the corpus you are asking MALLET to model. Once modeling is complete however, the script that reinterprets MALLET's outputs executes comparatively more quickly.

A detailed descriptions of the user options for TWiC's main Python script, `twic_corpus2vis.py` follow below.
 
 **'g' - Gather texts**
 
 This option copies txt files from the folder listed as `user_source_path` (in `twic_config.yaml`) to TWiC's corpus source folder, `twic/data/input/txt`.
 
 **'k' - Keep corpus source files** 
 
 This option can only be used in conjunction with option '_g_'. Using this option keeps current txt files in TWiC's corpus source folder, `data/input/txt`. Otherwise, using the '_g_' option will delete the current txt files that folder, and then copy files from the folder listed as `user_source_path` (in `twic_config.yaml`) to that folder.
 
 **Note:** TWiC does not delete files from the supplied `user_source_path` field.

**'c' - Clear old MALLET output files**

This option deletes MALLET output files stored in `twic/data/output/mallet` from previous runs of MALLET (called by `twic_corpus2vis.py` using the '_m_' option.

**'m' - Run MALLET**

This option runs MALLET using the parameters set in `twic_config.yaml` in the `twic` folder. Current MALLET parameters include `num_topics`, `num_intervals`. Other current parameters include `text_chunk_size_words`. 

Any additional MALLET parameters can be used however. Users can file a change request at github.com/jarmoza/twic/issues, or can be added in `twic_malletscript.y` found in the `twic/code/py/general/` folder – see methods `ImportDir()` and `TrainTopics()` in the `TWiC_MalletScript` class.

Another alternative to altering TWiC's script, is to run MALLET separately using your own parameters. Users can place MALLET's output files in the `twic/data/output/mallet` folder, and then run `twic_corpus2vis.py` with the _i_ option. This will require some knowledge of MALLET's parameters (one could also look at TWiC's `ImportDir()` and `TrainTopics()` methods for examples), but in any case, TWiC requires the files output by these MALLET parameters: `--output-state`, `--output-doc-topics`, `--output-topic-keys`, and `--topic-word-weights-file`.

**'i' - Interpret MALLET output files**

This option runs the script to interpret MALLET's keys, topics, state, and word weight files into TWiC's intermediate JSON files used for the D3 visualization component.

If users are interested in understanding how TWiC breaks down and re-interprets MALLET's output, the JSON files produced by `twic_corpus2vis.py` are in `twic/data/input/json`. These files include `twic_corpusmap.json`, `twic_corpusinfo.json`, `twic_corpus_wordweights.json`, and individual JSON files for each text in the collection.


<br/>
["howtouse_visualization"]
### Client-side - D3 Visualization

TWiC's primary component is a D3 JavaScript visualization accessible via a web browser. It uses data in the intermediate files output by TWiC's main Python script `twic_corpus2vis.py`. 

_It should be noted again that TWiC currently is best supported in Google's Chrome web browser._

#### Basic Steps

1. **Start** a local server in the `twic` folder. (This is to prevent cross-site scripting (XSS) errors in the browser that stop TWiC from running.)

	i. Navigate to the `twic` folder
	
	ii. Run `python -m SimpleHTTPServer 8080` if you are using Python 2.x and `python -m http.server 8080` if you are using Python 3.x. You can find out your version of Python via `python --version`. The latter '8080' is an arbitrary port number that will be used when you start TWiC in your browser.  
	
2. **Open** your web browser and type in the browser bar: `localhost:8080/twic.html`

**That's it! You're off to explore your topic model with TWiC.**

#### Notes

Though there are certainly other servers you could use to run TWiC, the suggested simple HTTP server via Python will suffice for most users.


<br/>
["howtouse_publicationview"]
## Client-Side - Alternate D3 Visualization for Publication Arrangements

Since the primary level of TWiC organizes a collection of texts chiefly by topic, another view was created for TWiC that instead features a user-provided arrangements of texts (see [Publication View](#panels_publicationview) for more details). 

An example of this can be accessed via `twic_publication.html`. It utilizes the JavaScript code found in `twic_publication_test.js` to set up a TWiC level that includes this Publication View panel. Normally TWiC uses `twic.js` to set up the level. For a discussion of how TWiC levels are constructed, see the [Custom Levels](#howtouse_customlevels) section below.

Currently setting up the Publication View for your own use is a manual process that requires some JavaScript and JSON knowledge – as well as knowledge of the arrangement in which you'd like to see the texts of your collection.

["howtouse_publicationview_basicsteps"]
#### Basic Steps

1. **Start a local server** in the `twic` folder. (This is to prevent cross-site scripting (XSS) errors in the browser that stop TWiC from running.)

	i. Navigate to the `twic` folder
	
	ii. Run `python -m SimpleHTTPServer 8080` if you are using Python 2.x and `python -m http.server 8080` if you are using Python 3.x. You can find out your version of Python via `python --version`. The latter '8080' is an arbitrary port number that will be used when you start TWiC in your browser.  
	
2. **Open your web browser** and type in the browser bar: `localhost:8080/twic_publication.html`

**That's it! You're off to explore your publication with TWiC.**

#### Notes

In order to use the [Publication View](#panels_publicationview) to view arrangements of their own texts (beyond the example of Emily Dickinson's fascicle books), users must construct their own JSON file and reference it in `twic_publication.html`. Below is a description of this setup process.

**1.** **Constructing a JSON file for the Publication View**

Use the example JSON file below (`twic_fascicle21.json` found in `twic/data/dickinson/input/json/`) as an example to construct your own Publication View-ready JSON files.

``` javascript 
{
    "title": "Fascicle 21, Franklin, R.W. \"The Manuscript Books of Emily Dickinson.\" 1981. pp.455-480.",
    "count": "17",
    "texts": [
        {
            "title": "I - Years had been - from Home",
            "page": "1",
            "file": "619"
        },
        {
            "title": "You'll find - it when you try to die -",
            "page": "1",
            "file": "621"
        },

        {
            "title": "I see thee better - in the Dark -",
            "page": "2",
            "file": "623"
        },
        {
            "title": "Could - I do more - for Thee -",
            "page": "2",
            "file": "625"
        },
        {
            "title": "It would have starved a Gnat",
            "page": "2",
            "file": "626"
        },
        {
            "title": "They shut me up in Prose -",
            "page": "2",
            "file": "627"
        },

        {
            "title": "This was a Poet - It is That",
            "page": "3",
            "file": "628"
        },
        {
            "title": "In falling Timbers buried",
            "page": "3",
            "file": "629"
        },
        {
            "title": "I died for Beauty - but was scarce",
            "page": "3",
            "file": "630"
        },
        {
            "title": "Dreams - are well - but Waking's better,",
            "page": "3",
            "file": "631"
        },

        {
            "title": "The Outer - from the Inner",
            "page": "4",
            "file": "632"
        },
        {
            "title": "At last - to be identified -",
            "page": "4",
            "file": "256"
        },
        {
            "title": "The Malay - took the Pearl -",
            "page": "4",
            "file": "633"
        },
        {
            "title": "Love - thou art high -",
            "page": "4",
            "file": "634"
        },

        {
            "title": "Our journey had advanced -",
            "page": "5",
            "file": "635"
        },
        {
            "title": "I rose - because He sank -",
            "page": "5",
            "file": "636"
        },
        {
            "title": "It was given to me by the Gods -",
            "page": "5",
            "file": "637"
        }       
    ]
}
```

**2.** Place that JSON file in the `/data/input/json/` folder

**3.** Open the file `twic_publication_test.js` in the `twic/code/js` folder and find the line that looks like this:

``` javascript
var publicationView =
    new TWiC.PublicationView({ x: 0, y: 0 }, // Panel position
                             { width: screenDims.width >> 1, height: screenDims.height - topicBarHeight }, // Panel size
                             twicLevel, // Level reference,
                             Publication View,
                             "twic_fascicle21.json");
```

**4.** Change "twic_fascicle21.json" to the name of your new JSON file. TWiC will know to look in `data/input/json/` for it.

**5.** Follow the steps above in the [Basic Steps](#howtouse_publicationview_basicsteps) section to run TWiC with this new arrangement of texts.

### Notes

Two example publication JSON are included at `twic/data/dickinson/input/json`. They are the 8th and 21st fascicle booklets of Emily Dickinson's poetry. For more on the Emily Dickinson poetry corpus included with TWiC, see [below](#dickinson).

<br/>
["howtouse_customlevels"]
## Custom Levels

TWiC houses all of its panels within a "level" object that also contains some code for the interoperation of windows, as well as the initial visualization resizing animation. Custom TWiC levels are ones where only certain panels are instantiated. This can be accomplished by editing `twic.js`. This takes some knowledge of JavaScript programming, however it is possible to only initially instantiate whichever of TWiC's panels you are interested in utilizing.

<br/>
["dickinson"]
## The Emily Dickinson Corpus Example

Provided with TWiC is an example corpus that models over 2000 of the available poems at ["The Emily Dickinson Archive"](http://www.edickinson.org). Its files are in the `twic/data/dickinson/` folder. In order to view this collection with TWiC's visualization, you will need to do the following.

### Basic Steps

**1.** Open `twic_level.js` in `twic/code/js/` and find the lines

``` javascript
    //namespace.Level.prototype.s_jsonDirectory = "data/dickinson/input/json/";
    namespace.Level.prototype.s_jsonDirectory = "data/input/json/";
```

**2.** Edit them to look like this instead, activating the first line by removing the '//', and de-activating the second by adding a '//' to the beginning of it. This redirects the TWiC visualization to read its intermediate JSON files from another directory.

``` javascript
    namespace.Level.prototype.s_jsonDirectory = "data/dickinson/input/json/";
    //namespace.Level.prototype.s_jsonDirectory = "data/input/json/";
```

**3.** Run the local server in TWiC's root folder as discussed in the [Client-side D3 Visualization](#howtouse_visualization) section.

### Notes

There could arise an instance where a user would like to re-model the Dickinson corpus – to change the number of topics, etc. In order to run the TWiC's main Python script for the Emily Dickinson corpus, as discussed in the [How to Use](#howtouse) section, users can locate a custom version of TWiC's Python scripts located in the `twic/code/py/dickinson/` folder. They should run the `dickinson_twic_corpus2vis.py` script from that folder using the same options. The Dickinson configuration file `dickinson_twic_config.yaml` is located in `twic/data/dickinson/input/yaml/`.
