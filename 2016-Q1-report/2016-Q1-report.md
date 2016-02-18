
# Computer Science

## Store

### Databases

- **Spatial Database**: Our existing spatial database, deployed on our local datascope lacks dynamic scalability.  Thus, when a large enough group of people are simultaneously accessing a large amount of data data, it is slow for everybody else.  Therefore, we have ported our infrastructure code to work in the commercial cloud, called **ndspdb**.  Preliminary testing suggests single threaded performance of approximately 40 Hz for 512x512 tiles. See [here](http://docs.neurodata.io/ndintro/cs_slides.html#ndspdb) for more details.  In the coming quarter, we will be extensively benchmarking and testing to determine the optimal price/speed trade-off for our use cases.


- **Object Metadata Database**: Our existing object metadata database had been designed exclusively for electron microscopy data.  Upon including CLARITY and multimodal magnetic resonance imaging (MRI) data, we decided to include ROI and Skeleton to support annotations appropriate for these data modalities into **ndramondb**.  For more details see [here](http://docs.neurodata.io/ndintro/cs_slides.html#ndramondb). In the coming quarter, we will be deploying this new system in the cloud in support of all of our data.


-  **Graphs with Rich Attributes Database**: In the process of generating 25 different resolution graphs for nearly 10,000 brains, yielding a total of almost a quarter million brain graphs, we began running out of space.  Thus, we have devised a new storage model for our brain graphs: (i) an edge list, (ii) a json/csv file containing metadata.  This representation is significantly smaller than the GraphML format that we had been using, and compresses even smaller.  We now store such graphs in our **ndgrutedb**. For more details see [here](http://docs.neurodata.io/ndintro/cs_slides.html#ndgrutedb). In the coming quarter, we will generate our graphs in this format, and publish it for others to use as well.


### Utilities

- **Content Distribution Network**: For our collaborators that live thousands of miles from us, or just want local copies of the data, we have built a content distribution network called **ndtilecache**.  It fetches cubes from the spatial database, and writes image tiles.  We have deployed ndtilecache in Amazon for our collaborators at the Allen institute, to obtain significantly faster download rates. For more details see [here](http://docs.neurodata.io/ndintro/cs_slides.html#ndtilecache). In the coming quarter we will more extensively benchmark performance of ndtilecache under various settings.

- **Laboratory Information Management System**: As we increase the number of different datasets and data types, we are compelled to also begin storing more enriched metadata for each dataset.  We built **ndlims** to store all this metadata, using MongoDB and Meteor.  We have used it, for example, to generate this  [table](http://docs.neurodata.io/ocp-journal-paper/#tab:images).   In the coming quarter we will continue to add metadata to ndlims, and try to standardize metadata fields a bit more.


- **Python API**: To enable neuroscientists and computer vision scientists to more easily upload and download the data, we have built **ndio**, a python wrapper around all of the RESTful API calls.  This software toolkit has been invaluable in expanding the number of people that can utilize our Web-services.  For more details see [here](http://docs.neurodata.io/ndintro/cs_slides.html#hardware). In the coming quarter we will continue to test different configurations to find the sweet spot of cost and speed.


### Hardware

We have outgrown our local datascope nodes for the reasons discussed above.  Therefore, we now utilize Amazon hardware, including a wide variety of different instances and configurations.  For more details see [here](http://docs.neurodata.io/nddocs/ndio/). In the coming quarter we will continue adding functionality to ndio to further enable discoveries.

## Explore

### Images

Our image explorer enables one to pan and zoom in 2D & 3D, as well as visualize multispectral data, overlay annotations with metadata, and has time-series support.  Importantly, it works on mobile devices.  We have added dataviews, that enable people to share mash-ups of different channels with different colors, starting in different locations and zooms. For example, we can now overlay Allen Brain Atlas on top of CLARITY brain images. For more details see [here](http://docs.neurodata.io/ndintro/cs_slides.html#ix). In the coming months we will add additional features to ix, including potentially multiple canonical views and histogram statistics.

### Graphs

Our graph explorer enables users to upload an arbitrary graph (in a specified format) and interactively explore it including plotting it, computing invariants, and finding communities in a variety of ways.  We added an option to directly load one of our many curated graphs, as well as generate random graphs from a variety of distributions.  For more details see [here](http://docs.neurodata.io/ndintro/cs_slides.html#gx).


### Vectors

Our vector explorer enables scientists without any coding experience to perform basic data analyses on their "tabular data".  This includes looking at heatmaps of the raw and transformed data, clustering features and samples, plotting distributions of features and samples, and embedding.  For more details see [here](http://docs.neurodata.io/ndintro/cs_slides.html#vx). In the coming quarter we will add some additional functionality and hopefully efficiency.



## Wrangle

### Image Processing

- **2D Stitching Artifact Removal**: For CLARITY and other imaging modalities, an image plane is actually a montage of many smaller images (due to the size of the camera vs the desired field of view).  Therefore, prior to obtaining our images, our collaborators montage the images together.  However, because each image has a different exposure, there are "stitching artifacts" that remain.  We realized that we could use off-the-shelf tools that perform streaming artifact removal that our JHU collaborators developed a few years ago.  We now routinely run that code in our analysis pipeline and have made the code and instructions open source.  For more details see [here](http://docs.neurodata.io/ndintro/cs_slides.html#2d). In the coming quarter we will make this a Web-service, so that when somebody ingests data they can opt to perform this step.

- **3D Histogram Normalization**:  For CLARITY and other imaging modalities, each 2D image plane can have different histogram statistics from its neighbors.  Thus, when panning in 3D, the image looks like a strobe light.  We developed a streaming 3D histogram normalization to correct for this issue.  We now routinely run that code in our analysis pipeline and have made the code and instructions open source.  For more details see [here](http://docs.neurodata.io/ndintro/cs_slides.html#3d). In the coming quarter we will make this a Web-service, so that when somebody ingests data they can opt to perform this step.

- **Volume Registration**: For CLARITY and MRI data there are standard atlases that are useful to register to.  We therefore built and open-sourced **ndreg** that performs a smooth invertible registrations between multi-modal data.  For more details see [here](http://docs.neurodata.io/ndintro/cs_slides.html#ndreg). In the coming quarter we will make this a Web-service, so that when somebody ingests data they can opt to perform this step.




### Object Detection

- **Cell Detection using Random Forest**: To estimate graphs in CLARITY and Calcium Imaging data, we first detect cell bodies. We build **ndparse** to enable scalable cell detection in mesoscale data.  We have successfully run it on histology data.  For more details see [here](http://docs.neurodata.io/ndintro/cs_slides.html#maxa). In the coming quarter we will apply this to the CLARITY data as well. 

- **Cell Detection using Deep Learning**: The random forest based cell detection code requires histogram normalization prior to application to make performance reasonable.  By training a convolution neural network on lots of training data, we believe we can outperform random forests.  We therefore built **nddl** and successfully applied it to histology data. For more details see [here](http://docs.neurodata.io/ndintro/cs_slides.html#nddl). In the coming quarter we will apply this to the CLARITY data as well.


## Analyze

### Images

One of the first image analyses to perform on data for quality control and exploratory purposes is computing the image intensity histogram and statistics thereof.  We built a Web-service to enable performing that operation on an arbitrary sub-cube using a distributed and streaming implementation. We have since successfully applied it to whole CLARITY brains.   For more details see [here](http://docs.neurodata.io/ndintro/cs_slides.html#images). In the coming quarter we will apply this to additional CLARITY brains.


### Shapes

Shapes are mathematically infinite dimensional objects that live in non-Euclidean metric spaces.  Thus, to perform statistical analyses on them we benefit from specifying a metric, or characterizing shapes via shape descriptors.  We released **ShapeSPH** which comprises a number of functions of shapes, including computing rotation invariant descriptors.  For more details see [here](http://docs.neurodata.io/ndintro/cs_slides.html#shapes). In the coming quarter we will further explore these functions with brain data.

### Graphs


Graphs are non-Euclidean and high-dimensional combinatorial objects. Scalable implementations of certain graph traversal algorithms required re-thinking from the ground up.  Previously, we built **FlashGraph** to enable such operations using a semi-external memory model.  Recently, we added R bindings, to enable people to use FlashGraph without having to install anything or run C code.  For more details see [here](http://docs.neurodata.io/ndintro/cs_slides.html#graphs). In the coming quarter we will move towards having an R package on CRAN to further lower the barrier to entry.

### Matrices

One of the main innovations associated with SIMPLEX is the realization that the semi-external memory computing model could be expanded beyond merely sparse graph traversal algorithms, to also incorporate matrix operations.  In particular, we can store low-rank approximations in memory while storing the large matrix on disk.  We have demonstrated this capability already in SVD, PCA, K-means, and NMF implementations.  For more details see [here](http://docs.neurodata.io/ndintro/cs_slides.html#matrices). In the coming quarter we will move towards having an R package on CRAN to further lower the barrier to entry.



# Statistical Science


## Time-Series

### High-Dimensional System Identification

For both calcium imaging and functional MRI (fMRI), we obtain very high-dimensional time-series data, but believe there are lower dimensional states governing the high-dimensions.  The Kalman Filtering framework is optimal for such settings under certain assumptions.  However, Kalman Filter and Smoother implementations typically require that the dimensionality of the observations is smaller than the number of time steps.  We devised the M-estimator for Reduced-rank System IDentification (**Mr. Sid**) to be able to both estimate the Kalman filter parameters and latent variables for wide time-series data.  For more details see [here](http://docs.neurodata.io/ndintro/stats.html#mrside). In the coming quarter we will move towards having a native R implementation.

## Graphs


### Vertex Nomination via Seeded Graph Matching

Vertex nomination is the practice of knowing some vertices in a graph are "of interest" and desiring to find other "similar" vertices.  When we have multiple graphs with a partial alignment, we can "pool" graphs to find similar vertices on the basis of their similar "role" within the multiple graphs.  We have deployed this approach to effectively nominate vertices in a particular application. For more details see [here](http://docs.neurodata.io/ndintro/stats.html#vnsgm). In the coming quarter we will continue developing these ideas, proving theorems, and improving the implementation.


### Hierarchical Block Modeling

Modeling graphs where each vertex is assigned one or many "clusters" is commonplace.  However, typically, those clusters lack structure.  We propose a new model where the clusters are arranged in a hierarchical tree.  We then prove that we can successfully estimate both the cluster assignments and organization under certain modeling assumptions, as well as provide a few numerical examples in both simulation and real data.  For more details see [here](http://docs.neurodata.io/ndintro/stats.html#hsbm). In the coming quarter we will continue developing these ideas, proving theorems, and improving the implementation.


### Law of Large Graphs

The empirical (or sample) average of a collection of matched graphs is of interest for many applications.  We have developed theory proving that a low-rank approximation to the sample average is more efficient than the naive estimator, meaning that it is unbiased, but has smaller variance.  Therefore, assuming the data are well matched to the model, one should always use our low-rank estimator. Our result is akin to the *law of large numbers*, but for graphs. For more details see [here](http://docs.neurodata.io/ndintro/stats.html#llg). In the coming quarter we will apply these results to different real data applications to obtain better finite sample estimators.


### Robust Law of Large Graphs

In the above, the improved performance requires data comes from a particular and simple model. In the face of model misspecifications, we desire an estimator that is "robust", that is, still performs well under modest perturbations.  Indeed, we have proven that our robust mean graph estimator is more efficient in the face of certain misspecifications, even mild ones.  The conclusion is that one should use our robust estimator in practice.   For more details see [here](http://docs.neurodata.io/ndintro/stats.html#rllg). In the coming quarter we will apply these results to different real data applications to obtain better finite sample estimators.


### Optimization Theoretical Vertex Clustering





## Vectors

### LOL

### Randomer Forest
In many classification tasks, we often opt to train random forest classifiers, as they empirically perform well and are efficient to train. However, random forests are restricted to finding axis-aligned decision boundaries at split nodes within each decision tree. We have developed a new classification algorithm called randomer forest (RerF), which can find oblique splits while maintaining the space and time complexity of random forest. We have demonstrated that it empirically outperforms random forest on simulated and benchmark datasets. In the coming quarter, we will further refine the method, begin to develop theory, and apply it to pertinent applications.

### Dependence Testing

### Discriminability
In the above, we always want to know how much signal there is to differentiate classes. We have designed a new statistics called discriminability which quantifies the amount signal in the data. It is defined to be the probability that within class distance to be smaller than the across distance. Our Discriminability statistics has the property of being robust and bound Bayes classification error under suitable settings. For more details see [here](http://docs.neurodata.io/ndintro/stats.html#rllg). In the coming quarter we will develope these ideas, prove theorems, and apply the framework to real data applications. 



# Datafication



## CLARITY

By annotating CLARITY brains scientific observations can made on the texture and shape of specific brain structures. Manual labeling of entire CLARITY brains is time-consuming hence the most efficient way to annotate a brain image is through atlas registration. Therefore ndreg was developed to align the Allen Mouse Brain Atlas to these types of images. In the procedure, a CLARITY brain is downloaded from the spatial database using ndio. The atlas annotations are registered to the brain using ndreg and ingested into the database using ndio. The deformed annotations can then be visualized over the CLARITY brain [using NeuroDataViz](http://viz.neurodata.io/project/Fear199KwameSimplex/5/269/358/697).

![NeuroDataViz](ndviz.png "NeuroDataViz")

## Expansion Microscopy

## X-Ray Histology

## Calcium Imaging

## Multimodal Magnetic Resonance Imaging



# Discovery

## Connectome vs. CCI

## Optimal Pipeline
In order to obtain good quanlity fMRI graph, one key question is which processing pipeline we use. By treating each subject as a class, we compute the Discriminability statistics of 12 fMRI data sets using 64 pipelines. We find the optimal pipeline, and be able to decide what to do at each processing step (like global signal regression or not). In the coming quarter we will summarize our findings, investigate the optimal number of ROIs for atlas, and compare the quanlity of fMRI and DTI.

## Cell Detection in XRM

## Cell Detection in Histology

## Kasthuri Claims





