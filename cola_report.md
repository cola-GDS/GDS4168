cola Report for GDS4168
==================

**Date**: 2019-12-25 21:13:41 CET, **cola version**: 1.3.2

----------------------------------------------------------------

<style type='text/css'>

body, td, th {
   font-family: Arial,Helvetica,sans-serif;
   background-color: white;
   font-size: 13px;
  max-width: 800px;
  margin: auto;
  margin-left:210px;
  padding: 0px 10px 0px 10px;
  border-left: 1px solid #EEEEEE;
  line-height: 150%;
}

tt, code, pre {
   font-family: 'DejaVu Sans Mono', 'Droid Sans Mono', 'Lucida Console', Consolas, Monaco, 

monospace;
}

h1 {
   font-size:2.2em;
}

h2 {
   font-size:1.8em;
}

h3 {
   font-size:1.4em;
}

h4 {
   font-size:1.0em;
}

h5 {
   font-size:0.9em;
}

h6 {
   font-size:0.8em;
}

a {
  text-decoration: none;
  color: #0366d6;
}

a:hover {
  text-decoration: underline;
}

a:visited {
   color: #0366d6;
}

pre, img {
  max-width: 100%;
}
pre {
  overflow-x: auto;
}
pre code {
   display: block; padding: 0.5em;
}

code {
  font-size: 92%;
  border: 1px solid #ccc;
}

code[class] {
  background-color: #F8F8F8;
}

table, td, th {
  border: 1px solid #ccc;
}

blockquote {
   color:#666666;
   margin:0;
   padding-left: 1em;
   border-left: 0.5em #EEE solid;
}

hr {
   height: 0px;
   border-bottom: none;
   border-top-width: thin;
   border-top-style: dotted;
   border-top-color: #999999;
}

@media print {
   * {
      background: transparent !important;
      color: black !important;
      filter:none !important;
      -ms-filter: none !important;
   }

   body {
      font-size:12pt;
      max-width:100%;
   }

   a, a:visited {
      text-decoration: underline;
   }

   hr {
      visibility: hidden;
      page-break-before: always;
   }

   pre, blockquote {
      padding-right: 1em;
      page-break-inside: avoid;
   }

   tr, img {
      page-break-inside: avoid;
   }

   img {
      max-width: 100% !important;
   }

   @page :left {
      margin: 15mm 20mm 15mm 10mm;
   }

   @page :right {
      margin: 15mm 10mm 15mm 20mm;
   }

   p, h2, h3 {
      orphans: 3; widows: 3;
   }

   h2, h3 {
      page-break-after: avoid;
   }
}
</style>




## Summary





All available functions which can be applied to this `res_list` object:


```r
res_list
```

```
#> A 'ConsensusPartitionList' object with 24 methods.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows are extracted by 'SD, CV, MAD, ATC' methods.
#>   Subgroups are detected by 'hclust, kmeans, skmeans, pam, mclust, NMF' method.
#>   Number of partitions are tried for k = 2, 3, 4, 5, 6.
#>   Performed in total 30000 partitions by row resampling.
#> 
#> Following methods can be applied to this 'ConsensusPartitionList' object:
#>  [1] "cola_report"           "collect_classes"       "collect_plots"         "collect_stats"        
#>  [5] "colnames"              "functional_enrichment" "get_anno_col"          "get_anno"             
#>  [9] "get_classes"           "get_matrix"            "get_membership"        "get_stats"            
#> [13] "is_best_k"             "is_stable_k"           "ncol"                  "nrow"                 
#> [17] "rownames"              "show"                  "suggest_best_k"        "test_to_known_factors"
#> [21] "top_rows_heatmap"      "top_rows_overlap"     
#> 
#> You can get result for a single method by, e.g. object["SD", "hclust"] or object["SD:hclust"]
#> or a subset of methods by object[c("SD", "CV")], c("hclust", "kmeans")]
```

The call of `run_all_consensus_partition_methods()` was:


```
#> run_all_consensus_partition_methods(data = mat, mc.cores = 4, anno = anno)
```

Dimension of the input matrix:


```r
mat = get_matrix(res_list)
dim(mat)
```

```
#> [1] 21512    52
```

### Density distribution

The density distribution for each sample is visualized as in one column in the
following heatmap. The clustering is based on the distance which is the
Kolmogorov-Smirnov statistic between two distributions.




```r
library(ComplexHeatmap)
densityHeatmap(mat, top_annotation = HeatmapAnnotation(df = get_anno(res_list), 
    col = get_anno_col(res_list)), ylab = "value", cluster_columns = TRUE, show_column_names = FALSE,
    mc.cores = 4)
```

![plot of chunk density-heatmap](figure_cola/density-heatmap-1.png)





### Suggest the best k



Folowing table shows the best `k` (number of partitions) for each combination
of top-value methods and partition methods. Clicking on the method name in
the table goes to the section for a single combination of methods.

[The cola vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13)
explains the definition of the metrics used for determining the best
number of partitions.


```r
suggest_best_k(res_list)
```


|                            | The best k| 1-PAC| Mean silhouette| Concordance|   |Optional k |
|:---------------------------|----------:|-----:|---------------:|-----------:|:--|:----------|
|[SD:pam](#SD-pam)           |          3| 1.000|           0.961|       0.984|** |2          |
|[SD:NMF](#SD-NMF)           |          3| 1.000|           0.963|       0.984|** |           |
|[CV:pam](#CV-pam)           |          3| 1.000|           0.969|       0.990|** |2          |
|[CV:NMF](#CV-NMF)           |          3| 1.000|           0.986|       0.994|** |           |
|[ATC:kmeans](#ATC-kmeans)   |          3| 1.000|           0.968|       0.988|** |2          |
|[ATC:pam](#ATC-pam)         |          2| 1.000|           0.997|       0.999|** |           |
|[MAD:pam](#MAD-pam)         |          3| 0.999|           0.956|       0.981|** |2          |
|[CV:skmeans](#CV-skmeans)   |          4| 0.964|           0.936|       0.973|** |2,3        |
|[MAD:skmeans](#MAD-skmeans) |          4| 0.961|           0.942|       0.975|** |2          |
|[SD:skmeans](#SD-skmeans)   |          4| 0.942|           0.892|       0.958|*  |2,3        |
|[ATC:skmeans](#ATC-skmeans) |          4| 0.923|           0.890|       0.954|*  |2,3        |
|[MAD:NMF](#MAD-NMF)         |          2| 0.920|           0.945|       0.974|*  |           |
|[MAD:mclust](#MAD-mclust)   |          5| 0.786|           0.774|       0.893|   |           |
|[MAD:kmeans](#MAD-kmeans)   |          3| 0.742|           0.902|       0.932|   |           |
|[MAD:hclust](#MAD-hclust)   |          2| 0.701|           0.854|       0.935|   |           |
|[SD:kmeans](#SD-kmeans)     |          3| 0.693|           0.883|       0.928|   |           |
|[CV:mclust](#CV-mclust)     |          3| 0.679|           0.834|       0.910|   |           |
|[CV:hclust](#CV-hclust)     |          2| 0.675|           0.893|       0.944|   |           |
|[ATC:NMF](#ATC-NMF)         |          4| 0.645|           0.785|       0.904|   |           |
|[ATC:hclust](#ATC-hclust)   |          5| 0.634|           0.695|       0.858|   |           |
|[CV:kmeans](#CV-kmeans)     |          3| 0.631|           0.867|       0.901|   |           |
|[SD:hclust](#SD-hclust)     |          2| 0.618|           0.892|       0.938|   |           |
|[SD:mclust](#SD-mclust)     |          3| 0.543|           0.665|       0.855|   |           |
|[ATC:mclust](#ATC-mclust)   |          5| 0.538|           0.780|       0.854|   |           |

\*\*: 1-PAC > 0.95, \*: 1-PAC > 0.9




### CDF of consensus matrices

Cumulative distribution function curves of consensus matrix for all methods.




```r
collect_plots(res_list, fun = plot_ecdf)
```

![plot of chunk collect-plots](figure_cola/collect-plots-1.png)



### Consensus heatmap

Consensus heatmaps for all methods. ([What is a consensus heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_9))


<style type='text/css'>



.ui-helper-hidden {
	display: none;
}
.ui-helper-hidden-accessible {
	border: 0;
	clip: rect(0 0 0 0);
	height: 1px;
	margin: -1px;
	overflow: hidden;
	padding: 0;
	position: absolute;
	width: 1px;
}
.ui-helper-reset {
	margin: 0;
	padding: 0;
	border: 0;
	outline: 0;
	line-height: 1.3;
	text-decoration: none;
	font-size: 100%;
	list-style: none;
}
.ui-helper-clearfix:before,
.ui-helper-clearfix:after {
	content: "";
	display: table;
	border-collapse: collapse;
}
.ui-helper-clearfix:after {
	clear: both;
}
.ui-helper-zfix {
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	position: absolute;
	opacity: 0;
	filter:Alpha(Opacity=0); 
}

.ui-front {
	z-index: 100;
}



.ui-state-disabled {
	cursor: default !important;
	pointer-events: none;
}



.ui-icon {
	display: inline-block;
	vertical-align: middle;
	margin-top: -.25em;
	position: relative;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
}

.ui-widget-icon-block {
	left: 50%;
	margin-left: -8px;
	display: block;
}




.ui-widget-overlay {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
}
.ui-accordion .ui-accordion-header {
	display: block;
	cursor: pointer;
	position: relative;
	margin: 2px 0 0 0;
	padding: .5em .5em .5em .7em;
	font-size: 100%;
}
.ui-accordion .ui-accordion-content {
	padding: 1em 2.2em;
	border-top: 0;
	overflow: auto;
}
.ui-autocomplete {
	position: absolute;
	top: 0;
	left: 0;
	cursor: default;
}
.ui-menu {
	list-style: none;
	padding: 0;
	margin: 0;
	display: block;
	outline: 0;
}
.ui-menu .ui-menu {
	position: absolute;
}
.ui-menu .ui-menu-item {
	margin: 0;
	cursor: pointer;
	
	list-style-image: url("data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7");
}
.ui-menu .ui-menu-item-wrapper {
	position: relative;
	padding: 3px 1em 3px .4em;
}
.ui-menu .ui-menu-divider {
	margin: 5px 0;
	height: 0;
	font-size: 0;
	line-height: 0;
	border-width: 1px 0 0 0;
}
.ui-menu .ui-state-focus,
.ui-menu .ui-state-active {
	margin: -1px;
}


.ui-menu-icons {
	position: relative;
}
.ui-menu-icons .ui-menu-item-wrapper {
	padding-left: 2em;
}


.ui-menu .ui-icon {
	position: absolute;
	top: 0;
	bottom: 0;
	left: .2em;
	margin: auto 0;
}


.ui-menu .ui-menu-icon {
	left: auto;
	right: 0;
}
.ui-button {
	padding: .4em 1em;
	display: inline-block;
	position: relative;
	line-height: normal;
	margin-right: .1em;
	cursor: pointer;
	vertical-align: middle;
	text-align: center;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;

	
	overflow: visible;
}

.ui-button,
.ui-button:link,
.ui-button:visited,
.ui-button:hover,
.ui-button:active {
	text-decoration: none;
}


.ui-button-icon-only {
	width: 2em;
	box-sizing: border-box;
	text-indent: -9999px;
	white-space: nowrap;
}


input.ui-button.ui-button-icon-only {
	text-indent: 0;
}


.ui-button-icon-only .ui-icon {
	position: absolute;
	top: 50%;
	left: 50%;
	margin-top: -8px;
	margin-left: -8px;
}

.ui-button.ui-icon-notext .ui-icon {
	padding: 0;
	width: 2.1em;
	height: 2.1em;
	text-indent: -9999px;
	white-space: nowrap;

}

input.ui-button.ui-icon-notext .ui-icon {
	width: auto;
	height: auto;
	text-indent: 0;
	white-space: normal;
	padding: .4em 1em;
}



input.ui-button::-moz-focus-inner,
button.ui-button::-moz-focus-inner {
	border: 0;
	padding: 0;
}
.ui-controlgroup {
	vertical-align: middle;
	display: inline-block;
}
.ui-controlgroup > .ui-controlgroup-item {
	float: left;
	margin-left: 0;
	margin-right: 0;
}
.ui-controlgroup > .ui-controlgroup-item:focus,
.ui-controlgroup > .ui-controlgroup-item.ui-visual-focus {
	z-index: 9999;
}
.ui-controlgroup-vertical > .ui-controlgroup-item {
	display: block;
	float: none;
	width: 100%;
	margin-top: 0;
	margin-bottom: 0;
	text-align: left;
}
.ui-controlgroup-vertical .ui-controlgroup-item {
	box-sizing: border-box;
}
.ui-controlgroup .ui-controlgroup-label {
	padding: .4em 1em;
}
.ui-controlgroup .ui-controlgroup-label span {
	font-size: 80%;
}
.ui-controlgroup-horizontal .ui-controlgroup-label + .ui-controlgroup-item {
	border-left: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label + .ui-controlgroup-item {
	border-top: none;
}
.ui-controlgroup-horizontal .ui-controlgroup-label.ui-widget-content {
	border-right: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label.ui-widget-content {
	border-bottom: none;
}


.ui-controlgroup-vertical .ui-spinner-input {

	
	width: 75%;
	width: calc( 100% - 2.4em );
}
.ui-controlgroup-vertical .ui-spinner .ui-spinner-up {
	border-top-style: solid;
}

.ui-checkboxradio-label .ui-icon-background {
	box-shadow: inset 1px 1px 1px #ccc;
	border-radius: .12em;
	border: none;
}
.ui-checkboxradio-radio-label .ui-icon-background {
	width: 16px;
	height: 16px;
	border-radius: 1em;
	overflow: visible;
	border: none;
}
.ui-checkboxradio-radio-label.ui-checkboxradio-checked .ui-icon,
.ui-checkboxradio-radio-label.ui-checkboxradio-checked:hover .ui-icon {
	background-image: none;
	width: 8px;
	height: 8px;
	border-width: 4px;
	border-style: solid;
}
.ui-checkboxradio-disabled {
	pointer-events: none;
}
.ui-datepicker {
	width: 17em;
	padding: .2em .2em 0;
	display: none;
}
.ui-datepicker .ui-datepicker-header {
	position: relative;
	padding: .2em 0;
}
.ui-datepicker .ui-datepicker-prev,
.ui-datepicker .ui-datepicker-next {
	position: absolute;
	top: 2px;
	width: 1.8em;
	height: 1.8em;
}
.ui-datepicker .ui-datepicker-prev-hover,
.ui-datepicker .ui-datepicker-next-hover {
	top: 1px;
}
.ui-datepicker .ui-datepicker-prev {
	left: 2px;
}
.ui-datepicker .ui-datepicker-next {
	right: 2px;
}
.ui-datepicker .ui-datepicker-prev-hover {
	left: 1px;
}
.ui-datepicker .ui-datepicker-next-hover {
	right: 1px;
}
.ui-datepicker .ui-datepicker-prev span,
.ui-datepicker .ui-datepicker-next span {
	display: block;
	position: absolute;
	left: 50%;
	margin-left: -8px;
	top: 50%;
	margin-top: -8px;
}
.ui-datepicker .ui-datepicker-title {
	margin: 0 2.3em;
	line-height: 1.8em;
	text-align: center;
}
.ui-datepicker .ui-datepicker-title select {
	font-size: 1em;
	margin: 1px 0;
}
.ui-datepicker select.ui-datepicker-month,
.ui-datepicker select.ui-datepicker-year {
	width: 45%;
}
.ui-datepicker table {
	width: 100%;
	font-size: .9em;
	border-collapse: collapse;
	margin: 0 0 .4em;
}
.ui-datepicker th {
	padding: .7em .3em;
	text-align: center;
	font-weight: bold;
	border: 0;
}
.ui-datepicker td {
	border: 0;
	padding: 1px;
}
.ui-datepicker td span,
.ui-datepicker td a {
	display: block;
	padding: .2em;
	text-align: right;
	text-decoration: none;
}
.ui-datepicker .ui-datepicker-buttonpane {
	background-image: none;
	margin: .7em 0 0 0;
	padding: 0 .2em;
	border-left: 0;
	border-right: 0;
	border-bottom: 0;
}
.ui-datepicker .ui-datepicker-buttonpane button {
	float: right;
	margin: .5em .2em .4em;
	cursor: pointer;
	padding: .2em .6em .3em .6em;
	width: auto;
	overflow: visible;
}
.ui-datepicker .ui-datepicker-buttonpane button.ui-datepicker-current {
	float: left;
}


.ui-datepicker.ui-datepicker-multi {
	width: auto;
}
.ui-datepicker-multi .ui-datepicker-group {
	float: left;
}
.ui-datepicker-multi .ui-datepicker-group table {
	width: 95%;
	margin: 0 auto .4em;
}
.ui-datepicker-multi-2 .ui-datepicker-group {
	width: 50%;
}
.ui-datepicker-multi-3 .ui-datepicker-group {
	width: 33.3%;
}
.ui-datepicker-multi-4 .ui-datepicker-group {
	width: 25%;
}
.ui-datepicker-multi .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-multi .ui-datepicker-group-middle .ui-datepicker-header {
	border-left-width: 0;
}
.ui-datepicker-multi .ui-datepicker-buttonpane {
	clear: left;
}
.ui-datepicker-row-break {
	clear: both;
	width: 100%;
	font-size: 0;
}


.ui-datepicker-rtl {
	direction: rtl;
}
.ui-datepicker-rtl .ui-datepicker-prev {
	right: 2px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next {
	left: 2px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-prev:hover {
	right: 1px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next:hover {
	left: 1px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane {
	clear: right;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button {
	float: left;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button.ui-datepicker-current,
.ui-datepicker-rtl .ui-datepicker-group {
	float: right;
}
.ui-datepicker-rtl .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-rtl .ui-datepicker-group-middle .ui-datepicker-header {
	border-right-width: 0;
	border-left-width: 1px;
}


.ui-datepicker .ui-icon {
	display: block;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
	left: .5em;
	top: .3em;
}
.ui-dialog {
	position: absolute;
	top: 0;
	left: 0;
	padding: .2em;
	outline: 0;
}
.ui-dialog .ui-dialog-titlebar {
	padding: .4em 1em;
	position: relative;
}
.ui-dialog .ui-dialog-title {
	float: left;
	margin: .1em 0;
	white-space: nowrap;
	width: 90%;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-dialog .ui-dialog-titlebar-close {
	position: absolute;
	right: .3em;
	top: 50%;
	width: 20px;
	margin: -10px 0 0 0;
	padding: 1px;
	height: 20px;
}
.ui-dialog .ui-dialog-content {
	position: relative;
	border: 0;
	padding: .5em 1em;
	background: none;
	overflow: auto;
}
.ui-dialog .ui-dialog-buttonpane {
	text-align: left;
	border-width: 1px 0 0 0;
	background-image: none;
	margin-top: .5em;
	padding: .3em 1em .5em .4em;
}
.ui-dialog .ui-dialog-buttonpane .ui-dialog-buttonset {
	float: right;
}
.ui-dialog .ui-dialog-buttonpane button {
	margin: .5em .4em .5em 0;
	cursor: pointer;
}
.ui-dialog .ui-resizable-n {
	height: 2px;
	top: 0;
}
.ui-dialog .ui-resizable-e {
	width: 2px;
	right: 0;
}
.ui-dialog .ui-resizable-s {
	height: 2px;
	bottom: 0;
}
.ui-dialog .ui-resizable-w {
	width: 2px;
	left: 0;
}
.ui-dialog .ui-resizable-se,
.ui-dialog .ui-resizable-sw,
.ui-dialog .ui-resizable-ne,
.ui-dialog .ui-resizable-nw {
	width: 7px;
	height: 7px;
}
.ui-dialog .ui-resizable-se {
	right: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-sw {
	left: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-ne {
	right: 0;
	top: 0;
}
.ui-dialog .ui-resizable-nw {
	left: 0;
	top: 0;
}
.ui-draggable .ui-dialog-titlebar {
	cursor: move;
}
.ui-draggable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable {
	position: relative;
}
.ui-resizable-handle {
	position: absolute;
	font-size: 0.1px;
	display: block;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable-disabled .ui-resizable-handle,
.ui-resizable-autohide .ui-resizable-handle {
	display: none;
}
.ui-resizable-n {
	cursor: n-resize;
	height: 7px;
	width: 100%;
	top: -5px;
	left: 0;
}
.ui-resizable-s {
	cursor: s-resize;
	height: 7px;
	width: 100%;
	bottom: -5px;
	left: 0;
}
.ui-resizable-e {
	cursor: e-resize;
	width: 7px;
	right: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-w {
	cursor: w-resize;
	width: 7px;
	left: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-se {
	cursor: se-resize;
	width: 12px;
	height: 12px;
	right: 1px;
	bottom: 1px;
}
.ui-resizable-sw {
	cursor: sw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	bottom: -5px;
}
.ui-resizable-nw {
	cursor: nw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	top: -5px;
}
.ui-resizable-ne {
	cursor: ne-resize;
	width: 9px;
	height: 9px;
	right: -5px;
	top: -5px;
}
.ui-progressbar {
	height: 2em;
	text-align: left;
	overflow: hidden;
}
.ui-progressbar .ui-progressbar-value {
	margin: -1px;
	height: 100%;
}
.ui-progressbar .ui-progressbar-overlay {
	background: url("data:image/gif;base64,R0lGODlhKAAoAIABAAAAAP///yH/C05FVFNDQVBFMi4wAwEAAAAh+QQJAQABACwAAAAAKAAoAAACkYwNqXrdC52DS06a7MFZI+4FHBCKoDeWKXqymPqGqxvJrXZbMx7Ttc+w9XgU2FB3lOyQRWET2IFGiU9m1frDVpxZZc6bfHwv4c1YXP6k1Vdy292Fb6UkuvFtXpvWSzA+HycXJHUXiGYIiMg2R6W459gnWGfHNdjIqDWVqemH2ekpObkpOlppWUqZiqr6edqqWQAAIfkECQEAAQAsAAAAACgAKAAAApSMgZnGfaqcg1E2uuzDmmHUBR8Qil95hiPKqWn3aqtLsS18y7G1SzNeowWBENtQd+T1JktP05nzPTdJZlR6vUxNWWjV+vUWhWNkWFwxl9VpZRedYcflIOLafaa28XdsH/ynlcc1uPVDZxQIR0K25+cICCmoqCe5mGhZOfeYSUh5yJcJyrkZWWpaR8doJ2o4NYq62lAAACH5BAkBAAEALAAAAAAoACgAAAKVDI4Yy22ZnINRNqosw0Bv7i1gyHUkFj7oSaWlu3ovC8GxNso5fluz3qLVhBVeT/Lz7ZTHyxL5dDalQWPVOsQWtRnuwXaFTj9jVVh8pma9JjZ4zYSj5ZOyma7uuolffh+IR5aW97cHuBUXKGKXlKjn+DiHWMcYJah4N0lYCMlJOXipGRr5qdgoSTrqWSq6WFl2ypoaUAAAIfkECQEAAQAsAAAAACgAKAAAApaEb6HLgd/iO7FNWtcFWe+ufODGjRfoiJ2akShbueb0wtI50zm02pbvwfWEMWBQ1zKGlLIhskiEPm9R6vRXxV4ZzWT2yHOGpWMyorblKlNp8HmHEb/lCXjcW7bmtXP8Xt229OVWR1fod2eWqNfHuMjXCPkIGNileOiImVmCOEmoSfn3yXlJWmoHGhqp6ilYuWYpmTqKUgAAIfkECQEAAQAsAAAAACgAKAAAApiEH6kb58biQ3FNWtMFWW3eNVcojuFGfqnZqSebuS06w5V80/X02pKe8zFwP6EFWOT1lDFk8rGERh1TTNOocQ61Hm4Xm2VexUHpzjymViHrFbiELsefVrn6XKfnt2Q9G/+Xdie499XHd2g4h7ioOGhXGJboGAnXSBnoBwKYyfioubZJ2Hn0RuRZaflZOil56Zp6iioKSXpUAAAh+QQJAQABACwAAAAAKAAoAAACkoQRqRvnxuI7kU1a1UU5bd5tnSeOZXhmn5lWK3qNTWvRdQxP8qvaC+/yaYQzXO7BMvaUEmJRd3TsiMAgswmNYrSgZdYrTX6tSHGZO73ezuAw2uxuQ+BbeZfMxsexY35+/Qe4J1inV0g4x3WHuMhIl2jXOKT2Q+VU5fgoSUI52VfZyfkJGkha6jmY+aaYdirq+lQAACH5BAkBAAEALAAAAAAoACgAAAKWBIKpYe0L3YNKToqswUlvznigd4wiR4KhZrKt9Upqip61i9E3vMvxRdHlbEFiEXfk9YARYxOZZD6VQ2pUunBmtRXo1Lf8hMVVcNl8JafV38aM2/Fu5V16Bn63r6xt97j09+MXSFi4BniGFae3hzbH9+hYBzkpuUh5aZmHuanZOZgIuvbGiNeomCnaxxap2upaCZsq+1kAACH5BAkBAAEALAAAAAAoACgAAAKXjI8By5zf4kOxTVrXNVlv1X0d8IGZGKLnNpYtm8Lr9cqVeuOSvfOW79D9aDHizNhDJidFZhNydEahOaDH6nomtJjp1tutKoNWkvA6JqfRVLHU/QUfau9l2x7G54d1fl995xcIGAdXqMfBNadoYrhH+Mg2KBlpVpbluCiXmMnZ2Sh4GBqJ+ckIOqqJ6LmKSllZmsoq6wpQAAAh+QQJAQABACwAAAAAKAAoAAAClYx/oLvoxuJDkU1a1YUZbJ59nSd2ZXhWqbRa2/gF8Gu2DY3iqs7yrq+xBYEkYvFSM8aSSObE+ZgRl1BHFZNr7pRCavZ5BW2142hY3AN/zWtsmf12p9XxxFl2lpLn1rseztfXZjdIWIf2s5dItwjYKBgo9yg5pHgzJXTEeGlZuenpyPmpGQoKOWkYmSpaSnqKileI2FAAACH5BAkBAAEALAAAAAAoACgAAAKVjB+gu+jG4kORTVrVhRlsnn2dJ3ZleFaptFrb+CXmO9OozeL5VfP99HvAWhpiUdcwkpBH3825AwYdU8xTqlLGhtCosArKMpvfa1mMRae9VvWZfeB2XfPkeLmm18lUcBj+p5dnN8jXZ3YIGEhYuOUn45aoCDkp16hl5IjYJvjWKcnoGQpqyPlpOhr3aElaqrq56Bq7VAAAOw==");
	height: 100%;
	filter: alpha(opacity=25); 
	opacity: 0.25;
}
.ui-progressbar-indeterminate .ui-progressbar-value {
	background-image: none;
}
.ui-selectable {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-selectable-helper {
	position: absolute;
	z-index: 100;
	border: 1px dotted black;
}
.ui-selectmenu-menu {
	padding: 0;
	margin: 0;
	position: absolute;
	top: 0;
	left: 0;
	display: none;
}
.ui-selectmenu-menu .ui-menu {
	overflow: auto;
	overflow-x: hidden;
	padding-bottom: 1px;
}
.ui-selectmenu-menu .ui-menu .ui-selectmenu-optgroup {
	font-size: 1em;
	font-weight: bold;
	line-height: 1.5;
	padding: 2px 0.4em;
	margin: 0.5em 0 0 0;
	height: auto;
	border: 0;
}
.ui-selectmenu-open {
	display: block;
}
.ui-selectmenu-text {
	display: block;
	margin-right: 20px;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-selectmenu-button.ui-button {
	text-align: left;
	white-space: nowrap;
	width: 14em;
}
.ui-selectmenu-icon.ui-icon {
	float: right;
	margin-top: 0;
}
.ui-slider {
	position: relative;
	text-align: left;
}
.ui-slider .ui-slider-handle {
	position: absolute;
	z-index: 2;
	width: 1.2em;
	height: 1.2em;
	cursor: default;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-slider .ui-slider-range {
	position: absolute;
	z-index: 1;
	font-size: .7em;
	display: block;
	border: 0;
	background-position: 0 0;
}


.ui-slider.ui-state-disabled .ui-slider-handle,
.ui-slider.ui-state-disabled .ui-slider-range {
	filter: inherit;
}

.ui-slider-horizontal {
	height: .8em;
}
.ui-slider-horizontal .ui-slider-handle {
	top: -.3em;
	margin-left: -.6em;
}
.ui-slider-horizontal .ui-slider-range {
	top: 0;
	height: 100%;
}
.ui-slider-horizontal .ui-slider-range-min {
	left: 0;
}
.ui-slider-horizontal .ui-slider-range-max {
	right: 0;
}

.ui-slider-vertical {
	width: .8em;
	height: 100px;
}
.ui-slider-vertical .ui-slider-handle {
	left: -.3em;
	margin-left: 0;
	margin-bottom: -.6em;
}
.ui-slider-vertical .ui-slider-range {
	left: 0;
	width: 100%;
}
.ui-slider-vertical .ui-slider-range-min {
	bottom: 0;
}
.ui-slider-vertical .ui-slider-range-max {
	top: 0;
}
.ui-sortable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-spinner {
	position: relative;
	display: inline-block;
	overflow: hidden;
	padding: 0;
	vertical-align: middle;
}
.ui-spinner-input {
	border: none;
	background: none;
	color: inherit;
	padding: .222em 0;
	margin: .2em 0;
	vertical-align: middle;
	margin-left: .4em;
	margin-right: 2em;
}
.ui-spinner-button {
	width: 1.6em;
	height: 50%;
	font-size: .5em;
	padding: 0;
	margin: 0;
	text-align: center;
	position: absolute;
	cursor: default;
	display: block;
	overflow: hidden;
	right: 0;
}

.ui-spinner a.ui-spinner-button {
	border-top-style: none;
	border-bottom-style: none;
	border-right-style: none;
}
.ui-spinner-up {
	top: 0;
}
.ui-spinner-down {
	bottom: 0;
}
.ui-tabs {
	position: relative;
	padding: .2em;
}
.ui-tabs .ui-tabs-nav {
	margin: 0;
	padding: .2em .2em 0;
}
.ui-tabs .ui-tabs-nav li {
	list-style: none;
	float: left;
	position: relative;
	top: 0;
	margin: 1px .2em 0 0;
	border-bottom-width: 0;
	padding: 0;
	white-space: nowrap;
}
.ui-tabs .ui-tabs-nav .ui-tabs-anchor {
	float: left;
	padding: .5em 1em;
	text-decoration: none;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active {
	margin-bottom: -1px;
	padding-bottom: 1px;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-state-disabled .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-tabs-loading .ui-tabs-anchor {
	cursor: text;
}
.ui-tabs-collapsible .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor {
	cursor: pointer;
}
.ui-tabs .ui-tabs-panel {
	display: block;
	border-width: 0;
	padding: 1em 1.4em;
	background: none;
}
.ui-tooltip {
	padding: 8px;
	position: absolute;
	z-index: 9999;
	max-width: 300px;
}
body .ui-tooltip {
	border-width: 2px;
}

.ui-widget {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget .ui-widget {
	font-size: 1em;
}
.ui-widget input,
.ui-widget select,
.ui-widget textarea,
.ui-widget button {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget.ui-widget-content {
	border: 1px solid #c5c5c5;
}
.ui-widget-content {
	border: 1px solid #dddddd;
	background: #ffffff;
	color: #333333;
}
.ui-widget-content a {
	color: #333333;
}
.ui-widget-header {
	border: 1px solid #dddddd;
	background: #e9e9e9;
	color: #333333;
	font-weight: bold;
}
.ui-widget-header a {
	color: #333333;
}


.ui-state-default,
.ui-widget-content .ui-state-default,
.ui-widget-header .ui-state-default,
.ui-button,


html .ui-button.ui-state-disabled:hover,
html .ui-button.ui-state-disabled:active {
	border: 1px solid #c5c5c5;
	background: #f6f6f6;
	font-weight: normal;
	color: #454545;
}
.ui-state-default a,
.ui-state-default a:link,
.ui-state-default a:visited,
a.ui-button,
a:link.ui-button,
a:visited.ui-button,
.ui-button {
	color: #454545;
	text-decoration: none;
}
.ui-state-hover,
.ui-widget-content .ui-state-hover,
.ui-widget-header .ui-state-hover,
.ui-state-focus,
.ui-widget-content .ui-state-focus,
.ui-widget-header .ui-state-focus,
.ui-button:hover,
.ui-button:focus {
	border: 1px solid #cccccc;
	background: #ededed;
	font-weight: normal;
	color: #2b2b2b;
}
.ui-state-hover a,
.ui-state-hover a:hover,
.ui-state-hover a:link,
.ui-state-hover a:visited,
.ui-state-focus a,
.ui-state-focus a:hover,
.ui-state-focus a:link,
.ui-state-focus a:visited,
a.ui-button:hover,
a.ui-button:focus {
	color: #2b2b2b;
	text-decoration: none;
}

.ui-visual-focus {
	box-shadow: 0 0 3px 1px rgb(94, 158, 214);
}
.ui-state-active,
.ui-widget-content .ui-state-active,
.ui-widget-header .ui-state-active,
a.ui-button:active,
.ui-button:active,
.ui-button.ui-state-active:hover {
	border: 1px solid #003eff;
	background: #007fff;
	font-weight: normal;
	color: #ffffff;
}
.ui-icon-background,
.ui-state-active .ui-icon-background {
	border: #003eff;
	background-color: #ffffff;
}
.ui-state-active a,
.ui-state-active a:link,
.ui-state-active a:visited {
	color: #ffffff;
	text-decoration: none;
}


.ui-state-highlight,
.ui-widget-content .ui-state-highlight,
.ui-widget-header .ui-state-highlight {
	border: 1px solid #dad55e;
	background: #fffa90;
	color: #777620;
}
.ui-state-checked {
	border: 1px solid #dad55e;
	background: #fffa90;
}
.ui-state-highlight a,
.ui-widget-content .ui-state-highlight a,
.ui-widget-header .ui-state-highlight a {
	color: #777620;
}
.ui-state-error,
.ui-widget-content .ui-state-error,
.ui-widget-header .ui-state-error {
	border: 1px solid #f1a899;
	background: #fddfdf;
	color: #5f3f3f;
}
.ui-state-error a,
.ui-widget-content .ui-state-error a,
.ui-widget-header .ui-state-error a {
	color: #5f3f3f;
}
.ui-state-error-text,
.ui-widget-content .ui-state-error-text,
.ui-widget-header .ui-state-error-text {
	color: #5f3f3f;
}
.ui-priority-primary,
.ui-widget-content .ui-priority-primary,
.ui-widget-header .ui-priority-primary {
	font-weight: bold;
}
.ui-priority-secondary,
.ui-widget-content .ui-priority-secondary,
.ui-widget-header .ui-priority-secondary {
	opacity: .7;
	filter:Alpha(Opacity=70); 
	font-weight: normal;
}
.ui-state-disabled,
.ui-widget-content .ui-state-disabled,
.ui-widget-header .ui-state-disabled {
	opacity: .35;
	filter:Alpha(Opacity=35); 
	background-image: none;
}
.ui-state-disabled .ui-icon {
	filter:Alpha(Opacity=35); 
}




.ui-icon {
	width: 16px;
	height: 16px;
}
.ui-icon,
.ui-widget-content .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-widget-header .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-state-hover .ui-icon,
.ui-state-focus .ui-icon,
.ui-button:hover .ui-icon,
.ui-button:focus .ui-icon {
	background-image: url("images/ui-icons_555555_256x240.png");
}
.ui-state-active .ui-icon,
.ui-button:active .ui-icon {
	background-image: url("images/ui-icons_ffffff_256x240.png");
}
.ui-state-highlight .ui-icon,
.ui-button .ui-state-highlight.ui-icon {
	background-image: url("images/ui-icons_777620_256x240.png");
}
.ui-state-error .ui-icon,
.ui-state-error-text .ui-icon {
	background-image: url("images/ui-icons_cc0000_256x240.png");
}
.ui-button .ui-icon {
	background-image: url("images/ui-icons_777777_256x240.png");
}


.ui-icon-blank { background-position: 16px 16px; }
.ui-icon-caret-1-n { background-position: 0 0; }
.ui-icon-caret-1-ne { background-position: -16px 0; }
.ui-icon-caret-1-e { background-position: -32px 0; }
.ui-icon-caret-1-se { background-position: -48px 0; }
.ui-icon-caret-1-s { background-position: -65px 0; }
.ui-icon-caret-1-sw { background-position: -80px 0; }
.ui-icon-caret-1-w { background-position: -96px 0; }
.ui-icon-caret-1-nw { background-position: -112px 0; }
.ui-icon-caret-2-n-s { background-position: -128px 0; }
.ui-icon-caret-2-e-w { background-position: -144px 0; }
.ui-icon-triangle-1-n { background-position: 0 -16px; }
.ui-icon-triangle-1-ne { background-position: -16px -16px; }
.ui-icon-triangle-1-e { background-position: -32px -16px; }
.ui-icon-triangle-1-se { background-position: -48px -16px; }
.ui-icon-triangle-1-s { background-position: -65px -16px; }
.ui-icon-triangle-1-sw { background-position: -80px -16px; }
.ui-icon-triangle-1-w { background-position: -96px -16px; }
.ui-icon-triangle-1-nw { background-position: -112px -16px; }
.ui-icon-triangle-2-n-s { background-position: -128px -16px; }
.ui-icon-triangle-2-e-w { background-position: -144px -16px; }
.ui-icon-arrow-1-n { background-position: 0 -32px; }
.ui-icon-arrow-1-ne { background-position: -16px -32px; }
.ui-icon-arrow-1-e { background-position: -32px -32px; }
.ui-icon-arrow-1-se { background-position: -48px -32px; }
.ui-icon-arrow-1-s { background-position: -65px -32px; }
.ui-icon-arrow-1-sw { background-position: -80px -32px; }
.ui-icon-arrow-1-w { background-position: -96px -32px; }
.ui-icon-arrow-1-nw { background-position: -112px -32px; }
.ui-icon-arrow-2-n-s { background-position: -128px -32px; }
.ui-icon-arrow-2-ne-sw { background-position: -144px -32px; }
.ui-icon-arrow-2-e-w { background-position: -160px -32px; }
.ui-icon-arrow-2-se-nw { background-position: -176px -32px; }
.ui-icon-arrowstop-1-n { background-position: -192px -32px; }
.ui-icon-arrowstop-1-e { background-position: -208px -32px; }
.ui-icon-arrowstop-1-s { background-position: -224px -32px; }
.ui-icon-arrowstop-1-w { background-position: -240px -32px; }
.ui-icon-arrowthick-1-n { background-position: 1px -48px; }
.ui-icon-arrowthick-1-ne { background-position: -16px -48px; }
.ui-icon-arrowthick-1-e { background-position: -32px -48px; }
.ui-icon-arrowthick-1-se { background-position: -48px -48px; }
.ui-icon-arrowthick-1-s { background-position: -64px -48px; }
.ui-icon-arrowthick-1-sw { background-position: -80px -48px; }
.ui-icon-arrowthick-1-w { background-position: -96px -48px; }
.ui-icon-arrowthick-1-nw { background-position: -112px -48px; }
.ui-icon-arrowthick-2-n-s { background-position: -128px -48px; }
.ui-icon-arrowthick-2-ne-sw { background-position: -144px -48px; }
.ui-icon-arrowthick-2-e-w { background-position: -160px -48px; }
.ui-icon-arrowthick-2-se-nw { background-position: -176px -48px; }
.ui-icon-arrowthickstop-1-n { background-position: -192px -48px; }
.ui-icon-arrowthickstop-1-e { background-position: -208px -48px; }
.ui-icon-arrowthickstop-1-s { background-position: -224px -48px; }
.ui-icon-arrowthickstop-1-w { background-position: -240px -48px; }
.ui-icon-arrowreturnthick-1-w { background-position: 0 -64px; }
.ui-icon-arrowreturnthick-1-n { background-position: -16px -64px; }
.ui-icon-arrowreturnthick-1-e { background-position: -32px -64px; }
.ui-icon-arrowreturnthick-1-s { background-position: -48px -64px; }
.ui-icon-arrowreturn-1-w { background-position: -64px -64px; }
.ui-icon-arrowreturn-1-n { background-position: -80px -64px; }
.ui-icon-arrowreturn-1-e { background-position: -96px -64px; }
.ui-icon-arrowreturn-1-s { background-position: -112px -64px; }
.ui-icon-arrowrefresh-1-w { background-position: -128px -64px; }
.ui-icon-arrowrefresh-1-n { background-position: -144px -64px; }
.ui-icon-arrowrefresh-1-e { background-position: -160px -64px; }
.ui-icon-arrowrefresh-1-s { background-position: -176px -64px; }
.ui-icon-arrow-4 { background-position: 0 -80px; }
.ui-icon-arrow-4-diag { background-position: -16px -80px; }
.ui-icon-extlink { background-position: -32px -80px; }
.ui-icon-newwin { background-position: -48px -80px; }
.ui-icon-refresh { background-position: -64px -80px; }
.ui-icon-shuffle { background-position: -80px -80px; }
.ui-icon-transfer-e-w { background-position: -96px -80px; }
.ui-icon-transferthick-e-w { background-position: -112px -80px; }
.ui-icon-folder-collapsed { background-position: 0 -96px; }
.ui-icon-folder-open { background-position: -16px -96px; }
.ui-icon-document { background-position: -32px -96px; }
.ui-icon-document-b { background-position: -48px -96px; }
.ui-icon-note { background-position: -64px -96px; }
.ui-icon-mail-closed { background-position: -80px -96px; }
.ui-icon-mail-open { background-position: -96px -96px; }
.ui-icon-suitcase { background-position: -112px -96px; }
.ui-icon-comment { background-position: -128px -96px; }
.ui-icon-person { background-position: -144px -96px; }
.ui-icon-print { background-position: -160px -96px; }
.ui-icon-trash { background-position: -176px -96px; }
.ui-icon-locked { background-position: -192px -96px; }
.ui-icon-unlocked { background-position: -208px -96px; }
.ui-icon-bookmark { background-position: -224px -96px; }
.ui-icon-tag { background-position: -240px -96px; }
.ui-icon-home { background-position: 0 -112px; }
.ui-icon-flag { background-position: -16px -112px; }
.ui-icon-calendar { background-position: -32px -112px; }
.ui-icon-cart { background-position: -48px -112px; }
.ui-icon-pencil { background-position: -64px -112px; }
.ui-icon-clock { background-position: -80px -112px; }
.ui-icon-disk { background-position: -96px -112px; }
.ui-icon-calculator { background-position: -112px -112px; }
.ui-icon-zoomin { background-position: -128px -112px; }
.ui-icon-zoomout { background-position: -144px -112px; }
.ui-icon-search { background-position: -160px -112px; }
.ui-icon-wrench { background-position: -176px -112px; }
.ui-icon-gear { background-position: -192px -112px; }
.ui-icon-heart { background-position: -208px -112px; }
.ui-icon-star { background-position: -224px -112px; }
.ui-icon-link { background-position: -240px -112px; }
.ui-icon-cancel { background-position: 0 -128px; }
.ui-icon-plus { background-position: -16px -128px; }
.ui-icon-plusthick { background-position: -32px -128px; }
.ui-icon-minus { background-position: -48px -128px; }
.ui-icon-minusthick { background-position: -64px -128px; }
.ui-icon-close { background-position: -80px -128px; }
.ui-icon-closethick { background-position: -96px -128px; }
.ui-icon-key { background-position: -112px -128px; }
.ui-icon-lightbulb { background-position: -128px -128px; }
.ui-icon-scissors { background-position: -144px -128px; }
.ui-icon-clipboard { background-position: -160px -128px; }
.ui-icon-copy { background-position: -176px -128px; }
.ui-icon-contact { background-position: -192px -128px; }
.ui-icon-image { background-position: -208px -128px; }
.ui-icon-video { background-position: -224px -128px; }
.ui-icon-script { background-position: -240px -128px; }
.ui-icon-alert { background-position: 0 -144px; }
.ui-icon-info { background-position: -16px -144px; }
.ui-icon-notice { background-position: -32px -144px; }
.ui-icon-help { background-position: -48px -144px; }
.ui-icon-check { background-position: -64px -144px; }
.ui-icon-bullet { background-position: -80px -144px; }
.ui-icon-radio-on { background-position: -96px -144px; }
.ui-icon-radio-off { background-position: -112px -144px; }
.ui-icon-pin-w { background-position: -128px -144px; }
.ui-icon-pin-s { background-position: -144px -144px; }
.ui-icon-play { background-position: 0 -160px; }
.ui-icon-pause { background-position: -16px -160px; }
.ui-icon-seek-next { background-position: -32px -160px; }
.ui-icon-seek-prev { background-position: -48px -160px; }
.ui-icon-seek-end { background-position: -64px -160px; }
.ui-icon-seek-start { background-position: -80px -160px; }

.ui-icon-seek-first { background-position: -80px -160px; }
.ui-icon-stop { background-position: -96px -160px; }
.ui-icon-eject { background-position: -112px -160px; }
.ui-icon-volume-off { background-position: -128px -160px; }
.ui-icon-volume-on { background-position: -144px -160px; }
.ui-icon-power { background-position: 0 -176px; }
.ui-icon-signal-diag { background-position: -16px -176px; }
.ui-icon-signal { background-position: -32px -176px; }
.ui-icon-battery-0 { background-position: -48px -176px; }
.ui-icon-battery-1 { background-position: -64px -176px; }
.ui-icon-battery-2 { background-position: -80px -176px; }
.ui-icon-battery-3 { background-position: -96px -176px; }
.ui-icon-circle-plus { background-position: 0 -192px; }
.ui-icon-circle-minus { background-position: -16px -192px; }
.ui-icon-circle-close { background-position: -32px -192px; }
.ui-icon-circle-triangle-e { background-position: -48px -192px; }
.ui-icon-circle-triangle-s { background-position: -64px -192px; }
.ui-icon-circle-triangle-w { background-position: -80px -192px; }
.ui-icon-circle-triangle-n { background-position: -96px -192px; }
.ui-icon-circle-arrow-e { background-position: -112px -192px; }
.ui-icon-circle-arrow-s { background-position: -128px -192px; }
.ui-icon-circle-arrow-w { background-position: -144px -192px; }
.ui-icon-circle-arrow-n { background-position: -160px -192px; }
.ui-icon-circle-zoomin { background-position: -176px -192px; }
.ui-icon-circle-zoomout { background-position: -192px -192px; }
.ui-icon-circle-check { background-position: -208px -192px; }
.ui-icon-circlesmall-plus { background-position: 0 -208px; }
.ui-icon-circlesmall-minus { background-position: -16px -208px; }
.ui-icon-circlesmall-close { background-position: -32px -208px; }
.ui-icon-squaresmall-plus { background-position: -48px -208px; }
.ui-icon-squaresmall-minus { background-position: -64px -208px; }
.ui-icon-squaresmall-close { background-position: -80px -208px; }
.ui-icon-grip-dotted-vertical { background-position: 0 -224px; }
.ui-icon-grip-dotted-horizontal { background-position: -16px -224px; }
.ui-icon-grip-solid-vertical { background-position: -32px -224px; }
.ui-icon-grip-solid-horizontal { background-position: -48px -224px; }
.ui-icon-gripsmall-diagonal-se { background-position: -64px -224px; }
.ui-icon-grip-diagonal-se { background-position: -80px -224px; }





.ui-corner-all,
.ui-corner-top,
.ui-corner-left,
.ui-corner-tl {
	border-top-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-top,
.ui-corner-right,
.ui-corner-tr {
	border-top-right-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-left,
.ui-corner-bl {
	border-bottom-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-right,
.ui-corner-br {
	border-bottom-right-radius: 3px;
}


.ui-widget-overlay {
	background: #aaaaaa;
	opacity: .3;
	filter: Alpha(Opacity=30); 
}
.ui-widget-shadow {
	-webkit-box-shadow: 0px 0px 5px #666666;
	box-shadow: 0px 0px 5px #666666;
} 
</style>
<script src='js/jquery-1.12.4.js'></script>
<script src='js/jquery-ui.js'></script>

<script>
$( function() {
	$( '#tabs-collect-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-consensus-heatmap'>
<ul>
<li><a href='#tab-collect-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-consensus-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-1-1.png" alt="plot of chunk tab-collect-consensus-heatmap-1"/></p>

</div>
<div id='tab-collect-consensus-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-2-1.png" alt="plot of chunk tab-collect-consensus-heatmap-2"/></p>

</div>
<div id='tab-collect-consensus-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-3-1.png" alt="plot of chunk tab-collect-consensus-heatmap-3"/></p>

</div>
<div id='tab-collect-consensus-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-4-1.png" alt="plot of chunk tab-collect-consensus-heatmap-4"/></p>

</div>
<div id='tab-collect-consensus-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-5-1.png" alt="plot of chunk tab-collect-consensus-heatmap-5"/></p>

</div>
</div>



### Membership heatmap

Membership heatmaps for all methods. ([What is a membership heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_12))


<script>
$( function() {
	$( '#tabs-collect-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-membership-heatmap'>
<ul>
<li><a href='#tab-collect-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-membership-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-1-1.png" alt="plot of chunk tab-collect-membership-heatmap-1"/></p>

</div>
<div id='tab-collect-membership-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-2-1.png" alt="plot of chunk tab-collect-membership-heatmap-2"/></p>

</div>
<div id='tab-collect-membership-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-3-1.png" alt="plot of chunk tab-collect-membership-heatmap-3"/></p>

</div>
<div id='tab-collect-membership-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-4-1.png" alt="plot of chunk tab-collect-membership-heatmap-4"/></p>

</div>
<div id='tab-collect-membership-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-5-1.png" alt="plot of chunk tab-collect-membership-heatmap-5"/></p>

</div>
</div>



### Signature heatmap

Signature heatmaps for all methods. ([What is a signature heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_22))


Note in following heatmaps, rows are scaled.



<script>
$( function() {
	$( '#tabs-collect-get-signatures' ).tabs();
} );
</script>
<div id='tabs-collect-get-signatures'>
<ul>
<li><a href='#tab-collect-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-collect-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-collect-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-collect-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-collect-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-collect-get-signatures-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-1-1.png" alt="plot of chunk tab-collect-get-signatures-1"/></p>

</div>
<div id='tab-collect-get-signatures-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-2-1.png" alt="plot of chunk tab-collect-get-signatures-2"/></p>

</div>
<div id='tab-collect-get-signatures-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-3-1.png" alt="plot of chunk tab-collect-get-signatures-3"/></p>

</div>
<div id='tab-collect-get-signatures-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-4-1.png" alt="plot of chunk tab-collect-get-signatures-4"/></p>

</div>
<div id='tab-collect-get-signatures-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-5-1.png" alt="plot of chunk tab-collect-get-signatures-5"/></p>

</div>
</div>



### Statistics table

The statistics used for measuring the stability of consensus partitioning.
([How are they
defined?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13))


<script>
$( function() {
	$( '#tabs-get-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-get-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-get-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-get-stats-from-consensus-partition-list-1'>
<pre><code class="r">get_stats(res_list, k = 2)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      2 0.707          0.8808       0.947          0.483 0.509   0.509
#&gt; CV:NMF      2 0.778          0.8855       0.952          0.474 0.527   0.527
#&gt; MAD:NMF     2 0.920          0.9447       0.974          0.484 0.509   0.509
#&gt; ATC:NMF     2 0.662          0.9312       0.947          0.239 0.735   0.735
#&gt; SD:skmeans  2 1.000          0.9698       0.987          0.501 0.497   0.497
#&gt; CV:skmeans  2 0.959          0.9492       0.979          0.493 0.509   0.509
#&gt; MAD:skmeans 2 1.000          0.9802       0.991          0.502 0.497   0.497
#&gt; ATC:skmeans 2 0.959          0.9639       0.983          0.496 0.509   0.509
#&gt; SD:mclust   2 0.487          0.6468       0.793          0.389 0.660   0.660
#&gt; CV:mclust   2 0.442          0.7007       0.750          0.405 0.566   0.566
#&gt; MAD:mclust  2 0.453          0.8646       0.894          0.412 0.599   0.599
#&gt; ATC:mclust  2 0.674          0.9132       0.949          0.289 0.683   0.683
#&gt; SD:kmeans   2 0.413          0.8512       0.855          0.415 0.566   0.566
#&gt; CV:kmeans   2 0.470          0.9023       0.901          0.418 0.566   0.566
#&gt; MAD:kmeans  2 0.471          0.8892       0.897          0.435 0.566   0.566
#&gt; ATC:kmeans  2 1.000          0.9500       0.981          0.366 0.638   0.638
#&gt; SD:pam      2 0.959          0.9471       0.978          0.371 0.638   0.638
#&gt; CV:pam      2 0.960          0.9297       0.942          0.385 0.599   0.599
#&gt; MAD:pam     2 0.999          0.9603       0.983          0.390 0.618   0.618
#&gt; ATC:pam     2 1.000          0.9975       0.999          0.339 0.660   0.660
#&gt; SD:hclust   2 0.618          0.8925       0.938          0.413 0.581   0.581
#&gt; CV:hclust   2 0.675          0.8934       0.944          0.394 0.581   0.581
#&gt; MAD:hclust  2 0.701          0.8535       0.935          0.433 0.581   0.581
#&gt; ATC:hclust  2 0.481          0.0879       0.599          0.413 0.683   0.683
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-2'>
<pre><code class="r">get_stats(res_list, k = 3)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      3 1.000           0.963       0.984          0.300 0.689   0.481
#&gt; CV:NMF      3 1.000           0.986       0.994          0.312 0.729   0.538
#&gt; MAD:NMF     3 0.770           0.796       0.922          0.335 0.798   0.620
#&gt; ATC:NMF     3 0.895           0.931       0.972          0.651 0.871   0.825
#&gt; SD:skmeans  3 0.978           0.914       0.966          0.305 0.697   0.474
#&gt; CV:skmeans  3 0.969           0.903       0.962          0.324 0.733   0.527
#&gt; MAD:skmeans 3 0.825           0.874       0.944          0.314 0.697   0.474
#&gt; ATC:skmeans 3 0.953           0.928       0.956          0.275 0.821   0.656
#&gt; SD:mclust   3 0.543           0.665       0.855          0.518 0.672   0.512
#&gt; CV:mclust   3 0.679           0.834       0.910          0.445 0.491   0.329
#&gt; MAD:mclust  3 0.588           0.803       0.892          0.563 0.602   0.399
#&gt; ATC:mclust  3 0.455           0.657       0.837          0.608 0.855   0.799
#&gt; SD:kmeans   3 0.693           0.883       0.928          0.470 0.799   0.648
#&gt; CV:kmeans   3 0.631           0.867       0.901          0.432 0.785   0.633
#&gt; MAD:kmeans  3 0.742           0.902       0.932          0.406 0.817   0.676
#&gt; ATC:kmeans  3 1.000           0.968       0.988          0.747 0.684   0.518
#&gt; SD:pam      3 1.000           0.961       0.984          0.635 0.701   0.551
#&gt; CV:pam      3 1.000           0.969       0.990          0.549 0.664   0.496
#&gt; MAD:pam     3 0.999           0.956       0.981          0.594 0.689   0.527
#&gt; ATC:pam     3 0.629           0.705       0.882          0.781 0.618   0.463
#&gt; SD:hclust   3 0.580           0.846       0.925          0.206 0.947   0.909
#&gt; CV:hclust   3 0.623           0.806       0.902          0.269 0.947   0.909
#&gt; MAD:hclust  3 0.745           0.855       0.937          0.129 0.947   0.909
#&gt; ATC:hclust  3 0.466           0.590       0.808          0.267 0.607   0.509
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-3'>
<pre><code class="r">get_stats(res_list, k = 4)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      4 0.662           0.598       0.831         0.1454 0.855   0.637
#&gt; CV:NMF      4 0.713           0.676       0.835         0.1624 0.839   0.591
#&gt; MAD:NMF     4 0.771           0.771       0.883         0.1092 0.837   0.596
#&gt; ATC:NMF     4 0.645           0.785       0.904         0.7185 0.701   0.512
#&gt; SD:skmeans  4 0.942           0.892       0.958         0.1456 0.885   0.680
#&gt; CV:skmeans  4 0.964           0.936       0.973         0.1529 0.885   0.680
#&gt; MAD:skmeans 4 0.961           0.942       0.975         0.1334 0.879   0.668
#&gt; ATC:skmeans 4 0.923           0.890       0.954         0.1167 0.927   0.798
#&gt; SD:mclust   4 0.770           0.856       0.918         0.0197 0.775   0.556
#&gt; CV:mclust   4 0.638           0.341       0.709         0.0826 0.774   0.515
#&gt; MAD:mclust  4 0.870           0.920       0.955        -0.0335 0.761   0.501
#&gt; ATC:mclust  4 0.361           0.408       0.727         0.1490 0.864   0.799
#&gt; SD:kmeans   4 0.705           0.680       0.849         0.1464 0.861   0.655
#&gt; CV:kmeans   4 0.676           0.683       0.841         0.1660 0.826   0.588
#&gt; MAD:kmeans  4 0.747           0.732       0.870         0.1285 0.937   0.837
#&gt; ATC:kmeans  4 0.586           0.599       0.721         0.1180 0.790   0.488
#&gt; SD:pam      4 0.766           0.810       0.916         0.2357 0.784   0.497
#&gt; CV:pam      4 0.732           0.773       0.895         0.2390 0.802   0.530
#&gt; MAD:pam     4 0.787           0.752       0.884         0.2137 0.855   0.631
#&gt; ATC:pam     4 0.572           0.708       0.830         0.1585 0.844   0.617
#&gt; SD:hclust   4 0.602           0.843       0.881         0.2201 0.837   0.692
#&gt; CV:hclust   4 0.640           0.865       0.894         0.1957 0.837   0.692
#&gt; MAD:hclust  4 0.571           0.782       0.861         0.3372 0.837   0.692
#&gt; ATC:hclust  4 0.484           0.701       0.731         0.1734 0.756   0.534
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-4'>
<pre><code class="r">get_stats(res_list, k = 5)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      5 0.670           0.667       0.787         0.0929 0.828   0.482
#&gt; CV:NMF      5 0.681           0.665       0.784         0.0893 0.865   0.548
#&gt; MAD:NMF     5 0.690           0.641       0.745         0.0906 0.851   0.540
#&gt; ATC:NMF     5 0.568           0.597       0.802         0.0765 0.898   0.695
#&gt; SD:skmeans  5 0.810           0.751       0.876         0.0588 0.931   0.744
#&gt; CV:skmeans  5 0.804           0.736       0.878         0.0548 0.941   0.775
#&gt; MAD:skmeans 5 0.799           0.778       0.882         0.0612 0.961   0.846
#&gt; ATC:skmeans 5 0.775           0.778       0.875         0.0930 0.897   0.663
#&gt; SD:mclust   5 0.639           0.636       0.823         0.2003 0.798   0.556
#&gt; CV:mclust   5 0.762           0.742       0.857         0.1606 0.779   0.409
#&gt; MAD:mclust  5 0.786           0.774       0.893         0.1355 0.925   0.807
#&gt; ATC:mclust  5 0.538           0.780       0.854         0.2513 0.652   0.465
#&gt; SD:kmeans   5 0.661           0.702       0.835         0.0854 0.890   0.653
#&gt; CV:kmeans   5 0.684           0.717       0.838         0.0933 0.900   0.678
#&gt; MAD:kmeans  5 0.691           0.635       0.807         0.1013 0.880   0.647
#&gt; ATC:kmeans  5 0.588           0.614       0.755         0.0765 0.922   0.718
#&gt; SD:pam      5 0.734           0.535       0.776         0.0504 0.873   0.578
#&gt; CV:pam      5 0.710           0.582       0.777         0.0520 0.875   0.586
#&gt; MAD:pam     5 0.766           0.671       0.825         0.0514 0.928   0.723
#&gt; ATC:pam     5 0.746           0.793       0.876         0.1046 0.888   0.627
#&gt; SD:hclust   5 0.720           0.740       0.859         0.1515 0.959   0.889
#&gt; CV:hclust   5 0.701           0.820       0.890         0.1477 0.932   0.817
#&gt; MAD:hclust  5 0.651           0.652       0.806         0.1062 0.863   0.642
#&gt; ATC:hclust  5 0.634           0.695       0.858         0.1523 0.956   0.848
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-5'>
<pre><code class="r">get_stats(res_list, k = 6)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      6 0.721           0.730       0.804         0.0516 0.939   0.734
#&gt; CV:NMF      6 0.786           0.751       0.855         0.0470 0.902   0.605
#&gt; MAD:NMF     6 0.738           0.675       0.826         0.0554 0.924   0.666
#&gt; ATC:NMF     6 0.526           0.484       0.746         0.0445 0.900   0.656
#&gt; SD:skmeans  6 0.774           0.683       0.814         0.0376 0.954   0.797
#&gt; CV:skmeans  6 0.768           0.615       0.808         0.0389 0.965   0.838
#&gt; MAD:skmeans 6 0.772           0.638       0.819         0.0397 0.959   0.814
#&gt; ATC:skmeans 6 0.813           0.747       0.857         0.0329 0.987   0.940
#&gt; SD:mclust   6 0.873           0.834       0.929         0.0417 0.909   0.700
#&gt; CV:mclust   6 0.852           0.836       0.922         0.0429 0.885   0.614
#&gt; MAD:mclust  6 0.749           0.701       0.807         0.0733 0.855   0.571
#&gt; ATC:mclust  6 0.649           0.627       0.801         0.1183 0.843   0.578
#&gt; SD:kmeans   6 0.711           0.744       0.813         0.0651 0.953   0.808
#&gt; CV:kmeans   6 0.727           0.754       0.833         0.0554 0.919   0.687
#&gt; MAD:kmeans  6 0.688           0.592       0.757         0.0655 0.941   0.758
#&gt; ATC:kmeans  6 0.619           0.588       0.713         0.0405 0.961   0.835
#&gt; SD:pam      6 0.855           0.774       0.912         0.0477 0.891   0.573
#&gt; CV:pam      6 0.781           0.685       0.860         0.0455 0.904   0.623
#&gt; MAD:pam     6 0.854           0.801       0.911         0.0381 0.959   0.799
#&gt; ATC:pam     6 0.814           0.825       0.902         0.0425 0.971   0.865
#&gt; SD:hclust   6 0.734           0.674       0.827         0.0740 0.911   0.734
#&gt; CV:hclust   6 0.727           0.851       0.884         0.0464 0.991   0.971
#&gt; MAD:hclust  6 0.676           0.556       0.727         0.0539 0.884   0.636
#&gt; ATC:hclust  6 0.639           0.665       0.800         0.0725 0.989   0.957
</code></pre>

</div>
</div>

Following heatmap plots the partition for each combination of methods and the
lightness correspond to the silhouette scores for samples in each method. On
top the consensus subgroup is inferred from all methods by taking the mean
silhouette scores as weight.


<script>
$( function() {
	$( '#tabs-collect-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-stats-from-consensus-partition-list-1'>
<pre><code class="r">collect_stats(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-2'>
<pre><code class="r">collect_stats(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-3'>
<pre><code class="r">collect_stats(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-4'>
<pre><code class="r">collect_stats(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-5'>
<pre><code class="r">collect_stats(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-5"/></p>

</div>
</div>

### Partition from all methods



Collect partitions from all methods:


<script>
$( function() {
	$( '#tabs-collect-classes-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-classes-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-classes-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-classes-from-consensus-partition-list-1'>
<pre><code class="r">collect_classes(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-2'>
<pre><code class="r">collect_classes(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-3'>
<pre><code class="r">collect_classes(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-4'>
<pre><code class="r">collect_classes(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-5'>
<pre><code class="r">collect_classes(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-5"/></p>

</div>
</div>



### Top rows overlap


Overlap of top rows from different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-euler' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-euler'>
<ul>
<li><a href='#tab-top-rows-overlap-by-euler-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-euler-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-5"/></p>

</div>
</div>

Also visualize the correspondance of rankings between different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-correspondance' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-correspondance'>
<ul>
<li><a href='#tab-top-rows-overlap-by-correspondance-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-correspondance-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-5"/></p>

</div>
</div>


Heatmaps of the top rows:



<script>
$( function() {
	$( '#tabs-top-rows-heatmap' ).tabs();
} );
</script>
<div id='tabs-top-rows-heatmap'>
<ul>
<li><a href='#tab-top-rows-heatmap-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-heatmap-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-heatmap-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-heatmap-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-heatmap-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-heatmap-1'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 1000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-1-1.png" alt="plot of chunk tab-top-rows-heatmap-1"/></p>

</div>
<div id='tab-top-rows-heatmap-2'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 2000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-2-1.png" alt="plot of chunk tab-top-rows-heatmap-2"/></p>

</div>
<div id='tab-top-rows-heatmap-3'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 3000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-3-1.png" alt="plot of chunk tab-top-rows-heatmap-3"/></p>

</div>
<div id='tab-top-rows-heatmap-4'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 4000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-4-1.png" alt="plot of chunk tab-top-rows-heatmap-4"/></p>

</div>
<div id='tab-top-rows-heatmap-5'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 5000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-5-1.png" alt="plot of chunk tab-top-rows-heatmap-5"/></p>

</div>
</div>




### Test to known annotations



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.


<script>
$( function() {
	$( '#tabs-test-to-known-factors-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-test-to-known-factors-from-consensus-partition-list'>
<ul>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-test-to-known-factors-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-test-to-known-factors-from-consensus-partition-list-1'>
<pre><code class="r">test_to_known_factors(res_list, k = 2)
</code></pre>

<pre><code>#&gt;              n disease.state(p) k
#&gt; SD:NMF      50         2.37e-01 2
#&gt; CV:NMF      50         2.99e-01 2
#&gt; MAD:NMF     51         2.06e-01 2
#&gt; ATC:NMF     51         9.92e-01 2
#&gt; SD:skmeans  52         3.51e-01 2
#&gt; CV:skmeans  51         4.76e-01 2
#&gt; MAD:skmeans 52         3.51e-01 2
#&gt; ATC:skmeans 52         5.14e-01 2
#&gt; SD:mclust   50         4.65e-10 2
#&gt; CV:mclust   51         3.04e-02 2
#&gt; MAD:mclust  49         4.52e-02 2
#&gt; ATC:mclust  52         2.33e-01 2
#&gt; SD:kmeans   52         5.15e-01 2
#&gt; CV:kmeans   52         5.15e-01 2
#&gt; MAD:kmeans  52         5.15e-01 2
#&gt; ATC:kmeans  51         1.00e+00 2
#&gt; SD:pam      51         3.64e-09 2
#&gt; CV:pam      52         5.58e-07 2
#&gt; MAD:pam     52         1.20e-07 2
#&gt; ATC:pam     52         1.00e+00 2
#&gt; SD:hclust   52         2.10e-01 2
#&gt; CV:hclust   52         2.10e-01 2
#&gt; MAD:hclust  48         2.95e-01 2
#&gt; ATC:hclust  10               NA 2
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-2'>
<pre><code class="r">test_to_known_factors(res_list, k = 3)
</code></pre>

<pre><code>#&gt;              n disease.state(p) k
#&gt; SD:NMF      52         8.38e-11 3
#&gt; CV:NMF      52         8.38e-11 3
#&gt; MAD:NMF     46         3.44e-08 3
#&gt; ATC:NMF     52         8.85e-01 3
#&gt; SD:skmeans  49         6.32e-08 3
#&gt; CV:skmeans  48         9.12e-08 3
#&gt; MAD:skmeans 50         2.80e-07 3
#&gt; ATC:skmeans 50         8.74e-02 3
#&gt; SD:mclust   40         2.38e-08 3
#&gt; CV:mclust   50         3.28e-09 3
#&gt; MAD:mclust  50         3.37e-05 3
#&gt; ATC:mclust  40         1.00e+00 3
#&gt; SD:kmeans   50         2.53e-10 3
#&gt; CV:kmeans   51         1.60e-10 3
#&gt; MAD:kmeans  51         2.17e-09 3
#&gt; ATC:kmeans  51         6.43e-01 3
#&gt; SD:pam      52         1.15e-08 3
#&gt; CV:pam      51         1.82e-09 3
#&gt; MAD:pam     51         1.89e-09 3
#&gt; ATC:pam     41         5.58e-01 3
#&gt; SD:hclust   51         7.39e-03 3
#&gt; CV:hclust   52         9.08e-03 3
#&gt; MAD:hclust  49         8.29e-03 3
#&gt; ATC:hclust  47         6.23e-01 3
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-3'>
<pre><code class="r">test_to_known_factors(res_list, k = 4)
</code></pre>

<pre><code>#&gt;              n disease.state(p) k
#&gt; SD:NMF      37         8.86e-07 4
#&gt; CV:NMF      41         9.19e-08 4
#&gt; MAD:NMF     45         1.14e-05 4
#&gt; ATC:NMF     47         1.14e-01 4
#&gt; SD:skmeans  48         7.35e-08 4
#&gt; CV:skmeans  51         8.58e-07 4
#&gt; MAD:skmeans 52         7.19e-06 4
#&gt; ATC:skmeans 51         1.20e-01 4
#&gt; SD:mclust   51         1.12e-08 4
#&gt; CV:mclust   17         1.55e-03 4
#&gt; MAD:mclust  52         7.78e-09 4
#&gt; ATC:mclust  26         1.19e-02 4
#&gt; SD:kmeans   38         6.55e-08 4
#&gt; CV:kmeans   41         8.21e-08 4
#&gt; MAD:kmeans  46         1.34e-08 4
#&gt; ATC:kmeans  36         2.18e-01 4
#&gt; SD:pam      49         2.10e-08 4
#&gt; CV:pam      44         1.53e-08 4
#&gt; MAD:pam     43         2.59e-07 4
#&gt; ATC:pam     45         6.19e-01 4
#&gt; SD:hclust   51         8.90e-10 4
#&gt; CV:hclust   52         4.65e-10 4
#&gt; MAD:hclust  46         8.29e-09 4
#&gt; ATC:hclust  47         1.21e-01 4
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-4'>
<pre><code class="r">test_to_known_factors(res_list, k = 5)
</code></pre>

<pre><code>#&gt;              n disease.state(p) k
#&gt; SD:NMF      41         4.46e-07 5
#&gt; CV:NMF      43         5.48e-06 5
#&gt; MAD:NMF     43         7.53e-06 5
#&gt; ATC:NMF     35         3.79e-02 5
#&gt; SD:skmeans  45         8.83e-07 5
#&gt; CV:skmeans  45         5.17e-08 5
#&gt; MAD:skmeans 46         3.89e-06 5
#&gt; ATC:skmeans 47         4.89e-02 5
#&gt; SD:mclust   38         3.48e-07 5
#&gt; CV:mclust   44         7.17e-08 5
#&gt; MAD:mclust  47         2.16e-08 5
#&gt; ATC:mclust  49         1.16e-02 5
#&gt; SD:kmeans   39         5.59e-07 5
#&gt; CV:kmeans   45         3.96e-08 5
#&gt; MAD:kmeans  37         5.89e-07 5
#&gt; ATC:kmeans  45         3.40e-02 5
#&gt; SD:pam      29         2.37e-04 5
#&gt; CV:pam      36         2.09e-06 5
#&gt; MAD:pam     45         4.32e-07 5
#&gt; ATC:pam     49         7.73e-01 5
#&gt; SD:hclust   49         6.19e-09 5
#&gt; CV:hclust   51         2.54e-09 5
#&gt; MAD:hclust  40         1.07e-07 5
#&gt; ATC:hclust  45         1.63e-01 5
</code></pre>

</div>
<div id='tab-test-to-known-factors-from-consensus-partition-list-5'>
<pre><code class="r">test_to_known_factors(res_list, k = 6)
</code></pre>

<pre><code>#&gt;              n disease.state(p) k
#&gt; SD:NMF      48         5.00e-07 6
#&gt; CV:NMF      47         7.36e-07 6
#&gt; MAD:NMF     43         3.44e-06 6
#&gt; ATC:NMF     30         2.41e-02 6
#&gt; SD:skmeans  43         4.14e-07 6
#&gt; CV:skmeans  38         1.15e-06 6
#&gt; MAD:skmeans 42         5.99e-06 6
#&gt; ATC:skmeans 43         1.54e-01 6
#&gt; SD:mclust   48         4.05e-08 6
#&gt; CV:mclust   49         2.64e-08 6
#&gt; MAD:mclust  43         2.07e-07 6
#&gt; ATC:mclust  38         4.70e-02 6
#&gt; SD:kmeans   51         1.02e-08 6
#&gt; CV:kmeans   49         2.23e-08 6
#&gt; MAD:kmeans  37         1.52e-06 6
#&gt; ATC:kmeans  40         7.61e-02 6
#&gt; SD:pam      43         3.33e-07 6
#&gt; CV:pam      40         1.17e-06 6
#&gt; MAD:pam     45         1.30e-06 6
#&gt; ATC:pam     50         8.05e-01 6
#&gt; SD:hclust   44         1.95e-07 6
#&gt; CV:hclust   51         6.96e-09 6
#&gt; MAD:hclust  34         1.58e-06 6
#&gt; ATC:hclust  47         2.72e-01 6
</code></pre>

</div>
</div>


 
## Results for each method


---------------------------------------------------




### SD:hclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "hclust"]
# you can also extract it by
# res = res_list["SD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-hclust-collect-plots](figure_cola/SD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-hclust-select-partition-number](figure_cola/SD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.618           0.892       0.938          0.413 0.581   0.581
#> 3 3 0.580           0.846       0.925          0.206 0.947   0.909
#> 4 4 0.602           0.843       0.881          0.220 0.837   0.692
#> 5 5 0.720           0.740       0.859          0.152 0.959   0.889
#> 6 6 0.734           0.674       0.827          0.074 0.911   0.734
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-classes'>
<ul>
<li><a href='#tab-SD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-hclust-get-classes-1'>
<p><a id='tab-SD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559433     1  0.0000      0.946 1.000 0.000
#&gt; GSM559434     2  0.6247      0.873 0.156 0.844
#&gt; GSM559436     1  0.0000      0.946 1.000 0.000
#&gt; GSM559437     1  0.0000      0.946 1.000 0.000
#&gt; GSM559438     2  0.7883      0.794 0.236 0.764
#&gt; GSM559440     2  0.7883      0.794 0.236 0.764
#&gt; GSM559441     1  0.1414      0.936 0.980 0.020
#&gt; GSM559442     1  0.6048      0.821 0.852 0.148
#&gt; GSM559444     2  0.5946      0.881 0.144 0.856
#&gt; GSM559445     1  0.0000      0.946 1.000 0.000
#&gt; GSM559446     1  0.0000      0.946 1.000 0.000
#&gt; GSM559448     1  0.0000      0.946 1.000 0.000
#&gt; GSM559450     2  0.5946      0.881 0.144 0.856
#&gt; GSM559451     1  0.0000      0.946 1.000 0.000
#&gt; GSM559452     2  0.8207      0.757 0.256 0.744
#&gt; GSM559454     1  0.0000      0.946 1.000 0.000
#&gt; GSM559455     1  0.1414      0.936 0.980 0.020
#&gt; GSM559456     1  0.0000      0.946 1.000 0.000
#&gt; GSM559457     1  0.0000      0.946 1.000 0.000
#&gt; GSM559458     1  0.1414      0.936 0.980 0.020
#&gt; GSM559459     1  0.0000      0.946 1.000 0.000
#&gt; GSM559460     1  0.0000      0.946 1.000 0.000
#&gt; GSM559461     1  0.0000      0.946 1.000 0.000
#&gt; GSM559462     1  0.0000      0.946 1.000 0.000
#&gt; GSM559463     1  0.0000      0.946 1.000 0.000
#&gt; GSM559464     1  0.0000      0.946 1.000 0.000
#&gt; GSM559465     1  0.0000      0.946 1.000 0.000
#&gt; GSM559467     1  0.8267      0.632 0.740 0.260
#&gt; GSM559468     1  0.5629      0.838 0.868 0.132
#&gt; GSM559469     1  0.5946      0.826 0.856 0.144
#&gt; GSM559470     1  0.8608      0.585 0.716 0.284
#&gt; GSM559471     1  0.0672      0.943 0.992 0.008
#&gt; GSM559472     1  0.0000      0.946 1.000 0.000
#&gt; GSM559473     2  0.4161      0.904 0.084 0.916
#&gt; GSM559475     2  0.4161      0.904 0.084 0.916
#&gt; GSM559477     2  0.1414      0.906 0.020 0.980
#&gt; GSM559478     2  0.1414      0.906 0.020 0.980
#&gt; GSM559479     2  0.1414      0.906 0.020 0.980
#&gt; GSM559480     2  0.1414      0.906 0.020 0.980
#&gt; GSM559481     2  0.1414      0.906 0.020 0.980
#&gt; GSM559482     2  0.1414      0.906 0.020 0.980
#&gt; GSM559435     1  0.1414      0.938 0.980 0.020
#&gt; GSM559439     1  0.1414      0.938 0.980 0.020
#&gt; GSM559443     1  0.1414      0.938 0.980 0.020
#&gt; GSM559447     1  0.1414      0.938 0.980 0.020
#&gt; GSM559449     1  0.1414      0.938 0.980 0.020
#&gt; GSM559453     1  0.1414      0.938 0.980 0.020
#&gt; GSM559466     1  0.1414      0.938 0.980 0.020
#&gt; GSM559474     1  0.8608      0.646 0.716 0.284
#&gt; GSM559476     1  0.1414      0.938 0.980 0.020
#&gt; GSM559483     2  0.1414      0.906 0.020 0.980
#&gt; GSM559484     1  0.8608      0.646 0.716 0.284
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-1-a').click(function(){
  $('#tab-SD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-2'>
<p><a id='tab-SD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559433     1  0.0000      0.914 1.000 0.000 0.000
#&gt; GSM559434     2  0.4731      0.824 0.128 0.840 0.032
#&gt; GSM559436     1  0.0000      0.914 1.000 0.000 0.000
#&gt; GSM559437     1  0.0237      0.913 0.996 0.000 0.004
#&gt; GSM559438     2  0.5803      0.734 0.212 0.760 0.028
#&gt; GSM559440     2  0.5803      0.734 0.212 0.760 0.028
#&gt; GSM559441     1  0.1315      0.902 0.972 0.008 0.020
#&gt; GSM559442     1  0.4677      0.791 0.840 0.132 0.028
#&gt; GSM559444     2  0.4397      0.835 0.116 0.856 0.028
#&gt; GSM559445     1  0.0237      0.913 0.996 0.000 0.004
#&gt; GSM559446     1  0.0237      0.913 0.996 0.000 0.004
#&gt; GSM559448     1  0.0000      0.914 1.000 0.000 0.000
#&gt; GSM559450     2  0.4397      0.835 0.116 0.856 0.028
#&gt; GSM559451     1  0.0000      0.914 1.000 0.000 0.000
#&gt; GSM559452     2  0.6402      0.684 0.236 0.724 0.040
#&gt; GSM559454     1  0.0000      0.914 1.000 0.000 0.000
#&gt; GSM559455     1  0.1315      0.902 0.972 0.008 0.020
#&gt; GSM559456     1  0.0000      0.914 1.000 0.000 0.000
#&gt; GSM559457     1  0.0000      0.914 1.000 0.000 0.000
#&gt; GSM559458     1  0.1453      0.900 0.968 0.008 0.024
#&gt; GSM559459     1  0.0000      0.914 1.000 0.000 0.000
#&gt; GSM559460     1  0.0000      0.914 1.000 0.000 0.000
#&gt; GSM559461     1  0.0000      0.914 1.000 0.000 0.000
#&gt; GSM559462     1  0.0000      0.914 1.000 0.000 0.000
#&gt; GSM559463     1  0.0000      0.914 1.000 0.000 0.000
#&gt; GSM559464     1  0.0000      0.914 1.000 0.000 0.000
#&gt; GSM559465     1  0.0000      0.914 1.000 0.000 0.000
#&gt; GSM559467     1  0.5737      0.607 0.732 0.256 0.012
#&gt; GSM559468     1  0.4397      0.807 0.856 0.116 0.028
#&gt; GSM559469     1  0.4609      0.794 0.844 0.128 0.028
#&gt; GSM559470     1  0.5986      0.553 0.704 0.284 0.012
#&gt; GSM559471     1  0.0829      0.909 0.984 0.004 0.012
#&gt; GSM559472     1  0.0237      0.913 0.996 0.000 0.004
#&gt; GSM559473     2  0.2743      0.860 0.052 0.928 0.020
#&gt; GSM559475     2  0.2743      0.860 0.052 0.928 0.020
#&gt; GSM559477     2  0.0000      0.857 0.000 1.000 0.000
#&gt; GSM559478     2  0.0000      0.857 0.000 1.000 0.000
#&gt; GSM559479     2  0.0000      0.857 0.000 1.000 0.000
#&gt; GSM559480     2  0.0000      0.857 0.000 1.000 0.000
#&gt; GSM559481     2  0.0000      0.857 0.000 1.000 0.000
#&gt; GSM559482     2  0.0000      0.857 0.000 1.000 0.000
#&gt; GSM559435     1  0.3551      0.833 0.868 0.000 0.132
#&gt; GSM559439     1  0.3551      0.833 0.868 0.000 0.132
#&gt; GSM559443     1  0.3551      0.833 0.868 0.000 0.132
#&gt; GSM559447     1  0.3551      0.833 0.868 0.000 0.132
#&gt; GSM559449     1  0.3941      0.814 0.844 0.000 0.156
#&gt; GSM559453     1  0.6291      0.221 0.532 0.000 0.468
#&gt; GSM559466     1  0.3551      0.833 0.868 0.000 0.132
#&gt; GSM559474     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559476     1  0.3551      0.833 0.868 0.000 0.132
#&gt; GSM559483     2  0.0000      0.857 0.000 1.000 0.000
#&gt; GSM559484     3  0.0000      1.000 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-2-a').click(function(){
  $('#tab-SD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-3'>
<p><a id='tab-SD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559433     1  0.0000      0.936 1.000 0.000 0.000 0.000
#&gt; GSM559434     2  0.3681      0.748 0.124 0.848 0.024 0.004
#&gt; GSM559436     1  0.0000      0.936 1.000 0.000 0.000 0.000
#&gt; GSM559437     1  0.0188      0.935 0.996 0.004 0.000 0.000
#&gt; GSM559438     2  0.4464      0.672 0.208 0.768 0.024 0.000
#&gt; GSM559440     2  0.4464      0.672 0.208 0.768 0.024 0.000
#&gt; GSM559441     1  0.1297      0.917 0.964 0.016 0.020 0.000
#&gt; GSM559442     1  0.4206      0.775 0.816 0.136 0.048 0.000
#&gt; GSM559444     2  0.3278      0.756 0.116 0.864 0.020 0.000
#&gt; GSM559445     1  0.0188      0.935 0.996 0.004 0.000 0.000
#&gt; GSM559446     1  0.0188      0.935 0.996 0.004 0.000 0.000
#&gt; GSM559448     1  0.0000      0.936 1.000 0.000 0.000 0.000
#&gt; GSM559450     2  0.3278      0.756 0.116 0.864 0.020 0.000
#&gt; GSM559451     1  0.0000      0.936 1.000 0.000 0.000 0.000
#&gt; GSM559452     2  0.5349      0.639 0.212 0.732 0.048 0.008
#&gt; GSM559454     1  0.0000      0.936 1.000 0.000 0.000 0.000
#&gt; GSM559455     1  0.1297      0.917 0.964 0.016 0.020 0.000
#&gt; GSM559456     1  0.0707      0.919 0.980 0.000 0.020 0.000
#&gt; GSM559457     1  0.0000      0.936 1.000 0.000 0.000 0.000
#&gt; GSM559458     1  0.1520      0.911 0.956 0.020 0.024 0.000
#&gt; GSM559459     1  0.0000      0.936 1.000 0.000 0.000 0.000
#&gt; GSM559460     1  0.0000      0.936 1.000 0.000 0.000 0.000
#&gt; GSM559461     1  0.0000      0.936 1.000 0.000 0.000 0.000
#&gt; GSM559462     1  0.0000      0.936 1.000 0.000 0.000 0.000
#&gt; GSM559463     1  0.0000      0.936 1.000 0.000 0.000 0.000
#&gt; GSM559464     1  0.0000      0.936 1.000 0.000 0.000 0.000
#&gt; GSM559465     1  0.0000      0.936 1.000 0.000 0.000 0.000
#&gt; GSM559467     1  0.4576      0.626 0.728 0.260 0.012 0.000
#&gt; GSM559468     1  0.3991      0.793 0.832 0.120 0.048 0.000
#&gt; GSM559469     1  0.4153      0.779 0.820 0.132 0.048 0.000
#&gt; GSM559470     1  0.4770      0.580 0.700 0.288 0.012 0.000
#&gt; GSM559471     1  0.0804      0.928 0.980 0.012 0.008 0.000
#&gt; GSM559472     1  0.0188      0.935 0.996 0.004 0.000 0.000
#&gt; GSM559473     2  0.1474      0.773 0.052 0.948 0.000 0.000
#&gt; GSM559475     2  0.1474      0.773 0.052 0.948 0.000 0.000
#&gt; GSM559477     2  0.3024      0.757 0.000 0.852 0.148 0.000
#&gt; GSM559478     2  0.4008      0.711 0.000 0.756 0.244 0.000
#&gt; GSM559479     2  0.3024      0.757 0.000 0.852 0.148 0.000
#&gt; GSM559480     2  0.4008      0.711 0.000 0.756 0.244 0.000
#&gt; GSM559481     2  0.4008      0.711 0.000 0.756 0.244 0.000
#&gt; GSM559482     2  0.3024      0.757 0.000 0.852 0.148 0.000
#&gt; GSM559435     3  0.4164      0.932 0.264 0.000 0.736 0.000
#&gt; GSM559439     3  0.4164      0.932 0.264 0.000 0.736 0.000
#&gt; GSM559443     3  0.4164      0.932 0.264 0.000 0.736 0.000
#&gt; GSM559447     3  0.4164      0.932 0.264 0.000 0.736 0.000
#&gt; GSM559449     3  0.4776      0.907 0.244 0.000 0.732 0.024
#&gt; GSM559453     3  0.6367      0.348 0.080 0.000 0.584 0.336
#&gt; GSM559466     3  0.4164      0.932 0.264 0.000 0.736 0.000
#&gt; GSM559474     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; GSM559476     3  0.4193      0.927 0.268 0.000 0.732 0.000
#&gt; GSM559483     2  0.3024      0.757 0.000 0.852 0.148 0.000
#&gt; GSM559484     4  0.0000      1.000 0.000 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-3-a').click(function(){
  $('#tab-SD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-4'>
<p><a id='tab-SD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559433     1  0.0510      0.878 0.984 0.000 0.000 0.016 0.000
#&gt; GSM559434     4  0.4805      0.848 0.008 0.432 0.004 0.552 0.004
#&gt; GSM559436     1  0.0000      0.875 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559437     1  0.3224      0.835 0.824 0.000 0.016 0.160 0.000
#&gt; GSM559438     4  0.5566      0.831 0.068 0.364 0.004 0.564 0.000
#&gt; GSM559440     4  0.5566      0.831 0.068 0.364 0.004 0.564 0.000
#&gt; GSM559441     1  0.2890      0.839 0.836 0.000 0.004 0.160 0.000
#&gt; GSM559442     1  0.4310      0.610 0.604 0.000 0.004 0.392 0.000
#&gt; GSM559444     4  0.4273      0.837 0.000 0.448 0.000 0.552 0.000
#&gt; GSM559445     1  0.3224      0.835 0.824 0.000 0.016 0.160 0.000
#&gt; GSM559446     1  0.3224      0.835 0.824 0.000 0.016 0.160 0.000
#&gt; GSM559448     1  0.0000      0.875 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559450     4  0.4273      0.837 0.000 0.448 0.000 0.552 0.000
#&gt; GSM559451     1  0.0404      0.877 0.988 0.000 0.000 0.012 0.000
#&gt; GSM559452     4  0.4299      0.784 0.000 0.316 0.004 0.672 0.008
#&gt; GSM559454     1  0.0000      0.875 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559455     1  0.2890      0.839 0.836 0.000 0.004 0.160 0.000
#&gt; GSM559456     1  0.3970      0.810 0.800 0.000 0.096 0.104 0.000
#&gt; GSM559457     1  0.1197      0.873 0.952 0.000 0.000 0.048 0.000
#&gt; GSM559458     1  0.3123      0.827 0.812 0.000 0.004 0.184 0.000
#&gt; GSM559459     1  0.0000      0.875 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559460     1  0.0162      0.876 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559461     1  0.0162      0.876 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559462     1  0.0404      0.877 0.988 0.000 0.000 0.012 0.000
#&gt; GSM559463     1  0.0000      0.875 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559464     1  0.0162      0.875 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559465     1  0.0162      0.875 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559467     1  0.4299      0.517 0.608 0.000 0.004 0.388 0.000
#&gt; GSM559468     1  0.4251      0.638 0.624 0.000 0.004 0.372 0.000
#&gt; GSM559469     1  0.4288      0.619 0.612 0.000 0.004 0.384 0.000
#&gt; GSM559470     1  0.5002      0.420 0.548 0.024 0.004 0.424 0.000
#&gt; GSM559471     1  0.0955      0.876 0.968 0.000 0.004 0.028 0.000
#&gt; GSM559472     1  0.0404      0.877 0.988 0.000 0.000 0.012 0.000
#&gt; GSM559473     2  0.4273     -0.652 0.000 0.552 0.000 0.448 0.000
#&gt; GSM559475     2  0.4273     -0.652 0.000 0.552 0.000 0.448 0.000
#&gt; GSM559477     2  0.0000      0.599 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559478     2  0.3895      0.526 0.000 0.680 0.000 0.320 0.000
#&gt; GSM559479     2  0.0000      0.599 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.3895      0.526 0.000 0.680 0.000 0.320 0.000
#&gt; GSM559481     2  0.3895      0.526 0.000 0.680 0.000 0.320 0.000
#&gt; GSM559482     2  0.0000      0.599 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.0510      0.945 0.016 0.000 0.984 0.000 0.000
#&gt; GSM559439     3  0.0510      0.945 0.016 0.000 0.984 0.000 0.000
#&gt; GSM559443     3  0.0510      0.945 0.016 0.000 0.984 0.000 0.000
#&gt; GSM559447     3  0.0510      0.945 0.016 0.000 0.984 0.000 0.000
#&gt; GSM559449     3  0.1106      0.929 0.012 0.000 0.964 0.000 0.024
#&gt; GSM559453     3  0.3966      0.502 0.000 0.000 0.664 0.000 0.336
#&gt; GSM559466     3  0.0510      0.945 0.016 0.000 0.984 0.000 0.000
#&gt; GSM559474     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559476     3  0.0609      0.940 0.020 0.000 0.980 0.000 0.000
#&gt; GSM559483     2  0.0000      0.599 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559484     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-4-a').click(function(){
  $('#tab-SD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-5'>
<p><a id='tab-SD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559433     1  0.1196      0.672 0.952 0.000 0.000 0.040 0.000 0.008
#&gt; GSM559434     6  0.2445      0.881 0.008 0.120 0.000 0.004 0.000 0.868
#&gt; GSM559436     1  0.0000      0.692 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559437     4  0.4682      0.916 0.396 0.000 0.000 0.556 0.000 0.048
#&gt; GSM559438     6  0.3178      0.839 0.012 0.052 0.000 0.092 0.000 0.844
#&gt; GSM559440     6  0.3178      0.839 0.012 0.052 0.000 0.092 0.000 0.844
#&gt; GSM559441     1  0.4422      0.161 0.680 0.000 0.000 0.252 0.000 0.068
#&gt; GSM559442     1  0.4619      0.227 0.600 0.000 0.000 0.052 0.000 0.348
#&gt; GSM559444     6  0.2234      0.882 0.000 0.124 0.000 0.004 0.000 0.872
#&gt; GSM559445     4  0.4682      0.916 0.396 0.000 0.000 0.556 0.000 0.048
#&gt; GSM559446     4  0.4682      0.916 0.396 0.000 0.000 0.556 0.000 0.048
#&gt; GSM559448     1  0.0000      0.692 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559450     6  0.2234      0.882 0.000 0.124 0.000 0.004 0.000 0.872
#&gt; GSM559451     1  0.2416      0.528 0.844 0.000 0.000 0.156 0.000 0.000
#&gt; GSM559452     6  0.1141      0.761 0.000 0.000 0.000 0.052 0.000 0.948
#&gt; GSM559454     1  0.0000      0.692 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559455     1  0.4422      0.161 0.680 0.000 0.000 0.252 0.000 0.068
#&gt; GSM559456     4  0.5034      0.749 0.404 0.000 0.076 0.520 0.000 0.000
#&gt; GSM559457     1  0.2230      0.618 0.892 0.000 0.000 0.084 0.000 0.024
#&gt; GSM559458     1  0.4663      0.151 0.660 0.000 0.000 0.252 0.000 0.088
#&gt; GSM559459     1  0.0000      0.692 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559460     1  0.0146      0.692 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; GSM559461     1  0.0146      0.692 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; GSM559462     1  0.2378      0.533 0.848 0.000 0.000 0.152 0.000 0.000
#&gt; GSM559463     1  0.0000      0.692 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559464     1  0.0146      0.692 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM559465     1  0.0146      0.692 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM559467     1  0.5855     -0.267 0.448 0.000 0.000 0.200 0.000 0.352
#&gt; GSM559468     1  0.4607      0.251 0.616 0.000 0.000 0.056 0.000 0.328
#&gt; GSM559469     1  0.4687      0.233 0.604 0.000 0.000 0.060 0.000 0.336
#&gt; GSM559470     1  0.6425     -0.307 0.388 0.024 0.000 0.208 0.000 0.380
#&gt; GSM559471     1  0.1168      0.680 0.956 0.000 0.000 0.028 0.000 0.016
#&gt; GSM559472     1  0.0713      0.682 0.972 0.000 0.000 0.028 0.000 0.000
#&gt; GSM559473     6  0.3136      0.804 0.000 0.228 0.000 0.004 0.000 0.768
#&gt; GSM559475     6  0.3136      0.804 0.000 0.228 0.000 0.004 0.000 0.768
#&gt; GSM559477     2  0.4899      0.784 0.000 0.560 0.016 0.388 0.000 0.036
#&gt; GSM559478     2  0.0000      0.695 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559479     2  0.4899      0.784 0.000 0.560 0.016 0.388 0.000 0.036
#&gt; GSM559480     2  0.0000      0.695 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559481     2  0.0000      0.695 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559482     2  0.4899      0.784 0.000 0.560 0.016 0.388 0.000 0.036
#&gt; GSM559435     3  0.0458      0.945 0.016 0.000 0.984 0.000 0.000 0.000
#&gt; GSM559439     3  0.0458      0.945 0.016 0.000 0.984 0.000 0.000 0.000
#&gt; GSM559443     3  0.0458      0.945 0.016 0.000 0.984 0.000 0.000 0.000
#&gt; GSM559447     3  0.0458      0.945 0.016 0.000 0.984 0.000 0.000 0.000
#&gt; GSM559449     3  0.0993      0.929 0.012 0.000 0.964 0.000 0.024 0.000
#&gt; GSM559453     3  0.3563      0.502 0.000 0.000 0.664 0.000 0.336 0.000
#&gt; GSM559466     3  0.0458      0.945 0.016 0.000 0.984 0.000 0.000 0.000
#&gt; GSM559474     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559476     3  0.0547      0.941 0.020 0.000 0.980 0.000 0.000 0.000
#&gt; GSM559483     2  0.4899      0.784 0.000 0.560 0.016 0.388 0.000 0.036
#&gt; GSM559484     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-5-a').click(function(){
  $('#tab-SD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-hclust-signature_compare](figure_cola/SD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-hclust-collect-classes](figure_cola/SD-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n disease.state(p) k
#> SD:hclust 52         2.10e-01 2
#> SD:hclust 51         7.39e-03 3
#> SD:hclust 51         8.90e-10 4
#> SD:hclust 49         6.19e-09 5
#> SD:hclust 44         1.95e-07 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:kmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "kmeans"]
# you can also extract it by
# res = res_list["SD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-kmeans-collect-plots](figure_cola/SD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-kmeans-select-partition-number](figure_cola/SD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.413           0.851       0.855         0.4153 0.566   0.566
#> 3 3 0.693           0.883       0.928         0.4700 0.799   0.648
#> 4 4 0.705           0.680       0.849         0.1464 0.861   0.655
#> 5 5 0.661           0.702       0.835         0.0854 0.890   0.653
#> 6 6 0.711           0.744       0.813         0.0651 0.953   0.808
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-classes'>
<ul>
<li><a href='#tab-SD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-kmeans-get-classes-1'>
<p><a id='tab-SD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559433     1   0.844      0.872 0.728 0.272
#&gt; GSM559434     2   0.118      0.957 0.016 0.984
#&gt; GSM559436     1   0.844      0.872 0.728 0.272
#&gt; GSM559437     1   0.850      0.869 0.724 0.276
#&gt; GSM559438     2   0.118      0.957 0.016 0.984
#&gt; GSM559440     2   0.118      0.957 0.016 0.984
#&gt; GSM559441     1   0.973      0.709 0.596 0.404
#&gt; GSM559442     1   0.844      0.872 0.728 0.272
#&gt; GSM559444     2   0.000      0.969 0.000 1.000
#&gt; GSM559445     1   0.975      0.702 0.592 0.408
#&gt; GSM559446     1   0.850      0.869 0.724 0.276
#&gt; GSM559448     1   0.839      0.871 0.732 0.268
#&gt; GSM559450     2   0.000      0.969 0.000 1.000
#&gt; GSM559451     1   0.844      0.872 0.728 0.272
#&gt; GSM559452     2   0.118      0.957 0.016 0.984
#&gt; GSM559454     1   0.844      0.872 0.728 0.272
#&gt; GSM559455     1   0.844      0.872 0.728 0.272
#&gt; GSM559456     1   0.605      0.812 0.852 0.148
#&gt; GSM559457     1   0.844      0.872 0.728 0.272
#&gt; GSM559458     1   0.605      0.812 0.852 0.148
#&gt; GSM559459     1   0.844      0.872 0.728 0.272
#&gt; GSM559460     1   0.844      0.872 0.728 0.272
#&gt; GSM559461     1   0.844      0.872 0.728 0.272
#&gt; GSM559462     1   0.844      0.872 0.728 0.272
#&gt; GSM559463     1   0.844      0.872 0.728 0.272
#&gt; GSM559464     1   0.844      0.872 0.728 0.272
#&gt; GSM559465     1   0.827      0.868 0.740 0.260
#&gt; GSM559467     1   0.850      0.869 0.724 0.276
#&gt; GSM559468     1   0.844      0.872 0.728 0.272
#&gt; GSM559469     1   0.844      0.872 0.728 0.272
#&gt; GSM559470     1   0.999      0.556 0.520 0.480
#&gt; GSM559471     1   0.844      0.872 0.728 0.272
#&gt; GSM559472     1   0.844      0.872 0.728 0.272
#&gt; GSM559473     2   0.000      0.969 0.000 1.000
#&gt; GSM559475     2   0.000      0.969 0.000 1.000
#&gt; GSM559477     2   0.000      0.969 0.000 1.000
#&gt; GSM559478     2   0.000      0.969 0.000 1.000
#&gt; GSM559479     2   0.000      0.969 0.000 1.000
#&gt; GSM559480     2   0.000      0.969 0.000 1.000
#&gt; GSM559481     2   0.000      0.969 0.000 1.000
#&gt; GSM559482     2   0.000      0.969 0.000 1.000
#&gt; GSM559435     1   0.000      0.722 1.000 0.000
#&gt; GSM559439     1   0.000      0.722 1.000 0.000
#&gt; GSM559443     1   0.000      0.722 1.000 0.000
#&gt; GSM559447     1   0.000      0.722 1.000 0.000
#&gt; GSM559449     1   0.000      0.722 1.000 0.000
#&gt; GSM559453     1   0.000      0.722 1.000 0.000
#&gt; GSM559466     1   0.000      0.722 1.000 0.000
#&gt; GSM559474     1   0.574      0.603 0.864 0.136
#&gt; GSM559476     1   0.000      0.722 1.000 0.000
#&gt; GSM559483     2   0.000      0.969 0.000 1.000
#&gt; GSM559484     2   0.844      0.635 0.272 0.728
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-1-a').click(function(){
  $('#tab-SD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-2'>
<p><a id='tab-SD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559433     1  0.0237      0.961 0.996 0.000 0.004
#&gt; GSM559434     2  0.5961      0.785 0.096 0.792 0.112
#&gt; GSM559436     1  0.0237      0.961 0.996 0.000 0.004
#&gt; GSM559437     1  0.3267      0.883 0.884 0.000 0.116
#&gt; GSM559438     2  0.8783      0.174 0.420 0.468 0.112
#&gt; GSM559440     2  0.5961      0.785 0.096 0.792 0.112
#&gt; GSM559441     1  0.3267      0.883 0.884 0.000 0.116
#&gt; GSM559442     1  0.0237      0.959 0.996 0.004 0.000
#&gt; GSM559444     2  0.3573      0.855 0.004 0.876 0.120
#&gt; GSM559445     1  0.3845      0.874 0.872 0.012 0.116
#&gt; GSM559446     1  0.3267      0.883 0.884 0.000 0.116
#&gt; GSM559448     1  0.0424      0.958 0.992 0.000 0.008
#&gt; GSM559450     2  0.3573      0.855 0.004 0.876 0.120
#&gt; GSM559451     1  0.0000      0.960 1.000 0.000 0.000
#&gt; GSM559452     2  0.4196      0.847 0.024 0.864 0.112
#&gt; GSM559454     1  0.0237      0.961 0.996 0.000 0.004
#&gt; GSM559455     1  0.1529      0.939 0.960 0.000 0.040
#&gt; GSM559456     1  0.0424      0.960 0.992 0.000 0.008
#&gt; GSM559457     1  0.0237      0.961 0.996 0.000 0.004
#&gt; GSM559458     1  0.0237      0.961 0.996 0.000 0.004
#&gt; GSM559459     1  0.0237      0.961 0.996 0.000 0.004
#&gt; GSM559460     1  0.0237      0.961 0.996 0.000 0.004
#&gt; GSM559461     1  0.0237      0.961 0.996 0.000 0.004
#&gt; GSM559462     1  0.0000      0.960 1.000 0.000 0.000
#&gt; GSM559463     1  0.0424      0.958 0.992 0.000 0.008
#&gt; GSM559464     1  0.0237      0.961 0.996 0.000 0.004
#&gt; GSM559465     1  0.0424      0.958 0.992 0.000 0.008
#&gt; GSM559467     1  0.3349      0.888 0.888 0.004 0.108
#&gt; GSM559468     1  0.0000      0.960 1.000 0.000 0.000
#&gt; GSM559469     1  0.1267      0.947 0.972 0.004 0.024
#&gt; GSM559470     1  0.4136      0.867 0.864 0.020 0.116
#&gt; GSM559471     1  0.0237      0.959 0.996 0.004 0.000
#&gt; GSM559472     1  0.0237      0.961 0.996 0.000 0.004
#&gt; GSM559473     2  0.0661      0.892 0.004 0.988 0.008
#&gt; GSM559475     2  0.0661      0.892 0.004 0.988 0.008
#&gt; GSM559477     2  0.0661      0.891 0.004 0.988 0.008
#&gt; GSM559478     2  0.0237      0.892 0.004 0.996 0.000
#&gt; GSM559479     2  0.0661      0.891 0.004 0.988 0.008
#&gt; GSM559480     2  0.0237      0.892 0.004 0.996 0.000
#&gt; GSM559481     2  0.0237      0.892 0.004 0.996 0.000
#&gt; GSM559482     2  0.0661      0.891 0.004 0.988 0.008
#&gt; GSM559435     3  0.3412      0.917 0.124 0.000 0.876
#&gt; GSM559439     3  0.3412      0.917 0.124 0.000 0.876
#&gt; GSM559443     3  0.3412      0.917 0.124 0.000 0.876
#&gt; GSM559447     3  0.3412      0.917 0.124 0.000 0.876
#&gt; GSM559449     3  0.3412      0.917 0.124 0.000 0.876
#&gt; GSM559453     3  0.3412      0.917 0.124 0.000 0.876
#&gt; GSM559466     3  0.3412      0.917 0.124 0.000 0.876
#&gt; GSM559474     3  0.0829      0.787 0.012 0.004 0.984
#&gt; GSM559476     3  0.4346      0.858 0.184 0.000 0.816
#&gt; GSM559483     2  0.0661      0.891 0.004 0.988 0.008
#&gt; GSM559484     3  0.6095      0.124 0.000 0.392 0.608
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-2-a').click(function(){
  $('#tab-SD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-3'>
<p><a id='tab-SD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559433     1  0.2011     0.8487 0.920 0.000 0.000 0.080
#&gt; GSM559434     4  0.5636     0.0288 0.024 0.424 0.000 0.552
#&gt; GSM559436     1  0.1302     0.8580 0.956 0.000 0.000 0.044
#&gt; GSM559437     4  0.4830     0.1317 0.392 0.000 0.000 0.608
#&gt; GSM559438     4  0.7779     0.3194 0.356 0.244 0.000 0.400
#&gt; GSM559440     4  0.5769     0.1736 0.036 0.376 0.000 0.588
#&gt; GSM559441     4  0.4967    -0.0500 0.452 0.000 0.000 0.548
#&gt; GSM559442     1  0.1474     0.8537 0.948 0.000 0.000 0.052
#&gt; GSM559444     2  0.4804     0.3856 0.000 0.616 0.000 0.384
#&gt; GSM559445     4  0.3791     0.4873 0.200 0.004 0.000 0.796
#&gt; GSM559446     4  0.3975     0.4467 0.240 0.000 0.000 0.760
#&gt; GSM559448     1  0.0707     0.8528 0.980 0.000 0.000 0.020
#&gt; GSM559450     2  0.4776     0.4031 0.000 0.624 0.000 0.376
#&gt; GSM559451     1  0.1792     0.8545 0.932 0.000 0.000 0.068
#&gt; GSM559452     4  0.6089     0.2168 0.064 0.328 0.000 0.608
#&gt; GSM559454     1  0.0817     0.8645 0.976 0.000 0.000 0.024
#&gt; GSM559455     1  0.4843     0.4351 0.604 0.000 0.000 0.396
#&gt; GSM559456     1  0.4406     0.6184 0.700 0.000 0.000 0.300
#&gt; GSM559457     1  0.2973     0.8025 0.856 0.000 0.000 0.144
#&gt; GSM559458     1  0.4277     0.6123 0.720 0.000 0.000 0.280
#&gt; GSM559459     1  0.0817     0.8645 0.976 0.000 0.000 0.024
#&gt; GSM559460     1  0.0000     0.8624 1.000 0.000 0.000 0.000
#&gt; GSM559461     1  0.0000     0.8624 1.000 0.000 0.000 0.000
#&gt; GSM559462     1  0.1792     0.8545 0.932 0.000 0.000 0.068
#&gt; GSM559463     1  0.0707     0.8528 0.980 0.000 0.000 0.020
#&gt; GSM559464     1  0.0000     0.8624 1.000 0.000 0.000 0.000
#&gt; GSM559465     1  0.0336     0.8596 0.992 0.000 0.000 0.008
#&gt; GSM559467     1  0.4661     0.5293 0.652 0.000 0.000 0.348
#&gt; GSM559468     1  0.0921     0.8628 0.972 0.000 0.000 0.028
#&gt; GSM559469     1  0.1716     0.8466 0.936 0.000 0.000 0.064
#&gt; GSM559470     1  0.5296     0.0651 0.496 0.008 0.000 0.496
#&gt; GSM559471     1  0.0921     0.8628 0.972 0.000 0.000 0.028
#&gt; GSM559472     1  0.0817     0.8645 0.976 0.000 0.000 0.024
#&gt; GSM559473     2  0.0779     0.8882 0.000 0.980 0.004 0.016
#&gt; GSM559475     2  0.0779     0.8882 0.000 0.980 0.004 0.016
#&gt; GSM559477     2  0.0592     0.8871 0.000 0.984 0.000 0.016
#&gt; GSM559478     2  0.0927     0.8885 0.000 0.976 0.016 0.008
#&gt; GSM559479     2  0.0592     0.8871 0.000 0.984 0.000 0.016
#&gt; GSM559480     2  0.0927     0.8885 0.000 0.976 0.016 0.008
#&gt; GSM559481     2  0.0927     0.8885 0.000 0.976 0.016 0.008
#&gt; GSM559482     2  0.0592     0.8871 0.000 0.984 0.000 0.016
#&gt; GSM559435     3  0.0592     0.9660 0.016 0.000 0.984 0.000
#&gt; GSM559439     3  0.0592     0.9660 0.016 0.000 0.984 0.000
#&gt; GSM559443     3  0.1297     0.9528 0.016 0.000 0.964 0.020
#&gt; GSM559447     3  0.0592     0.9660 0.016 0.000 0.984 0.000
#&gt; GSM559449     3  0.0592     0.9660 0.016 0.000 0.984 0.000
#&gt; GSM559453     3  0.0592     0.9660 0.016 0.000 0.984 0.000
#&gt; GSM559466     3  0.0592     0.9660 0.016 0.000 0.984 0.000
#&gt; GSM559474     4  0.4916    -0.1917 0.000 0.000 0.424 0.576
#&gt; GSM559476     3  0.3806     0.7601 0.156 0.000 0.824 0.020
#&gt; GSM559483     2  0.0592     0.8871 0.000 0.984 0.000 0.016
#&gt; GSM559484     4  0.7349    -0.1491 0.000 0.160 0.384 0.456
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-3-a').click(function(){
  $('#tab-SD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-4'>
<p><a id='tab-SD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559433     1  0.2249      0.835 0.896 0.000 0.000 0.096 0.008
#&gt; GSM559434     4  0.6163      0.222 0.016 0.196 0.000 0.612 0.176
#&gt; GSM559436     1  0.1444      0.879 0.948 0.000 0.000 0.012 0.040
#&gt; GSM559437     4  0.4541      0.489 0.112 0.000 0.000 0.752 0.136
#&gt; GSM559438     4  0.6212      0.405 0.120 0.108 0.000 0.668 0.104
#&gt; GSM559440     4  0.5481      0.327 0.016 0.136 0.000 0.692 0.156
#&gt; GSM559441     4  0.2648      0.569 0.152 0.000 0.000 0.848 0.000
#&gt; GSM559442     1  0.4216      0.749 0.780 0.000 0.000 0.100 0.120
#&gt; GSM559444     2  0.6477      0.284 0.000 0.464 0.000 0.340 0.196
#&gt; GSM559445     4  0.2966      0.410 0.016 0.000 0.000 0.848 0.136
#&gt; GSM559446     4  0.4010      0.459 0.072 0.000 0.000 0.792 0.136
#&gt; GSM559448     1  0.1205      0.880 0.956 0.000 0.000 0.004 0.040
#&gt; GSM559450     2  0.6439      0.302 0.000 0.476 0.000 0.332 0.192
#&gt; GSM559451     1  0.2439      0.840 0.876 0.000 0.000 0.120 0.004
#&gt; GSM559452     4  0.6530      0.109 0.032 0.104 0.000 0.524 0.340
#&gt; GSM559454     1  0.0912      0.886 0.972 0.000 0.000 0.012 0.016
#&gt; GSM559455     4  0.4118      0.482 0.336 0.000 0.000 0.660 0.004
#&gt; GSM559456     4  0.4655      0.164 0.476 0.000 0.000 0.512 0.012
#&gt; GSM559457     1  0.3715      0.564 0.736 0.000 0.000 0.260 0.004
#&gt; GSM559458     4  0.5854      0.196 0.436 0.000 0.000 0.468 0.096
#&gt; GSM559459     1  0.0807      0.887 0.976 0.000 0.000 0.012 0.012
#&gt; GSM559460     1  0.0955      0.885 0.968 0.000 0.000 0.004 0.028
#&gt; GSM559461     1  0.0609      0.887 0.980 0.000 0.000 0.000 0.020
#&gt; GSM559462     1  0.1671      0.858 0.924 0.000 0.000 0.076 0.000
#&gt; GSM559463     1  0.1121      0.880 0.956 0.000 0.000 0.000 0.044
#&gt; GSM559464     1  0.1124      0.886 0.960 0.000 0.000 0.004 0.036
#&gt; GSM559465     1  0.0963      0.886 0.964 0.000 0.000 0.000 0.036
#&gt; GSM559467     4  0.4525      0.425 0.360 0.000 0.000 0.624 0.016
#&gt; GSM559468     1  0.3849      0.782 0.808 0.000 0.000 0.080 0.112
#&gt; GSM559469     1  0.4164      0.754 0.784 0.000 0.000 0.096 0.120
#&gt; GSM559470     4  0.3586      0.572 0.188 0.000 0.000 0.792 0.020
#&gt; GSM559471     1  0.3090      0.825 0.860 0.000 0.000 0.088 0.052
#&gt; GSM559472     1  0.0807      0.887 0.976 0.000 0.000 0.012 0.012
#&gt; GSM559473     2  0.3281      0.764 0.000 0.848 0.000 0.092 0.060
#&gt; GSM559475     2  0.2914      0.778 0.000 0.872 0.000 0.076 0.052
#&gt; GSM559477     2  0.0510      0.809 0.000 0.984 0.000 0.000 0.016
#&gt; GSM559478     2  0.2700      0.793 0.000 0.884 0.004 0.024 0.088
#&gt; GSM559479     2  0.0510      0.809 0.000 0.984 0.000 0.000 0.016
#&gt; GSM559480     2  0.2700      0.793 0.000 0.884 0.004 0.024 0.088
#&gt; GSM559481     2  0.2700      0.793 0.000 0.884 0.004 0.024 0.088
#&gt; GSM559482     2  0.0510      0.809 0.000 0.984 0.000 0.000 0.016
#&gt; GSM559435     3  0.0162      0.945 0.004 0.000 0.996 0.000 0.000
#&gt; GSM559439     3  0.0162      0.945 0.004 0.000 0.996 0.000 0.000
#&gt; GSM559443     3  0.0771      0.926 0.004 0.000 0.976 0.000 0.020
#&gt; GSM559447     3  0.0162      0.945 0.004 0.000 0.996 0.000 0.000
#&gt; GSM559449     3  0.0162      0.945 0.004 0.000 0.996 0.000 0.000
#&gt; GSM559453     3  0.0162      0.945 0.004 0.000 0.996 0.000 0.000
#&gt; GSM559466     3  0.0162      0.945 0.004 0.000 0.996 0.000 0.000
#&gt; GSM559474     5  0.5354      0.837 0.000 0.000 0.208 0.128 0.664
#&gt; GSM559476     3  0.3655      0.614 0.160 0.000 0.804 0.000 0.036
#&gt; GSM559483     2  0.0510      0.809 0.000 0.984 0.000 0.000 0.016
#&gt; GSM559484     5  0.5157      0.838 0.000 0.052 0.204 0.032 0.712
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-4-a').click(function(){
  $('#tab-SD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-5'>
<p><a id='tab-SD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559433     1  0.2212      0.747 0.880 0.000 0.000 0.112 0.008 0.000
#&gt; GSM559434     6  0.4439      0.738 0.004 0.088 0.000 0.152 0.012 0.744
#&gt; GSM559436     1  0.2058      0.766 0.916 0.000 0.000 0.024 0.048 0.012
#&gt; GSM559437     4  0.2380      0.753 0.036 0.000 0.000 0.900 0.048 0.016
#&gt; GSM559438     6  0.4917      0.695 0.036 0.052 0.000 0.200 0.008 0.704
#&gt; GSM559440     6  0.4170      0.723 0.008 0.060 0.000 0.192 0.000 0.740
#&gt; GSM559441     4  0.2679      0.758 0.040 0.000 0.000 0.864 0.000 0.096
#&gt; GSM559442     1  0.6239      0.547 0.496 0.000 0.000 0.052 0.116 0.336
#&gt; GSM559444     6  0.5840      0.592 0.000 0.324 0.000 0.068 0.060 0.548
#&gt; GSM559445     4  0.2519      0.724 0.008 0.000 0.000 0.888 0.048 0.056
#&gt; GSM559446     4  0.2450      0.740 0.016 0.000 0.000 0.896 0.048 0.040
#&gt; GSM559448     1  0.2390      0.764 0.900 0.000 0.004 0.024 0.060 0.012
#&gt; GSM559450     6  0.5687      0.564 0.000 0.340 0.000 0.052 0.060 0.548
#&gt; GSM559451     1  0.3332      0.745 0.832 0.000 0.000 0.108 0.016 0.044
#&gt; GSM559452     6  0.4033      0.613 0.004 0.036 0.000 0.068 0.092 0.800
#&gt; GSM559454     1  0.0622      0.783 0.980 0.000 0.000 0.008 0.012 0.000
#&gt; GSM559455     4  0.3124      0.759 0.140 0.000 0.000 0.828 0.008 0.024
#&gt; GSM559456     4  0.3706      0.691 0.184 0.000 0.000 0.776 0.016 0.024
#&gt; GSM559457     1  0.4086     -0.135 0.528 0.000 0.000 0.464 0.008 0.000
#&gt; GSM559458     4  0.6153      0.535 0.124 0.000 0.000 0.592 0.088 0.196
#&gt; GSM559459     1  0.0603      0.786 0.980 0.000 0.000 0.004 0.016 0.000
#&gt; GSM559460     1  0.3677      0.772 0.816 0.000 0.000 0.024 0.088 0.072
#&gt; GSM559461     1  0.3246      0.782 0.848 0.000 0.000 0.028 0.076 0.048
#&gt; GSM559462     1  0.2786      0.759 0.864 0.000 0.000 0.100 0.024 0.012
#&gt; GSM559463     1  0.2340      0.769 0.900 0.000 0.000 0.016 0.060 0.024
#&gt; GSM559464     1  0.3314      0.773 0.828 0.000 0.000 0.004 0.092 0.076
#&gt; GSM559465     1  0.3249      0.776 0.840 0.000 0.000 0.012 0.088 0.060
#&gt; GSM559467     4  0.4670      0.695 0.152 0.000 0.000 0.708 0.008 0.132
#&gt; GSM559468     1  0.6159      0.571 0.520 0.000 0.000 0.048 0.120 0.312
#&gt; GSM559469     1  0.6230      0.550 0.500 0.000 0.000 0.052 0.116 0.332
#&gt; GSM559470     4  0.4437      0.679 0.088 0.000 0.000 0.724 0.008 0.180
#&gt; GSM559471     1  0.5242      0.670 0.668 0.000 0.000 0.052 0.072 0.208
#&gt; GSM559472     1  0.1088      0.784 0.960 0.000 0.000 0.024 0.016 0.000
#&gt; GSM559473     2  0.4330      0.607 0.000 0.680 0.000 0.008 0.036 0.276
#&gt; GSM559475     2  0.3860      0.731 0.000 0.756 0.000 0.008 0.036 0.200
#&gt; GSM559477     2  0.0000      0.847 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559478     2  0.3862      0.825 0.000 0.796 0.000 0.016 0.088 0.100
#&gt; GSM559479     2  0.0000      0.847 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.3862      0.825 0.000 0.796 0.000 0.016 0.088 0.100
#&gt; GSM559481     2  0.3862      0.825 0.000 0.796 0.000 0.016 0.088 0.100
#&gt; GSM559482     2  0.0000      0.847 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.0000      0.947 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559439     3  0.0000      0.947 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559443     3  0.0363      0.940 0.000 0.000 0.988 0.000 0.012 0.000
#&gt; GSM559447     3  0.0000      0.947 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559449     3  0.0508      0.942 0.000 0.000 0.984 0.004 0.000 0.012
#&gt; GSM559453     3  0.0508      0.942 0.000 0.000 0.984 0.004 0.000 0.012
#&gt; GSM559466     3  0.0000      0.947 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559474     5  0.4917      0.896 0.000 0.000 0.128 0.116 0.716 0.040
#&gt; GSM559476     3  0.4679      0.641 0.092 0.000 0.772 0.036 0.048 0.052
#&gt; GSM559483     2  0.0000      0.847 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559484     5  0.5151      0.891 0.000 0.024 0.120 0.048 0.728 0.080
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-5-a').click(function(){
  $('#tab-SD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-kmeans-signature_compare](figure_cola/SD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-kmeans-collect-classes](figure_cola/SD-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n disease.state(p) k
#> SD:kmeans 52         5.15e-01 2
#> SD:kmeans 50         2.53e-10 3
#> SD:kmeans 38         6.55e-08 4
#> SD:kmeans 39         5.59e-07 5
#> SD:kmeans 51         1.02e-08 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "skmeans"]
# you can also extract it by
# res = res_list["SD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-skmeans-collect-plots](figure_cola/SD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-skmeans-select-partition-number](figure_cola/SD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.970       0.987         0.5014 0.497   0.497
#> 3 3 0.978           0.914       0.966         0.3055 0.697   0.474
#> 4 4 0.942           0.892       0.958         0.1456 0.885   0.680
#> 5 5 0.810           0.751       0.876         0.0588 0.931   0.744
#> 6 6 0.774           0.683       0.814         0.0376 0.954   0.797
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-classes'>
<ul>
<li><a href='#tab-SD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-skmeans-get-classes-1'>
<p><a id='tab-SD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559433     1   0.000      0.994 1.000 0.000
#&gt; GSM559434     2   0.000      0.975 0.000 1.000
#&gt; GSM559436     1   0.000      0.994 1.000 0.000
#&gt; GSM559437     1   0.634      0.802 0.840 0.160
#&gt; GSM559438     2   0.000      0.975 0.000 1.000
#&gt; GSM559440     2   0.000      0.975 0.000 1.000
#&gt; GSM559441     2   0.605      0.824 0.148 0.852
#&gt; GSM559442     1   0.000      0.994 1.000 0.000
#&gt; GSM559444     2   0.000      0.975 0.000 1.000
#&gt; GSM559445     2   0.000      0.975 0.000 1.000
#&gt; GSM559446     2   0.891      0.557 0.308 0.692
#&gt; GSM559448     1   0.000      0.994 1.000 0.000
#&gt; GSM559450     2   0.000      0.975 0.000 1.000
#&gt; GSM559451     1   0.000      0.994 1.000 0.000
#&gt; GSM559452     2   0.000      0.975 0.000 1.000
#&gt; GSM559454     1   0.000      0.994 1.000 0.000
#&gt; GSM559455     1   0.000      0.994 1.000 0.000
#&gt; GSM559456     1   0.000      0.994 1.000 0.000
#&gt; GSM559457     1   0.000      0.994 1.000 0.000
#&gt; GSM559458     1   0.000      0.994 1.000 0.000
#&gt; GSM559459     1   0.000      0.994 1.000 0.000
#&gt; GSM559460     1   0.000      0.994 1.000 0.000
#&gt; GSM559461     1   0.000      0.994 1.000 0.000
#&gt; GSM559462     1   0.000      0.994 1.000 0.000
#&gt; GSM559463     1   0.000      0.994 1.000 0.000
#&gt; GSM559464     1   0.000      0.994 1.000 0.000
#&gt; GSM559465     1   0.000      0.994 1.000 0.000
#&gt; GSM559467     2   0.000      0.975 0.000 1.000
#&gt; GSM559468     1   0.000      0.994 1.000 0.000
#&gt; GSM559469     2   0.388      0.908 0.076 0.924
#&gt; GSM559470     2   0.000      0.975 0.000 1.000
#&gt; GSM559471     1   0.000      0.994 1.000 0.000
#&gt; GSM559472     1   0.000      0.994 1.000 0.000
#&gt; GSM559473     2   0.000      0.975 0.000 1.000
#&gt; GSM559475     2   0.000      0.975 0.000 1.000
#&gt; GSM559477     2   0.000      0.975 0.000 1.000
#&gt; GSM559478     2   0.000      0.975 0.000 1.000
#&gt; GSM559479     2   0.000      0.975 0.000 1.000
#&gt; GSM559480     2   0.000      0.975 0.000 1.000
#&gt; GSM559481     2   0.000      0.975 0.000 1.000
#&gt; GSM559482     2   0.000      0.975 0.000 1.000
#&gt; GSM559435     1   0.000      0.994 1.000 0.000
#&gt; GSM559439     1   0.000      0.994 1.000 0.000
#&gt; GSM559443     1   0.000      0.994 1.000 0.000
#&gt; GSM559447     1   0.000      0.994 1.000 0.000
#&gt; GSM559449     1   0.000      0.994 1.000 0.000
#&gt; GSM559453     1   0.000      0.994 1.000 0.000
#&gt; GSM559466     1   0.000      0.994 1.000 0.000
#&gt; GSM559474     2   0.000      0.975 0.000 1.000
#&gt; GSM559476     1   0.000      0.994 1.000 0.000
#&gt; GSM559483     2   0.000      0.975 0.000 1.000
#&gt; GSM559484     2   0.000      0.975 0.000 1.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-1-a').click(function(){
  $('#tab-SD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-2'>
<p><a id='tab-SD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559433     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559434     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559436     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559437     1  0.1643      0.922 0.956 0.000 0.044
#&gt; GSM559438     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559440     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559441     1  0.6373      0.338 0.588 0.408 0.004
#&gt; GSM559442     1  0.0592      0.944 0.988 0.012 0.000
#&gt; GSM559444     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559445     2  0.3148      0.883 0.048 0.916 0.036
#&gt; GSM559446     1  0.1643      0.922 0.956 0.000 0.044
#&gt; GSM559448     3  0.2711      0.899 0.088 0.000 0.912
#&gt; GSM559450     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559451     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559452     2  0.3879      0.803 0.000 0.848 0.152
#&gt; GSM559454     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559455     1  0.0237      0.950 0.996 0.000 0.004
#&gt; GSM559456     1  0.1643      0.922 0.956 0.000 0.044
#&gt; GSM559457     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559458     3  0.0000      0.990 0.000 0.000 1.000
#&gt; GSM559459     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559460     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559461     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559462     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559463     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559464     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559465     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559467     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559468     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559469     1  0.1031      0.936 0.976 0.024 0.000
#&gt; GSM559470     1  0.6225      0.277 0.568 0.432 0.000
#&gt; GSM559471     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559472     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559473     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559475     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559477     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559478     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559479     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559480     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559481     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559482     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559435     3  0.0000      0.990 0.000 0.000 1.000
#&gt; GSM559439     3  0.0000      0.990 0.000 0.000 1.000
#&gt; GSM559443     3  0.0000      0.990 0.000 0.000 1.000
#&gt; GSM559447     3  0.0000      0.990 0.000 0.000 1.000
#&gt; GSM559449     3  0.0000      0.990 0.000 0.000 1.000
#&gt; GSM559453     3  0.0000      0.990 0.000 0.000 1.000
#&gt; GSM559466     3  0.0000      0.990 0.000 0.000 1.000
#&gt; GSM559474     3  0.0000      0.990 0.000 0.000 1.000
#&gt; GSM559476     3  0.0000      0.990 0.000 0.000 1.000
#&gt; GSM559483     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559484     2  0.6235      0.232 0.000 0.564 0.436
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-2-a').click(function(){
  $('#tab-SD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-3'>
<p><a id='tab-SD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559433     1  0.3123      0.822 0.844 0.000 0.000 0.156
#&gt; GSM559434     2  0.0188      0.961 0.000 0.996 0.000 0.004
#&gt; GSM559436     1  0.0000      0.973 1.000 0.000 0.000 0.000
#&gt; GSM559437     4  0.0188      0.931 0.004 0.000 0.000 0.996
#&gt; GSM559438     2  0.0188      0.961 0.000 0.996 0.000 0.004
#&gt; GSM559440     2  0.0188      0.961 0.000 0.996 0.000 0.004
#&gt; GSM559441     4  0.0188      0.931 0.004 0.000 0.000 0.996
#&gt; GSM559442     1  0.0188      0.970 0.996 0.000 0.000 0.004
#&gt; GSM559444     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; GSM559445     4  0.0000      0.927 0.000 0.000 0.000 1.000
#&gt; GSM559446     4  0.0188      0.931 0.004 0.000 0.000 0.996
#&gt; GSM559448     3  0.4877      0.298 0.408 0.000 0.592 0.000
#&gt; GSM559450     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; GSM559451     1  0.2469      0.882 0.892 0.000 0.000 0.108
#&gt; GSM559452     2  0.2053      0.896 0.000 0.924 0.072 0.004
#&gt; GSM559454     1  0.0000      0.973 1.000 0.000 0.000 0.000
#&gt; GSM559455     4  0.0188      0.931 0.004 0.000 0.000 0.996
#&gt; GSM559456     4  0.0188      0.931 0.004 0.000 0.000 0.996
#&gt; GSM559457     4  0.4898      0.237 0.416 0.000 0.000 0.584
#&gt; GSM559458     3  0.1118      0.885 0.000 0.000 0.964 0.036
#&gt; GSM559459     1  0.0000      0.973 1.000 0.000 0.000 0.000
#&gt; GSM559460     1  0.0000      0.973 1.000 0.000 0.000 0.000
#&gt; GSM559461     1  0.0000      0.973 1.000 0.000 0.000 0.000
#&gt; GSM559462     1  0.2281      0.893 0.904 0.000 0.000 0.096
#&gt; GSM559463     1  0.0000      0.973 1.000 0.000 0.000 0.000
#&gt; GSM559464     1  0.0000      0.973 1.000 0.000 0.000 0.000
#&gt; GSM559465     1  0.0000      0.973 1.000 0.000 0.000 0.000
#&gt; GSM559467     4  0.0336      0.928 0.008 0.000 0.000 0.992
#&gt; GSM559468     1  0.0000      0.973 1.000 0.000 0.000 0.000
#&gt; GSM559469     1  0.0188      0.970 0.996 0.000 0.000 0.004
#&gt; GSM559470     4  0.1004      0.910 0.004 0.024 0.000 0.972
#&gt; GSM559471     1  0.0000      0.973 1.000 0.000 0.000 0.000
#&gt; GSM559472     1  0.0000      0.973 1.000 0.000 0.000 0.000
#&gt; GSM559473     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; GSM559475     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; GSM559477     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; GSM559478     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; GSM559479     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; GSM559480     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; GSM559481     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; GSM559482     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; GSM559435     3  0.0000      0.909 0.000 0.000 1.000 0.000
#&gt; GSM559439     3  0.0000      0.909 0.000 0.000 1.000 0.000
#&gt; GSM559443     3  0.0000      0.909 0.000 0.000 1.000 0.000
#&gt; GSM559447     3  0.0000      0.909 0.000 0.000 1.000 0.000
#&gt; GSM559449     3  0.0000      0.909 0.000 0.000 1.000 0.000
#&gt; GSM559453     3  0.0000      0.909 0.000 0.000 1.000 0.000
#&gt; GSM559466     3  0.0000      0.909 0.000 0.000 1.000 0.000
#&gt; GSM559474     3  0.4713      0.431 0.000 0.000 0.640 0.360
#&gt; GSM559476     3  0.0000      0.909 0.000 0.000 1.000 0.000
#&gt; GSM559483     2  0.0000      0.963 0.000 1.000 0.000 0.000
#&gt; GSM559484     2  0.4955      0.216 0.000 0.556 0.444 0.000
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-3-a').click(function(){
  $('#tab-SD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-4'>
<p><a id='tab-SD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559433     1  0.2136     0.6670 0.904 0.000 0.000 0.088 0.008
#&gt; GSM559434     2  0.1908     0.8663 0.000 0.908 0.000 0.000 0.092
#&gt; GSM559436     1  0.0000     0.7174 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559437     4  0.0000     0.9061 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559438     2  0.3366     0.7277 0.000 0.768 0.000 0.000 0.232
#&gt; GSM559440     2  0.2280     0.8449 0.000 0.880 0.000 0.000 0.120
#&gt; GSM559441     4  0.0290     0.9056 0.000 0.000 0.000 0.992 0.008
#&gt; GSM559442     5  0.2929     0.8131 0.180 0.000 0.000 0.000 0.820
#&gt; GSM559444     2  0.0963     0.8924 0.000 0.964 0.000 0.000 0.036
#&gt; GSM559445     4  0.0162     0.9059 0.000 0.000 0.000 0.996 0.004
#&gt; GSM559446     4  0.0290     0.9048 0.000 0.000 0.000 0.992 0.008
#&gt; GSM559448     1  0.4425     0.1092 0.544 0.000 0.452 0.000 0.004
#&gt; GSM559450     2  0.0880     0.8937 0.000 0.968 0.000 0.000 0.032
#&gt; GSM559451     1  0.3888     0.6265 0.804 0.000 0.000 0.076 0.120
#&gt; GSM559452     2  0.4872     0.4244 0.000 0.540 0.024 0.000 0.436
#&gt; GSM559454     1  0.0290     0.7188 0.992 0.000 0.000 0.000 0.008
#&gt; GSM559455     4  0.1168     0.8956 0.032 0.000 0.000 0.960 0.008
#&gt; GSM559456     4  0.2362     0.8555 0.084 0.000 0.008 0.900 0.008
#&gt; GSM559457     1  0.4065     0.4530 0.720 0.000 0.000 0.264 0.016
#&gt; GSM559458     3  0.5921     0.5509 0.000 0.000 0.596 0.184 0.220
#&gt; GSM559459     1  0.1270     0.7178 0.948 0.000 0.000 0.000 0.052
#&gt; GSM559460     1  0.3774     0.4695 0.704 0.000 0.000 0.000 0.296
#&gt; GSM559461     1  0.3109     0.6186 0.800 0.000 0.000 0.000 0.200
#&gt; GSM559462     1  0.4179     0.6398 0.776 0.000 0.000 0.072 0.152
#&gt; GSM559463     1  0.1608     0.7088 0.928 0.000 0.000 0.000 0.072
#&gt; GSM559464     1  0.3752     0.4780 0.708 0.000 0.000 0.000 0.292
#&gt; GSM559465     1  0.3210     0.5988 0.788 0.000 0.000 0.000 0.212
#&gt; GSM559467     4  0.4150     0.7238 0.036 0.000 0.000 0.748 0.216
#&gt; GSM559468     5  0.3612     0.7982 0.268 0.000 0.000 0.000 0.732
#&gt; GSM559469     5  0.2891     0.8200 0.176 0.000 0.000 0.000 0.824
#&gt; GSM559470     4  0.4410     0.7410 0.008 0.044 0.000 0.752 0.196
#&gt; GSM559471     5  0.4150     0.5840 0.388 0.000 0.000 0.000 0.612
#&gt; GSM559472     1  0.0290     0.7170 0.992 0.000 0.000 0.000 0.008
#&gt; GSM559473     2  0.0000     0.9029 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559475     2  0.0000     0.9029 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559477     2  0.0000     0.9029 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559478     2  0.0000     0.9029 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559479     2  0.0000     0.9029 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.0000     0.9029 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559481     2  0.0000     0.9029 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559482     2  0.0000     0.9029 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.0000     0.9097 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559439     3  0.0000     0.9097 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559443     3  0.0000     0.9097 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559447     3  0.0000     0.9097 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559449     3  0.0000     0.9097 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559453     3  0.0000     0.9097 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559466     3  0.0000     0.9097 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559474     3  0.5989     0.3698 0.000 0.000 0.536 0.336 0.128
#&gt; GSM559476     3  0.0000     0.9097 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559483     2  0.0000     0.9029 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559484     2  0.6161    -0.0241 0.000 0.444 0.424 0.000 0.132
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-4-a').click(function(){
  $('#tab-SD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-5'>
<p><a id='tab-SD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559433     1  0.4053     0.6080 0.788 0.000 0.000 0.100 0.084 0.028
#&gt; GSM559434     2  0.3756     0.6957 0.000 0.712 0.000 0.000 0.268 0.020
#&gt; GSM559436     1  0.0937     0.6859 0.960 0.000 0.000 0.000 0.040 0.000
#&gt; GSM559437     4  0.0458     0.7984 0.000 0.000 0.000 0.984 0.016 0.000
#&gt; GSM559438     2  0.5030     0.5607 0.000 0.632 0.000 0.004 0.256 0.108
#&gt; GSM559440     2  0.4347     0.6318 0.000 0.668 0.000 0.004 0.288 0.040
#&gt; GSM559441     4  0.1049     0.7972 0.000 0.000 0.000 0.960 0.032 0.008
#&gt; GSM559442     6  0.3492     0.7439 0.076 0.000 0.000 0.000 0.120 0.804
#&gt; GSM559444     2  0.2902     0.7662 0.000 0.800 0.000 0.000 0.196 0.004
#&gt; GSM559445     4  0.1584     0.7853 0.000 0.000 0.000 0.928 0.064 0.008
#&gt; GSM559446     4  0.1524     0.7873 0.000 0.000 0.000 0.932 0.060 0.008
#&gt; GSM559448     3  0.4969     0.1510 0.436 0.000 0.508 0.000 0.048 0.008
#&gt; GSM559450     2  0.2933     0.7626 0.000 0.796 0.000 0.000 0.200 0.004
#&gt; GSM559451     1  0.5761     0.4828 0.628 0.000 0.000 0.052 0.156 0.164
#&gt; GSM559452     5  0.5537     0.3873 0.000 0.236 0.004 0.000 0.576 0.184
#&gt; GSM559454     1  0.0603     0.6912 0.980 0.000 0.000 0.000 0.016 0.004
#&gt; GSM559455     4  0.2515     0.7752 0.052 0.000 0.000 0.892 0.040 0.016
#&gt; GSM559456     4  0.4181     0.7040 0.104 0.000 0.028 0.796 0.052 0.020
#&gt; GSM559457     1  0.5241     0.4846 0.652 0.000 0.000 0.224 0.096 0.028
#&gt; GSM559458     3  0.7068     0.0298 0.004 0.000 0.492 0.140 0.184 0.180
#&gt; GSM559459     1  0.1471     0.6932 0.932 0.000 0.000 0.000 0.004 0.064
#&gt; GSM559460     1  0.4362     0.3662 0.584 0.000 0.000 0.000 0.028 0.388
#&gt; GSM559461     1  0.3950     0.5447 0.696 0.000 0.000 0.000 0.028 0.276
#&gt; GSM559462     1  0.5693     0.5188 0.628 0.000 0.000 0.044 0.144 0.184
#&gt; GSM559463     1  0.2560     0.6714 0.872 0.000 0.000 0.000 0.036 0.092
#&gt; GSM559464     1  0.3979     0.4365 0.628 0.000 0.000 0.000 0.012 0.360
#&gt; GSM559465     1  0.4435     0.4967 0.648 0.000 0.004 0.000 0.040 0.308
#&gt; GSM559467     4  0.6123     0.5648 0.048 0.004 0.000 0.584 0.152 0.212
#&gt; GSM559468     6  0.2454     0.7751 0.160 0.000 0.000 0.000 0.000 0.840
#&gt; GSM559469     6  0.2006     0.7925 0.080 0.000 0.000 0.000 0.016 0.904
#&gt; GSM559470     4  0.6262     0.5467 0.008 0.048 0.000 0.576 0.160 0.208
#&gt; GSM559471     6  0.4783     0.5566 0.276 0.000 0.000 0.000 0.088 0.636
#&gt; GSM559472     1  0.1528     0.6882 0.936 0.000 0.000 0.000 0.048 0.016
#&gt; GSM559473     2  0.0260     0.8703 0.000 0.992 0.000 0.000 0.008 0.000
#&gt; GSM559475     2  0.0260     0.8693 0.000 0.992 0.000 0.000 0.008 0.000
#&gt; GSM559477     2  0.0000     0.8712 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559478     2  0.0777     0.8634 0.000 0.972 0.000 0.000 0.024 0.004
#&gt; GSM559479     2  0.0000     0.8712 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.0891     0.8620 0.000 0.968 0.000 0.000 0.024 0.008
#&gt; GSM559481     2  0.0891     0.8620 0.000 0.968 0.000 0.000 0.024 0.008
#&gt; GSM559482     2  0.0000     0.8712 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.0000     0.8525 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559439     3  0.0000     0.8525 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559443     3  0.0000     0.8525 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559447     3  0.0000     0.8525 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559449     3  0.0000     0.8525 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559453     3  0.0363     0.8426 0.000 0.000 0.988 0.000 0.012 0.000
#&gt; GSM559466     3  0.0000     0.8525 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559474     5  0.6208     0.3394 0.000 0.000 0.296 0.236 0.456 0.012
#&gt; GSM559476     3  0.0000     0.8525 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559483     2  0.0000     0.8712 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559484     5  0.6266     0.5499 0.000 0.276 0.232 0.004 0.476 0.012
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-5-a').click(function(){
  $('#tab-SD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-skmeans-signature_compare](figure_cola/SD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-skmeans-collect-classes](figure_cola/SD-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) k
#> SD:skmeans 52         3.51e-01 2
#> SD:skmeans 49         6.32e-08 3
#> SD:skmeans 48         7.35e-08 4
#> SD:skmeans 45         8.83e-07 5
#> SD:skmeans 43         4.14e-07 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "pam"]
# you can also extract it by
# res = res_list["SD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-pam-collect-plots](figure_cola/SD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-pam-select-partition-number](figure_cola/SD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.959           0.947       0.978         0.3709 0.638   0.638
#> 3 3 1.000           0.961       0.984         0.6353 0.701   0.551
#> 4 4 0.766           0.810       0.916         0.2357 0.784   0.497
#> 5 5 0.734           0.535       0.776         0.0504 0.873   0.578
#> 6 6 0.855           0.774       0.912         0.0477 0.891   0.573
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-classes'>
<ul>
<li><a href='#tab-SD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-pam-get-classes-1'>
<p><a id='tab-SD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559433     2  0.0000      0.979 0.000 1.000
#&gt; GSM559434     2  0.0000      0.979 0.000 1.000
#&gt; GSM559436     2  0.0000      0.979 0.000 1.000
#&gt; GSM559437     2  0.0000      0.979 0.000 1.000
#&gt; GSM559438     2  0.0000      0.979 0.000 1.000
#&gt; GSM559440     2  0.0000      0.979 0.000 1.000
#&gt; GSM559441     2  0.0000      0.979 0.000 1.000
#&gt; GSM559442     2  0.0000      0.979 0.000 1.000
#&gt; GSM559444     2  0.0000      0.979 0.000 1.000
#&gt; GSM559445     2  0.0000      0.979 0.000 1.000
#&gt; GSM559446     2  0.0000      0.979 0.000 1.000
#&gt; GSM559448     1  0.0376      0.963 0.996 0.004
#&gt; GSM559450     2  0.0000      0.979 0.000 1.000
#&gt; GSM559451     2  0.0000      0.979 0.000 1.000
#&gt; GSM559452     2  0.0376      0.975 0.004 0.996
#&gt; GSM559454     2  0.0000      0.979 0.000 1.000
#&gt; GSM559455     2  0.0000      0.979 0.000 1.000
#&gt; GSM559456     2  0.7950      0.670 0.240 0.760
#&gt; GSM559457     2  0.0000      0.979 0.000 1.000
#&gt; GSM559458     1  0.9358      0.445 0.648 0.352
#&gt; GSM559459     2  0.0000      0.979 0.000 1.000
#&gt; GSM559460     2  0.0000      0.979 0.000 1.000
#&gt; GSM559461     2  0.0000      0.979 0.000 1.000
#&gt; GSM559462     2  0.0000      0.979 0.000 1.000
#&gt; GSM559463     2  0.8267      0.647 0.260 0.740
#&gt; GSM559464     2  0.0000      0.979 0.000 1.000
#&gt; GSM559465     2  0.8386      0.632 0.268 0.732
#&gt; GSM559467     2  0.0000      0.979 0.000 1.000
#&gt; GSM559468     2  0.0000      0.979 0.000 1.000
#&gt; GSM559469     2  0.0000      0.979 0.000 1.000
#&gt; GSM559470     2  0.0000      0.979 0.000 1.000
#&gt; GSM559471     2  0.0000      0.979 0.000 1.000
#&gt; GSM559472     2  0.0000      0.979 0.000 1.000
#&gt; GSM559473     2  0.0000      0.979 0.000 1.000
#&gt; GSM559475     2  0.0000      0.979 0.000 1.000
#&gt; GSM559477     2  0.0000      0.979 0.000 1.000
#&gt; GSM559478     2  0.0000      0.979 0.000 1.000
#&gt; GSM559479     2  0.0000      0.979 0.000 1.000
#&gt; GSM559480     2  0.0000      0.979 0.000 1.000
#&gt; GSM559481     2  0.0000      0.979 0.000 1.000
#&gt; GSM559482     2  0.0000      0.979 0.000 1.000
#&gt; GSM559435     1  0.0000      0.967 1.000 0.000
#&gt; GSM559439     1  0.0000      0.967 1.000 0.000
#&gt; GSM559443     1  0.0000      0.967 1.000 0.000
#&gt; GSM559447     1  0.0000      0.967 1.000 0.000
#&gt; GSM559449     1  0.0000      0.967 1.000 0.000
#&gt; GSM559453     1  0.0000      0.967 1.000 0.000
#&gt; GSM559466     1  0.0000      0.967 1.000 0.000
#&gt; GSM559474     1  0.0000      0.967 1.000 0.000
#&gt; GSM559476     1  0.0000      0.967 1.000 0.000
#&gt; GSM559483     2  0.0000      0.979 0.000 1.000
#&gt; GSM559484     1  0.0000      0.967 1.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-1-a').click(function(){
  $('#tab-SD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-2'>
<p><a id='tab-SD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559433     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559434     1  0.5098      0.678 0.752 0.248 0.000
#&gt; GSM559436     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559437     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559438     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559440     1  0.5291      0.645 0.732 0.268 0.000
#&gt; GSM559441     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559442     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559444     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559445     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559446     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559448     1  0.0424      0.973 0.992 0.000 0.008
#&gt; GSM559450     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559451     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559452     3  0.3129      0.878 0.008 0.088 0.904
#&gt; GSM559454     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559455     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559456     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559457     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559458     3  0.4887      0.681 0.228 0.000 0.772
#&gt; GSM559459     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559460     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559461     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559462     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559463     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559464     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559465     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559467     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559468     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559469     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559470     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559471     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559472     1  0.0000      0.981 1.000 0.000 0.000
#&gt; GSM559473     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559475     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559477     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559478     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559479     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559480     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559481     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559482     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559435     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM559439     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM559443     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM559447     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM559449     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM559453     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM559466     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM559474     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM559476     3  0.0000      0.964 0.000 0.000 1.000
#&gt; GSM559483     2  0.0000      1.000 0.000 1.000 0.000
#&gt; GSM559484     3  0.0000      0.964 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-2-a').click(function(){
  $('#tab-SD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-3'>
<p><a id='tab-SD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559433     4  0.0000      0.852 0.000 0.000 0.000 1.000
#&gt; GSM559434     4  0.3074      0.752 0.000 0.152 0.000 0.848
#&gt; GSM559436     1  0.4585      0.565 0.668 0.000 0.000 0.332
#&gt; GSM559437     4  0.0000      0.852 0.000 0.000 0.000 1.000
#&gt; GSM559438     4  0.1118      0.840 0.036 0.000 0.000 0.964
#&gt; GSM559440     4  0.1940      0.814 0.000 0.076 0.000 0.924
#&gt; GSM559441     4  0.0000      0.852 0.000 0.000 0.000 1.000
#&gt; GSM559442     1  0.1118      0.836 0.964 0.000 0.000 0.036
#&gt; GSM559444     4  0.3528      0.703 0.000 0.192 0.000 0.808
#&gt; GSM559445     4  0.0000      0.852 0.000 0.000 0.000 1.000
#&gt; GSM559446     4  0.0000      0.852 0.000 0.000 0.000 1.000
#&gt; GSM559448     3  0.7512     -0.035 0.348 0.000 0.460 0.192
#&gt; GSM559450     2  0.1302      0.948 0.000 0.956 0.000 0.044
#&gt; GSM559451     4  0.3726      0.676 0.212 0.000 0.000 0.788
#&gt; GSM559452     4  0.4522      0.537 0.320 0.000 0.000 0.680
#&gt; GSM559454     4  0.4500      0.487 0.316 0.000 0.000 0.684
#&gt; GSM559455     4  0.0000      0.852 0.000 0.000 0.000 1.000
#&gt; GSM559456     4  0.2011      0.811 0.080 0.000 0.000 0.920
#&gt; GSM559457     4  0.4072      0.614 0.252 0.000 0.000 0.748
#&gt; GSM559458     3  0.6469      0.573 0.192 0.000 0.644 0.164
#&gt; GSM559459     1  0.3528      0.766 0.808 0.000 0.000 0.192
#&gt; GSM559460     1  0.0000      0.855 1.000 0.000 0.000 0.000
#&gt; GSM559461     1  0.3219      0.785 0.836 0.000 0.000 0.164
#&gt; GSM559462     1  0.3528      0.766 0.808 0.000 0.000 0.192
#&gt; GSM559463     1  0.0592      0.854 0.984 0.000 0.000 0.016
#&gt; GSM559464     1  0.0000      0.855 1.000 0.000 0.000 0.000
#&gt; GSM559465     1  0.0000      0.855 1.000 0.000 0.000 0.000
#&gt; GSM559467     4  0.4134      0.626 0.260 0.000 0.000 0.740
#&gt; GSM559468     1  0.0000      0.855 1.000 0.000 0.000 0.000
#&gt; GSM559469     1  0.0000      0.855 1.000 0.000 0.000 0.000
#&gt; GSM559470     4  0.0000      0.852 0.000 0.000 0.000 1.000
#&gt; GSM559471     1  0.1792      0.823 0.932 0.000 0.000 0.068
#&gt; GSM559472     1  0.4972      0.202 0.544 0.000 0.000 0.456
#&gt; GSM559473     2  0.2345      0.887 0.000 0.900 0.000 0.100
#&gt; GSM559475     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559477     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559478     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559479     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559480     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559481     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559482     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559435     3  0.0000      0.918 0.000 0.000 1.000 0.000
#&gt; GSM559439     3  0.0000      0.918 0.000 0.000 1.000 0.000
#&gt; GSM559443     3  0.0000      0.918 0.000 0.000 1.000 0.000
#&gt; GSM559447     3  0.0000      0.918 0.000 0.000 1.000 0.000
#&gt; GSM559449     3  0.0000      0.918 0.000 0.000 1.000 0.000
#&gt; GSM559453     3  0.0000      0.918 0.000 0.000 1.000 0.000
#&gt; GSM559466     3  0.0000      0.918 0.000 0.000 1.000 0.000
#&gt; GSM559474     3  0.0000      0.918 0.000 0.000 1.000 0.000
#&gt; GSM559476     3  0.0000      0.918 0.000 0.000 1.000 0.000
#&gt; GSM559483     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559484     3  0.0000      0.918 0.000 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-3-a').click(function(){
  $('#tab-SD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-4'>
<p><a id='tab-SD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559433     4   0.223    0.55336 0.000 0.000 0.116 0.884 0.000
#&gt; GSM559434     3   0.541   -0.57831 0.000 0.056 0.472 0.472 0.000
#&gt; GSM559436     4   0.403   -0.08415 0.352 0.000 0.000 0.648 0.000
#&gt; GSM559437     4   0.430    0.56970 0.000 0.000 0.472 0.528 0.000
#&gt; GSM559438     4   0.455    0.56535 0.008 0.000 0.472 0.520 0.000
#&gt; GSM559440     4   0.505    0.53752 0.000 0.032 0.472 0.496 0.000
#&gt; GSM559441     4   0.430    0.56970 0.000 0.000 0.472 0.528 0.000
#&gt; GSM559442     1   0.088    0.74392 0.968 0.000 0.000 0.032 0.000
#&gt; GSM559444     3   0.551   -0.57144 0.000 0.064 0.472 0.464 0.000
#&gt; GSM559445     4   0.430    0.56970 0.000 0.000 0.472 0.528 0.000
#&gt; GSM559446     4   0.373    0.58851 0.000 0.000 0.288 0.712 0.000
#&gt; GSM559448     4   0.635   -0.03700 0.176 0.000 0.288 0.532 0.004
#&gt; GSM559450     2   0.252    0.82613 0.000 0.860 0.140 0.000 0.000
#&gt; GSM559451     4   0.247    0.41232 0.136 0.000 0.000 0.864 0.000
#&gt; GSM559452     1   0.595   -0.00795 0.520 0.000 0.116 0.364 0.000
#&gt; GSM559454     4   0.285    0.36360 0.172 0.000 0.000 0.828 0.000
#&gt; GSM559455     4   0.430    0.56970 0.000 0.000 0.472 0.528 0.000
#&gt; GSM559456     4   0.152    0.49697 0.044 0.000 0.012 0.944 0.000
#&gt; GSM559457     4   0.256    0.40276 0.144 0.000 0.000 0.856 0.000
#&gt; GSM559458     3   0.724   -0.09714 0.300 0.000 0.472 0.044 0.184
#&gt; GSM559459     1   0.424    0.40335 0.572 0.000 0.000 0.428 0.000
#&gt; GSM559460     1   0.000    0.76505 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559461     1   0.402    0.49908 0.652 0.000 0.000 0.348 0.000
#&gt; GSM559462     1   0.423    0.41078 0.580 0.000 0.000 0.420 0.000
#&gt; GSM559463     1   0.285    0.70284 0.828 0.000 0.000 0.172 0.000
#&gt; GSM559464     1   0.000    0.76505 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559465     1   0.000    0.76505 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559467     4   0.531    0.37216 0.220 0.000 0.116 0.664 0.000
#&gt; GSM559468     1   0.000    0.76505 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559469     1   0.000    0.76505 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559470     4   0.430    0.56970 0.000 0.000 0.472 0.528 0.000
#&gt; GSM559471     1   0.277    0.68977 0.836 0.000 0.000 0.164 0.000
#&gt; GSM559472     4   0.382    0.07835 0.304 0.000 0.000 0.696 0.000
#&gt; GSM559473     2   0.212    0.86155 0.000 0.900 0.004 0.096 0.000
#&gt; GSM559475     2   0.000    0.96667 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559477     2   0.000    0.96667 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559478     2   0.000    0.96667 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559479     2   0.000    0.96667 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559480     2   0.000    0.96667 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559481     2   0.000    0.96667 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559482     2   0.000    0.96667 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559435     3   0.430    0.41260 0.000 0.000 0.528 0.000 0.472
#&gt; GSM559439     3   0.430    0.41260 0.000 0.000 0.528 0.000 0.472
#&gt; GSM559443     3   0.430    0.41260 0.000 0.000 0.528 0.000 0.472
#&gt; GSM559447     3   0.430    0.41260 0.000 0.000 0.528 0.000 0.472
#&gt; GSM559449     3   0.430    0.41260 0.000 0.000 0.528 0.000 0.472
#&gt; GSM559453     3   0.430    0.41260 0.000 0.000 0.528 0.000 0.472
#&gt; GSM559466     3   0.430    0.41260 0.000 0.000 0.528 0.000 0.472
#&gt; GSM559474     5   0.000    1.00000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559476     3   0.430    0.41260 0.000 0.000 0.528 0.000 0.472
#&gt; GSM559483     2   0.000    0.96667 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559484     5   0.000    1.00000 0.000 0.000 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-4-a').click(function(){
  $('#tab-SD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-5'>
<p><a id='tab-SD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4 p5    p6
#&gt; GSM559433     1  0.3804     0.2952 0.576 0.000 0.000 0.424  0 0.000
#&gt; GSM559434     4  0.0000     0.9524 0.000 0.000 0.000 1.000  0 0.000
#&gt; GSM559436     1  0.0000     0.7639 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM559437     4  0.0363     0.9549 0.012 0.000 0.000 0.988  0 0.000
#&gt; GSM559438     4  0.0000     0.9524 0.000 0.000 0.000 1.000  0 0.000
#&gt; GSM559440     4  0.0000     0.9524 0.000 0.000 0.000 1.000  0 0.000
#&gt; GSM559441     4  0.0363     0.9549 0.012 0.000 0.000 0.988  0 0.000
#&gt; GSM559442     6  0.0000     0.7333 0.000 0.000 0.000 0.000  0 1.000
#&gt; GSM559444     4  0.0000     0.9524 0.000 0.000 0.000 1.000  0 0.000
#&gt; GSM559445     4  0.0363     0.9549 0.012 0.000 0.000 0.988  0 0.000
#&gt; GSM559446     4  0.3198     0.5674 0.260 0.000 0.000 0.740  0 0.000
#&gt; GSM559448     1  0.0260     0.7599 0.992 0.000 0.008 0.000  0 0.000
#&gt; GSM559450     2  0.2664     0.7452 0.000 0.816 0.000 0.184  0 0.000
#&gt; GSM559451     1  0.0000     0.7639 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM559452     6  0.3823     0.1440 0.000 0.000 0.000 0.436  0 0.564
#&gt; GSM559454     1  0.0000     0.7639 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM559455     4  0.0458     0.9519 0.016 0.000 0.000 0.984  0 0.000
#&gt; GSM559456     1  0.2762     0.6556 0.804 0.000 0.000 0.196  0 0.000
#&gt; GSM559457     1  0.0000     0.7639 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM559458     3  0.4798     0.4272 0.000 0.000 0.612 0.076  0 0.312
#&gt; GSM559459     1  0.3499     0.4764 0.680 0.000 0.000 0.000  0 0.320
#&gt; GSM559460     6  0.0000     0.7333 0.000 0.000 0.000 0.000  0 1.000
#&gt; GSM559461     6  0.3747     0.0513 0.396 0.000 0.000 0.000  0 0.604
#&gt; GSM559462     1  0.3810     0.2974 0.572 0.000 0.000 0.000  0 0.428
#&gt; GSM559463     6  0.3867     0.2341 0.488 0.000 0.000 0.000  0 0.512
#&gt; GSM559464     6  0.0000     0.7333 0.000 0.000 0.000 0.000  0 1.000
#&gt; GSM559465     6  0.0000     0.7333 0.000 0.000 0.000 0.000  0 1.000
#&gt; GSM559467     1  0.5451     0.3955 0.532 0.000 0.000 0.328  0 0.140
#&gt; GSM559468     6  0.0000     0.7333 0.000 0.000 0.000 0.000  0 1.000
#&gt; GSM559469     6  0.0000     0.7333 0.000 0.000 0.000 0.000  0 1.000
#&gt; GSM559470     4  0.0363     0.9549 0.012 0.000 0.000 0.988  0 0.000
#&gt; GSM559471     6  0.3804     0.3541 0.424 0.000 0.000 0.000  0 0.576
#&gt; GSM559472     1  0.0146     0.7621 0.996 0.000 0.000 0.000  0 0.004
#&gt; GSM559473     2  0.1910     0.8401 0.000 0.892 0.000 0.108  0 0.000
#&gt; GSM559475     2  0.0000     0.9574 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM559477     2  0.0000     0.9574 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM559478     2  0.0000     0.9574 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM559479     2  0.0000     0.9574 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM559480     2  0.0000     0.9574 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM559481     2  0.0000     0.9574 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM559482     2  0.0000     0.9574 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM559435     3  0.0000     0.9416 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM559439     3  0.0000     0.9416 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM559443     3  0.0000     0.9416 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM559447     3  0.0000     0.9416 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM559449     3  0.0000     0.9416 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM559453     3  0.0000     0.9416 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM559466     3  0.0000     0.9416 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM559474     5  0.0000     1.0000 0.000 0.000 0.000 0.000  1 0.000
#&gt; GSM559476     3  0.0000     0.9416 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM559483     2  0.0000     0.9574 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM559484     5  0.0000     1.0000 0.000 0.000 0.000 0.000  1 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-5-a').click(function(){
  $('#tab-SD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-SD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-pam-signature_compare](figure_cola/SD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-SD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-pam-collect-classes](figure_cola/SD-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n disease.state(p) k
#> SD:pam 51         3.64e-09 2
#> SD:pam 52         1.15e-08 3
#> SD:pam 49         2.10e-08 4
#> SD:pam 29         2.37e-04 5
#> SD:pam 43         3.33e-07 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "mclust"]
# you can also extract it by
# res = res_list["SD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-mclust-collect-plots](figure_cola/SD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-mclust-select-partition-number](figure_cola/SD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.487           0.647       0.793         0.3893 0.660   0.660
#> 3 3 0.543           0.665       0.855         0.5185 0.672   0.512
#> 4 4 0.770           0.856       0.918         0.0197 0.775   0.556
#> 5 5 0.639           0.636       0.823         0.2003 0.798   0.556
#> 6 6 0.873           0.834       0.929         0.0417 0.909   0.700
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-classes'>
<ul>
<li><a href='#tab-SD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-mclust-get-classes-1'>
<p><a id='tab-SD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559433     2   0.000      0.651 0.000 1.000
#&gt; GSM559434     2   0.995      0.572 0.460 0.540
#&gt; GSM559436     2   0.000      0.651 0.000 1.000
#&gt; GSM559437     2   0.995      0.572 0.460 0.540
#&gt; GSM559438     2   0.402      0.651 0.080 0.920
#&gt; GSM559440     2   0.995      0.572 0.460 0.540
#&gt; GSM559441     2   0.995      0.572 0.460 0.540
#&gt; GSM559442     2   0.184      0.654 0.028 0.972
#&gt; GSM559444     2   0.995      0.572 0.460 0.540
#&gt; GSM559445     2   0.995      0.572 0.460 0.540
#&gt; GSM559446     2   0.995      0.572 0.460 0.540
#&gt; GSM559448     2   1.000      0.496 0.496 0.504
#&gt; GSM559450     2   0.995      0.572 0.460 0.540
#&gt; GSM559451     2   0.000      0.651 0.000 1.000
#&gt; GSM559452     1   1.000     -0.515 0.512 0.488
#&gt; GSM559454     2   0.000      0.651 0.000 1.000
#&gt; GSM559455     2   0.891      0.604 0.308 0.692
#&gt; GSM559456     2   0.995      0.572 0.460 0.540
#&gt; GSM559457     2   0.000      0.651 0.000 1.000
#&gt; GSM559458     2   0.995      0.572 0.460 0.540
#&gt; GSM559459     2   0.000      0.651 0.000 1.000
#&gt; GSM559460     2   0.000      0.651 0.000 1.000
#&gt; GSM559461     2   0.000      0.651 0.000 1.000
#&gt; GSM559462     2   0.000      0.651 0.000 1.000
#&gt; GSM559463     2   0.430      0.650 0.088 0.912
#&gt; GSM559464     2   0.000      0.651 0.000 1.000
#&gt; GSM559465     2   0.000      0.651 0.000 1.000
#&gt; GSM559467     2   0.204      0.654 0.032 0.968
#&gt; GSM559468     2   0.000      0.651 0.000 1.000
#&gt; GSM559469     2   0.278      0.654 0.048 0.952
#&gt; GSM559470     2   0.991      0.576 0.444 0.556
#&gt; GSM559471     2   0.000      0.651 0.000 1.000
#&gt; GSM559472     2   0.000      0.651 0.000 1.000
#&gt; GSM559473     2   0.995      0.572 0.460 0.540
#&gt; GSM559475     2   0.995      0.572 0.460 0.540
#&gt; GSM559477     2   0.995      0.572 0.460 0.540
#&gt; GSM559478     2   0.995      0.572 0.460 0.540
#&gt; GSM559479     2   0.995      0.572 0.460 0.540
#&gt; GSM559480     2   0.995      0.572 0.460 0.540
#&gt; GSM559481     2   0.995      0.572 0.460 0.540
#&gt; GSM559482     2   0.995      0.572 0.460 0.540
#&gt; GSM559435     1   0.000      0.922 1.000 0.000
#&gt; GSM559439     1   0.000      0.922 1.000 0.000
#&gt; GSM559443     1   0.000      0.922 1.000 0.000
#&gt; GSM559447     1   0.000      0.922 1.000 0.000
#&gt; GSM559449     1   0.000      0.922 1.000 0.000
#&gt; GSM559453     1   0.000      0.922 1.000 0.000
#&gt; GSM559466     1   0.000      0.922 1.000 0.000
#&gt; GSM559474     1   0.000      0.922 1.000 0.000
#&gt; GSM559476     1   0.000      0.922 1.000 0.000
#&gt; GSM559483     2   0.995      0.572 0.460 0.540
#&gt; GSM559484     1   0.000      0.922 1.000 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-1-a').click(function(){
  $('#tab-SD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-2'>
<p><a id='tab-SD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559433     1  0.0237     0.8560 0.996 0.000 0.004
#&gt; GSM559434     2  0.6627     0.5358 0.336 0.644 0.020
#&gt; GSM559436     1  0.0592     0.8526 0.988 0.000 0.012
#&gt; GSM559437     2  0.6937     0.4315 0.404 0.576 0.020
#&gt; GSM559438     1  0.6798     0.1697 0.584 0.400 0.016
#&gt; GSM559440     2  0.6661     0.4443 0.400 0.588 0.012
#&gt; GSM559441     2  0.6713     0.4092 0.416 0.572 0.012
#&gt; GSM559442     1  0.0424     0.8550 0.992 0.008 0.000
#&gt; GSM559444     2  0.2959     0.7063 0.100 0.900 0.000
#&gt; GSM559445     2  0.6937     0.4315 0.404 0.576 0.020
#&gt; GSM559446     2  0.6937     0.4315 0.404 0.576 0.020
#&gt; GSM559448     1  0.6828     0.4209 0.656 0.312 0.032
#&gt; GSM559450     2  0.2796     0.7081 0.092 0.908 0.000
#&gt; GSM559451     1  0.0000     0.8567 1.000 0.000 0.000
#&gt; GSM559452     2  0.7004     0.2226 0.428 0.552 0.020
#&gt; GSM559454     1  0.0000     0.8567 1.000 0.000 0.000
#&gt; GSM559455     1  0.3941     0.7227 0.844 0.156 0.000
#&gt; GSM559456     1  0.6627     0.3685 0.644 0.336 0.020
#&gt; GSM559457     1  0.0592     0.8531 0.988 0.012 0.000
#&gt; GSM559458     1  0.7001     0.3624 0.628 0.340 0.032
#&gt; GSM559459     1  0.0592     0.8526 0.988 0.000 0.012
#&gt; GSM559460     1  0.0000     0.8567 1.000 0.000 0.000
#&gt; GSM559461     1  0.0000     0.8567 1.000 0.000 0.000
#&gt; GSM559462     1  0.0592     0.8526 0.988 0.000 0.012
#&gt; GSM559463     1  0.0592     0.8526 0.988 0.000 0.012
#&gt; GSM559464     1  0.0237     0.8560 0.996 0.000 0.004
#&gt; GSM559465     1  0.0424     0.8550 0.992 0.008 0.000
#&gt; GSM559467     1  0.3686     0.7451 0.860 0.140 0.000
#&gt; GSM559468     1  0.0000     0.8567 1.000 0.000 0.000
#&gt; GSM559469     1  0.3412     0.7637 0.876 0.124 0.000
#&gt; GSM559470     1  0.6600     0.2359 0.604 0.384 0.012
#&gt; GSM559471     1  0.0237     0.8561 0.996 0.004 0.000
#&gt; GSM559472     1  0.0592     0.8526 0.988 0.000 0.012
#&gt; GSM559473     2  0.0592     0.7157 0.012 0.988 0.000
#&gt; GSM559475     2  0.0592     0.7157 0.012 0.988 0.000
#&gt; GSM559477     2  0.0592     0.7157 0.012 0.988 0.000
#&gt; GSM559478     2  0.0592     0.7157 0.012 0.988 0.000
#&gt; GSM559479     2  0.0592     0.7157 0.012 0.988 0.000
#&gt; GSM559480     2  0.0000     0.7038 0.000 1.000 0.000
#&gt; GSM559481     2  0.0000     0.7038 0.000 1.000 0.000
#&gt; GSM559482     2  0.0592     0.7157 0.012 0.988 0.000
#&gt; GSM559435     3  0.0000     0.7683 0.000 0.000 1.000
#&gt; GSM559439     3  0.0000     0.7683 0.000 0.000 1.000
#&gt; GSM559443     3  0.4750     0.7074 0.000 0.216 0.784
#&gt; GSM559447     3  0.0000     0.7683 0.000 0.000 1.000
#&gt; GSM559449     3  0.1411     0.7680 0.000 0.036 0.964
#&gt; GSM559453     3  0.4750     0.7074 0.000 0.216 0.784
#&gt; GSM559466     3  0.0000     0.7683 0.000 0.000 1.000
#&gt; GSM559474     3  0.5760     0.5825 0.000 0.328 0.672
#&gt; GSM559476     3  0.9991    -0.0274 0.344 0.312 0.344
#&gt; GSM559483     2  0.0592     0.7157 0.012 0.988 0.000
#&gt; GSM559484     3  0.5785     0.5779 0.000 0.332 0.668
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-2-a').click(function(){
  $('#tab-SD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-3'>
<p><a id='tab-SD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559433     1  0.0188      0.907 0.996 0.000 0.000 0.004
#&gt; GSM559434     1  0.4574      0.670 0.756 0.220 0.000 0.024
#&gt; GSM559436     1  0.0188      0.907 0.996 0.000 0.000 0.004
#&gt; GSM559437     1  0.4543      0.657 0.676 0.000 0.000 0.324
#&gt; GSM559438     1  0.1520      0.894 0.956 0.020 0.000 0.024
#&gt; GSM559440     1  0.3143      0.832 0.876 0.100 0.000 0.024
#&gt; GSM559441     1  0.1474      0.892 0.948 0.000 0.000 0.052
#&gt; GSM559442     1  0.0188      0.907 0.996 0.000 0.000 0.004
#&gt; GSM559444     2  0.4963      0.571 0.284 0.696 0.000 0.020
#&gt; GSM559445     1  0.4543      0.657 0.676 0.000 0.000 0.324
#&gt; GSM559446     1  0.4543      0.657 0.676 0.000 0.000 0.324
#&gt; GSM559448     1  0.0469      0.906 0.988 0.000 0.000 0.012
#&gt; GSM559450     2  0.4963      0.571 0.284 0.696 0.000 0.020
#&gt; GSM559451     1  0.0188      0.907 0.996 0.000 0.000 0.004
#&gt; GSM559452     1  0.5673      0.303 0.596 0.372 0.000 0.032
#&gt; GSM559454     1  0.0188      0.907 0.996 0.000 0.000 0.004
#&gt; GSM559455     1  0.1557      0.891 0.944 0.000 0.000 0.056
#&gt; GSM559456     1  0.4543      0.657 0.676 0.000 0.000 0.324
#&gt; GSM559457     1  0.0188      0.907 0.996 0.000 0.000 0.004
#&gt; GSM559458     1  0.4585      0.649 0.668 0.000 0.000 0.332
#&gt; GSM559459     1  0.0188      0.907 0.996 0.000 0.000 0.004
#&gt; GSM559460     1  0.0188      0.907 0.996 0.000 0.000 0.004
#&gt; GSM559461     1  0.0188      0.907 0.996 0.000 0.000 0.004
#&gt; GSM559462     1  0.0000      0.907 1.000 0.000 0.000 0.000
#&gt; GSM559463     1  0.0188      0.907 0.996 0.000 0.000 0.004
#&gt; GSM559464     1  0.0188      0.907 0.996 0.000 0.000 0.004
#&gt; GSM559465     1  0.0188      0.907 0.996 0.000 0.000 0.004
#&gt; GSM559467     1  0.0817      0.903 0.976 0.000 0.000 0.024
#&gt; GSM559468     1  0.0188      0.907 0.996 0.000 0.000 0.004
#&gt; GSM559469     1  0.0469      0.905 0.988 0.000 0.000 0.012
#&gt; GSM559470     1  0.1389      0.893 0.952 0.000 0.000 0.048
#&gt; GSM559471     1  0.0000      0.907 1.000 0.000 0.000 0.000
#&gt; GSM559472     1  0.0188      0.907 0.996 0.000 0.000 0.004
#&gt; GSM559473     2  0.1302      0.843 0.044 0.956 0.000 0.000
#&gt; GSM559475     2  0.2921      0.756 0.140 0.860 0.000 0.000
#&gt; GSM559477     2  0.0000      0.869 0.000 1.000 0.000 0.000
#&gt; GSM559478     2  0.0000      0.869 0.000 1.000 0.000 0.000
#&gt; GSM559479     2  0.0000      0.869 0.000 1.000 0.000 0.000
#&gt; GSM559480     2  0.0000      0.869 0.000 1.000 0.000 0.000
#&gt; GSM559481     2  0.0000      0.869 0.000 1.000 0.000 0.000
#&gt; GSM559482     2  0.0000      0.869 0.000 1.000 0.000 0.000
#&gt; GSM559435     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM559439     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM559443     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM559447     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM559449     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM559453     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM559466     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; GSM559474     4  0.4500      1.000 0.000 0.000 0.316 0.684
#&gt; GSM559476     1  0.3581      0.819 0.852 0.000 0.116 0.032
#&gt; GSM559483     2  0.0000      0.869 0.000 1.000 0.000 0.000
#&gt; GSM559484     4  0.4500      1.000 0.000 0.000 0.316 0.684
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-3-a').click(function(){
  $('#tab-SD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-4'>
<p><a id='tab-SD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559433     1  0.0000    0.84372 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559434     4  0.2351    0.54674 0.016 0.088 0.000 0.896 0.000
#&gt; GSM559436     1  0.0000    0.84372 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559437     5  0.6693    0.17486 0.240 0.000 0.000 0.364 0.396
#&gt; GSM559438     4  0.5096   -0.07681 0.444 0.036 0.000 0.520 0.000
#&gt; GSM559440     4  0.4010    0.44915 0.160 0.056 0.000 0.784 0.000
#&gt; GSM559441     1  0.4074    0.44631 0.636 0.000 0.000 0.364 0.000
#&gt; GSM559442     1  0.2561    0.76045 0.856 0.000 0.000 0.144 0.000
#&gt; GSM559444     4  0.3849    0.56839 0.016 0.232 0.000 0.752 0.000
#&gt; GSM559445     4  0.4949   -0.03274 0.032 0.000 0.000 0.572 0.396
#&gt; GSM559446     5  0.6671    0.16925 0.232 0.000 0.000 0.372 0.396
#&gt; GSM559448     1  0.5153    0.57871 0.684 0.000 0.112 0.204 0.000
#&gt; GSM559450     4  0.4227    0.52014 0.016 0.292 0.000 0.692 0.000
#&gt; GSM559451     1  0.0000    0.84372 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559452     4  0.4971    0.34891 0.004 0.080 0.212 0.704 0.000
#&gt; GSM559454     1  0.0000    0.84372 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559455     1  0.2848    0.74170 0.840 0.000 0.000 0.156 0.004
#&gt; GSM559456     1  0.6233   -0.00581 0.460 0.000 0.000 0.144 0.396
#&gt; GSM559457     1  0.0000    0.84372 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559458     1  0.7428   -0.02561 0.456 0.000 0.324 0.148 0.072
#&gt; GSM559459     1  0.0000    0.84372 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559460     1  0.0000    0.84372 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559461     1  0.0000    0.84372 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559462     1  0.0000    0.84372 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559463     1  0.2020    0.79222 0.900 0.000 0.000 0.100 0.000
#&gt; GSM559464     1  0.0000    0.84372 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559465     1  0.0404    0.83779 0.988 0.000 0.000 0.012 0.000
#&gt; GSM559467     1  0.3003    0.70154 0.812 0.000 0.000 0.188 0.000
#&gt; GSM559468     1  0.0162    0.84254 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559469     1  0.3039    0.73181 0.808 0.000 0.000 0.192 0.000
#&gt; GSM559470     1  0.3932    0.50596 0.672 0.000 0.000 0.328 0.000
#&gt; GSM559471     1  0.2230    0.78105 0.884 0.000 0.000 0.116 0.000
#&gt; GSM559472     1  0.0000    0.84372 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559473     4  0.4192    0.35230 0.000 0.404 0.000 0.596 0.000
#&gt; GSM559475     4  0.4114    0.40084 0.000 0.376 0.000 0.624 0.000
#&gt; GSM559477     2  0.0000    1.00000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559478     2  0.0000    1.00000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559479     2  0.0000    1.00000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.0000    1.00000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559481     2  0.0000    1.00000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559482     2  0.0000    1.00000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.0000    0.85260 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559439     3  0.0000    0.85260 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559443     3  0.1197    0.80129 0.000 0.000 0.952 0.048 0.000
#&gt; GSM559447     3  0.0000    0.85260 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559449     3  0.0000    0.85260 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559453     3  0.1341    0.81496 0.000 0.000 0.944 0.000 0.056
#&gt; GSM559466     3  0.0000    0.85260 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559474     5  0.4171   -0.07705 0.000 0.000 0.396 0.000 0.604
#&gt; GSM559476     3  0.5938   -0.03108 0.376 0.000 0.512 0.112 0.000
#&gt; GSM559483     2  0.0000    1.00000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559484     5  0.4171   -0.07705 0.000 0.000 0.396 0.000 0.604
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-4-a').click(function(){
  $('#tab-SD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-5'>
<p><a id='tab-SD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559433     1  0.0000     0.9241 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559434     6  0.0692     0.8213 0.004 0.020 0.000 0.000 0.000 0.976
#&gt; GSM559436     1  0.0000     0.9241 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559437     4  0.2263     0.6376 0.016 0.000 0.000 0.884 0.000 0.100
#&gt; GSM559438     6  0.2624     0.6839 0.148 0.004 0.000 0.004 0.000 0.844
#&gt; GSM559440     6  0.0881     0.8171 0.008 0.012 0.000 0.008 0.000 0.972
#&gt; GSM559441     4  0.5220     0.4469 0.372 0.000 0.000 0.528 0.000 0.100
#&gt; GSM559442     1  0.0000     0.9241 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559444     6  0.1082     0.8229 0.004 0.040 0.000 0.000 0.000 0.956
#&gt; GSM559445     4  0.2100     0.6188 0.004 0.000 0.000 0.884 0.000 0.112
#&gt; GSM559446     4  0.2263     0.6376 0.016 0.000 0.000 0.884 0.000 0.100
#&gt; GSM559448     1  0.2121     0.8318 0.892 0.000 0.000 0.096 0.000 0.012
#&gt; GSM559450     6  0.1471     0.8153 0.004 0.064 0.000 0.000 0.000 0.932
#&gt; GSM559451     1  0.0000     0.9241 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559452     6  0.0547     0.8203 0.000 0.020 0.000 0.000 0.000 0.980
#&gt; GSM559454     1  0.0000     0.9241 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559455     1  0.3418     0.6479 0.784 0.000 0.000 0.184 0.000 0.032
#&gt; GSM559456     4  0.4118     0.5008 0.352 0.000 0.000 0.628 0.000 0.020
#&gt; GSM559457     1  0.0000     0.9241 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559458     1  0.4326     0.0392 0.572 0.000 0.000 0.404 0.000 0.024
#&gt; GSM559459     1  0.0000     0.9241 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559460     1  0.0000     0.9241 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559461     1  0.0000     0.9241 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559462     1  0.0000     0.9241 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559463     1  0.0405     0.9172 0.988 0.000 0.000 0.004 0.000 0.008
#&gt; GSM559464     1  0.0000     0.9241 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559465     1  0.0260     0.9195 0.992 0.000 0.000 0.000 0.000 0.008
#&gt; GSM559467     1  0.1204     0.8742 0.944 0.000 0.000 0.000 0.000 0.056
#&gt; GSM559468     1  0.0000     0.9241 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559469     1  0.0458     0.9106 0.984 0.000 0.000 0.000 0.000 0.016
#&gt; GSM559470     6  0.4326     0.1213 0.404 0.000 0.000 0.024 0.000 0.572
#&gt; GSM559471     1  0.0000     0.9241 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559472     1  0.0000     0.9241 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559473     6  0.2378     0.7570 0.000 0.152 0.000 0.000 0.000 0.848
#&gt; GSM559475     6  0.2260     0.7673 0.000 0.140 0.000 0.000 0.000 0.860
#&gt; GSM559477     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559478     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559479     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559481     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559482     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.0000     0.9864 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559439     3  0.0000     0.9864 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559443     3  0.1461     0.9439 0.000 0.000 0.940 0.044 0.000 0.016
#&gt; GSM559447     3  0.0000     0.9864 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559449     3  0.0146     0.9851 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; GSM559453     3  0.0632     0.9738 0.000 0.000 0.976 0.000 0.024 0.000
#&gt; GSM559466     3  0.0000     0.9864 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559474     5  0.0000     1.0000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559476     1  0.5829     0.1964 0.524 0.000 0.336 0.116 0.000 0.024
#&gt; GSM559483     2  0.0000     1.0000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559484     5  0.0000     1.0000 0.000 0.000 0.000 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-5-a').click(function(){
  $('#tab-SD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-mclust-signature_compare](figure_cola/SD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-mclust-collect-classes](figure_cola/SD-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n disease.state(p) k
#> SD:mclust 50         4.65e-10 2
#> SD:mclust 40         2.38e-08 3
#> SD:mclust 51         1.12e-08 4
#> SD:mclust 38         3.48e-07 5
#> SD:mclust 48         4.05e-08 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### SD:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "NMF"]
# you can also extract it by
# res = res_list["SD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-NMF-collect-plots](figure_cola/SD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-NMF-select-partition-number](figure_cola/SD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.707           0.881       0.947         0.4827 0.509   0.509
#> 3 3 1.000           0.963       0.984         0.2996 0.689   0.481
#> 4 4 0.662           0.598       0.831         0.1454 0.855   0.637
#> 5 5 0.670           0.667       0.787         0.0929 0.828   0.482
#> 6 6 0.721           0.730       0.804         0.0516 0.939   0.734
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-classes'>
<ul>
<li><a href='#tab-SD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-NMF-get-classes-1'>
<p><a id='tab-SD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559433     1  0.4562      0.884 0.904 0.096
#&gt; GSM559434     2  0.0000      0.919 0.000 1.000
#&gt; GSM559436     1  0.0000      0.952 1.000 0.000
#&gt; GSM559437     1  0.5946      0.836 0.856 0.144
#&gt; GSM559438     2  0.0000      0.919 0.000 1.000
#&gt; GSM559440     2  0.0000      0.919 0.000 1.000
#&gt; GSM559441     2  0.9732      0.328 0.404 0.596
#&gt; GSM559442     1  0.2948      0.919 0.948 0.052
#&gt; GSM559444     2  0.0000      0.919 0.000 1.000
#&gt; GSM559445     2  0.7056      0.741 0.192 0.808
#&gt; GSM559446     1  0.7528      0.744 0.784 0.216
#&gt; GSM559448     1  0.0000      0.952 1.000 0.000
#&gt; GSM559450     2  0.0000      0.919 0.000 1.000
#&gt; GSM559451     1  0.5294      0.863 0.880 0.120
#&gt; GSM559452     2  0.0672      0.914 0.008 0.992
#&gt; GSM559454     1  0.0000      0.952 1.000 0.000
#&gt; GSM559455     1  0.5408      0.859 0.876 0.124
#&gt; GSM559456     1  0.0000      0.952 1.000 0.000
#&gt; GSM559457     1  0.0938      0.945 0.988 0.012
#&gt; GSM559458     1  0.0000      0.952 1.000 0.000
#&gt; GSM559459     1  0.0000      0.952 1.000 0.000
#&gt; GSM559460     1  0.0000      0.952 1.000 0.000
#&gt; GSM559461     1  0.0000      0.952 1.000 0.000
#&gt; GSM559462     1  0.8016      0.700 0.756 0.244
#&gt; GSM559463     1  0.0000      0.952 1.000 0.000
#&gt; GSM559464     1  0.0000      0.952 1.000 0.000
#&gt; GSM559465     1  0.0000      0.952 1.000 0.000
#&gt; GSM559467     2  0.8267      0.643 0.260 0.740
#&gt; GSM559468     1  0.0000      0.952 1.000 0.000
#&gt; GSM559469     2  0.9754      0.317 0.408 0.592
#&gt; GSM559470     2  0.0376      0.917 0.004 0.996
#&gt; GSM559471     1  0.8608      0.626 0.716 0.284
#&gt; GSM559472     1  0.0000      0.952 1.000 0.000
#&gt; GSM559473     2  0.0000      0.919 0.000 1.000
#&gt; GSM559475     2  0.0000      0.919 0.000 1.000
#&gt; GSM559477     2  0.0000      0.919 0.000 1.000
#&gt; GSM559478     2  0.0000      0.919 0.000 1.000
#&gt; GSM559479     2  0.0000      0.919 0.000 1.000
#&gt; GSM559480     2  0.0000      0.919 0.000 1.000
#&gt; GSM559481     2  0.0000      0.919 0.000 1.000
#&gt; GSM559482     2  0.0000      0.919 0.000 1.000
#&gt; GSM559435     1  0.0000      0.952 1.000 0.000
#&gt; GSM559439     1  0.0000      0.952 1.000 0.000
#&gt; GSM559443     1  0.0000      0.952 1.000 0.000
#&gt; GSM559447     1  0.0000      0.952 1.000 0.000
#&gt; GSM559449     1  0.0000      0.952 1.000 0.000
#&gt; GSM559453     1  0.0000      0.952 1.000 0.000
#&gt; GSM559466     1  0.0000      0.952 1.000 0.000
#&gt; GSM559474     1  0.0000      0.952 1.000 0.000
#&gt; GSM559476     1  0.0000      0.952 1.000 0.000
#&gt; GSM559483     2  0.0000      0.919 0.000 1.000
#&gt; GSM559484     2  0.6973      0.745 0.188 0.812
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-1-a').click(function(){
  $('#tab-SD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-2'>
<p><a id='tab-SD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559433     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM559434     2  0.0000      0.986 0.000 1.000 0.000
#&gt; GSM559436     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM559437     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM559438     2  0.2165      0.912 0.064 0.936 0.000
#&gt; GSM559440     2  0.0000      0.986 0.000 1.000 0.000
#&gt; GSM559441     1  0.1163      0.953 0.972 0.028 0.000
#&gt; GSM559442     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM559444     2  0.0000      0.986 0.000 1.000 0.000
#&gt; GSM559445     1  0.5706      0.545 0.680 0.320 0.000
#&gt; GSM559446     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM559448     1  0.1031      0.955 0.976 0.000 0.024
#&gt; GSM559450     2  0.0000      0.986 0.000 1.000 0.000
#&gt; GSM559451     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM559452     2  0.3267      0.868 0.000 0.884 0.116
#&gt; GSM559454     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM559455     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM559456     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM559457     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM559458     1  0.1031      0.956 0.976 0.000 0.024
#&gt; GSM559459     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM559460     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM559461     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM559462     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM559463     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM559464     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM559465     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM559467     1  0.0892      0.960 0.980 0.020 0.000
#&gt; GSM559468     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM559469     1  0.0424      0.968 0.992 0.008 0.000
#&gt; GSM559470     1  0.4931      0.709 0.768 0.232 0.000
#&gt; GSM559471     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM559472     1  0.0000      0.973 1.000 0.000 0.000
#&gt; GSM559473     2  0.0000      0.986 0.000 1.000 0.000
#&gt; GSM559475     2  0.0000      0.986 0.000 1.000 0.000
#&gt; GSM559477     2  0.0000      0.986 0.000 1.000 0.000
#&gt; GSM559478     2  0.0000      0.986 0.000 1.000 0.000
#&gt; GSM559479     2  0.0000      0.986 0.000 1.000 0.000
#&gt; GSM559480     2  0.0000      0.986 0.000 1.000 0.000
#&gt; GSM559481     2  0.0000      0.986 0.000 1.000 0.000
#&gt; GSM559482     2  0.0000      0.986 0.000 1.000 0.000
#&gt; GSM559435     3  0.0000      0.998 0.000 0.000 1.000
#&gt; GSM559439     3  0.0000      0.998 0.000 0.000 1.000
#&gt; GSM559443     3  0.0237      0.995 0.004 0.000 0.996
#&gt; GSM559447     3  0.0000      0.998 0.000 0.000 1.000
#&gt; GSM559449     3  0.0000      0.998 0.000 0.000 1.000
#&gt; GSM559453     3  0.0000      0.998 0.000 0.000 1.000
#&gt; GSM559466     3  0.0000      0.998 0.000 0.000 1.000
#&gt; GSM559474     3  0.0000      0.998 0.000 0.000 1.000
#&gt; GSM559476     3  0.0592      0.986 0.012 0.000 0.988
#&gt; GSM559483     2  0.0000      0.986 0.000 1.000 0.000
#&gt; GSM559484     3  0.0000      0.998 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-2-a').click(function(){
  $('#tab-SD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-3'>
<p><a id='tab-SD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559433     1  0.1302     0.7582 0.956 0.000 0.000 0.044
#&gt; GSM559434     2  0.0376     0.9486 0.000 0.992 0.004 0.004
#&gt; GSM559436     4  0.4331     0.4158 0.288 0.000 0.000 0.712
#&gt; GSM559437     1  0.1209     0.7479 0.964 0.000 0.032 0.004
#&gt; GSM559438     2  0.4008     0.6358 0.244 0.756 0.000 0.000
#&gt; GSM559440     2  0.2773     0.8522 0.116 0.880 0.000 0.004
#&gt; GSM559441     1  0.0712     0.7600 0.984 0.004 0.004 0.008
#&gt; GSM559442     4  0.4372     0.4155 0.268 0.000 0.004 0.728
#&gt; GSM559444     2  0.2131     0.9167 0.040 0.936 0.016 0.008
#&gt; GSM559445     1  0.4134     0.5852 0.796 0.008 0.188 0.008
#&gt; GSM559446     1  0.2944     0.6628 0.868 0.000 0.128 0.004
#&gt; GSM559448     4  0.0707     0.4386 0.020 0.000 0.000 0.980
#&gt; GSM559450     2  0.1114     0.9405 0.004 0.972 0.016 0.008
#&gt; GSM559451     1  0.1118     0.7613 0.964 0.000 0.000 0.036
#&gt; GSM559452     2  0.2924     0.8676 0.000 0.884 0.100 0.016
#&gt; GSM559454     1  0.4761     0.3956 0.628 0.000 0.000 0.372
#&gt; GSM559455     1  0.0000     0.7640 1.000 0.000 0.000 0.000
#&gt; GSM559456     1  0.0469     0.7654 0.988 0.000 0.000 0.012
#&gt; GSM559457     1  0.0592     0.7652 0.984 0.000 0.000 0.016
#&gt; GSM559458     1  0.2500     0.7430 0.916 0.000 0.044 0.040
#&gt; GSM559459     1  0.4193     0.5702 0.732 0.000 0.000 0.268
#&gt; GSM559460     1  0.3975     0.6068 0.760 0.000 0.000 0.240
#&gt; GSM559461     1  0.4643     0.4319 0.656 0.000 0.000 0.344
#&gt; GSM559462     1  0.0188     0.7650 0.996 0.000 0.000 0.004
#&gt; GSM559463     4  0.2469     0.5135 0.108 0.000 0.000 0.892
#&gt; GSM559464     4  0.4994    -0.0930 0.480 0.000 0.000 0.520
#&gt; GSM559465     4  0.4994    -0.0868 0.480 0.000 0.000 0.520
#&gt; GSM559467     1  0.0188     0.7639 0.996 0.004 0.000 0.000
#&gt; GSM559468     1  0.4981     0.1558 0.536 0.000 0.000 0.464
#&gt; GSM559469     1  0.7210     0.0304 0.448 0.120 0.004 0.428
#&gt; GSM559470     1  0.1256     0.7524 0.964 0.028 0.000 0.008
#&gt; GSM559471     1  0.4998     0.0783 0.512 0.000 0.000 0.488
#&gt; GSM559472     4  0.4994    -0.0623 0.480 0.000 0.000 0.520
#&gt; GSM559473     2  0.0469     0.9468 0.000 0.988 0.000 0.012
#&gt; GSM559475     2  0.0000     0.9495 0.000 1.000 0.000 0.000
#&gt; GSM559477     2  0.0000     0.9495 0.000 1.000 0.000 0.000
#&gt; GSM559478     2  0.0188     0.9492 0.000 0.996 0.000 0.004
#&gt; GSM559479     2  0.0188     0.9489 0.000 0.996 0.000 0.004
#&gt; GSM559480     2  0.0336     0.9485 0.000 0.992 0.000 0.008
#&gt; GSM559481     2  0.0336     0.9485 0.000 0.992 0.000 0.008
#&gt; GSM559482     2  0.0000     0.9495 0.000 1.000 0.000 0.000
#&gt; GSM559435     4  0.4967    -0.5400 0.000 0.000 0.452 0.548
#&gt; GSM559439     4  0.4961    -0.5318 0.000 0.000 0.448 0.552
#&gt; GSM559443     4  0.2408     0.3166 0.000 0.000 0.104 0.896
#&gt; GSM559447     3  0.4925     0.6503 0.000 0.000 0.572 0.428
#&gt; GSM559449     3  0.4008     0.7710 0.000 0.000 0.756 0.244
#&gt; GSM559453     3  0.2589     0.7846 0.000 0.000 0.884 0.116
#&gt; GSM559466     3  0.4855     0.6810 0.000 0.000 0.600 0.400
#&gt; GSM559474     3  0.0000     0.7518 0.000 0.000 1.000 0.000
#&gt; GSM559476     4  0.1940     0.3600 0.000 0.000 0.076 0.924
#&gt; GSM559483     2  0.0000     0.9495 0.000 1.000 0.000 0.000
#&gt; GSM559484     3  0.0376     0.7520 0.000 0.004 0.992 0.004
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-3-a').click(function(){
  $('#tab-SD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-4'>
<p><a id='tab-SD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559433     4  0.1300     0.9169 0.028 0.000 0.000 0.956 0.016
#&gt; GSM559434     2  0.5533     0.7612 0.192 0.680 0.000 0.016 0.112
#&gt; GSM559436     5  0.8583    -0.2962 0.224 0.000 0.232 0.256 0.288
#&gt; GSM559437     4  0.1012     0.9137 0.012 0.000 0.000 0.968 0.020
#&gt; GSM559438     2  0.6500     0.6772 0.236 0.600 0.000 0.052 0.112
#&gt; GSM559440     2  0.7530     0.6126 0.212 0.516 0.000 0.148 0.124
#&gt; GSM559441     4  0.1059     0.9018 0.020 0.008 0.000 0.968 0.004
#&gt; GSM559442     1  0.2529     0.6299 0.900 0.004 0.056 0.000 0.040
#&gt; GSM559444     2  0.6440     0.7347 0.144 0.644 0.000 0.092 0.120
#&gt; GSM559445     4  0.2577     0.8158 0.016 0.008 0.000 0.892 0.084
#&gt; GSM559446     4  0.1357     0.8789 0.004 0.000 0.000 0.948 0.048
#&gt; GSM559448     3  0.6244     0.4350 0.120 0.008 0.556 0.004 0.312
#&gt; GSM559450     2  0.5370     0.7746 0.148 0.708 0.000 0.020 0.124
#&gt; GSM559451     4  0.3067     0.7996 0.140 0.000 0.004 0.844 0.012
#&gt; GSM559452     1  0.5685     0.0671 0.556 0.048 0.004 0.012 0.380
#&gt; GSM559454     1  0.6060     0.6531 0.576 0.000 0.000 0.216 0.208
#&gt; GSM559455     4  0.0609     0.9194 0.020 0.000 0.000 0.980 0.000
#&gt; GSM559456     4  0.1043     0.9173 0.040 0.000 0.000 0.960 0.000
#&gt; GSM559457     4  0.0794     0.9197 0.028 0.000 0.000 0.972 0.000
#&gt; GSM559458     1  0.4642     0.7130 0.772 0.000 0.028 0.136 0.064
#&gt; GSM559459     1  0.5638     0.6965 0.632 0.000 0.000 0.216 0.152
#&gt; GSM559460     1  0.2648     0.7626 0.848 0.000 0.000 0.152 0.000
#&gt; GSM559461     1  0.5159     0.7299 0.688 0.000 0.000 0.188 0.124
#&gt; GSM559462     4  0.3010     0.7659 0.172 0.000 0.000 0.824 0.004
#&gt; GSM559463     1  0.6743     0.3376 0.516 0.000 0.212 0.016 0.256
#&gt; GSM559464     1  0.3247     0.7659 0.840 0.000 0.008 0.136 0.016
#&gt; GSM559465     1  0.4409     0.7550 0.752 0.000 0.000 0.176 0.072
#&gt; GSM559467     4  0.1956     0.8886 0.076 0.000 0.000 0.916 0.008
#&gt; GSM559468     1  0.3155     0.7607 0.848 0.000 0.016 0.128 0.008
#&gt; GSM559469     1  0.3460     0.7416 0.828 0.000 0.000 0.128 0.044
#&gt; GSM559470     4  0.1173     0.9184 0.020 0.012 0.000 0.964 0.004
#&gt; GSM559471     1  0.3151     0.7664 0.836 0.000 0.000 0.144 0.020
#&gt; GSM559472     1  0.7335     0.4994 0.444 0.000 0.036 0.256 0.264
#&gt; GSM559473     2  0.1251     0.8467 0.008 0.956 0.000 0.000 0.036
#&gt; GSM559475     2  0.0703     0.8403 0.000 0.976 0.000 0.000 0.024
#&gt; GSM559477     2  0.0798     0.8476 0.008 0.976 0.000 0.000 0.016
#&gt; GSM559478     2  0.0794     0.8393 0.000 0.972 0.000 0.000 0.028
#&gt; GSM559479     2  0.3967     0.8095 0.108 0.800 0.000 0.000 0.092
#&gt; GSM559480     2  0.0955     0.8381 0.004 0.968 0.000 0.000 0.028
#&gt; GSM559481     2  0.0955     0.8381 0.004 0.968 0.000 0.000 0.028
#&gt; GSM559482     2  0.0162     0.8445 0.000 0.996 0.000 0.000 0.004
#&gt; GSM559435     3  0.2873     0.5964 0.016 0.000 0.856 0.000 0.128
#&gt; GSM559439     3  0.2573     0.5967 0.016 0.000 0.880 0.000 0.104
#&gt; GSM559443     3  0.5641     0.4887 0.120 0.000 0.612 0.000 0.268
#&gt; GSM559447     3  0.0162     0.5572 0.004 0.000 0.996 0.000 0.000
#&gt; GSM559449     3  0.2516     0.3898 0.000 0.000 0.860 0.000 0.140
#&gt; GSM559453     3  0.4114    -0.1702 0.000 0.000 0.624 0.000 0.376
#&gt; GSM559466     3  0.0794     0.5334 0.000 0.000 0.972 0.000 0.028
#&gt; GSM559474     5  0.4410     0.2496 0.000 0.000 0.440 0.004 0.556
#&gt; GSM559476     3  0.5641     0.4870 0.120 0.000 0.612 0.000 0.268
#&gt; GSM559483     2  0.0703     0.8475 0.000 0.976 0.000 0.000 0.024
#&gt; GSM559484     5  0.4410     0.2496 0.000 0.000 0.440 0.004 0.556
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-4-a').click(function(){
  $('#tab-SD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-5'>
<p><a id='tab-SD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559433     4  0.3146      0.782 0.000 0.000 0.012 0.848 0.060 0.080
#&gt; GSM559434     6  0.3771      0.748 0.044 0.172 0.000 0.008 0.000 0.776
#&gt; GSM559436     4  0.8660      0.136 0.196 0.012 0.252 0.336 0.108 0.096
#&gt; GSM559437     4  0.0632      0.802 0.000 0.000 0.000 0.976 0.000 0.024
#&gt; GSM559438     6  0.4516      0.732 0.084 0.184 0.000 0.012 0.000 0.720
#&gt; GSM559440     6  0.3196      0.743 0.016 0.108 0.000 0.036 0.000 0.840
#&gt; GSM559441     4  0.1814      0.782 0.000 0.000 0.000 0.900 0.000 0.100
#&gt; GSM559442     1  0.2527      0.707 0.832 0.000 0.000 0.000 0.000 0.168
#&gt; GSM559444     6  0.3807      0.717 0.000 0.192 0.000 0.052 0.000 0.756
#&gt; GSM559445     4  0.2278      0.773 0.000 0.000 0.000 0.868 0.004 0.128
#&gt; GSM559446     4  0.2274      0.802 0.008 0.000 0.000 0.892 0.012 0.088
#&gt; GSM559448     3  0.3441      0.729 0.004 0.036 0.836 0.000 0.096 0.028
#&gt; GSM559450     6  0.3163      0.718 0.000 0.232 0.000 0.004 0.000 0.764
#&gt; GSM559451     4  0.5106      0.674 0.160 0.000 0.012 0.708 0.032 0.088
#&gt; GSM559452     6  0.4607      0.354 0.328 0.000 0.000 0.000 0.056 0.616
#&gt; GSM559454     1  0.6287      0.670 0.664 0.028 0.024 0.100 0.072 0.112
#&gt; GSM559455     4  0.0547      0.802 0.000 0.000 0.000 0.980 0.000 0.020
#&gt; GSM559456     4  0.0717      0.803 0.016 0.000 0.000 0.976 0.000 0.008
#&gt; GSM559457     4  0.0777      0.805 0.004 0.000 0.000 0.972 0.000 0.024
#&gt; GSM559458     1  0.5180      0.560 0.680 0.000 0.004 0.020 0.148 0.148
#&gt; GSM559459     1  0.5717      0.683 0.684 0.004 0.012 0.100 0.088 0.112
#&gt; GSM559460     1  0.1714      0.759 0.908 0.000 0.000 0.000 0.000 0.092
#&gt; GSM559461     1  0.4994      0.737 0.752 0.000 0.040 0.076 0.080 0.052
#&gt; GSM559462     4  0.4881      0.508 0.276 0.000 0.000 0.644 0.012 0.068
#&gt; GSM559463     1  0.6125      0.605 0.632 0.008 0.180 0.008 0.092 0.080
#&gt; GSM559464     1  0.0363      0.774 0.988 0.000 0.000 0.000 0.000 0.012
#&gt; GSM559465     1  0.3116      0.763 0.868 0.004 0.004 0.044 0.028 0.052
#&gt; GSM559467     4  0.5011      0.642 0.104 0.160 0.000 0.704 0.004 0.028
#&gt; GSM559468     1  0.1957      0.750 0.888 0.000 0.000 0.000 0.000 0.112
#&gt; GSM559469     1  0.1700      0.762 0.916 0.000 0.000 0.000 0.004 0.080
#&gt; GSM559470     4  0.3812      0.747 0.056 0.028 0.000 0.812 0.004 0.100
#&gt; GSM559471     1  0.1577      0.774 0.940 0.000 0.000 0.008 0.016 0.036
#&gt; GSM559472     1  0.6693      0.523 0.580 0.020 0.012 0.212 0.068 0.108
#&gt; GSM559473     2  0.2331      0.869 0.032 0.888 0.000 0.000 0.000 0.080
#&gt; GSM559475     2  0.0692      0.907 0.004 0.976 0.000 0.000 0.000 0.020
#&gt; GSM559477     2  0.2340      0.840 0.000 0.852 0.000 0.000 0.000 0.148
#&gt; GSM559478     2  0.0260      0.908 0.000 0.992 0.000 0.000 0.000 0.008
#&gt; GSM559479     6  0.3828      0.381 0.000 0.440 0.000 0.000 0.000 0.560
#&gt; GSM559480     2  0.0146      0.905 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM559481     2  0.0000      0.907 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559482     2  0.1910      0.879 0.000 0.892 0.000 0.000 0.000 0.108
#&gt; GSM559435     3  0.0000      0.810 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559439     3  0.0937      0.811 0.000 0.000 0.960 0.000 0.040 0.000
#&gt; GSM559443     3  0.2191      0.759 0.004 0.000 0.876 0.000 0.120 0.000
#&gt; GSM559447     3  0.1501      0.799 0.000 0.000 0.924 0.000 0.076 0.000
#&gt; GSM559449     3  0.2631      0.713 0.000 0.000 0.820 0.000 0.180 0.000
#&gt; GSM559453     3  0.3789      0.210 0.000 0.000 0.584 0.000 0.416 0.000
#&gt; GSM559466     3  0.1957      0.779 0.000 0.000 0.888 0.000 0.112 0.000
#&gt; GSM559474     5  0.2278      1.000 0.000 0.000 0.128 0.000 0.868 0.004
#&gt; GSM559476     3  0.1806      0.782 0.004 0.000 0.908 0.000 0.088 0.000
#&gt; GSM559483     2  0.2178      0.858 0.000 0.868 0.000 0.000 0.000 0.132
#&gt; GSM559484     5  0.2278      1.000 0.000 0.000 0.128 0.000 0.868 0.004
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-5-a').click(function(){
  $('#tab-SD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-NMF-signature_compare](figure_cola/SD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-SD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-NMF-collect-classes](figure_cola/SD-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n disease.state(p) k
#> SD:NMF 50         2.37e-01 2
#> SD:NMF 52         8.38e-11 3
#> SD:NMF 37         8.86e-07 4
#> SD:NMF 41         4.46e-07 5
#> SD:NMF 48         5.00e-07 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:hclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "hclust"]
# you can also extract it by
# res = res_list["CV:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-hclust-collect-plots](figure_cola/CV-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-hclust-select-partition-number](figure_cola/CV-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.675           0.893       0.944         0.3941 0.581   0.581
#> 3 3 0.623           0.806       0.902         0.2687 0.947   0.909
#> 4 4 0.640           0.865       0.894         0.1957 0.837   0.692
#> 5 5 0.701           0.820       0.890         0.1477 0.932   0.817
#> 6 6 0.727           0.851       0.884         0.0464 0.991   0.971
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-classes'>
<ul>
<li><a href='#tab-CV-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-hclust-get-classes-1'>
<p><a id='tab-CV-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559433     1  0.0376      0.963 0.996 0.004
#&gt; GSM559434     2  0.8713      0.711 0.292 0.708
#&gt; GSM559436     1  0.0000      0.966 1.000 0.000
#&gt; GSM559437     1  0.0000      0.966 1.000 0.000
#&gt; GSM559438     2  0.9358      0.615 0.352 0.648
#&gt; GSM559440     2  0.9358      0.615 0.352 0.648
#&gt; GSM559441     1  0.0938      0.959 0.988 0.012
#&gt; GSM559442     1  0.8144      0.615 0.748 0.252
#&gt; GSM559444     2  0.6247      0.838 0.156 0.844
#&gt; GSM559445     1  0.0000      0.966 1.000 0.000
#&gt; GSM559446     1  0.0000      0.966 1.000 0.000
#&gt; GSM559448     1  0.0000      0.966 1.000 0.000
#&gt; GSM559450     2  0.6247      0.838 0.156 0.844
#&gt; GSM559451     1  0.0000      0.966 1.000 0.000
#&gt; GSM559452     2  0.8608      0.721 0.284 0.716
#&gt; GSM559454     1  0.0000      0.966 1.000 0.000
#&gt; GSM559455     1  0.0938      0.959 0.988 0.012
#&gt; GSM559456     1  0.0000      0.966 1.000 0.000
#&gt; GSM559457     1  0.0000      0.966 1.000 0.000
#&gt; GSM559458     1  0.0938      0.959 0.988 0.012
#&gt; GSM559459     1  0.0000      0.966 1.000 0.000
#&gt; GSM559460     1  0.0000      0.966 1.000 0.000
#&gt; GSM559461     1  0.0000      0.966 1.000 0.000
#&gt; GSM559462     1  0.0000      0.966 1.000 0.000
#&gt; GSM559463     1  0.0000      0.966 1.000 0.000
#&gt; GSM559464     1  0.0000      0.966 1.000 0.000
#&gt; GSM559465     1  0.0000      0.966 1.000 0.000
#&gt; GSM559467     1  0.3431      0.907 0.936 0.064
#&gt; GSM559468     1  0.2236      0.940 0.964 0.036
#&gt; GSM559469     1  0.2236      0.940 0.964 0.036
#&gt; GSM559470     1  0.3733      0.898 0.928 0.072
#&gt; GSM559471     1  0.2236      0.941 0.964 0.036
#&gt; GSM559472     1  0.0000      0.966 1.000 0.000
#&gt; GSM559473     2  0.6048      0.842 0.148 0.852
#&gt; GSM559475     2  0.6048      0.842 0.148 0.852
#&gt; GSM559477     2  0.0000      0.854 0.000 1.000
#&gt; GSM559478     2  0.0000      0.854 0.000 1.000
#&gt; GSM559479     2  0.0000      0.854 0.000 1.000
#&gt; GSM559480     2  0.0000      0.854 0.000 1.000
#&gt; GSM559481     2  0.0000      0.854 0.000 1.000
#&gt; GSM559482     2  0.0000      0.854 0.000 1.000
#&gt; GSM559435     1  0.0000      0.966 1.000 0.000
#&gt; GSM559439     1  0.0000      0.966 1.000 0.000
#&gt; GSM559443     1  0.0000      0.966 1.000 0.000
#&gt; GSM559447     1  0.0000      0.966 1.000 0.000
#&gt; GSM559449     1  0.0000      0.966 1.000 0.000
#&gt; GSM559453     1  0.0000      0.966 1.000 0.000
#&gt; GSM559466     1  0.0000      0.966 1.000 0.000
#&gt; GSM559474     1  0.8207      0.615 0.744 0.256
#&gt; GSM559476     1  0.0000      0.966 1.000 0.000
#&gt; GSM559483     2  0.0000      0.854 0.000 1.000
#&gt; GSM559484     1  0.8207      0.615 0.744 0.256
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-1-a').click(function(){
  $('#tab-CV-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-2'>
<p><a id='tab-CV-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559433     1  0.0237      0.897 0.996 0.000 0.004
#&gt; GSM559434     2  0.6771      0.655 0.276 0.684 0.040
#&gt; GSM559436     1  0.0000      0.898 1.000 0.000 0.000
#&gt; GSM559437     1  0.0000      0.898 1.000 0.000 0.000
#&gt; GSM559438     2  0.7186      0.588 0.336 0.624 0.040
#&gt; GSM559440     2  0.7186      0.588 0.336 0.624 0.040
#&gt; GSM559441     1  0.1031      0.888 0.976 0.000 0.024
#&gt; GSM559442     1  0.6715      0.550 0.716 0.228 0.056
#&gt; GSM559444     2  0.5159      0.778 0.140 0.820 0.040
#&gt; GSM559445     1  0.0000      0.898 1.000 0.000 0.000
#&gt; GSM559446     1  0.0000      0.898 1.000 0.000 0.000
#&gt; GSM559448     1  0.0592      0.894 0.988 0.000 0.012
#&gt; GSM559450     2  0.5159      0.778 0.140 0.820 0.040
#&gt; GSM559451     1  0.0000      0.898 1.000 0.000 0.000
#&gt; GSM559452     2  0.6950      0.679 0.252 0.692 0.056
#&gt; GSM559454     1  0.0000      0.898 1.000 0.000 0.000
#&gt; GSM559455     1  0.1031      0.888 0.976 0.000 0.024
#&gt; GSM559456     1  0.0000      0.898 1.000 0.000 0.000
#&gt; GSM559457     1  0.0000      0.898 1.000 0.000 0.000
#&gt; GSM559458     1  0.1031      0.888 0.976 0.000 0.024
#&gt; GSM559459     1  0.0000      0.898 1.000 0.000 0.000
#&gt; GSM559460     1  0.0000      0.898 1.000 0.000 0.000
#&gt; GSM559461     1  0.0000      0.898 1.000 0.000 0.000
#&gt; GSM559462     1  0.0000      0.898 1.000 0.000 0.000
#&gt; GSM559463     1  0.0000      0.898 1.000 0.000 0.000
#&gt; GSM559464     1  0.0000      0.898 1.000 0.000 0.000
#&gt; GSM559465     1  0.0000      0.898 1.000 0.000 0.000
#&gt; GSM559467     1  0.2165      0.855 0.936 0.064 0.000
#&gt; GSM559468     1  0.2339      0.868 0.940 0.012 0.048
#&gt; GSM559469     1  0.2339      0.868 0.940 0.012 0.048
#&gt; GSM559470     1  0.2356      0.849 0.928 0.072 0.000
#&gt; GSM559471     1  0.2031      0.879 0.952 0.016 0.032
#&gt; GSM559472     1  0.0000      0.898 1.000 0.000 0.000
#&gt; GSM559473     2  0.5028      0.781 0.132 0.828 0.040
#&gt; GSM559475     2  0.5028      0.781 0.132 0.828 0.040
#&gt; GSM559477     2  0.0000      0.771 0.000 1.000 0.000
#&gt; GSM559478     2  0.0000      0.771 0.000 1.000 0.000
#&gt; GSM559479     2  0.0000      0.771 0.000 1.000 0.000
#&gt; GSM559480     2  0.0000      0.771 0.000 1.000 0.000
#&gt; GSM559481     2  0.0000      0.771 0.000 1.000 0.000
#&gt; GSM559482     2  0.0000      0.771 0.000 1.000 0.000
#&gt; GSM559435     1  0.5591      0.641 0.696 0.000 0.304
#&gt; GSM559439     1  0.5397      0.668 0.720 0.000 0.280
#&gt; GSM559443     1  0.5397      0.668 0.720 0.000 0.280
#&gt; GSM559447     1  0.5591      0.641 0.696 0.000 0.304
#&gt; GSM559449     1  0.5591      0.641 0.696 0.000 0.304
#&gt; GSM559453     1  0.5678      0.625 0.684 0.000 0.316
#&gt; GSM559466     1  0.5591      0.641 0.696 0.000 0.304
#&gt; GSM559474     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559476     1  0.5216      0.690 0.740 0.000 0.260
#&gt; GSM559483     2  0.0000      0.771 0.000 1.000 0.000
#&gt; GSM559484     3  0.0000      1.000 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-2-a').click(function(){
  $('#tab-CV-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-3'>
<p><a id='tab-CV-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559433     1  0.0188      0.962 0.996 0.004 0.000 0.000
#&gt; GSM559434     2  0.4539      0.625 0.272 0.720 0.008 0.000
#&gt; GSM559436     1  0.0000      0.962 1.000 0.000 0.000 0.000
#&gt; GSM559437     1  0.0592      0.956 0.984 0.000 0.016 0.000
#&gt; GSM559438     2  0.4781      0.559 0.336 0.660 0.004 0.000
#&gt; GSM559440     2  0.4781      0.559 0.336 0.660 0.004 0.000
#&gt; GSM559441     1  0.1004      0.951 0.972 0.024 0.004 0.000
#&gt; GSM559442     1  0.4927      0.569 0.712 0.264 0.024 0.000
#&gt; GSM559444     2  0.2921      0.725 0.140 0.860 0.000 0.000
#&gt; GSM559445     1  0.0592      0.956 0.984 0.000 0.016 0.000
#&gt; GSM559446     1  0.0592      0.956 0.984 0.000 0.016 0.000
#&gt; GSM559448     1  0.0469      0.957 0.988 0.000 0.012 0.000
#&gt; GSM559450     2  0.2921      0.725 0.140 0.860 0.000 0.000
#&gt; GSM559451     1  0.0000      0.962 1.000 0.000 0.000 0.000
#&gt; GSM559452     2  0.4807      0.639 0.248 0.728 0.024 0.000
#&gt; GSM559454     1  0.0000      0.962 1.000 0.000 0.000 0.000
#&gt; GSM559455     1  0.1004      0.951 0.972 0.024 0.004 0.000
#&gt; GSM559456     1  0.1022      0.943 0.968 0.000 0.032 0.000
#&gt; GSM559457     1  0.0000      0.962 1.000 0.000 0.000 0.000
#&gt; GSM559458     1  0.1004      0.951 0.972 0.024 0.004 0.000
#&gt; GSM559459     1  0.0000      0.962 1.000 0.000 0.000 0.000
#&gt; GSM559460     1  0.0000      0.962 1.000 0.000 0.000 0.000
#&gt; GSM559461     1  0.0000      0.962 1.000 0.000 0.000 0.000
#&gt; GSM559462     1  0.0000      0.962 1.000 0.000 0.000 0.000
#&gt; GSM559463     1  0.0188      0.961 0.996 0.000 0.004 0.000
#&gt; GSM559464     1  0.0000      0.962 1.000 0.000 0.000 0.000
#&gt; GSM559465     1  0.0188      0.961 0.996 0.000 0.004 0.000
#&gt; GSM559467     1  0.1824      0.911 0.936 0.060 0.004 0.000
#&gt; GSM559468     1  0.1854      0.926 0.940 0.048 0.012 0.000
#&gt; GSM559469     1  0.1854      0.926 0.940 0.048 0.012 0.000
#&gt; GSM559470     1  0.1978      0.904 0.928 0.068 0.004 0.000
#&gt; GSM559471     1  0.1584      0.939 0.952 0.036 0.012 0.000
#&gt; GSM559472     1  0.0000      0.962 1.000 0.000 0.000 0.000
#&gt; GSM559473     2  0.2814      0.726 0.132 0.868 0.000 0.000
#&gt; GSM559475     2  0.2814      0.726 0.132 0.868 0.000 0.000
#&gt; GSM559477     2  0.3852      0.673 0.000 0.808 0.180 0.012
#&gt; GSM559478     2  0.2888      0.698 0.000 0.872 0.124 0.004
#&gt; GSM559479     2  0.3852      0.673 0.000 0.808 0.180 0.012
#&gt; GSM559480     2  0.2888      0.698 0.000 0.872 0.124 0.004
#&gt; GSM559481     2  0.2888      0.698 0.000 0.872 0.124 0.004
#&gt; GSM559482     2  0.3852      0.673 0.000 0.808 0.180 0.012
#&gt; GSM559435     3  0.3444      0.963 0.184 0.000 0.816 0.000
#&gt; GSM559439     3  0.3837      0.944 0.224 0.000 0.776 0.000
#&gt; GSM559443     3  0.3837      0.944 0.224 0.000 0.776 0.000
#&gt; GSM559447     3  0.3444      0.963 0.184 0.000 0.816 0.000
#&gt; GSM559449     3  0.3444      0.963 0.184 0.000 0.816 0.000
#&gt; GSM559453     3  0.3764      0.946 0.172 0.000 0.816 0.012
#&gt; GSM559466     3  0.3444      0.963 0.184 0.000 0.816 0.000
#&gt; GSM559474     4  0.0469      1.000 0.000 0.000 0.012 0.988
#&gt; GSM559476     3  0.4008      0.916 0.244 0.000 0.756 0.000
#&gt; GSM559483     2  0.3852      0.673 0.000 0.808 0.180 0.012
#&gt; GSM559484     4  0.0469      1.000 0.000 0.000 0.012 0.988
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-3-a').click(function(){
  $('#tab-CV-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-4'>
<p><a id='tab-CV-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559433     1  0.1908      0.909 0.908 0.000 0.000 0.092 0.000
#&gt; GSM559434     4  0.5538      0.670 0.088 0.324 0.000 0.588 0.000
#&gt; GSM559436     1  0.0162      0.912 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559437     1  0.2616      0.902 0.880 0.000 0.020 0.100 0.000
#&gt; GSM559438     4  0.5797      0.642 0.132 0.276 0.000 0.592 0.000
#&gt; GSM559440     4  0.5797      0.642 0.132 0.276 0.000 0.592 0.000
#&gt; GSM559441     1  0.2891      0.868 0.824 0.000 0.000 0.176 0.000
#&gt; GSM559442     4  0.4297     -0.198 0.472 0.000 0.000 0.528 0.000
#&gt; GSM559444     4  0.4273      0.636 0.000 0.448 0.000 0.552 0.000
#&gt; GSM559445     1  0.2616      0.902 0.880 0.000 0.020 0.100 0.000
#&gt; GSM559446     1  0.2616      0.902 0.880 0.000 0.020 0.100 0.000
#&gt; GSM559448     1  0.0693      0.908 0.980 0.000 0.012 0.008 0.000
#&gt; GSM559450     4  0.4273      0.636 0.000 0.448 0.000 0.552 0.000
#&gt; GSM559451     1  0.0794      0.916 0.972 0.000 0.000 0.028 0.000
#&gt; GSM559452     4  0.4009      0.653 0.004 0.312 0.000 0.684 0.000
#&gt; GSM559454     1  0.0162      0.912 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559455     1  0.2891      0.868 0.824 0.000 0.000 0.176 0.000
#&gt; GSM559456     1  0.2959      0.896 0.864 0.000 0.036 0.100 0.000
#&gt; GSM559457     1  0.0794      0.916 0.972 0.000 0.000 0.028 0.000
#&gt; GSM559458     1  0.2891      0.868 0.824 0.000 0.000 0.176 0.000
#&gt; GSM559459     1  0.0162      0.912 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559460     1  0.0162      0.912 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559461     1  0.0162      0.912 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559462     1  0.1043      0.915 0.960 0.000 0.000 0.040 0.000
#&gt; GSM559463     1  0.0451      0.911 0.988 0.000 0.004 0.008 0.000
#&gt; GSM559464     1  0.0290      0.911 0.992 0.000 0.000 0.008 0.000
#&gt; GSM559465     1  0.0451      0.911 0.988 0.000 0.004 0.008 0.000
#&gt; GSM559467     1  0.2891      0.863 0.824 0.000 0.000 0.176 0.000
#&gt; GSM559468     1  0.3395      0.792 0.764 0.000 0.000 0.236 0.000
#&gt; GSM559469     1  0.3395      0.792 0.764 0.000 0.000 0.236 0.000
#&gt; GSM559470     1  0.3143      0.840 0.796 0.000 0.000 0.204 0.000
#&gt; GSM559471     1  0.2690      0.872 0.844 0.000 0.000 0.156 0.000
#&gt; GSM559472     1  0.0510      0.915 0.984 0.000 0.000 0.016 0.000
#&gt; GSM559473     4  0.4287      0.623 0.000 0.460 0.000 0.540 0.000
#&gt; GSM559475     4  0.4287      0.623 0.000 0.460 0.000 0.540 0.000
#&gt; GSM559477     2  0.0000      0.741 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559478     2  0.4547      0.680 0.000 0.588 0.012 0.400 0.000
#&gt; GSM559479     2  0.0000      0.741 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.4547      0.680 0.000 0.588 0.012 0.400 0.000
#&gt; GSM559481     2  0.4547      0.680 0.000 0.588 0.012 0.400 0.000
#&gt; GSM559482     2  0.0000      0.741 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.0404      0.960 0.012 0.000 0.988 0.000 0.000
#&gt; GSM559439     3  0.1341      0.937 0.056 0.000 0.944 0.000 0.000
#&gt; GSM559443     3  0.1341      0.937 0.056 0.000 0.944 0.000 0.000
#&gt; GSM559447     3  0.0404      0.960 0.012 0.000 0.988 0.000 0.000
#&gt; GSM559449     3  0.0404      0.960 0.012 0.000 0.988 0.000 0.000
#&gt; GSM559453     3  0.0404      0.943 0.000 0.000 0.988 0.000 0.012
#&gt; GSM559466     3  0.0404      0.960 0.012 0.000 0.988 0.000 0.000
#&gt; GSM559474     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559476     3  0.2046      0.911 0.068 0.000 0.916 0.016 0.000
#&gt; GSM559483     2  0.0000      0.741 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559484     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-4-a').click(function(){
  $('#tab-CV-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-5'>
<p><a id='tab-CV-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559433     1  0.1700      0.877 0.928 0.000 0.000 0.024 0.000 0.048
#&gt; GSM559434     6  0.3146      0.730 0.080 0.060 0.000 0.012 0.000 0.848
#&gt; GSM559436     1  0.2003      0.864 0.884 0.000 0.000 0.116 0.000 0.000
#&gt; GSM559437     1  0.2602      0.859 0.888 0.000 0.020 0.052 0.000 0.040
#&gt; GSM559438     6  0.3313      0.679 0.160 0.024 0.000 0.008 0.000 0.808
#&gt; GSM559440     6  0.3313      0.679 0.160 0.024 0.000 0.008 0.000 0.808
#&gt; GSM559441     1  0.3457      0.825 0.820 0.000 0.012 0.052 0.000 0.116
#&gt; GSM559442     6  0.4907     -0.140 0.464 0.000 0.012 0.036 0.000 0.488
#&gt; GSM559444     6  0.2257      0.733 0.000 0.116 0.000 0.008 0.000 0.876
#&gt; GSM559445     1  0.2602      0.859 0.888 0.000 0.020 0.052 0.000 0.040
#&gt; GSM559446     1  0.2602      0.859 0.888 0.000 0.020 0.052 0.000 0.040
#&gt; GSM559448     1  0.2357      0.865 0.872 0.000 0.012 0.116 0.000 0.000
#&gt; GSM559450     6  0.2257      0.733 0.000 0.116 0.000 0.008 0.000 0.876
#&gt; GSM559451     1  0.1296      0.879 0.948 0.000 0.004 0.044 0.000 0.004
#&gt; GSM559452     6  0.0935      0.708 0.004 0.000 0.000 0.032 0.000 0.964
#&gt; GSM559454     1  0.1814      0.870 0.900 0.000 0.000 0.100 0.000 0.000
#&gt; GSM559455     1  0.3457      0.825 0.820 0.000 0.012 0.052 0.000 0.116
#&gt; GSM559456     1  0.2911      0.854 0.872 0.000 0.036 0.052 0.000 0.040
#&gt; GSM559457     1  0.0937      0.879 0.960 0.000 0.000 0.040 0.000 0.000
#&gt; GSM559458     1  0.3457      0.825 0.820 0.000 0.012 0.052 0.000 0.116
#&gt; GSM559459     1  0.2003      0.864 0.884 0.000 0.000 0.116 0.000 0.000
#&gt; GSM559460     1  0.2006      0.869 0.892 0.000 0.004 0.104 0.000 0.000
#&gt; GSM559461     1  0.2006      0.869 0.892 0.000 0.004 0.104 0.000 0.000
#&gt; GSM559462     1  0.1528      0.876 0.936 0.000 0.000 0.048 0.000 0.016
#&gt; GSM559463     1  0.2048      0.865 0.880 0.000 0.000 0.120 0.000 0.000
#&gt; GSM559464     1  0.2048      0.863 0.880 0.000 0.000 0.120 0.000 0.000
#&gt; GSM559465     1  0.2003      0.867 0.884 0.000 0.000 0.116 0.000 0.000
#&gt; GSM559467     1  0.2972      0.829 0.836 0.000 0.000 0.036 0.000 0.128
#&gt; GSM559468     1  0.3986      0.754 0.748 0.000 0.012 0.036 0.000 0.204
#&gt; GSM559469     1  0.3957      0.756 0.752 0.000 0.012 0.036 0.000 0.200
#&gt; GSM559470     1  0.3247      0.810 0.808 0.000 0.000 0.036 0.000 0.156
#&gt; GSM559471     1  0.3358      0.841 0.824 0.000 0.008 0.052 0.000 0.116
#&gt; GSM559472     1  0.1267      0.876 0.940 0.000 0.000 0.060 0.000 0.000
#&gt; GSM559473     6  0.2389      0.728 0.000 0.128 0.000 0.008 0.000 0.864
#&gt; GSM559475     6  0.2389      0.728 0.000 0.128 0.000 0.008 0.000 0.864
#&gt; GSM559477     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559478     4  0.2980      1.000 0.000 0.192 0.000 0.800 0.000 0.008
#&gt; GSM559479     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559480     4  0.2980      1.000 0.000 0.192 0.000 0.800 0.000 0.008
#&gt; GSM559481     4  0.2980      1.000 0.000 0.192 0.000 0.800 0.000 0.008
#&gt; GSM559482     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.0000      0.961 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559439     3  0.1007      0.938 0.044 0.000 0.956 0.000 0.000 0.000
#&gt; GSM559443     3  0.1007      0.938 0.044 0.000 0.956 0.000 0.000 0.000
#&gt; GSM559447     3  0.0000      0.961 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559449     3  0.0000      0.961 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559453     3  0.0363      0.951 0.000 0.000 0.988 0.000 0.012 0.000
#&gt; GSM559466     3  0.0000      0.961 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559474     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559476     3  0.1820      0.912 0.056 0.000 0.924 0.008 0.000 0.012
#&gt; GSM559483     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559484     5  0.0000      1.000 0.000 0.000 0.000 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-5-a').click(function(){
  $('#tab-CV-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-hclust-signature_compare](figure_cola/CV-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-hclust-collect-classes](figure_cola/CV-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n disease.state(p) k
#> CV:hclust 52         2.10e-01 2
#> CV:hclust 52         9.08e-03 3
#> CV:hclust 52         4.65e-10 4
#> CV:hclust 51         2.54e-09 5
#> CV:hclust 51         6.96e-09 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:kmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "kmeans"]
# you can also extract it by
# res = res_list["CV:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-kmeans-collect-plots](figure_cola/CV-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-kmeans-select-partition-number](figure_cola/CV-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.470           0.902       0.901         0.4179 0.566   0.566
#> 3 3 0.631           0.867       0.901         0.4323 0.785   0.633
#> 4 4 0.676           0.683       0.841         0.1660 0.826   0.588
#> 5 5 0.684           0.717       0.838         0.0933 0.900   0.678
#> 6 6 0.727           0.754       0.833         0.0554 0.919   0.687
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-classes'>
<ul>
<li><a href='#tab-CV-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-kmeans-get-classes-1'>
<p><a id='tab-CV-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559433     1   0.000      0.923 1.000 0.000
#&gt; GSM559434     2   0.802      0.919 0.244 0.756
#&gt; GSM559436     1   0.000      0.923 1.000 0.000
#&gt; GSM559437     1   0.000      0.923 1.000 0.000
#&gt; GSM559438     2   0.802      0.919 0.244 0.756
#&gt; GSM559440     2   0.802      0.919 0.244 0.756
#&gt; GSM559441     1   0.327      0.869 0.940 0.060
#&gt; GSM559442     1   0.000      0.923 1.000 0.000
#&gt; GSM559444     2   0.689      0.962 0.184 0.816
#&gt; GSM559445     1   0.327      0.869 0.940 0.060
#&gt; GSM559446     1   0.000      0.923 1.000 0.000
#&gt; GSM559448     1   0.184      0.914 0.972 0.028
#&gt; GSM559450     2   0.689      0.962 0.184 0.816
#&gt; GSM559451     1   0.000      0.923 1.000 0.000
#&gt; GSM559452     2   0.861      0.867 0.284 0.716
#&gt; GSM559454     1   0.000      0.923 1.000 0.000
#&gt; GSM559455     1   0.000      0.923 1.000 0.000
#&gt; GSM559456     1   0.402      0.891 0.920 0.080
#&gt; GSM559457     1   0.000      0.923 1.000 0.000
#&gt; GSM559458     1   0.402      0.891 0.920 0.080
#&gt; GSM559459     1   0.000      0.923 1.000 0.000
#&gt; GSM559460     1   0.000      0.923 1.000 0.000
#&gt; GSM559461     1   0.000      0.923 1.000 0.000
#&gt; GSM559462     1   0.000      0.923 1.000 0.000
#&gt; GSM559463     1   0.000      0.923 1.000 0.000
#&gt; GSM559464     1   0.000      0.923 1.000 0.000
#&gt; GSM559465     1   0.260      0.908 0.956 0.044
#&gt; GSM559467     1   0.000      0.923 1.000 0.000
#&gt; GSM559468     1   0.000      0.923 1.000 0.000
#&gt; GSM559469     1   0.000      0.923 1.000 0.000
#&gt; GSM559470     1   0.518      0.798 0.884 0.116
#&gt; GSM559471     1   0.000      0.923 1.000 0.000
#&gt; GSM559472     1   0.000      0.923 1.000 0.000
#&gt; GSM559473     2   0.689      0.962 0.184 0.816
#&gt; GSM559475     2   0.689      0.962 0.184 0.816
#&gt; GSM559477     2   0.689      0.962 0.184 0.816
#&gt; GSM559478     2   0.689      0.962 0.184 0.816
#&gt; GSM559479     2   0.689      0.962 0.184 0.816
#&gt; GSM559480     2   0.689      0.962 0.184 0.816
#&gt; GSM559481     2   0.689      0.962 0.184 0.816
#&gt; GSM559482     2   0.689      0.962 0.184 0.816
#&gt; GSM559435     1   0.689      0.828 0.816 0.184
#&gt; GSM559439     1   0.689      0.828 0.816 0.184
#&gt; GSM559443     1   0.689      0.828 0.816 0.184
#&gt; GSM559447     1   0.689      0.828 0.816 0.184
#&gt; GSM559449     1   0.689      0.828 0.816 0.184
#&gt; GSM559453     1   0.689      0.828 0.816 0.184
#&gt; GSM559466     1   0.689      0.828 0.816 0.184
#&gt; GSM559474     1   0.891      0.726 0.692 0.308
#&gt; GSM559476     1   0.689      0.828 0.816 0.184
#&gt; GSM559483     2   0.689      0.962 0.184 0.816
#&gt; GSM559484     2   0.000      0.770 0.000 1.000
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-1-a').click(function(){
  $('#tab-CV-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-2'>
<p><a id='tab-CV-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559433     1  0.0237      0.932 0.996 0.000 0.004
#&gt; GSM559434     2  0.8303      0.636 0.196 0.632 0.172
#&gt; GSM559436     1  0.0000      0.933 1.000 0.000 0.000
#&gt; GSM559437     1  0.4062      0.832 0.836 0.000 0.164
#&gt; GSM559438     1  0.7165      0.691 0.716 0.112 0.172
#&gt; GSM559440     2  0.8303      0.636 0.196 0.632 0.172
#&gt; GSM559441     1  0.4178      0.826 0.828 0.000 0.172
#&gt; GSM559442     1  0.0000      0.933 1.000 0.000 0.000
#&gt; GSM559444     2  0.4062      0.807 0.000 0.836 0.164
#&gt; GSM559445     1  0.4346      0.813 0.816 0.000 0.184
#&gt; GSM559446     1  0.4235      0.822 0.824 0.000 0.176
#&gt; GSM559448     1  0.0000      0.933 1.000 0.000 0.000
#&gt; GSM559450     2  0.4002      0.808 0.000 0.840 0.160
#&gt; GSM559451     1  0.0000      0.933 1.000 0.000 0.000
#&gt; GSM559452     2  0.8683      0.577 0.236 0.592 0.172
#&gt; GSM559454     1  0.0000      0.933 1.000 0.000 0.000
#&gt; GSM559455     1  0.2796      0.884 0.908 0.000 0.092
#&gt; GSM559456     1  0.0592      0.929 0.988 0.000 0.012
#&gt; GSM559457     1  0.0424      0.931 0.992 0.000 0.008
#&gt; GSM559458     1  0.0424      0.931 0.992 0.000 0.008
#&gt; GSM559459     1  0.0000      0.933 1.000 0.000 0.000
#&gt; GSM559460     1  0.0000      0.933 1.000 0.000 0.000
#&gt; GSM559461     1  0.0000      0.933 1.000 0.000 0.000
#&gt; GSM559462     1  0.0237      0.932 0.996 0.000 0.004
#&gt; GSM559463     1  0.0000      0.933 1.000 0.000 0.000
#&gt; GSM559464     1  0.0000      0.933 1.000 0.000 0.000
#&gt; GSM559465     1  0.0000      0.933 1.000 0.000 0.000
#&gt; GSM559467     1  0.3816      0.845 0.852 0.000 0.148
#&gt; GSM559468     1  0.0000      0.933 1.000 0.000 0.000
#&gt; GSM559469     1  0.1163      0.921 0.972 0.000 0.028
#&gt; GSM559470     1  0.4178      0.826 0.828 0.000 0.172
#&gt; GSM559471     1  0.0000      0.933 1.000 0.000 0.000
#&gt; GSM559472     1  0.0000      0.933 1.000 0.000 0.000
#&gt; GSM559473     2  0.1860      0.848 0.000 0.948 0.052
#&gt; GSM559475     2  0.1860      0.848 0.000 0.948 0.052
#&gt; GSM559477     2  0.0237      0.852 0.000 0.996 0.004
#&gt; GSM559478     2  0.0000      0.853 0.000 1.000 0.000
#&gt; GSM559479     2  0.0237      0.852 0.000 0.996 0.004
#&gt; GSM559480     2  0.0000      0.853 0.000 1.000 0.000
#&gt; GSM559481     2  0.0000      0.853 0.000 1.000 0.000
#&gt; GSM559482     2  0.0237      0.852 0.000 0.996 0.004
#&gt; GSM559435     3  0.4654      0.960 0.208 0.000 0.792
#&gt; GSM559439     3  0.4654      0.960 0.208 0.000 0.792
#&gt; GSM559443     3  0.4654      0.960 0.208 0.000 0.792
#&gt; GSM559447     3  0.4654      0.960 0.208 0.000 0.792
#&gt; GSM559449     3  0.4654      0.960 0.208 0.000 0.792
#&gt; GSM559453     3  0.4399      0.941 0.188 0.000 0.812
#&gt; GSM559466     3  0.4654      0.960 0.208 0.000 0.792
#&gt; GSM559474     3  0.0237      0.679 0.000 0.004 0.996
#&gt; GSM559476     3  0.4702      0.956 0.212 0.000 0.788
#&gt; GSM559483     2  0.0237      0.852 0.000 0.996 0.004
#&gt; GSM559484     2  0.6291      0.458 0.000 0.532 0.468
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-2-a').click(function(){
  $('#tab-CV-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-3'>
<p><a id='tab-CV-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559433     1  0.2345    0.80506 0.900 0.000 0.000 0.100
#&gt; GSM559434     4  0.4724    0.50650 0.076 0.136 0.000 0.788
#&gt; GSM559436     1  0.0188    0.86666 0.996 0.000 0.000 0.004
#&gt; GSM559437     4  0.4933    0.24978 0.432 0.000 0.000 0.568
#&gt; GSM559438     4  0.5113    0.54895 0.252 0.036 0.000 0.712
#&gt; GSM559440     4  0.4724    0.50650 0.076 0.136 0.000 0.788
#&gt; GSM559441     4  0.4877    0.31952 0.408 0.000 0.000 0.592
#&gt; GSM559442     1  0.2921    0.75473 0.860 0.000 0.000 0.140
#&gt; GSM559444     4  0.4967   -0.25643 0.000 0.452 0.000 0.548
#&gt; GSM559445     4  0.3801    0.55882 0.220 0.000 0.000 0.780
#&gt; GSM559446     4  0.4855    0.32776 0.400 0.000 0.000 0.600
#&gt; GSM559448     1  0.0000    0.86660 1.000 0.000 0.000 0.000
#&gt; GSM559450     2  0.4981    0.34842 0.000 0.536 0.000 0.464
#&gt; GSM559451     1  0.1389    0.84657 0.952 0.000 0.000 0.048
#&gt; GSM559452     4  0.4784    0.54697 0.112 0.100 0.000 0.788
#&gt; GSM559454     1  0.0188    0.86666 0.996 0.000 0.000 0.004
#&gt; GSM559455     1  0.4989    0.00461 0.528 0.000 0.000 0.472
#&gt; GSM559456     1  0.3975    0.62267 0.760 0.000 0.000 0.240
#&gt; GSM559457     1  0.2281    0.80864 0.904 0.000 0.000 0.096
#&gt; GSM559458     1  0.4866    0.24447 0.596 0.000 0.000 0.404
#&gt; GSM559459     1  0.0188    0.86666 0.996 0.000 0.000 0.004
#&gt; GSM559460     1  0.0000    0.86660 1.000 0.000 0.000 0.000
#&gt; GSM559461     1  0.0000    0.86660 1.000 0.000 0.000 0.000
#&gt; GSM559462     1  0.0336    0.86590 0.992 0.000 0.000 0.008
#&gt; GSM559463     1  0.0000    0.86660 1.000 0.000 0.000 0.000
#&gt; GSM559464     1  0.0000    0.86660 1.000 0.000 0.000 0.000
#&gt; GSM559465     1  0.0000    0.86660 1.000 0.000 0.000 0.000
#&gt; GSM559467     1  0.4866    0.25613 0.596 0.000 0.000 0.404
#&gt; GSM559468     1  0.0188    0.86598 0.996 0.000 0.000 0.004
#&gt; GSM559469     1  0.2973    0.74892 0.856 0.000 0.000 0.144
#&gt; GSM559470     4  0.4941    0.25125 0.436 0.000 0.000 0.564
#&gt; GSM559471     1  0.0188    0.86598 0.996 0.000 0.000 0.004
#&gt; GSM559472     1  0.0188    0.86666 0.996 0.000 0.000 0.004
#&gt; GSM559473     2  0.3486    0.80593 0.000 0.812 0.000 0.188
#&gt; GSM559475     2  0.3486    0.80593 0.000 0.812 0.000 0.188
#&gt; GSM559477     2  0.0188    0.88230 0.000 0.996 0.004 0.000
#&gt; GSM559478     2  0.2142    0.88335 0.000 0.928 0.016 0.056
#&gt; GSM559479     2  0.0188    0.88230 0.000 0.996 0.004 0.000
#&gt; GSM559480     2  0.2142    0.88335 0.000 0.928 0.016 0.056
#&gt; GSM559481     2  0.2142    0.88335 0.000 0.928 0.016 0.056
#&gt; GSM559482     2  0.0188    0.88230 0.000 0.996 0.004 0.000
#&gt; GSM559435     3  0.1004    0.99687 0.024 0.000 0.972 0.004
#&gt; GSM559439     3  0.1004    0.99687 0.024 0.000 0.972 0.004
#&gt; GSM559443     3  0.0921    0.99292 0.028 0.000 0.972 0.000
#&gt; GSM559447     3  0.1004    0.99687 0.024 0.000 0.972 0.004
#&gt; GSM559449     3  0.1004    0.99687 0.024 0.000 0.972 0.004
#&gt; GSM559453     3  0.0707    0.98992 0.020 0.000 0.980 0.000
#&gt; GSM559466     3  0.1004    0.99687 0.024 0.000 0.972 0.004
#&gt; GSM559474     4  0.4989   -0.31382 0.000 0.000 0.472 0.528
#&gt; GSM559476     3  0.1109    0.99265 0.028 0.000 0.968 0.004
#&gt; GSM559483     2  0.0188    0.88230 0.000 0.996 0.004 0.000
#&gt; GSM559484     4  0.7698   -0.16217 0.000 0.236 0.324 0.440
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-3-a').click(function(){
  $('#tab-CV-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-4'>
<p><a id='tab-CV-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559433     1  0.3551     0.6654 0.772 0.000 0.000 0.220 0.008
#&gt; GSM559434     4  0.4347     0.4726 0.012 0.012 0.000 0.712 0.264
#&gt; GSM559436     1  0.1356     0.8814 0.956 0.000 0.004 0.012 0.028
#&gt; GSM559437     4  0.3163     0.6280 0.164 0.000 0.000 0.824 0.012
#&gt; GSM559438     4  0.4757     0.4928 0.036 0.012 0.000 0.704 0.248
#&gt; GSM559440     4  0.4296     0.4785 0.012 0.012 0.000 0.720 0.256
#&gt; GSM559441     4  0.2462     0.6355 0.112 0.000 0.000 0.880 0.008
#&gt; GSM559442     1  0.4879     0.6742 0.720 0.000 0.000 0.124 0.156
#&gt; GSM559444     4  0.6823    -0.2483 0.000 0.336 0.000 0.344 0.320
#&gt; GSM559445     4  0.1661     0.5807 0.036 0.000 0.000 0.940 0.024
#&gt; GSM559446     4  0.3123     0.6288 0.160 0.000 0.000 0.828 0.012
#&gt; GSM559448     1  0.1356     0.8814 0.956 0.000 0.004 0.012 0.028
#&gt; GSM559450     2  0.6788     0.0925 0.000 0.384 0.000 0.296 0.320
#&gt; GSM559451     1  0.2470     0.8341 0.884 0.000 0.000 0.104 0.012
#&gt; GSM559452     4  0.5028     0.2935 0.012 0.016 0.000 0.560 0.412
#&gt; GSM559454     1  0.0162     0.8899 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559455     4  0.3461     0.6147 0.224 0.000 0.000 0.772 0.004
#&gt; GSM559456     4  0.4648     0.1588 0.464 0.000 0.000 0.524 0.012
#&gt; GSM559457     1  0.3885     0.5780 0.724 0.000 0.000 0.268 0.008
#&gt; GSM559458     4  0.5224     0.5206 0.276 0.000 0.000 0.644 0.080
#&gt; GSM559459     1  0.0000     0.8904 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559460     1  0.0703     0.8896 0.976 0.000 0.000 0.000 0.024
#&gt; GSM559461     1  0.0566     0.8907 0.984 0.000 0.000 0.004 0.012
#&gt; GSM559462     1  0.0566     0.8898 0.984 0.000 0.000 0.004 0.012
#&gt; GSM559463     1  0.1285     0.8833 0.956 0.000 0.004 0.004 0.036
#&gt; GSM559464     1  0.0609     0.8901 0.980 0.000 0.000 0.000 0.020
#&gt; GSM559465     1  0.0771     0.8904 0.976 0.000 0.000 0.004 0.020
#&gt; GSM559467     4  0.4594     0.5684 0.284 0.000 0.000 0.680 0.036
#&gt; GSM559468     1  0.3229     0.8177 0.840 0.000 0.000 0.032 0.128
#&gt; GSM559469     1  0.4723     0.6880 0.736 0.000 0.000 0.136 0.128
#&gt; GSM559470     4  0.3386     0.6381 0.128 0.000 0.000 0.832 0.040
#&gt; GSM559471     1  0.2676     0.8465 0.884 0.000 0.000 0.036 0.080
#&gt; GSM559472     1  0.0807     0.8880 0.976 0.000 0.000 0.012 0.012
#&gt; GSM559473     2  0.5253     0.6356 0.000 0.676 0.000 0.124 0.200
#&gt; GSM559475     2  0.5227     0.6401 0.000 0.676 0.000 0.116 0.208
#&gt; GSM559477     2  0.0000     0.7747 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559478     2  0.3488     0.7614 0.000 0.808 0.000 0.024 0.168
#&gt; GSM559479     2  0.0000     0.7747 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.3488     0.7614 0.000 0.808 0.000 0.024 0.168
#&gt; GSM559481     2  0.3488     0.7614 0.000 0.808 0.000 0.024 0.168
#&gt; GSM559482     2  0.0000     0.7747 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.0404     0.9824 0.012 0.000 0.988 0.000 0.000
#&gt; GSM559439     3  0.0404     0.9824 0.012 0.000 0.988 0.000 0.000
#&gt; GSM559443     3  0.0807     0.9731 0.012 0.000 0.976 0.000 0.012
#&gt; GSM559447     3  0.0566     0.9825 0.012 0.000 0.984 0.000 0.004
#&gt; GSM559449     3  0.0566     0.9825 0.012 0.000 0.984 0.000 0.004
#&gt; GSM559453     3  0.0162     0.9643 0.000 0.000 0.996 0.000 0.004
#&gt; GSM559466     3  0.0566     0.9825 0.012 0.000 0.984 0.000 0.004
#&gt; GSM559474     5  0.6279     0.6878 0.000 0.000 0.280 0.192 0.528
#&gt; GSM559476     3  0.1493     0.9377 0.028 0.000 0.948 0.000 0.024
#&gt; GSM559483     2  0.0000     0.7747 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559484     5  0.6048     0.7094 0.000 0.092 0.220 0.044 0.644
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-4-a').click(function(){
  $('#tab-CV-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-5'>
<p><a id='tab-CV-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559433     1  0.4105      0.594 0.720 0.000 0.000 0.236 0.036 0.008
#&gt; GSM559434     6  0.3337      0.641 0.000 0.000 0.000 0.260 0.004 0.736
#&gt; GSM559436     1  0.2220      0.819 0.908 0.000 0.000 0.020 0.052 0.020
#&gt; GSM559437     4  0.1873      0.773 0.048 0.000 0.000 0.924 0.020 0.008
#&gt; GSM559438     6  0.4094      0.613 0.004 0.000 0.000 0.264 0.032 0.700
#&gt; GSM559440     6  0.3383      0.636 0.004 0.000 0.000 0.268 0.000 0.728
#&gt; GSM559441     4  0.1930      0.774 0.048 0.000 0.000 0.916 0.000 0.036
#&gt; GSM559442     1  0.5955      0.576 0.576 0.000 0.000 0.052 0.112 0.260
#&gt; GSM559444     6  0.5656      0.624 0.000 0.172 0.000 0.136 0.052 0.640
#&gt; GSM559445     4  0.1594      0.710 0.000 0.000 0.000 0.932 0.016 0.052
#&gt; GSM559446     4  0.1718      0.771 0.044 0.000 0.000 0.932 0.016 0.008
#&gt; GSM559448     1  0.2279      0.820 0.904 0.000 0.000 0.024 0.056 0.016
#&gt; GSM559450     6  0.5642      0.612 0.000 0.196 0.000 0.116 0.052 0.636
#&gt; GSM559451     1  0.3815      0.745 0.792 0.000 0.000 0.136 0.056 0.016
#&gt; GSM559452     6  0.3963      0.596 0.000 0.000 0.000 0.164 0.080 0.756
#&gt; GSM559454     1  0.1194      0.833 0.956 0.000 0.000 0.008 0.032 0.004
#&gt; GSM559455     4  0.2170      0.783 0.100 0.000 0.000 0.888 0.000 0.012
#&gt; GSM559456     4  0.3516      0.689 0.220 0.000 0.000 0.760 0.016 0.004
#&gt; GSM559457     4  0.4596      0.214 0.460 0.000 0.000 0.508 0.028 0.004
#&gt; GSM559458     4  0.4691      0.707 0.112 0.000 0.000 0.744 0.056 0.088
#&gt; GSM559459     1  0.0291      0.840 0.992 0.000 0.000 0.000 0.004 0.004
#&gt; GSM559460     1  0.1765      0.837 0.924 0.000 0.000 0.000 0.052 0.024
#&gt; GSM559461     1  0.1464      0.842 0.944 0.000 0.000 0.004 0.036 0.016
#&gt; GSM559462     1  0.1408      0.842 0.944 0.000 0.000 0.000 0.036 0.020
#&gt; GSM559463     1  0.1914      0.828 0.920 0.000 0.000 0.008 0.056 0.016
#&gt; GSM559464     1  0.1765      0.837 0.924 0.000 0.000 0.000 0.052 0.024
#&gt; GSM559465     1  0.1461      0.840 0.940 0.000 0.000 0.000 0.044 0.016
#&gt; GSM559467     4  0.4996      0.712 0.128 0.000 0.000 0.708 0.040 0.124
#&gt; GSM559468     1  0.5332      0.668 0.652 0.000 0.000 0.028 0.120 0.200
#&gt; GSM559469     1  0.5730      0.633 0.624 0.000 0.000 0.052 0.120 0.204
#&gt; GSM559470     4  0.4198      0.698 0.056 0.000 0.000 0.772 0.036 0.136
#&gt; GSM559471     1  0.4597      0.728 0.732 0.000 0.000 0.028 0.080 0.160
#&gt; GSM559472     1  0.1794      0.829 0.924 0.000 0.000 0.040 0.036 0.000
#&gt; GSM559473     6  0.4528      0.190 0.000 0.404 0.000 0.004 0.028 0.564
#&gt; GSM559475     6  0.4549      0.155 0.000 0.416 0.000 0.004 0.028 0.552
#&gt; GSM559477     2  0.0146      0.843 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM559478     2  0.4985      0.776 0.000 0.696 0.000 0.032 0.096 0.176
#&gt; GSM559479     2  0.0146      0.843 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM559480     2  0.4985      0.776 0.000 0.696 0.000 0.032 0.096 0.176
#&gt; GSM559481     2  0.4985      0.776 0.000 0.696 0.000 0.032 0.096 0.176
#&gt; GSM559482     2  0.0146      0.843 0.000 0.996 0.000 0.000 0.000 0.004
#&gt; GSM559435     3  0.0000      0.987 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559439     3  0.0000      0.987 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559443     3  0.0146      0.984 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; GSM559447     3  0.0000      0.987 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559449     3  0.0000      0.987 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559453     3  0.0458      0.977 0.000 0.000 0.984 0.000 0.000 0.016
#&gt; GSM559466     3  0.0000      0.987 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559474     5  0.4923      0.850 0.000 0.000 0.116 0.116 0.720 0.048
#&gt; GSM559476     3  0.1498      0.931 0.012 0.000 0.948 0.004 0.012 0.024
#&gt; GSM559483     2  0.0000      0.843 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559484     5  0.4947      0.841 0.000 0.016 0.112 0.024 0.728 0.120
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-5-a').click(function(){
  $('#tab-CV-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-kmeans-signature_compare](figure_cola/CV-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-kmeans-collect-classes](figure_cola/CV-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n disease.state(p) k
#> CV:kmeans 52         5.15e-01 2
#> CV:kmeans 51         1.60e-10 3
#> CV:kmeans 41         8.21e-08 4
#> CV:kmeans 45         3.96e-08 5
#> CV:kmeans 49         2.23e-08 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "skmeans"]
# you can also extract it by
# res = res_list["CV:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-skmeans-collect-plots](figure_cola/CV-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-skmeans-select-partition-number](figure_cola/CV-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.959           0.949       0.979         0.4926 0.509   0.509
#> 3 3 0.969           0.903       0.962         0.3243 0.733   0.527
#> 4 4 0.964           0.936       0.973         0.1529 0.885   0.680
#> 5 5 0.804           0.736       0.878         0.0548 0.941   0.775
#> 6 6 0.768           0.615       0.808         0.0389 0.965   0.838
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-classes'>
<ul>
<li><a href='#tab-CV-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-skmeans-get-classes-1'>
<p><a id='tab-CV-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559433     1   0.000      0.977 1.000 0.000
#&gt; GSM559434     2   0.000      0.979 0.000 1.000
#&gt; GSM559436     1   0.000      0.977 1.000 0.000
#&gt; GSM559437     1   0.295      0.927 0.948 0.052
#&gt; GSM559438     2   0.000      0.979 0.000 1.000
#&gt; GSM559440     2   0.000      0.979 0.000 1.000
#&gt; GSM559441     2   0.844      0.624 0.272 0.728
#&gt; GSM559442     1   0.000      0.977 1.000 0.000
#&gt; GSM559444     2   0.000      0.979 0.000 1.000
#&gt; GSM559445     2   0.000      0.979 0.000 1.000
#&gt; GSM559446     1   0.738      0.728 0.792 0.208
#&gt; GSM559448     1   0.000      0.977 1.000 0.000
#&gt; GSM559450     2   0.000      0.979 0.000 1.000
#&gt; GSM559451     1   0.000      0.977 1.000 0.000
#&gt; GSM559452     2   0.000      0.979 0.000 1.000
#&gt; GSM559454     1   0.000      0.977 1.000 0.000
#&gt; GSM559455     1   0.000      0.977 1.000 0.000
#&gt; GSM559456     1   0.000      0.977 1.000 0.000
#&gt; GSM559457     1   0.000      0.977 1.000 0.000
#&gt; GSM559458     1   0.000      0.977 1.000 0.000
#&gt; GSM559459     1   0.000      0.977 1.000 0.000
#&gt; GSM559460     1   0.000      0.977 1.000 0.000
#&gt; GSM559461     1   0.000      0.977 1.000 0.000
#&gt; GSM559462     1   0.000      0.977 1.000 0.000
#&gt; GSM559463     1   0.000      0.977 1.000 0.000
#&gt; GSM559464     1   0.000      0.977 1.000 0.000
#&gt; GSM559465     1   0.000      0.977 1.000 0.000
#&gt; GSM559467     2   0.574      0.836 0.136 0.864
#&gt; GSM559468     1   0.000      0.977 1.000 0.000
#&gt; GSM559469     1   0.975      0.292 0.592 0.408
#&gt; GSM559470     2   0.000      0.979 0.000 1.000
#&gt; GSM559471     1   0.000      0.977 1.000 0.000
#&gt; GSM559472     1   0.000      0.977 1.000 0.000
#&gt; GSM559473     2   0.000      0.979 0.000 1.000
#&gt; GSM559475     2   0.000      0.979 0.000 1.000
#&gt; GSM559477     2   0.000      0.979 0.000 1.000
#&gt; GSM559478     2   0.000      0.979 0.000 1.000
#&gt; GSM559479     2   0.000      0.979 0.000 1.000
#&gt; GSM559480     2   0.000      0.979 0.000 1.000
#&gt; GSM559481     2   0.000      0.979 0.000 1.000
#&gt; GSM559482     2   0.000      0.979 0.000 1.000
#&gt; GSM559435     1   0.000      0.977 1.000 0.000
#&gt; GSM559439     1   0.000      0.977 1.000 0.000
#&gt; GSM559443     1   0.000      0.977 1.000 0.000
#&gt; GSM559447     1   0.000      0.977 1.000 0.000
#&gt; GSM559449     1   0.000      0.977 1.000 0.000
#&gt; GSM559453     1   0.000      0.977 1.000 0.000
#&gt; GSM559466     1   0.000      0.977 1.000 0.000
#&gt; GSM559474     2   0.000      0.979 0.000 1.000
#&gt; GSM559476     1   0.000      0.977 1.000 0.000
#&gt; GSM559483     2   0.000      0.979 0.000 1.000
#&gt; GSM559484     2   0.000      0.979 0.000 1.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-1-a').click(function(){
  $('#tab-CV-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-2'>
<p><a id='tab-CV-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559433     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559434     2  0.0000      0.940 0.000 1.000 0.000
#&gt; GSM559436     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559437     1  0.2165      0.907 0.936 0.000 0.064
#&gt; GSM559438     2  0.0000      0.940 0.000 1.000 0.000
#&gt; GSM559440     2  0.0000      0.940 0.000 1.000 0.000
#&gt; GSM559441     1  0.6667      0.400 0.616 0.368 0.016
#&gt; GSM559442     1  0.0892      0.937 0.980 0.020 0.000
#&gt; GSM559444     2  0.0000      0.940 0.000 1.000 0.000
#&gt; GSM559445     2  0.8010      0.205 0.384 0.548 0.068
#&gt; GSM559446     1  0.2066      0.910 0.940 0.000 0.060
#&gt; GSM559448     3  0.2261      0.923 0.068 0.000 0.932
#&gt; GSM559450     2  0.0000      0.940 0.000 1.000 0.000
#&gt; GSM559451     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559452     2  0.1860      0.894 0.000 0.948 0.052
#&gt; GSM559454     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559455     1  0.0892      0.940 0.980 0.000 0.020
#&gt; GSM559456     1  0.2165      0.907 0.936 0.000 0.064
#&gt; GSM559457     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559458     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM559459     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559460     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559461     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559462     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559463     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559464     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559465     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559467     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559468     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559469     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559470     1  0.6204      0.271 0.576 0.424 0.000
#&gt; GSM559471     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559472     1  0.0000      0.952 1.000 0.000 0.000
#&gt; GSM559473     2  0.0000      0.940 0.000 1.000 0.000
#&gt; GSM559475     2  0.0000      0.940 0.000 1.000 0.000
#&gt; GSM559477     2  0.0000      0.940 0.000 1.000 0.000
#&gt; GSM559478     2  0.0000      0.940 0.000 1.000 0.000
#&gt; GSM559479     2  0.0000      0.940 0.000 1.000 0.000
#&gt; GSM559480     2  0.0000      0.940 0.000 1.000 0.000
#&gt; GSM559481     2  0.0000      0.940 0.000 1.000 0.000
#&gt; GSM559482     2  0.0000      0.940 0.000 1.000 0.000
#&gt; GSM559435     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM559439     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM559443     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM559447     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM559449     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM559453     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM559466     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM559474     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM559476     3  0.0000      0.992 0.000 0.000 1.000
#&gt; GSM559483     2  0.0000      0.940 0.000 1.000 0.000
#&gt; GSM559484     2  0.5968      0.395 0.000 0.636 0.364
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-2-a').click(function(){
  $('#tab-CV-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-3'>
<p><a id='tab-CV-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559433     1  0.4933      0.224 0.568 0.000 0.000 0.432
#&gt; GSM559434     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559436     1  0.0336      0.959 0.992 0.000 0.000 0.008
#&gt; GSM559437     4  0.0000      0.967 0.000 0.000 0.000 1.000
#&gt; GSM559438     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559440     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559441     4  0.0000      0.967 0.000 0.000 0.000 1.000
#&gt; GSM559442     1  0.0000      0.960 1.000 0.000 0.000 0.000
#&gt; GSM559444     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559445     4  0.0000      0.967 0.000 0.000 0.000 1.000
#&gt; GSM559446     4  0.0000      0.967 0.000 0.000 0.000 1.000
#&gt; GSM559448     3  0.2647      0.850 0.120 0.000 0.880 0.000
#&gt; GSM559450     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559451     1  0.2216      0.879 0.908 0.000 0.000 0.092
#&gt; GSM559452     2  0.1151      0.957 0.008 0.968 0.024 0.000
#&gt; GSM559454     1  0.0336      0.959 0.992 0.000 0.000 0.008
#&gt; GSM559455     4  0.0000      0.967 0.000 0.000 0.000 1.000
#&gt; GSM559456     4  0.0000      0.967 0.000 0.000 0.000 1.000
#&gt; GSM559457     4  0.3649      0.723 0.204 0.000 0.000 0.796
#&gt; GSM559458     3  0.2342      0.899 0.008 0.000 0.912 0.080
#&gt; GSM559459     1  0.0336      0.959 0.992 0.000 0.000 0.008
#&gt; GSM559460     1  0.0000      0.960 1.000 0.000 0.000 0.000
#&gt; GSM559461     1  0.0188      0.960 0.996 0.000 0.000 0.004
#&gt; GSM559462     1  0.0336      0.959 0.992 0.000 0.000 0.008
#&gt; GSM559463     1  0.0000      0.960 1.000 0.000 0.000 0.000
#&gt; GSM559464     1  0.0000      0.960 1.000 0.000 0.000 0.000
#&gt; GSM559465     1  0.0000      0.960 1.000 0.000 0.000 0.000
#&gt; GSM559467     4  0.0592      0.955 0.016 0.000 0.000 0.984
#&gt; GSM559468     1  0.0000      0.960 1.000 0.000 0.000 0.000
#&gt; GSM559469     1  0.0000      0.960 1.000 0.000 0.000 0.000
#&gt; GSM559470     4  0.0188      0.963 0.000 0.004 0.000 0.996
#&gt; GSM559471     1  0.0188      0.960 0.996 0.000 0.000 0.004
#&gt; GSM559472     1  0.0336      0.959 0.992 0.000 0.000 0.008
#&gt; GSM559473     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559475     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559477     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559478     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559479     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559480     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559481     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559482     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559435     3  0.0000      0.963 0.000 0.000 1.000 0.000
#&gt; GSM559439     3  0.0000      0.963 0.000 0.000 1.000 0.000
#&gt; GSM559443     3  0.0000      0.963 0.000 0.000 1.000 0.000
#&gt; GSM559447     3  0.0000      0.963 0.000 0.000 1.000 0.000
#&gt; GSM559449     3  0.0000      0.963 0.000 0.000 1.000 0.000
#&gt; GSM559453     3  0.0000      0.963 0.000 0.000 1.000 0.000
#&gt; GSM559466     3  0.0000      0.963 0.000 0.000 1.000 0.000
#&gt; GSM559474     3  0.2868      0.847 0.000 0.000 0.864 0.136
#&gt; GSM559476     3  0.0000      0.963 0.000 0.000 1.000 0.000
#&gt; GSM559483     2  0.0000      0.982 0.000 1.000 0.000 0.000
#&gt; GSM559484     2  0.3907      0.703 0.000 0.768 0.232 0.000
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-3-a').click(function(){
  $('#tab-CV-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-4'>
<p><a id='tab-CV-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559433     1  0.3954      0.597 0.772 0.000 0.000 0.192 0.036
#&gt; GSM559434     2  0.3196      0.782 0.000 0.804 0.000 0.004 0.192
#&gt; GSM559436     1  0.0510      0.790 0.984 0.000 0.000 0.000 0.016
#&gt; GSM559437     4  0.0162      0.923 0.004 0.000 0.000 0.996 0.000
#&gt; GSM559438     2  0.3715      0.687 0.000 0.736 0.000 0.004 0.260
#&gt; GSM559440     2  0.3266      0.770 0.000 0.796 0.000 0.004 0.200
#&gt; GSM559441     4  0.0162      0.922 0.000 0.000 0.000 0.996 0.004
#&gt; GSM559442     5  0.2891      0.547 0.176 0.000 0.000 0.000 0.824
#&gt; GSM559444     2  0.1638      0.874 0.000 0.932 0.000 0.004 0.064
#&gt; GSM559445     4  0.0162      0.922 0.000 0.000 0.000 0.996 0.004
#&gt; GSM559446     4  0.0451      0.922 0.004 0.000 0.000 0.988 0.008
#&gt; GSM559448     3  0.4627      0.185 0.444 0.000 0.544 0.000 0.012
#&gt; GSM559450     2  0.1430      0.880 0.000 0.944 0.000 0.004 0.052
#&gt; GSM559451     1  0.2514      0.758 0.896 0.000 0.000 0.044 0.060
#&gt; GSM559452     5  0.4410     -0.252 0.000 0.440 0.000 0.004 0.556
#&gt; GSM559454     1  0.0404      0.794 0.988 0.000 0.000 0.000 0.012
#&gt; GSM559455     4  0.0807      0.919 0.012 0.000 0.000 0.976 0.012
#&gt; GSM559456     4  0.2228      0.881 0.056 0.000 0.008 0.916 0.020
#&gt; GSM559457     1  0.4798      0.253 0.580 0.000 0.000 0.396 0.024
#&gt; GSM559458     3  0.6131      0.391 0.000 0.000 0.536 0.156 0.308
#&gt; GSM559459     1  0.1270      0.799 0.948 0.000 0.000 0.000 0.052
#&gt; GSM559460     1  0.2813      0.749 0.832 0.000 0.000 0.000 0.168
#&gt; GSM559461     1  0.2280      0.778 0.880 0.000 0.000 0.000 0.120
#&gt; GSM559462     1  0.2305      0.786 0.896 0.000 0.000 0.012 0.092
#&gt; GSM559463     1  0.1121      0.798 0.956 0.000 0.000 0.000 0.044
#&gt; GSM559464     1  0.2773      0.751 0.836 0.000 0.000 0.000 0.164
#&gt; GSM559465     1  0.2848      0.756 0.840 0.000 0.004 0.000 0.156
#&gt; GSM559467     4  0.4593      0.732 0.080 0.000 0.000 0.736 0.184
#&gt; GSM559468     5  0.4088      0.316 0.368 0.000 0.000 0.000 0.632
#&gt; GSM559469     5  0.3635      0.519 0.248 0.004 0.000 0.000 0.748
#&gt; GSM559470     4  0.3694      0.822 0.024 0.020 0.000 0.824 0.132
#&gt; GSM559471     1  0.4262      0.146 0.560 0.000 0.000 0.000 0.440
#&gt; GSM559472     1  0.0609      0.791 0.980 0.000 0.000 0.000 0.020
#&gt; GSM559473     2  0.0000      0.903 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559475     2  0.0000      0.903 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559477     2  0.0000      0.903 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559478     2  0.0000      0.903 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559479     2  0.0000      0.903 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.0000      0.903 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559481     2  0.0000      0.903 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559482     2  0.0000      0.903 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.0000      0.869 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559439     3  0.0000      0.869 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559443     3  0.0000      0.869 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559447     3  0.0000      0.869 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559449     3  0.0000      0.869 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559453     3  0.0000      0.869 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559466     3  0.0000      0.869 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559474     3  0.5264      0.600 0.000 0.000 0.676 0.196 0.128
#&gt; GSM559476     3  0.0000      0.869 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559483     2  0.0000      0.903 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559484     2  0.5977      0.279 0.000 0.540 0.332 0.000 0.128
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-4-a').click(function(){
  $('#tab-CV-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-5'>
<p><a id='tab-CV-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559433     1  0.5821      0.474 0.632 0.000 0.000 0.176 0.080 0.112
#&gt; GSM559434     2  0.4639     -0.430 0.000 0.512 0.000 0.000 0.040 0.448
#&gt; GSM559436     1  0.1723      0.725 0.928 0.000 0.000 0.000 0.036 0.036
#&gt; GSM559437     4  0.0458      0.797 0.000 0.000 0.000 0.984 0.000 0.016
#&gt; GSM559438     6  0.5421      0.480 0.000 0.432 0.000 0.004 0.100 0.464
#&gt; GSM559440     6  0.5108      0.469 0.000 0.432 0.000 0.004 0.068 0.496
#&gt; GSM559441     4  0.1572      0.792 0.000 0.000 0.000 0.936 0.028 0.036
#&gt; GSM559442     5  0.4494      0.518 0.092 0.000 0.000 0.000 0.692 0.216
#&gt; GSM559444     2  0.3266      0.365 0.000 0.728 0.000 0.000 0.000 0.272
#&gt; GSM559445     4  0.1391      0.791 0.000 0.000 0.000 0.944 0.016 0.040
#&gt; GSM559446     4  0.1528      0.791 0.000 0.000 0.000 0.936 0.016 0.048
#&gt; GSM559448     3  0.4802      0.340 0.368 0.000 0.584 0.000 0.028 0.020
#&gt; GSM559450     2  0.3221      0.386 0.000 0.736 0.000 0.000 0.000 0.264
#&gt; GSM559451     1  0.4666      0.603 0.736 0.000 0.000 0.036 0.092 0.136
#&gt; GSM559452     6  0.5365      0.273 0.000 0.164 0.000 0.000 0.256 0.580
#&gt; GSM559454     1  0.0909      0.744 0.968 0.000 0.000 0.000 0.020 0.012
#&gt; GSM559455     4  0.2745      0.771 0.012 0.000 0.000 0.876 0.056 0.056
#&gt; GSM559456     4  0.4611      0.682 0.100 0.000 0.008 0.764 0.060 0.068
#&gt; GSM559457     1  0.6253      0.278 0.512 0.000 0.000 0.320 0.068 0.100
#&gt; GSM559458     3  0.7131      0.208 0.004 0.000 0.448 0.256 0.192 0.100
#&gt; GSM559459     1  0.1563      0.742 0.932 0.000 0.000 0.000 0.056 0.012
#&gt; GSM559460     1  0.3403      0.624 0.768 0.000 0.000 0.000 0.212 0.020
#&gt; GSM559461     1  0.2491      0.720 0.868 0.000 0.000 0.000 0.112 0.020
#&gt; GSM559462     1  0.4210      0.630 0.756 0.000 0.000 0.008 0.120 0.116
#&gt; GSM559463     1  0.1049      0.744 0.960 0.000 0.000 0.000 0.032 0.008
#&gt; GSM559464     1  0.3136      0.657 0.796 0.000 0.000 0.000 0.188 0.016
#&gt; GSM559465     1  0.2872      0.693 0.832 0.000 0.000 0.004 0.152 0.012
#&gt; GSM559467     4  0.6712      0.302 0.084 0.000 0.000 0.456 0.324 0.136
#&gt; GSM559468     5  0.3126      0.690 0.248 0.000 0.000 0.000 0.752 0.000
#&gt; GSM559469     5  0.2744      0.717 0.144 0.000 0.000 0.000 0.840 0.016
#&gt; GSM559470     4  0.5992      0.530 0.020 0.008 0.000 0.572 0.236 0.164
#&gt; GSM559471     5  0.5001      0.384 0.384 0.000 0.000 0.000 0.540 0.076
#&gt; GSM559472     1  0.2265      0.717 0.896 0.000 0.000 0.000 0.052 0.052
#&gt; GSM559473     2  0.0458      0.766 0.000 0.984 0.000 0.000 0.000 0.016
#&gt; GSM559475     2  0.0520      0.772 0.000 0.984 0.000 0.000 0.008 0.008
#&gt; GSM559477     2  0.0000      0.776 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559478     2  0.0291      0.775 0.000 0.992 0.000 0.000 0.004 0.004
#&gt; GSM559479     2  0.0000      0.776 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.0291      0.775 0.000 0.992 0.000 0.000 0.004 0.004
#&gt; GSM559481     2  0.0291      0.775 0.000 0.992 0.000 0.000 0.004 0.004
#&gt; GSM559482     2  0.0000      0.776 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.0146      0.842 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; GSM559439     3  0.0000      0.842 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559443     3  0.0146      0.842 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; GSM559447     3  0.0000      0.842 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559449     3  0.0000      0.842 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559453     3  0.0260      0.839 0.000 0.000 0.992 0.000 0.008 0.000
#&gt; GSM559466     3  0.0000      0.842 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559474     3  0.6589      0.214 0.000 0.000 0.420 0.216 0.036 0.328
#&gt; GSM559476     3  0.0146      0.842 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; GSM559483     2  0.0146      0.776 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559484     2  0.6559     -0.164 0.000 0.428 0.208 0.000 0.036 0.328
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-5-a').click(function(){
  $('#tab-CV-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-skmeans-signature_compare](figure_cola/CV-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-skmeans-collect-classes](figure_cola/CV-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) k
#> CV:skmeans 51         4.76e-01 2
#> CV:skmeans 48         9.12e-08 3
#> CV:skmeans 51         8.58e-07 4
#> CV:skmeans 45         5.17e-08 5
#> CV:skmeans 38         1.15e-06 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "pam"]
# you can also extract it by
# res = res_list["CV:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-pam-collect-plots](figure_cola/CV-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-pam-select-partition-number](figure_cola/CV-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.960           0.930       0.942         0.3852 0.599   0.599
#> 3 3 1.000           0.969       0.990         0.5489 0.664   0.496
#> 4 4 0.732           0.773       0.895         0.2390 0.802   0.530
#> 5 5 0.710           0.582       0.777         0.0520 0.875   0.586
#> 6 6 0.781           0.685       0.860         0.0455 0.904   0.623
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-classes'>
<ul>
<li><a href='#tab-CV-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-pam-get-classes-1'>
<p><a id='tab-CV-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559433     2   0.000      0.965 0.000 1.000
#&gt; GSM559434     2   0.000      0.965 0.000 1.000
#&gt; GSM559436     2   0.000      0.965 0.000 1.000
#&gt; GSM559437     2   0.000      0.965 0.000 1.000
#&gt; GSM559438     2   0.000      0.965 0.000 1.000
#&gt; GSM559440     2   0.000      0.965 0.000 1.000
#&gt; GSM559441     2   0.000      0.965 0.000 1.000
#&gt; GSM559442     2   0.000      0.965 0.000 1.000
#&gt; GSM559444     2   0.456      0.916 0.096 0.904
#&gt; GSM559445     2   0.000      0.965 0.000 1.000
#&gt; GSM559446     2   0.000      0.965 0.000 1.000
#&gt; GSM559448     1   0.529      0.919 0.880 0.120
#&gt; GSM559450     2   0.456      0.916 0.096 0.904
#&gt; GSM559451     2   0.000      0.965 0.000 1.000
#&gt; GSM559452     2   0.163      0.945 0.024 0.976
#&gt; GSM559454     2   0.000      0.965 0.000 1.000
#&gt; GSM559455     2   0.000      0.965 0.000 1.000
#&gt; GSM559456     2   0.000      0.965 0.000 1.000
#&gt; GSM559457     2   0.000      0.965 0.000 1.000
#&gt; GSM559458     1   0.925      0.662 0.660 0.340
#&gt; GSM559459     2   0.000      0.965 0.000 1.000
#&gt; GSM559460     2   0.000      0.965 0.000 1.000
#&gt; GSM559461     2   0.000      0.965 0.000 1.000
#&gt; GSM559462     2   0.000      0.965 0.000 1.000
#&gt; GSM559463     1   0.795      0.813 0.760 0.240
#&gt; GSM559464     2   0.000      0.965 0.000 1.000
#&gt; GSM559465     1   0.955      0.591 0.624 0.376
#&gt; GSM559467     2   0.000      0.965 0.000 1.000
#&gt; GSM559468     2   0.000      0.965 0.000 1.000
#&gt; GSM559469     2   0.000      0.965 0.000 1.000
#&gt; GSM559470     2   0.000      0.965 0.000 1.000
#&gt; GSM559471     2   0.000      0.965 0.000 1.000
#&gt; GSM559472     2   0.000      0.965 0.000 1.000
#&gt; GSM559473     2   0.456      0.916 0.096 0.904
#&gt; GSM559475     2   0.456      0.916 0.096 0.904
#&gt; GSM559477     2   0.456      0.916 0.096 0.904
#&gt; GSM559478     2   0.456      0.916 0.096 0.904
#&gt; GSM559479     2   0.456      0.916 0.096 0.904
#&gt; GSM559480     2   0.456      0.916 0.096 0.904
#&gt; GSM559481     2   0.456      0.916 0.096 0.904
#&gt; GSM559482     2   0.456      0.916 0.096 0.904
#&gt; GSM559435     1   0.456      0.932 0.904 0.096
#&gt; GSM559439     1   0.456      0.932 0.904 0.096
#&gt; GSM559443     1   0.456      0.932 0.904 0.096
#&gt; GSM559447     1   0.456      0.932 0.904 0.096
#&gt; GSM559449     1   0.456      0.932 0.904 0.096
#&gt; GSM559453     1   0.456      0.932 0.904 0.096
#&gt; GSM559466     1   0.456      0.932 0.904 0.096
#&gt; GSM559474     1   0.456      0.932 0.904 0.096
#&gt; GSM559476     1   0.456      0.932 0.904 0.096
#&gt; GSM559483     2   0.456      0.916 0.096 0.904
#&gt; GSM559484     1   0.000      0.852 1.000 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-1-a').click(function(){
  $('#tab-CV-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-2'>
<p><a id='tab-CV-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559433     1  0.0237     0.9977 0.996 0.004 0.000
#&gt; GSM559434     1  0.0237     0.9977 0.996 0.004 0.000
#&gt; GSM559436     1  0.0000     0.9979 1.000 0.000 0.000
#&gt; GSM559437     1  0.0237     0.9977 0.996 0.004 0.000
#&gt; GSM559438     1  0.0237     0.9977 0.996 0.004 0.000
#&gt; GSM559440     1  0.0237     0.9977 0.996 0.004 0.000
#&gt; GSM559441     1  0.0237     0.9977 0.996 0.004 0.000
#&gt; GSM559442     1  0.0000     0.9979 1.000 0.000 0.000
#&gt; GSM559444     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM559445     1  0.0237     0.9977 0.996 0.004 0.000
#&gt; GSM559446     1  0.0237     0.9977 0.996 0.004 0.000
#&gt; GSM559448     1  0.0000     0.9979 1.000 0.000 0.000
#&gt; GSM559450     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM559451     1  0.0237     0.9977 0.996 0.004 0.000
#&gt; GSM559452     3  0.0475     0.9331 0.004 0.004 0.992
#&gt; GSM559454     1  0.0000     0.9979 1.000 0.000 0.000
#&gt; GSM559455     1  0.0237     0.9977 0.996 0.004 0.000
#&gt; GSM559456     1  0.0237     0.9977 0.996 0.004 0.000
#&gt; GSM559457     1  0.0000     0.9979 1.000 0.000 0.000
#&gt; GSM559458     3  0.6509     0.0965 0.472 0.004 0.524
#&gt; GSM559459     1  0.0000     0.9979 1.000 0.000 0.000
#&gt; GSM559460     1  0.0000     0.9979 1.000 0.000 0.000
#&gt; GSM559461     1  0.0000     0.9979 1.000 0.000 0.000
#&gt; GSM559462     1  0.0000     0.9979 1.000 0.000 0.000
#&gt; GSM559463     1  0.0000     0.9979 1.000 0.000 0.000
#&gt; GSM559464     1  0.0000     0.9979 1.000 0.000 0.000
#&gt; GSM559465     1  0.0000     0.9979 1.000 0.000 0.000
#&gt; GSM559467     1  0.0237     0.9977 0.996 0.004 0.000
#&gt; GSM559468     1  0.0000     0.9979 1.000 0.000 0.000
#&gt; GSM559469     1  0.0000     0.9979 1.000 0.000 0.000
#&gt; GSM559470     1  0.0237     0.9977 0.996 0.004 0.000
#&gt; GSM559471     1  0.0000     0.9979 1.000 0.000 0.000
#&gt; GSM559472     1  0.0237     0.9977 0.996 0.004 0.000
#&gt; GSM559473     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM559475     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM559477     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM559478     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM559479     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM559480     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM559481     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM559482     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM559435     3  0.0000     0.9406 0.000 0.000 1.000
#&gt; GSM559439     3  0.0000     0.9406 0.000 0.000 1.000
#&gt; GSM559443     3  0.0000     0.9406 0.000 0.000 1.000
#&gt; GSM559447     3  0.0000     0.9406 0.000 0.000 1.000
#&gt; GSM559449     3  0.0000     0.9406 0.000 0.000 1.000
#&gt; GSM559453     3  0.0000     0.9406 0.000 0.000 1.000
#&gt; GSM559466     3  0.0000     0.9406 0.000 0.000 1.000
#&gt; GSM559474     3  0.0000     0.9406 0.000 0.000 1.000
#&gt; GSM559476     3  0.0000     0.9406 0.000 0.000 1.000
#&gt; GSM559483     2  0.0000     1.0000 0.000 1.000 0.000
#&gt; GSM559484     3  0.0000     0.9406 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-2-a').click(function(){
  $('#tab-CV-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-3'>
<p><a id='tab-CV-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559433     4  0.0592      0.843 0.016 0.000 0.000 0.984
#&gt; GSM559434     4  0.0817      0.829 0.024 0.000 0.000 0.976
#&gt; GSM559436     1  0.4222      0.715 0.728 0.000 0.000 0.272
#&gt; GSM559437     4  0.0469      0.844 0.012 0.000 0.000 0.988
#&gt; GSM559438     4  0.1211      0.821 0.040 0.000 0.000 0.960
#&gt; GSM559440     4  0.0817      0.829 0.024 0.000 0.000 0.976
#&gt; GSM559441     4  0.0469      0.844 0.012 0.000 0.000 0.988
#&gt; GSM559442     1  0.3400      0.595 0.820 0.000 0.000 0.180
#&gt; GSM559444     4  0.4222      0.475 0.000 0.272 0.000 0.728
#&gt; GSM559445     4  0.0469      0.844 0.012 0.000 0.000 0.988
#&gt; GSM559446     4  0.0469      0.844 0.012 0.000 0.000 0.988
#&gt; GSM559448     1  0.7799      0.384 0.420 0.000 0.308 0.272
#&gt; GSM559450     2  0.2469      0.843 0.000 0.892 0.000 0.108
#&gt; GSM559451     4  0.4331      0.489 0.288 0.000 0.000 0.712
#&gt; GSM559452     4  0.4222      0.553 0.272 0.000 0.000 0.728
#&gt; GSM559454     1  0.4972      0.337 0.544 0.000 0.000 0.456
#&gt; GSM559455     4  0.0469      0.844 0.012 0.000 0.000 0.988
#&gt; GSM559456     4  0.1716      0.813 0.064 0.000 0.000 0.936
#&gt; GSM559457     4  0.4431      0.452 0.304 0.000 0.000 0.696
#&gt; GSM559458     3  0.5110      0.462 0.016 0.000 0.656 0.328
#&gt; GSM559459     1  0.4222      0.715 0.728 0.000 0.000 0.272
#&gt; GSM559460     1  0.1118      0.769 0.964 0.000 0.000 0.036
#&gt; GSM559461     1  0.4222      0.715 0.728 0.000 0.000 0.272
#&gt; GSM559462     1  0.4222      0.715 0.728 0.000 0.000 0.272
#&gt; GSM559463     1  0.3801      0.740 0.780 0.000 0.000 0.220
#&gt; GSM559464     1  0.0817      0.765 0.976 0.000 0.000 0.024
#&gt; GSM559465     1  0.1867      0.771 0.928 0.000 0.000 0.072
#&gt; GSM559467     4  0.2868      0.754 0.136 0.000 0.000 0.864
#&gt; GSM559468     1  0.0000      0.755 1.000 0.000 0.000 0.000
#&gt; GSM559469     1  0.0188      0.754 0.996 0.000 0.000 0.004
#&gt; GSM559470     4  0.0469      0.844 0.012 0.000 0.000 0.988
#&gt; GSM559471     1  0.1022      0.763 0.968 0.000 0.000 0.032
#&gt; GSM559472     4  0.4817      0.193 0.388 0.000 0.000 0.612
#&gt; GSM559473     2  0.4941      0.235 0.000 0.564 0.000 0.436
#&gt; GSM559475     2  0.0000      0.933 0.000 1.000 0.000 0.000
#&gt; GSM559477     2  0.0000      0.933 0.000 1.000 0.000 0.000
#&gt; GSM559478     2  0.0000      0.933 0.000 1.000 0.000 0.000
#&gt; GSM559479     2  0.0000      0.933 0.000 1.000 0.000 0.000
#&gt; GSM559480     2  0.0000      0.933 0.000 1.000 0.000 0.000
#&gt; GSM559481     2  0.0000      0.933 0.000 1.000 0.000 0.000
#&gt; GSM559482     2  0.0000      0.933 0.000 1.000 0.000 0.000
#&gt; GSM559435     3  0.0000      0.960 0.000 0.000 1.000 0.000
#&gt; GSM559439     3  0.0000      0.960 0.000 0.000 1.000 0.000
#&gt; GSM559443     3  0.0000      0.960 0.000 0.000 1.000 0.000
#&gt; GSM559447     3  0.0000      0.960 0.000 0.000 1.000 0.000
#&gt; GSM559449     3  0.0000      0.960 0.000 0.000 1.000 0.000
#&gt; GSM559453     3  0.0000      0.960 0.000 0.000 1.000 0.000
#&gt; GSM559466     3  0.0000      0.960 0.000 0.000 1.000 0.000
#&gt; GSM559474     3  0.0000      0.960 0.000 0.000 1.000 0.000
#&gt; GSM559476     3  0.0000      0.960 0.000 0.000 1.000 0.000
#&gt; GSM559483     2  0.0000      0.933 0.000 1.000 0.000 0.000
#&gt; GSM559484     3  0.0000      0.960 0.000 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-3-a').click(function(){
  $('#tab-CV-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-4'>
<p><a id='tab-CV-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559433     4  0.4201     0.4418 0.408 0.000 0.000 0.592 0.000
#&gt; GSM559434     4  0.0000     0.7288 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559436     1  0.1608     0.5498 0.928 0.000 0.000 0.072 0.000
#&gt; GSM559437     4  0.2329     0.7502 0.124 0.000 0.000 0.876 0.000
#&gt; GSM559438     4  0.0000     0.7288 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559440     4  0.0000     0.7288 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559441     4  0.2329     0.7502 0.124 0.000 0.000 0.876 0.000
#&gt; GSM559442     3  0.6451    -0.4734 0.364 0.000 0.452 0.184 0.000
#&gt; GSM559444     4  0.1908     0.6683 0.000 0.092 0.000 0.908 0.000
#&gt; GSM559445     4  0.2329     0.7502 0.124 0.000 0.000 0.876 0.000
#&gt; GSM559446     4  0.3966     0.5489 0.336 0.000 0.000 0.664 0.000
#&gt; GSM559448     1  0.4736     0.4020 0.712 0.000 0.216 0.072 0.000
#&gt; GSM559450     2  0.3395     0.6906 0.000 0.764 0.000 0.236 0.000
#&gt; GSM559451     1  0.4030     0.1378 0.648 0.000 0.000 0.352 0.000
#&gt; GSM559452     4  0.5322     0.3661 0.072 0.000 0.320 0.608 0.000
#&gt; GSM559454     1  0.3452     0.3612 0.756 0.000 0.000 0.244 0.000
#&gt; GSM559455     4  0.2516     0.7420 0.140 0.000 0.000 0.860 0.000
#&gt; GSM559456     4  0.4305     0.2803 0.488 0.000 0.000 0.512 0.000
#&gt; GSM559457     1  0.4015     0.1493 0.652 0.000 0.000 0.348 0.000
#&gt; GSM559458     3  0.7515     0.0615 0.252 0.000 0.500 0.140 0.108
#&gt; GSM559459     1  0.3471     0.5813 0.836 0.000 0.092 0.072 0.000
#&gt; GSM559460     1  0.4242     0.5069 0.572 0.000 0.428 0.000 0.000
#&gt; GSM559461     1  0.3966     0.5888 0.796 0.000 0.132 0.072 0.000
#&gt; GSM559462     1  0.3966     0.5888 0.796 0.000 0.132 0.072 0.000
#&gt; GSM559463     1  0.3764     0.5942 0.800 0.000 0.156 0.044 0.000
#&gt; GSM559464     1  0.4278     0.4914 0.548 0.000 0.452 0.000 0.000
#&gt; GSM559465     1  0.4717     0.5326 0.584 0.000 0.396 0.020 0.000
#&gt; GSM559467     1  0.4897    -0.2211 0.516 0.000 0.024 0.460 0.000
#&gt; GSM559468     1  0.4811     0.4742 0.528 0.000 0.452 0.020 0.000
#&gt; GSM559469     1  0.4890     0.4718 0.524 0.000 0.452 0.024 0.000
#&gt; GSM559470     4  0.2329     0.7502 0.124 0.000 0.000 0.876 0.000
#&gt; GSM559471     1  0.4768     0.4986 0.592 0.000 0.384 0.024 0.000
#&gt; GSM559472     1  0.3837     0.2442 0.692 0.000 0.000 0.308 0.000
#&gt; GSM559473     4  0.4306    -0.0383 0.000 0.492 0.000 0.508 0.000
#&gt; GSM559475     2  0.1043     0.9364 0.000 0.960 0.000 0.040 0.000
#&gt; GSM559477     2  0.0000     0.9532 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559478     2  0.0404     0.9499 0.000 0.988 0.000 0.012 0.000
#&gt; GSM559479     2  0.0000     0.9532 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.1043     0.9364 0.000 0.960 0.000 0.040 0.000
#&gt; GSM559481     2  0.0000     0.9532 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559482     2  0.0000     0.9532 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.4278     0.6545 0.000 0.000 0.548 0.000 0.452
#&gt; GSM559439     3  0.4278     0.6545 0.000 0.000 0.548 0.000 0.452
#&gt; GSM559443     3  0.4278     0.6545 0.000 0.000 0.548 0.000 0.452
#&gt; GSM559447     3  0.4278     0.6545 0.000 0.000 0.548 0.000 0.452
#&gt; GSM559449     3  0.4278     0.6545 0.000 0.000 0.548 0.000 0.452
#&gt; GSM559453     3  0.4278     0.6545 0.000 0.000 0.548 0.000 0.452
#&gt; GSM559466     3  0.4278     0.6545 0.000 0.000 0.548 0.000 0.452
#&gt; GSM559474     5  0.0000     1.0000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559476     3  0.4278     0.6545 0.000 0.000 0.548 0.000 0.452
#&gt; GSM559483     2  0.0000     0.9532 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559484     5  0.0000     1.0000 0.000 0.000 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-4-a').click(function(){
  $('#tab-CV-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-5'>
<p><a id='tab-CV-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4 p5    p6
#&gt; GSM559433     1  0.3804     0.0243 0.576 0.000 0.000 0.424  0 0.000
#&gt; GSM559434     4  0.0146     0.6877 0.004 0.000 0.000 0.996  0 0.000
#&gt; GSM559436     1  0.0000     0.7121 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM559437     4  0.2996     0.6924 0.228 0.000 0.000 0.772  0 0.000
#&gt; GSM559438     4  0.0000     0.6857 0.000 0.000 0.000 1.000  0 0.000
#&gt; GSM559440     4  0.0000     0.6857 0.000 0.000 0.000 1.000  0 0.000
#&gt; GSM559441     4  0.2996     0.6924 0.228 0.000 0.000 0.772  0 0.000
#&gt; GSM559442     6  0.0865     0.8302 0.000 0.000 0.000 0.036  0 0.964
#&gt; GSM559444     4  0.0000     0.6857 0.000 0.000 0.000 1.000  0 0.000
#&gt; GSM559445     4  0.2996     0.6924 0.228 0.000 0.000 0.772  0 0.000
#&gt; GSM559446     4  0.3851     0.2344 0.460 0.000 0.000 0.540  0 0.000
#&gt; GSM559448     1  0.0000     0.7121 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM559450     2  0.3151     0.6333 0.000 0.748 0.000 0.252  0 0.000
#&gt; GSM559451     1  0.0000     0.7121 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM559452     4  0.3862     0.1650 0.000 0.000 0.000 0.524  0 0.476
#&gt; GSM559454     1  0.0000     0.7121 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM559455     4  0.3221     0.6525 0.264 0.000 0.000 0.736  0 0.000
#&gt; GSM559456     1  0.3330     0.3935 0.716 0.000 0.000 0.284  0 0.000
#&gt; GSM559457     1  0.0000     0.7121 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM559458     3  0.5360     0.2808 0.344 0.000 0.556 0.088  0 0.012
#&gt; GSM559459     1  0.3817     0.3153 0.568 0.000 0.000 0.000  0 0.432
#&gt; GSM559460     6  0.1267     0.8585 0.060 0.000 0.000 0.000  0 0.940
#&gt; GSM559461     1  0.3867     0.2031 0.512 0.000 0.000 0.000  0 0.488
#&gt; GSM559462     1  0.3867     0.2031 0.512 0.000 0.000 0.000  0 0.488
#&gt; GSM559463     1  0.3351     0.3456 0.712 0.000 0.000 0.000  0 0.288
#&gt; GSM559464     6  0.0865     0.8652 0.036 0.000 0.000 0.000  0 0.964
#&gt; GSM559465     6  0.2300     0.7599 0.144 0.000 0.000 0.000  0 0.856
#&gt; GSM559467     1  0.4587     0.3678 0.640 0.000 0.000 0.296  0 0.064
#&gt; GSM559468     6  0.1007     0.8649 0.044 0.000 0.000 0.000  0 0.956
#&gt; GSM559469     6  0.0865     0.8652 0.036 0.000 0.000 0.000  0 0.964
#&gt; GSM559470     4  0.2996     0.6924 0.228 0.000 0.000 0.772  0 0.000
#&gt; GSM559471     6  0.3804     0.3361 0.424 0.000 0.000 0.000  0 0.576
#&gt; GSM559472     1  0.0000     0.7121 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM559473     4  0.4537    -0.0304 0.000 0.412 0.000 0.552  0 0.036
#&gt; GSM559475     2  0.1714     0.8790 0.000 0.908 0.000 0.092  0 0.000
#&gt; GSM559477     2  0.0000     0.9205 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM559478     2  0.1320     0.9109 0.000 0.948 0.000 0.016  0 0.036
#&gt; GSM559479     2  0.0000     0.9205 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM559480     2  0.2560     0.8664 0.000 0.872 0.000 0.092  0 0.036
#&gt; GSM559481     2  0.0865     0.9132 0.000 0.964 0.000 0.000  0 0.036
#&gt; GSM559482     2  0.0000     0.9205 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM559435     3  0.0000     0.9293 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM559439     3  0.0000     0.9293 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM559443     3  0.0000     0.9293 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM559447     3  0.0000     0.9293 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM559449     3  0.0000     0.9293 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM559453     3  0.0000     0.9293 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM559466     3  0.0000     0.9293 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM559474     5  0.0000     1.0000 0.000 0.000 0.000 0.000  1 0.000
#&gt; GSM559476     3  0.0000     0.9293 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM559483     2  0.0000     0.9205 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM559484     5  0.0000     1.0000 0.000 0.000 0.000 0.000  1 0.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-5-a').click(function(){
  $('#tab-CV-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-membership-heatmap'>
<ul>
<li><a href='#tab-CV-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-pam-signature_compare](figure_cola/CV-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-pam-dimension-reduction'>
<ul>
<li><a href='#tab-CV-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-pam-collect-classes](figure_cola/CV-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n disease.state(p) k
#> CV:pam 52         5.58e-07 2
#> CV:pam 51         1.82e-09 3
#> CV:pam 44         1.53e-08 4
#> CV:pam 36         2.09e-06 5
#> CV:pam 40         1.17e-06 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "mclust"]
# you can also extract it by
# res = res_list["CV:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-mclust-collect-plots](figure_cola/CV-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-mclust-select-partition-number](figure_cola/CV-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.442           0.701       0.750         0.4046 0.566   0.566
#> 3 3 0.679           0.834       0.910         0.4448 0.491   0.329
#> 4 4 0.638           0.341       0.709         0.0826 0.774   0.515
#> 5 5 0.762           0.742       0.857         0.1606 0.779   0.409
#> 6 6 0.852           0.836       0.922         0.0429 0.885   0.614
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-classes'>
<ul>
<li><a href='#tab-CV-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-mclust-get-classes-1'>
<p><a id='tab-CV-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559433     1  0.9970      0.913 0.532 0.468
#&gt; GSM559434     2  0.0000      0.711 0.000 1.000
#&gt; GSM559436     1  0.9661      0.941 0.608 0.392
#&gt; GSM559437     2  0.0376      0.708 0.004 0.996
#&gt; GSM559438     2  0.4562      0.536 0.096 0.904
#&gt; GSM559440     2  0.0000      0.711 0.000 1.000
#&gt; GSM559441     2  0.0376      0.708 0.004 0.996
#&gt; GSM559442     1  0.9977      0.915 0.528 0.472
#&gt; GSM559444     2  0.0000      0.711 0.000 1.000
#&gt; GSM559445     2  0.0376      0.708 0.004 0.996
#&gt; GSM559446     2  0.0376      0.708 0.004 0.996
#&gt; GSM559448     2  0.4690      0.639 0.100 0.900
#&gt; GSM559450     2  0.0000      0.711 0.000 1.000
#&gt; GSM559451     1  0.9944      0.924 0.544 0.456
#&gt; GSM559452     2  0.4022      0.665 0.080 0.920
#&gt; GSM559454     1  0.9661      0.941 0.608 0.392
#&gt; GSM559455     2  0.3879      0.586 0.076 0.924
#&gt; GSM559456     2  0.0376      0.708 0.004 0.996
#&gt; GSM559457     2  0.9983     -0.840 0.476 0.524
#&gt; GSM559458     2  0.3733      0.668 0.072 0.928
#&gt; GSM559459     1  0.9661      0.941 0.608 0.392
#&gt; GSM559460     1  0.9661      0.941 0.608 0.392
#&gt; GSM559461     1  0.9710      0.942 0.600 0.400
#&gt; GSM559462     1  0.9661      0.941 0.608 0.392
#&gt; GSM559463     2  0.5842      0.577 0.140 0.860
#&gt; GSM559464     1  0.9661      0.941 0.608 0.392
#&gt; GSM559465     1  0.9661      0.941 0.608 0.392
#&gt; GSM559467     1  0.9983      0.908 0.524 0.476
#&gt; GSM559468     1  0.9850      0.938 0.572 0.428
#&gt; GSM559469     1  0.9977      0.912 0.528 0.472
#&gt; GSM559470     2  0.0376      0.708 0.004 0.996
#&gt; GSM559471     1  0.9963      0.918 0.536 0.464
#&gt; GSM559472     1  0.9795      0.941 0.584 0.416
#&gt; GSM559473     2  0.0376      0.713 0.004 0.996
#&gt; GSM559475     2  0.0376      0.713 0.004 0.996
#&gt; GSM559477     2  0.0376      0.713 0.004 0.996
#&gt; GSM559478     2  0.0376      0.713 0.004 0.996
#&gt; GSM559479     2  0.0376      0.713 0.004 0.996
#&gt; GSM559480     2  0.0376      0.713 0.004 0.996
#&gt; GSM559481     2  0.0376      0.713 0.004 0.996
#&gt; GSM559482     2  0.0376      0.713 0.004 0.996
#&gt; GSM559435     2  0.9970      0.520 0.468 0.532
#&gt; GSM559439     2  0.9970      0.520 0.468 0.532
#&gt; GSM559443     2  0.9970      0.520 0.468 0.532
#&gt; GSM559447     2  0.9970      0.520 0.468 0.532
#&gt; GSM559449     2  0.9970      0.520 0.468 0.532
#&gt; GSM559453     2  0.9970      0.520 0.468 0.532
#&gt; GSM559466     2  0.9970      0.520 0.468 0.532
#&gt; GSM559474     2  0.9970      0.520 0.468 0.532
#&gt; GSM559476     2  0.9970      0.520 0.468 0.532
#&gt; GSM559483     2  0.0376      0.713 0.004 0.996
#&gt; GSM559484     2  0.9970      0.520 0.468 0.532
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-1-a').click(function(){
  $('#tab-CV-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-2'>
<p><a id='tab-CV-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559433     1  0.0000      0.869 1.000 0.000 0.000
#&gt; GSM559434     1  0.6274      0.391 0.544 0.456 0.000
#&gt; GSM559436     1  0.0000      0.869 1.000 0.000 0.000
#&gt; GSM559437     1  0.6079      0.541 0.612 0.388 0.000
#&gt; GSM559438     1  0.5650      0.626 0.688 0.312 0.000
#&gt; GSM559440     1  0.6215      0.412 0.572 0.428 0.000
#&gt; GSM559441     1  0.5216      0.719 0.740 0.260 0.000
#&gt; GSM559442     1  0.1031      0.870 0.976 0.024 0.000
#&gt; GSM559444     2  0.4062      0.758 0.164 0.836 0.000
#&gt; GSM559445     1  0.6126      0.518 0.600 0.400 0.000
#&gt; GSM559446     1  0.6045      0.555 0.620 0.380 0.000
#&gt; GSM559448     1  0.1643      0.864 0.956 0.044 0.000
#&gt; GSM559450     2  0.2711      0.865 0.088 0.912 0.000
#&gt; GSM559451     1  0.0892      0.870 0.980 0.020 0.000
#&gt; GSM559452     1  0.5643      0.726 0.760 0.220 0.020
#&gt; GSM559454     1  0.0000      0.869 1.000 0.000 0.000
#&gt; GSM559455     1  0.3038      0.845 0.896 0.104 0.000
#&gt; GSM559456     1  0.3038      0.845 0.896 0.104 0.000
#&gt; GSM559457     1  0.2625      0.855 0.916 0.084 0.000
#&gt; GSM559458     1  0.2625      0.849 0.916 0.084 0.000
#&gt; GSM559459     1  0.0000      0.869 1.000 0.000 0.000
#&gt; GSM559460     1  0.0000      0.869 1.000 0.000 0.000
#&gt; GSM559461     1  0.0000      0.869 1.000 0.000 0.000
#&gt; GSM559462     1  0.0000      0.869 1.000 0.000 0.000
#&gt; GSM559463     1  0.0237      0.869 0.996 0.004 0.000
#&gt; GSM559464     1  0.0000      0.869 1.000 0.000 0.000
#&gt; GSM559465     1  0.2066      0.863 0.940 0.060 0.000
#&gt; GSM559467     1  0.1031      0.870 0.976 0.024 0.000
#&gt; GSM559468     1  0.0892      0.870 0.980 0.020 0.000
#&gt; GSM559469     1  0.1031      0.870 0.976 0.024 0.000
#&gt; GSM559470     1  0.4702      0.749 0.788 0.212 0.000
#&gt; GSM559471     1  0.1031      0.870 0.976 0.024 0.000
#&gt; GSM559472     1  0.0000      0.869 1.000 0.000 0.000
#&gt; GSM559473     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559475     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559477     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559478     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559479     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559480     2  0.0892      0.940 0.000 0.980 0.020
#&gt; GSM559481     2  0.0892      0.940 0.000 0.980 0.020
#&gt; GSM559482     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559435     3  0.0892      0.943 0.020 0.000 0.980
#&gt; GSM559439     3  0.0892      0.943 0.020 0.000 0.980
#&gt; GSM559443     3  0.1129      0.942 0.020 0.004 0.976
#&gt; GSM559447     3  0.0892      0.943 0.020 0.000 0.980
#&gt; GSM559449     3  0.0892      0.943 0.020 0.000 0.980
#&gt; GSM559453     3  0.1129      0.942 0.020 0.004 0.976
#&gt; GSM559466     3  0.0892      0.943 0.020 0.000 0.980
#&gt; GSM559474     3  0.4291      0.769 0.000 0.180 0.820
#&gt; GSM559476     1  0.5069      0.766 0.828 0.044 0.128
#&gt; GSM559483     2  0.0000      0.956 0.000 1.000 0.000
#&gt; GSM559484     3  0.4399      0.760 0.000 0.188 0.812
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-2-a').click(function(){
  $('#tab-CV-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-3'>
<p><a id='tab-CV-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559433     1  0.4999  -2.84e-01 0.508 0.000 0.000 0.492
#&gt; GSM559434     4  0.5147   9.81e-02 0.460 0.004 0.000 0.536
#&gt; GSM559436     4  0.5000   2.40e-01 0.496 0.000 0.000 0.504
#&gt; GSM559437     1  0.0000   3.18e-01 1.000 0.000 0.000 0.000
#&gt; GSM559438     1  0.4907  -5.64e-03 0.580 0.000 0.000 0.420
#&gt; GSM559440     1  0.4977  -6.59e-02 0.540 0.000 0.000 0.460
#&gt; GSM559441     1  0.3024   2.99e-01 0.852 0.000 0.000 0.148
#&gt; GSM559442     1  0.4998  -1.16e-01 0.512 0.000 0.000 0.488
#&gt; GSM559444     4  0.6648   7.70e-02 0.092 0.372 0.000 0.536
#&gt; GSM559445     1  0.0000   3.18e-01 1.000 0.000 0.000 0.000
#&gt; GSM559446     1  0.0000   3.18e-01 1.000 0.000 0.000 0.000
#&gt; GSM559448     4  0.5143   7.76e-02 0.456 0.000 0.004 0.540
#&gt; GSM559450     4  0.5894  -9.88e-02 0.036 0.428 0.000 0.536
#&gt; GSM559451     4  0.5000   2.40e-01 0.496 0.000 0.000 0.504
#&gt; GSM559452     4  0.6009   1.15e-01 0.380 0.048 0.000 0.572
#&gt; GSM559454     4  0.5000   2.40e-01 0.496 0.000 0.000 0.504
#&gt; GSM559455     1  0.3649   2.72e-01 0.796 0.000 0.000 0.204
#&gt; GSM559456     1  0.0188   3.16e-01 0.996 0.000 0.004 0.000
#&gt; GSM559457     1  0.4624   1.38e-01 0.660 0.000 0.000 0.340
#&gt; GSM559458     1  0.1305   2.89e-01 0.960 0.000 0.004 0.036
#&gt; GSM559459     4  0.5000   2.21e-01 0.500 0.000 0.000 0.500
#&gt; GSM559460     4  0.5000   2.40e-01 0.496 0.000 0.000 0.504
#&gt; GSM559461     4  0.5000   2.40e-01 0.496 0.000 0.000 0.504
#&gt; GSM559462     1  0.4992  -2.03e-01 0.524 0.000 0.000 0.476
#&gt; GSM559463     1  0.4992  -2.03e-01 0.524 0.000 0.000 0.476
#&gt; GSM559464     4  0.5000   2.40e-01 0.496 0.000 0.000 0.504
#&gt; GSM559465     1  0.4972  -1.58e-01 0.544 0.000 0.000 0.456
#&gt; GSM559467     1  0.4888  -6.82e-05 0.588 0.000 0.000 0.412
#&gt; GSM559468     1  0.5000  -2.96e-01 0.504 0.000 0.000 0.496
#&gt; GSM559469     1  0.4948  -3.31e-02 0.560 0.000 0.000 0.440
#&gt; GSM559470     1  0.4888  -6.82e-05 0.588 0.000 0.000 0.412
#&gt; GSM559471     1  0.4994  -2.04e-01 0.520 0.000 0.000 0.480
#&gt; GSM559472     4  0.5000   2.40e-01 0.496 0.000 0.000 0.504
#&gt; GSM559473     2  0.4562   7.16e-01 0.028 0.764 0.000 0.208
#&gt; GSM559475     2  0.5768   2.39e-01 0.028 0.516 0.000 0.456
#&gt; GSM559477     2  0.0000   8.93e-01 0.000 1.000 0.000 0.000
#&gt; GSM559478     2  0.0000   8.93e-01 0.000 1.000 0.000 0.000
#&gt; GSM559479     2  0.0000   8.93e-01 0.000 1.000 0.000 0.000
#&gt; GSM559480     2  0.0000   8.93e-01 0.000 1.000 0.000 0.000
#&gt; GSM559481     2  0.0000   8.93e-01 0.000 1.000 0.000 0.000
#&gt; GSM559482     2  0.0000   8.93e-01 0.000 1.000 0.000 0.000
#&gt; GSM559435     3  0.0000   8.74e-01 0.000 0.000 1.000 0.000
#&gt; GSM559439     3  0.0000   8.74e-01 0.000 0.000 1.000 0.000
#&gt; GSM559443     3  0.0000   8.74e-01 0.000 0.000 1.000 0.000
#&gt; GSM559447     3  0.0000   8.74e-01 0.000 0.000 1.000 0.000
#&gt; GSM559449     3  0.0000   8.74e-01 0.000 0.000 1.000 0.000
#&gt; GSM559453     3  0.0000   8.74e-01 0.000 0.000 1.000 0.000
#&gt; GSM559466     3  0.0000   8.74e-01 0.000 0.000 1.000 0.000
#&gt; GSM559474     3  0.4605   7.14e-01 0.000 0.000 0.664 0.336
#&gt; GSM559476     3  0.6648   1.37e-01 0.372 0.000 0.536 0.092
#&gt; GSM559483     2  0.0000   8.93e-01 0.000 1.000 0.000 0.000
#&gt; GSM559484     3  0.4605   7.14e-01 0.000 0.000 0.664 0.336
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-3-a').click(function(){
  $('#tab-CV-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-4'>
<p><a id='tab-CV-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559433     1  0.0000     0.8956 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559434     4  0.0162     0.6171 0.004 0.000 0.000 0.996 0.000
#&gt; GSM559436     1  0.0000     0.8956 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559437     4  0.6038     0.6366 0.184 0.000 0.000 0.576 0.240
#&gt; GSM559438     4  0.3336     0.6164 0.228 0.000 0.000 0.772 0.000
#&gt; GSM559440     4  0.0963     0.6254 0.036 0.000 0.000 0.964 0.000
#&gt; GSM559441     4  0.6044     0.6362 0.188 0.000 0.000 0.576 0.236
#&gt; GSM559442     1  0.0510     0.8886 0.984 0.000 0.000 0.016 0.000
#&gt; GSM559444     4  0.2763     0.4671 0.004 0.148 0.000 0.848 0.000
#&gt; GSM559445     4  0.5309     0.6438 0.104 0.000 0.000 0.656 0.240
#&gt; GSM559446     4  0.6038     0.6366 0.184 0.000 0.000 0.576 0.240
#&gt; GSM559448     1  0.1638     0.8528 0.932 0.000 0.004 0.064 0.000
#&gt; GSM559450     2  0.4425     0.4987 0.000 0.544 0.000 0.452 0.004
#&gt; GSM559451     1  0.0000     0.8956 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559452     4  0.0613     0.6182 0.008 0.004 0.004 0.984 0.000
#&gt; GSM559454     1  0.0000     0.8956 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559455     1  0.5779     0.4143 0.616 0.000 0.000 0.212 0.172
#&gt; GSM559456     1  0.5565     0.4746 0.632 0.000 0.000 0.128 0.240
#&gt; GSM559457     1  0.2536     0.7900 0.868 0.000 0.000 0.004 0.128
#&gt; GSM559458     1  0.6076     0.3814 0.588 0.000 0.004 0.172 0.236
#&gt; GSM559459     1  0.0000     0.8956 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559460     1  0.0000     0.8956 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559461     1  0.0000     0.8956 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559462     1  0.0000     0.8956 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559463     1  0.0290     0.8920 0.992 0.000 0.000 0.008 0.000
#&gt; GSM559464     1  0.0000     0.8956 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559465     1  0.0000     0.8956 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559467     1  0.4171     0.1382 0.604 0.000 0.000 0.396 0.000
#&gt; GSM559468     1  0.0000     0.8956 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559469     1  0.1478     0.8550 0.936 0.000 0.000 0.064 0.000
#&gt; GSM559470     4  0.4182     0.4480 0.400 0.000 0.000 0.600 0.000
#&gt; GSM559471     1  0.0290     0.8920 0.992 0.000 0.000 0.008 0.000
#&gt; GSM559472     1  0.0000     0.8956 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559473     2  0.3861     0.6750 0.000 0.712 0.000 0.284 0.004
#&gt; GSM559475     2  0.4375     0.5517 0.000 0.576 0.000 0.420 0.004
#&gt; GSM559477     2  0.0000     0.8530 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559478     2  0.0162     0.8515 0.000 0.996 0.000 0.000 0.004
#&gt; GSM559479     2  0.0000     0.8530 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.0000     0.8530 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559481     2  0.0000     0.8530 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559482     2  0.0000     0.8530 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.0000     0.8157 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559439     3  0.0000     0.8157 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559443     3  0.1341     0.7512 0.000 0.000 0.944 0.056 0.000
#&gt; GSM559447     3  0.0000     0.8157 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559449     3  0.0000     0.8157 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559453     3  0.1608     0.7491 0.000 0.000 0.928 0.000 0.072
#&gt; GSM559466     3  0.0000     0.8157 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559474     5  0.3452     1.0000 0.000 0.000 0.244 0.000 0.756
#&gt; GSM559476     3  0.5697     0.0508 0.404 0.000 0.512 0.084 0.000
#&gt; GSM559483     2  0.0000     0.8530 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559484     5  0.3452     1.0000 0.000 0.000 0.244 0.000 0.756
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-4-a').click(function(){
  $('#tab-CV-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-5'>
<p><a id='tab-CV-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559433     1  0.0146      0.944 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM559434     6  0.2288      0.870 0.000 0.004 0.000 0.116 0.004 0.876
#&gt; GSM559436     1  0.0000      0.946 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559437     4  0.1168      0.631 0.016 0.000 0.000 0.956 0.000 0.028
#&gt; GSM559438     6  0.2553      0.742 0.144 0.000 0.000 0.008 0.000 0.848
#&gt; GSM559440     6  0.2361      0.874 0.012 0.004 0.000 0.104 0.000 0.880
#&gt; GSM559441     4  0.2868      0.673 0.132 0.000 0.000 0.840 0.000 0.028
#&gt; GSM559442     1  0.0000      0.946 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559444     6  0.2554      0.884 0.000 0.028 0.000 0.092 0.004 0.876
#&gt; GSM559445     4  0.1151      0.625 0.012 0.000 0.000 0.956 0.000 0.032
#&gt; GSM559446     4  0.1168      0.631 0.016 0.000 0.000 0.956 0.000 0.028
#&gt; GSM559448     1  0.2800      0.792 0.860 0.000 0.000 0.036 0.004 0.100
#&gt; GSM559450     6  0.2358      0.865 0.000 0.108 0.000 0.016 0.000 0.876
#&gt; GSM559451     1  0.0146      0.944 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM559452     6  0.2637      0.883 0.000 0.024 0.000 0.096 0.008 0.872
#&gt; GSM559454     1  0.0000      0.946 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559455     4  0.4338      0.307 0.484 0.000 0.000 0.496 0.000 0.020
#&gt; GSM559456     4  0.3734      0.651 0.264 0.000 0.000 0.716 0.000 0.020
#&gt; GSM559457     1  0.0508      0.935 0.984 0.000 0.000 0.012 0.000 0.004
#&gt; GSM559458     4  0.4131      0.592 0.356 0.000 0.000 0.624 0.000 0.020
#&gt; GSM559459     1  0.0000      0.946 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559460     1  0.0000      0.946 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559461     1  0.0000      0.946 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559462     1  0.0000      0.946 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559463     1  0.0363      0.936 0.988 0.000 0.000 0.012 0.000 0.000
#&gt; GSM559464     1  0.0000      0.946 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559465     1  0.0146      0.945 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; GSM559467     1  0.0547      0.931 0.980 0.000 0.000 0.000 0.000 0.020
#&gt; GSM559468     1  0.0000      0.946 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559469     1  0.0363      0.937 0.988 0.000 0.000 0.000 0.000 0.012
#&gt; GSM559470     1  0.6227     -0.410 0.376 0.000 0.000 0.320 0.004 0.300
#&gt; GSM559471     1  0.0000      0.946 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559472     1  0.0000      0.946 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559473     6  0.2730      0.793 0.000 0.192 0.000 0.000 0.000 0.808
#&gt; GSM559475     6  0.2278      0.853 0.000 0.128 0.000 0.004 0.000 0.868
#&gt; GSM559477     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559478     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559479     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559481     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559482     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.0000      0.875 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559439     3  0.0000      0.875 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559443     3  0.2740      0.769 0.000 0.000 0.852 0.028 0.000 0.120
#&gt; GSM559447     3  0.0000      0.875 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559449     3  0.0000      0.875 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559453     3  0.0458      0.866 0.000 0.000 0.984 0.000 0.016 0.000
#&gt; GSM559466     3  0.0000      0.875 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559474     5  0.0260      0.995 0.000 0.000 0.008 0.000 0.992 0.000
#&gt; GSM559476     3  0.6292      0.220 0.340 0.000 0.496 0.036 0.008 0.120
#&gt; GSM559483     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559484     5  0.0146      0.995 0.000 0.000 0.004 0.000 0.996 0.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-5-a').click(function(){
  $('#tab-CV-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-mclust-signature_compare](figure_cola/CV-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-mclust-collect-classes](figure_cola/CV-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>            n disease.state(p) k
#> CV:mclust 51         3.04e-02 2
#> CV:mclust 50         3.28e-09 3
#> CV:mclust 17         1.55e-03 4
#> CV:mclust 44         7.17e-08 5
#> CV:mclust 49         2.64e-08 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### CV:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "NMF"]
# you can also extract it by
# res = res_list["CV:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-NMF-collect-plots](figure_cola/CV-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-NMF-select-partition-number](figure_cola/CV-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.778           0.885       0.952         0.4737 0.527   0.527
#> 3 3 1.000           0.986       0.994         0.3120 0.729   0.538
#> 4 4 0.713           0.676       0.835         0.1624 0.839   0.591
#> 5 5 0.681           0.665       0.784         0.0893 0.865   0.548
#> 6 6 0.786           0.751       0.855         0.0470 0.902   0.605
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-classes'>
<ul>
<li><a href='#tab-CV-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-NMF-get-classes-1'>
<p><a id='tab-CV-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559433     1  0.2603     0.9260 0.956 0.044
#&gt; GSM559434     2  0.0000     0.9423 0.000 1.000
#&gt; GSM559436     1  0.0000     0.9488 1.000 0.000
#&gt; GSM559437     1  0.3733     0.9066 0.928 0.072
#&gt; GSM559438     2  0.0000     0.9423 0.000 1.000
#&gt; GSM559440     2  0.0000     0.9423 0.000 1.000
#&gt; GSM559441     1  0.9998     0.0482 0.508 0.492
#&gt; GSM559442     1  0.1184     0.9414 0.984 0.016
#&gt; GSM559444     2  0.0000     0.9423 0.000 1.000
#&gt; GSM559445     2  0.8443     0.5935 0.272 0.728
#&gt; GSM559446     1  0.5519     0.8590 0.872 0.128
#&gt; GSM559448     1  0.0000     0.9488 1.000 0.000
#&gt; GSM559450     2  0.0000     0.9423 0.000 1.000
#&gt; GSM559451     1  0.2778     0.9234 0.952 0.048
#&gt; GSM559452     2  0.0376     0.9396 0.004 0.996
#&gt; GSM559454     1  0.0000     0.9488 1.000 0.000
#&gt; GSM559455     1  0.4562     0.8880 0.904 0.096
#&gt; GSM559456     1  0.0000     0.9488 1.000 0.000
#&gt; GSM559457     1  0.0000     0.9488 1.000 0.000
#&gt; GSM559458     1  0.0000     0.9488 1.000 0.000
#&gt; GSM559459     1  0.0000     0.9488 1.000 0.000
#&gt; GSM559460     1  0.0000     0.9488 1.000 0.000
#&gt; GSM559461     1  0.0000     0.9488 1.000 0.000
#&gt; GSM559462     1  0.5629     0.8540 0.868 0.132
#&gt; GSM559463     1  0.0000     0.9488 1.000 0.000
#&gt; GSM559464     1  0.0000     0.9488 1.000 0.000
#&gt; GSM559465     1  0.0000     0.9488 1.000 0.000
#&gt; GSM559467     2  0.9977     0.0225 0.472 0.528
#&gt; GSM559468     1  0.0000     0.9488 1.000 0.000
#&gt; GSM559469     1  0.8081     0.6924 0.752 0.248
#&gt; GSM559470     2  0.2043     0.9173 0.032 0.968
#&gt; GSM559471     1  0.5294     0.8669 0.880 0.120
#&gt; GSM559472     1  0.0000     0.9488 1.000 0.000
#&gt; GSM559473     2  0.0000     0.9423 0.000 1.000
#&gt; GSM559475     2  0.0000     0.9423 0.000 1.000
#&gt; GSM559477     2  0.0000     0.9423 0.000 1.000
#&gt; GSM559478     2  0.0000     0.9423 0.000 1.000
#&gt; GSM559479     2  0.0000     0.9423 0.000 1.000
#&gt; GSM559480     2  0.0000     0.9423 0.000 1.000
#&gt; GSM559481     2  0.0000     0.9423 0.000 1.000
#&gt; GSM559482     2  0.0000     0.9423 0.000 1.000
#&gt; GSM559435     1  0.0000     0.9488 1.000 0.000
#&gt; GSM559439     1  0.0000     0.9488 1.000 0.000
#&gt; GSM559443     1  0.0000     0.9488 1.000 0.000
#&gt; GSM559447     1  0.0000     0.9488 1.000 0.000
#&gt; GSM559449     1  0.0000     0.9488 1.000 0.000
#&gt; GSM559453     1  0.0000     0.9488 1.000 0.000
#&gt; GSM559466     1  0.0000     0.9488 1.000 0.000
#&gt; GSM559474     1  0.5946     0.8194 0.856 0.144
#&gt; GSM559476     1  0.0000     0.9488 1.000 0.000
#&gt; GSM559483     2  0.0000     0.9423 0.000 1.000
#&gt; GSM559484     2  0.6531     0.7795 0.168 0.832
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-1-a').click(function(){
  $('#tab-CV-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-2'>
<p><a id='tab-CV-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559433     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM559434     2  0.0000      0.982 0.000 1.000 0.000
#&gt; GSM559436     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM559437     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM559438     2  0.4291      0.747 0.180 0.820 0.000
#&gt; GSM559440     2  0.0000      0.982 0.000 1.000 0.000
#&gt; GSM559441     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM559442     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM559444     2  0.0000      0.982 0.000 1.000 0.000
#&gt; GSM559445     1  0.1031      0.975 0.976 0.024 0.000
#&gt; GSM559446     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM559448     1  0.0424      0.990 0.992 0.000 0.008
#&gt; GSM559450     2  0.0000      0.982 0.000 1.000 0.000
#&gt; GSM559451     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM559452     2  0.0892      0.965 0.000 0.980 0.020
#&gt; GSM559454     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM559455     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM559456     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM559457     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM559458     1  0.1031      0.976 0.976 0.000 0.024
#&gt; GSM559459     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM559460     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM559461     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM559462     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM559463     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM559464     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM559465     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM559467     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM559468     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM559469     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM559470     1  0.1031      0.975 0.976 0.024 0.000
#&gt; GSM559471     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM559472     1  0.0000      0.997 1.000 0.000 0.000
#&gt; GSM559473     2  0.0000      0.982 0.000 1.000 0.000
#&gt; GSM559475     2  0.0000      0.982 0.000 1.000 0.000
#&gt; GSM559477     2  0.0000      0.982 0.000 1.000 0.000
#&gt; GSM559478     2  0.0000      0.982 0.000 1.000 0.000
#&gt; GSM559479     2  0.0000      0.982 0.000 1.000 0.000
#&gt; GSM559480     2  0.0000      0.982 0.000 1.000 0.000
#&gt; GSM559481     2  0.0000      0.982 0.000 1.000 0.000
#&gt; GSM559482     2  0.0000      0.982 0.000 1.000 0.000
#&gt; GSM559435     3  0.0000      0.997 0.000 0.000 1.000
#&gt; GSM559439     3  0.0000      0.997 0.000 0.000 1.000
#&gt; GSM559443     3  0.0000      0.997 0.000 0.000 1.000
#&gt; GSM559447     3  0.0000      0.997 0.000 0.000 1.000
#&gt; GSM559449     3  0.0000      0.997 0.000 0.000 1.000
#&gt; GSM559453     3  0.0000      0.997 0.000 0.000 1.000
#&gt; GSM559466     3  0.0000      0.997 0.000 0.000 1.000
#&gt; GSM559474     3  0.0000      0.997 0.000 0.000 1.000
#&gt; GSM559476     3  0.0424      0.989 0.008 0.000 0.992
#&gt; GSM559483     2  0.0000      0.982 0.000 1.000 0.000
#&gt; GSM559484     3  0.0747      0.984 0.000 0.016 0.984
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-2-a').click(function(){
  $('#tab-CV-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-3'>
<p><a id='tab-CV-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559433     1  0.0921    0.82599 0.972 0.000 0.000 0.028
#&gt; GSM559434     2  0.0188    0.92638 0.000 0.996 0.000 0.004
#&gt; GSM559436     4  0.4040    0.61257 0.248 0.000 0.000 0.752
#&gt; GSM559437     1  0.0657    0.82546 0.984 0.000 0.012 0.004
#&gt; GSM559438     2  0.5244    0.31115 0.388 0.600 0.000 0.012
#&gt; GSM559440     2  0.4819    0.52238 0.344 0.652 0.000 0.004
#&gt; GSM559441     1  0.0336    0.82961 0.992 0.000 0.000 0.008
#&gt; GSM559442     4  0.3801    0.61714 0.220 0.000 0.000 0.780
#&gt; GSM559444     2  0.1398    0.89766 0.040 0.956 0.000 0.004
#&gt; GSM559445     1  0.2799    0.72477 0.884 0.000 0.108 0.008
#&gt; GSM559446     1  0.2124    0.77146 0.924 0.000 0.068 0.008
#&gt; GSM559448     4  0.0921    0.41478 0.028 0.000 0.000 0.972
#&gt; GSM559450     2  0.0188    0.92638 0.000 0.996 0.000 0.004
#&gt; GSM559451     1  0.1118    0.82006 0.964 0.000 0.000 0.036
#&gt; GSM559452     2  0.1629    0.89801 0.000 0.952 0.024 0.024
#&gt; GSM559454     1  0.4933   -0.19512 0.568 0.000 0.000 0.432
#&gt; GSM559455     1  0.0000    0.83230 1.000 0.000 0.000 0.000
#&gt; GSM559456     1  0.0469    0.83290 0.988 0.000 0.000 0.012
#&gt; GSM559457     1  0.0336    0.83329 0.992 0.000 0.000 0.008
#&gt; GSM559458     1  0.3793    0.72332 0.844 0.000 0.044 0.112
#&gt; GSM559459     1  0.4585    0.22819 0.668 0.000 0.000 0.332
#&gt; GSM559460     1  0.4843   -0.00856 0.604 0.000 0.000 0.396
#&gt; GSM559461     4  0.4989    0.45322 0.472 0.000 0.000 0.528
#&gt; GSM559462     1  0.0707    0.83049 0.980 0.000 0.000 0.020
#&gt; GSM559463     4  0.1557    0.48859 0.056 0.000 0.000 0.944
#&gt; GSM559464     4  0.4898    0.54352 0.416 0.000 0.000 0.584
#&gt; GSM559465     4  0.4761    0.58118 0.372 0.000 0.000 0.628
#&gt; GSM559467     1  0.0188    0.83302 0.996 0.000 0.000 0.004
#&gt; GSM559468     4  0.4961    0.48680 0.448 0.000 0.000 0.552
#&gt; GSM559469     4  0.5070    0.53943 0.416 0.004 0.000 0.580
#&gt; GSM559470     1  0.0895    0.82024 0.976 0.020 0.000 0.004
#&gt; GSM559471     4  0.4961    0.49138 0.448 0.000 0.000 0.552
#&gt; GSM559472     4  0.4898    0.54960 0.416 0.000 0.000 0.584
#&gt; GSM559473     2  0.0000    0.92793 0.000 1.000 0.000 0.000
#&gt; GSM559475     2  0.0000    0.92793 0.000 1.000 0.000 0.000
#&gt; GSM559477     2  0.0000    0.92793 0.000 1.000 0.000 0.000
#&gt; GSM559478     2  0.0000    0.92793 0.000 1.000 0.000 0.000
#&gt; GSM559479     2  0.0000    0.92793 0.000 1.000 0.000 0.000
#&gt; GSM559480     2  0.0000    0.92793 0.000 1.000 0.000 0.000
#&gt; GSM559481     2  0.0000    0.92793 0.000 1.000 0.000 0.000
#&gt; GSM559482     2  0.0000    0.92793 0.000 1.000 0.000 0.000
#&gt; GSM559435     3  0.4972    0.74125 0.000 0.000 0.544 0.456
#&gt; GSM559439     3  0.4967    0.74448 0.000 0.000 0.548 0.452
#&gt; GSM559443     4  0.4072   -0.20395 0.000 0.000 0.252 0.748
#&gt; GSM559447     3  0.4661    0.81064 0.000 0.000 0.652 0.348
#&gt; GSM559449     3  0.3610    0.81758 0.000 0.000 0.800 0.200
#&gt; GSM559453     3  0.2011    0.79593 0.000 0.000 0.920 0.080
#&gt; GSM559466     3  0.4624    0.81302 0.000 0.000 0.660 0.340
#&gt; GSM559474     3  0.0188    0.76328 0.000 0.000 0.996 0.004
#&gt; GSM559476     4  0.3569   -0.03512 0.000 0.000 0.196 0.804
#&gt; GSM559483     2  0.0000    0.92793 0.000 1.000 0.000 0.000
#&gt; GSM559484     3  0.0336    0.76154 0.000 0.008 0.992 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-3-a').click(function(){
  $('#tab-CV-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-4'>
<p><a id='tab-CV-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559433     4  0.0451    0.94150 0.008 0.000 0.004 0.988 0.000
#&gt; GSM559434     2  0.5684    0.70802 0.248 0.644 0.096 0.008 0.004
#&gt; GSM559436     3  0.6146   -0.10914 0.240 0.000 0.560 0.200 0.000
#&gt; GSM559437     4  0.0162    0.94149 0.000 0.000 0.000 0.996 0.004
#&gt; GSM559438     2  0.6291    0.57063 0.368 0.516 0.096 0.020 0.000
#&gt; GSM559440     2  0.8130    0.31371 0.200 0.360 0.100 0.336 0.004
#&gt; GSM559441     4  0.0290    0.93895 0.008 0.000 0.000 0.992 0.000
#&gt; GSM559442     1  0.1502    0.66674 0.940 0.000 0.056 0.004 0.000
#&gt; GSM559444     2  0.6163    0.73306 0.144 0.676 0.100 0.076 0.004
#&gt; GSM559445     4  0.1704    0.88010 0.004 0.000 0.000 0.928 0.068
#&gt; GSM559446     4  0.1285    0.92032 0.004 0.000 0.004 0.956 0.036
#&gt; GSM559448     3  0.2230    0.50892 0.116 0.000 0.884 0.000 0.000
#&gt; GSM559450     2  0.4909    0.76750 0.144 0.744 0.100 0.008 0.004
#&gt; GSM559451     4  0.2017    0.87451 0.080 0.000 0.008 0.912 0.000
#&gt; GSM559452     1  0.6646   -0.00366 0.520 0.032 0.100 0.004 0.344
#&gt; GSM559454     1  0.5784    0.71678 0.616 0.000 0.208 0.176 0.000
#&gt; GSM559455     4  0.0162    0.94255 0.000 0.000 0.004 0.996 0.000
#&gt; GSM559456     4  0.0451    0.94192 0.008 0.000 0.004 0.988 0.000
#&gt; GSM559457     4  0.0324    0.94215 0.004 0.000 0.004 0.992 0.000
#&gt; GSM559458     1  0.4871    0.70558 0.760 0.000 0.032 0.128 0.080
#&gt; GSM559459     1  0.5761    0.72038 0.620 0.000 0.184 0.196 0.000
#&gt; GSM559460     1  0.3039    0.77418 0.836 0.000 0.012 0.152 0.000
#&gt; GSM559461     1  0.5680    0.70404 0.620 0.000 0.240 0.140 0.000
#&gt; GSM559462     4  0.3366    0.66628 0.212 0.000 0.004 0.784 0.000
#&gt; GSM559463     1  0.4735    0.39984 0.524 0.000 0.460 0.016 0.000
#&gt; GSM559464     1  0.3339    0.77693 0.836 0.000 0.040 0.124 0.000
#&gt; GSM559465     1  0.4717    0.76418 0.736 0.000 0.120 0.144 0.000
#&gt; GSM559467     4  0.1357    0.91473 0.048 0.000 0.004 0.948 0.000
#&gt; GSM559468     1  0.2921    0.77313 0.856 0.000 0.020 0.124 0.000
#&gt; GSM559469     1  0.2951    0.76899 0.860 0.000 0.028 0.112 0.000
#&gt; GSM559470     4  0.0162    0.94209 0.000 0.000 0.004 0.996 0.000
#&gt; GSM559471     1  0.3194    0.77635 0.832 0.000 0.020 0.148 0.000
#&gt; GSM559472     1  0.6482    0.56122 0.468 0.000 0.332 0.200 0.000
#&gt; GSM559473     2  0.2291    0.81597 0.036 0.908 0.056 0.000 0.000
#&gt; GSM559475     2  0.0671    0.82199 0.004 0.980 0.016 0.000 0.000
#&gt; GSM559477     2  0.0162    0.82591 0.000 0.996 0.004 0.000 0.000
#&gt; GSM559478     2  0.0671    0.82199 0.004 0.980 0.016 0.000 0.000
#&gt; GSM559479     2  0.4225    0.78493 0.112 0.788 0.096 0.000 0.004
#&gt; GSM559480     2  0.0671    0.82199 0.004 0.980 0.016 0.000 0.000
#&gt; GSM559481     2  0.0671    0.82199 0.004 0.980 0.016 0.000 0.000
#&gt; GSM559482     2  0.0162    0.82598 0.000 0.996 0.004 0.000 0.000
#&gt; GSM559435     3  0.4193    0.39865 0.012 0.000 0.684 0.000 0.304
#&gt; GSM559439     3  0.4290    0.39955 0.016 0.000 0.680 0.000 0.304
#&gt; GSM559443     3  0.3346    0.54173 0.092 0.000 0.844 0.000 0.064
#&gt; GSM559447     3  0.4287    0.02077 0.000 0.000 0.540 0.000 0.460
#&gt; GSM559449     5  0.4283   -0.03062 0.000 0.000 0.456 0.000 0.544
#&gt; GSM559453     5  0.3274    0.56251 0.000 0.000 0.220 0.000 0.780
#&gt; GSM559466     3  0.4291    0.00578 0.000 0.000 0.536 0.000 0.464
#&gt; GSM559474     5  0.0162    0.66632 0.000 0.000 0.004 0.000 0.996
#&gt; GSM559476     3  0.3812    0.54257 0.096 0.000 0.812 0.000 0.092
#&gt; GSM559483     2  0.0404    0.82510 0.000 0.988 0.012 0.000 0.000
#&gt; GSM559484     5  0.0162    0.66632 0.000 0.000 0.004 0.000 0.996
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-4-a').click(function(){
  $('#tab-CV-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-5'>
<p><a id='tab-CV-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559433     4  0.0551     0.9238 0.004 0.000 0.000 0.984 0.004 0.008
#&gt; GSM559434     6  0.2939     0.7088 0.024 0.052 0.000 0.040 0.008 0.876
#&gt; GSM559436     1  0.7094     0.0925 0.388 0.004 0.196 0.336 0.076 0.000
#&gt; GSM559437     4  0.0000     0.9235 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559438     6  0.2907     0.7260 0.028 0.096 0.000 0.016 0.000 0.860
#&gt; GSM559440     6  0.2664     0.6729 0.000 0.016 0.000 0.136 0.000 0.848
#&gt; GSM559441     4  0.1007     0.9129 0.000 0.000 0.000 0.956 0.000 0.044
#&gt; GSM559442     1  0.3844     0.6120 0.676 0.000 0.004 0.000 0.008 0.312
#&gt; GSM559444     6  0.4278     0.6593 0.000 0.212 0.000 0.076 0.000 0.712
#&gt; GSM559445     4  0.0865     0.9151 0.000 0.000 0.000 0.964 0.000 0.036
#&gt; GSM559446     4  0.0806     0.9222 0.008 0.000 0.000 0.972 0.000 0.020
#&gt; GSM559448     3  0.2398     0.8102 0.028 0.004 0.888 0.000 0.080 0.000
#&gt; GSM559450     6  0.3756     0.6139 0.000 0.268 0.000 0.020 0.000 0.712
#&gt; GSM559451     4  0.3304     0.7624 0.172 0.000 0.004 0.804 0.008 0.012
#&gt; GSM559452     6  0.2949     0.5256 0.140 0.000 0.000 0.000 0.028 0.832
#&gt; GSM559454     1  0.2389     0.7262 0.888 0.000 0.000 0.052 0.060 0.000
#&gt; GSM559455     4  0.0000     0.9235 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559456     4  0.0937     0.9107 0.040 0.000 0.000 0.960 0.000 0.000
#&gt; GSM559457     4  0.0790     0.9152 0.032 0.000 0.000 0.968 0.000 0.000
#&gt; GSM559458     1  0.6120     0.3967 0.500 0.000 0.008 0.008 0.184 0.300
#&gt; GSM559459     1  0.2328     0.7255 0.892 0.000 0.000 0.052 0.056 0.000
#&gt; GSM559460     1  0.2389     0.7249 0.864 0.000 0.000 0.000 0.008 0.128
#&gt; GSM559461     1  0.2992     0.7230 0.864 0.000 0.024 0.044 0.068 0.000
#&gt; GSM559462     1  0.4440     0.1993 0.556 0.000 0.000 0.420 0.008 0.016
#&gt; GSM559463     1  0.2884     0.7085 0.864 0.000 0.064 0.008 0.064 0.000
#&gt; GSM559464     1  0.1524     0.7402 0.932 0.000 0.000 0.000 0.008 0.060
#&gt; GSM559465     1  0.1080     0.7427 0.960 0.000 0.000 0.004 0.004 0.032
#&gt; GSM559467     4  0.3883     0.7489 0.144 0.088 0.000 0.768 0.000 0.000
#&gt; GSM559468     1  0.3490     0.6521 0.724 0.000 0.000 0.000 0.008 0.268
#&gt; GSM559469     1  0.3271     0.6698 0.760 0.000 0.000 0.000 0.008 0.232
#&gt; GSM559470     4  0.2106     0.8861 0.032 0.000 0.000 0.904 0.000 0.064
#&gt; GSM559471     1  0.1411     0.7418 0.936 0.000 0.000 0.000 0.004 0.060
#&gt; GSM559472     1  0.3896     0.6695 0.792 0.008 0.008 0.132 0.060 0.000
#&gt; GSM559473     2  0.3301     0.7318 0.008 0.772 0.000 0.000 0.004 0.216
#&gt; GSM559475     2  0.0405     0.8792 0.000 0.988 0.000 0.000 0.008 0.004
#&gt; GSM559477     2  0.2762     0.7873 0.000 0.804 0.000 0.000 0.000 0.196
#&gt; GSM559478     2  0.0146     0.8788 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559479     6  0.3756     0.3653 0.000 0.400 0.000 0.000 0.000 0.600
#&gt; GSM559480     2  0.0146     0.8788 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559481     2  0.0260     0.8774 0.000 0.992 0.000 0.000 0.008 0.000
#&gt; GSM559482     2  0.2300     0.8411 0.000 0.856 0.000 0.000 0.000 0.144
#&gt; GSM559435     3  0.0000     0.8821 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559439     3  0.0458     0.8830 0.000 0.000 0.984 0.000 0.016 0.000
#&gt; GSM559443     3  0.1643     0.8418 0.008 0.000 0.924 0.000 0.068 0.000
#&gt; GSM559447     3  0.0937     0.8789 0.000 0.000 0.960 0.000 0.040 0.000
#&gt; GSM559449     3  0.1610     0.8485 0.000 0.000 0.916 0.000 0.084 0.000
#&gt; GSM559453     3  0.3756     0.2721 0.000 0.000 0.600 0.000 0.400 0.000
#&gt; GSM559466     3  0.0937     0.8787 0.000 0.000 0.960 0.000 0.040 0.000
#&gt; GSM559474     5  0.1714     1.0000 0.000 0.000 0.092 0.000 0.908 0.000
#&gt; GSM559476     3  0.0725     0.8754 0.012 0.000 0.976 0.000 0.012 0.000
#&gt; GSM559483     2  0.1863     0.8620 0.000 0.896 0.000 0.000 0.000 0.104
#&gt; GSM559484     5  0.1714     1.0000 0.000 0.000 0.092 0.000 0.908 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-5-a').click(function(){
  $('#tab-CV-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-NMF-signature_compare](figure_cola/CV-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-CV-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-NMF-collect-classes](figure_cola/CV-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>         n disease.state(p) k
#> CV:NMF 50         2.99e-01 2
#> CV:NMF 52         8.38e-11 3
#> CV:NMF 41         9.19e-08 4
#> CV:NMF 43         5.48e-06 5
#> CV:NMF 47         7.36e-07 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:hclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "hclust"]
# you can also extract it by
# res = res_list["MAD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-hclust-collect-plots](figure_cola/MAD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-hclust-select-partition-number](figure_cola/MAD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.701           0.854       0.935         0.4330 0.581   0.581
#> 3 3 0.745           0.855       0.937         0.1293 0.947   0.909
#> 4 4 0.571           0.782       0.861         0.3372 0.837   0.692
#> 5 5 0.651           0.652       0.806         0.1062 0.863   0.642
#> 6 6 0.676           0.556       0.727         0.0539 0.884   0.636
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-classes'>
<ul>
<li><a href='#tab-MAD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-hclust-get-classes-1'>
<p><a id='tab-MAD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559433     1  0.0000      0.927 1.000 0.000
#&gt; GSM559434     2  0.5178      0.864 0.116 0.884
#&gt; GSM559436     1  0.0000      0.927 1.000 0.000
#&gt; GSM559437     1  0.0672      0.924 0.992 0.008
#&gt; GSM559438     2  0.7528      0.758 0.216 0.784
#&gt; GSM559440     2  0.7453      0.764 0.212 0.788
#&gt; GSM559441     1  0.1414      0.917 0.980 0.020
#&gt; GSM559442     1  0.9358      0.465 0.648 0.352
#&gt; GSM559444     2  0.0376      0.924 0.004 0.996
#&gt; GSM559445     1  0.0672      0.924 0.992 0.008
#&gt; GSM559446     1  0.0672      0.924 0.992 0.008
#&gt; GSM559448     1  0.0000      0.927 1.000 0.000
#&gt; GSM559450     2  0.0376      0.924 0.004 0.996
#&gt; GSM559451     1  0.0000      0.927 1.000 0.000
#&gt; GSM559452     2  0.8267      0.677 0.260 0.740
#&gt; GSM559454     1  0.0000      0.927 1.000 0.000
#&gt; GSM559455     1  0.1414      0.917 0.980 0.020
#&gt; GSM559456     1  0.0000      0.927 1.000 0.000
#&gt; GSM559457     1  0.0000      0.927 1.000 0.000
#&gt; GSM559458     1  0.3431      0.883 0.936 0.064
#&gt; GSM559459     1  0.0000      0.927 1.000 0.000
#&gt; GSM559460     1  0.0000      0.927 1.000 0.000
#&gt; GSM559461     1  0.0000      0.927 1.000 0.000
#&gt; GSM559462     1  0.0000      0.927 1.000 0.000
#&gt; GSM559463     1  0.0000      0.927 1.000 0.000
#&gt; GSM559464     1  0.0000      0.927 1.000 0.000
#&gt; GSM559465     1  0.0000      0.927 1.000 0.000
#&gt; GSM559467     1  0.8861      0.566 0.696 0.304
#&gt; GSM559468     1  0.5059      0.835 0.888 0.112
#&gt; GSM559469     1  0.9248      0.493 0.660 0.340
#&gt; GSM559470     1  0.8909      0.559 0.692 0.308
#&gt; GSM559471     1  0.1184      0.919 0.984 0.016
#&gt; GSM559472     1  0.0672      0.923 0.992 0.008
#&gt; GSM559473     2  0.3584      0.900 0.068 0.932
#&gt; GSM559475     2  0.3584      0.900 0.068 0.932
#&gt; GSM559477     2  0.0000      0.924 0.000 1.000
#&gt; GSM559478     2  0.0000      0.924 0.000 1.000
#&gt; GSM559479     2  0.0000      0.924 0.000 1.000
#&gt; GSM559480     2  0.0000      0.924 0.000 1.000
#&gt; GSM559481     2  0.0000      0.924 0.000 1.000
#&gt; GSM559482     2  0.0000      0.924 0.000 1.000
#&gt; GSM559435     1  0.0000      0.927 1.000 0.000
#&gt; GSM559439     1  0.0000      0.927 1.000 0.000
#&gt; GSM559443     1  0.0000      0.927 1.000 0.000
#&gt; GSM559447     1  0.0000      0.927 1.000 0.000
#&gt; GSM559449     1  0.0000      0.927 1.000 0.000
#&gt; GSM559453     1  0.0000      0.927 1.000 0.000
#&gt; GSM559466     1  0.0000      0.927 1.000 0.000
#&gt; GSM559474     1  0.9833      0.286 0.576 0.424
#&gt; GSM559476     1  0.0000      0.927 1.000 0.000
#&gt; GSM559483     2  0.0000      0.924 0.000 1.000
#&gt; GSM559484     1  0.9833      0.286 0.576 0.424
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-1-a').click(function(){
  $('#tab-MAD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-2'>
<p><a id='tab-MAD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559433     1  0.0000      0.931 1.000 0.000 0.000
#&gt; GSM559434     2  0.4196      0.800 0.112 0.864 0.024
#&gt; GSM559436     1  0.0000      0.931 1.000 0.000 0.000
#&gt; GSM559437     1  0.0592      0.928 0.988 0.000 0.012
#&gt; GSM559438     2  0.5921      0.677 0.212 0.756 0.032
#&gt; GSM559440     2  0.5874      0.682 0.208 0.760 0.032
#&gt; GSM559441     1  0.1031      0.921 0.976 0.000 0.024
#&gt; GSM559442     1  0.6906      0.463 0.644 0.324 0.032
#&gt; GSM559444     2  0.0829      0.871 0.004 0.984 0.012
#&gt; GSM559445     1  0.0592      0.928 0.988 0.000 0.012
#&gt; GSM559446     1  0.0592      0.928 0.988 0.000 0.012
#&gt; GSM559448     1  0.0000      0.931 1.000 0.000 0.000
#&gt; GSM559450     2  0.0829      0.871 0.004 0.984 0.012
#&gt; GSM559451     1  0.0000      0.931 1.000 0.000 0.000
#&gt; GSM559452     2  0.7331      0.558 0.256 0.672 0.072
#&gt; GSM559454     1  0.0000      0.931 1.000 0.000 0.000
#&gt; GSM559455     1  0.1031      0.921 0.976 0.000 0.024
#&gt; GSM559456     1  0.0000      0.931 1.000 0.000 0.000
#&gt; GSM559457     1  0.0000      0.931 1.000 0.000 0.000
#&gt; GSM559458     1  0.2261      0.892 0.932 0.000 0.068
#&gt; GSM559459     1  0.0000      0.931 1.000 0.000 0.000
#&gt; GSM559460     1  0.0000      0.931 1.000 0.000 0.000
#&gt; GSM559461     1  0.0000      0.931 1.000 0.000 0.000
#&gt; GSM559462     1  0.0000      0.931 1.000 0.000 0.000
#&gt; GSM559463     1  0.0000      0.931 1.000 0.000 0.000
#&gt; GSM559464     1  0.0000      0.931 1.000 0.000 0.000
#&gt; GSM559465     1  0.0000      0.931 1.000 0.000 0.000
#&gt; GSM559467     1  0.6475      0.566 0.692 0.280 0.028
#&gt; GSM559468     1  0.3722      0.843 0.888 0.088 0.024
#&gt; GSM559469     1  0.6828      0.491 0.656 0.312 0.032
#&gt; GSM559470     1  0.6507      0.558 0.688 0.284 0.028
#&gt; GSM559471     1  0.0848      0.926 0.984 0.008 0.008
#&gt; GSM559472     1  0.0424      0.929 0.992 0.000 0.008
#&gt; GSM559473     2  0.3045      0.843 0.064 0.916 0.020
#&gt; GSM559475     2  0.3045      0.843 0.064 0.916 0.020
#&gt; GSM559477     2  0.0000      0.873 0.000 1.000 0.000
#&gt; GSM559478     2  0.0000      0.873 0.000 1.000 0.000
#&gt; GSM559479     2  0.0000      0.873 0.000 1.000 0.000
#&gt; GSM559480     2  0.0000      0.873 0.000 1.000 0.000
#&gt; GSM559481     2  0.0000      0.873 0.000 1.000 0.000
#&gt; GSM559482     2  0.0000      0.873 0.000 1.000 0.000
#&gt; GSM559435     1  0.0237      0.930 0.996 0.000 0.004
#&gt; GSM559439     1  0.0237      0.930 0.996 0.000 0.004
#&gt; GSM559443     1  0.0237      0.930 0.996 0.000 0.004
#&gt; GSM559447     1  0.0747      0.925 0.984 0.000 0.016
#&gt; GSM559449     1  0.2878      0.859 0.904 0.000 0.096
#&gt; GSM559453     1  0.5948      0.455 0.640 0.000 0.360
#&gt; GSM559466     1  0.0747      0.925 0.984 0.000 0.016
#&gt; GSM559474     3  0.0237      1.000 0.000 0.004 0.996
#&gt; GSM559476     1  0.0237      0.930 0.996 0.000 0.004
#&gt; GSM559483     2  0.0000      0.873 0.000 1.000 0.000
#&gt; GSM559484     3  0.0237      1.000 0.000 0.004 0.996
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-2-a').click(function(){
  $('#tab-MAD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-3'>
<p><a id='tab-MAD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559433     1  0.0592      0.821 0.984 0.000 0.016 0.000
#&gt; GSM559434     2  0.3899      0.832 0.052 0.840 0.108 0.000
#&gt; GSM559436     1  0.1389      0.816 0.952 0.000 0.048 0.000
#&gt; GSM559437     1  0.2345      0.795 0.900 0.000 0.100 0.000
#&gt; GSM559438     2  0.5630      0.723 0.136 0.724 0.140 0.000
#&gt; GSM559440     2  0.5581      0.728 0.132 0.728 0.140 0.000
#&gt; GSM559441     1  0.2868      0.780 0.864 0.000 0.136 0.000
#&gt; GSM559442     1  0.7664      0.211 0.460 0.292 0.248 0.000
#&gt; GSM559444     2  0.1356      0.889 0.008 0.960 0.032 0.000
#&gt; GSM559445     1  0.2345      0.795 0.900 0.000 0.100 0.000
#&gt; GSM559446     1  0.2345      0.795 0.900 0.000 0.100 0.000
#&gt; GSM559448     1  0.1389      0.816 0.952 0.000 0.048 0.000
#&gt; GSM559450     2  0.1356      0.889 0.008 0.960 0.032 0.000
#&gt; GSM559451     1  0.0817      0.821 0.976 0.000 0.024 0.000
#&gt; GSM559452     2  0.6917      0.639 0.080 0.640 0.240 0.040
#&gt; GSM559454     1  0.1389      0.816 0.952 0.000 0.048 0.000
#&gt; GSM559455     1  0.2868      0.780 0.864 0.000 0.136 0.000
#&gt; GSM559456     1  0.4855      0.209 0.600 0.000 0.400 0.000
#&gt; GSM559457     1  0.0707      0.822 0.980 0.000 0.020 0.000
#&gt; GSM559458     1  0.4888      0.688 0.740 0.000 0.224 0.036
#&gt; GSM559459     1  0.1389      0.816 0.952 0.000 0.048 0.000
#&gt; GSM559460     1  0.1557      0.814 0.944 0.000 0.056 0.000
#&gt; GSM559461     1  0.1557      0.814 0.944 0.000 0.056 0.000
#&gt; GSM559462     1  0.0817      0.821 0.976 0.000 0.024 0.000
#&gt; GSM559463     1  0.1389      0.816 0.952 0.000 0.048 0.000
#&gt; GSM559464     1  0.1557      0.814 0.944 0.000 0.056 0.000
#&gt; GSM559465     1  0.1867      0.806 0.928 0.000 0.072 0.000
#&gt; GSM559467     1  0.6690      0.475 0.608 0.248 0.144 0.000
#&gt; GSM559468     1  0.4364      0.721 0.808 0.056 0.136 0.000
#&gt; GSM559469     1  0.7622      0.248 0.472 0.280 0.248 0.000
#&gt; GSM559470     1  0.6715      0.466 0.604 0.252 0.144 0.000
#&gt; GSM559471     1  0.1356      0.819 0.960 0.008 0.032 0.000
#&gt; GSM559472     1  0.1118      0.822 0.964 0.000 0.036 0.000
#&gt; GSM559473     2  0.2796      0.862 0.016 0.892 0.092 0.000
#&gt; GSM559475     2  0.2796      0.862 0.016 0.892 0.092 0.000
#&gt; GSM559477     2  0.0336      0.888 0.000 0.992 0.008 0.000
#&gt; GSM559478     2  0.0000      0.890 0.000 1.000 0.000 0.000
#&gt; GSM559479     2  0.0336      0.888 0.000 0.992 0.008 0.000
#&gt; GSM559480     2  0.0000      0.890 0.000 1.000 0.000 0.000
#&gt; GSM559481     2  0.0000      0.890 0.000 1.000 0.000 0.000
#&gt; GSM559482     2  0.0336      0.888 0.000 0.992 0.008 0.000
#&gt; GSM559435     3  0.3610      0.919 0.200 0.000 0.800 0.000
#&gt; GSM559439     3  0.3649      0.918 0.204 0.000 0.796 0.000
#&gt; GSM559443     3  0.3649      0.918 0.204 0.000 0.796 0.000
#&gt; GSM559447     3  0.3852      0.916 0.192 0.000 0.800 0.008
#&gt; GSM559449     3  0.4920      0.839 0.136 0.000 0.776 0.088
#&gt; GSM559453     3  0.6785      0.470 0.108 0.000 0.540 0.352
#&gt; GSM559466     3  0.3852      0.916 0.192 0.000 0.800 0.008
#&gt; GSM559474     4  0.0000      1.000 0.000 0.000 0.000 1.000
#&gt; GSM559476     3  0.3649      0.918 0.204 0.000 0.796 0.000
#&gt; GSM559483     2  0.0336      0.888 0.000 0.992 0.008 0.000
#&gt; GSM559484     4  0.0000      1.000 0.000 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-3-a').click(function(){
  $('#tab-MAD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-4'>
<p><a id='tab-MAD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559433     1   0.161      0.846 0.928 0.000 0.000 0.072 0.000
#&gt; GSM559434     4   0.475     -0.201 0.016 0.484 0.000 0.500 0.000
#&gt; GSM559436     1   0.029      0.847 0.992 0.000 0.008 0.000 0.000
#&gt; GSM559437     1   0.489      0.676 0.680 0.000 0.064 0.256 0.000
#&gt; GSM559438     4   0.499      0.235 0.044 0.340 0.000 0.616 0.000
#&gt; GSM559440     4   0.494      0.226 0.040 0.344 0.000 0.616 0.000
#&gt; GSM559441     1   0.450      0.695 0.712 0.000 0.044 0.244 0.000
#&gt; GSM559442     4   0.557      0.363 0.364 0.000 0.080 0.556 0.000
#&gt; GSM559444     2   0.269      0.691 0.000 0.844 0.000 0.156 0.000
#&gt; GSM559445     1   0.489      0.676 0.680 0.000 0.064 0.256 0.000
#&gt; GSM559446     1   0.489      0.676 0.680 0.000 0.064 0.256 0.000
#&gt; GSM559448     1   0.029      0.847 0.992 0.000 0.008 0.000 0.000
#&gt; GSM559450     2   0.269      0.691 0.000 0.844 0.000 0.156 0.000
#&gt; GSM559451     1   0.228      0.829 0.880 0.000 0.000 0.120 0.000
#&gt; GSM559452     4   0.607      0.240 0.000 0.260 0.080 0.620 0.040
#&gt; GSM559454     1   0.029      0.847 0.992 0.000 0.008 0.000 0.000
#&gt; GSM559455     1   0.450      0.695 0.712 0.000 0.044 0.244 0.000
#&gt; GSM559456     3   0.642     -0.174 0.412 0.000 0.416 0.172 0.000
#&gt; GSM559457     1   0.213      0.843 0.908 0.000 0.012 0.080 0.000
#&gt; GSM559458     1   0.594      0.591 0.660 0.000 0.112 0.192 0.036
#&gt; GSM559459     1   0.029      0.847 0.992 0.000 0.008 0.000 0.000
#&gt; GSM559460     1   0.051      0.845 0.984 0.000 0.016 0.000 0.000
#&gt; GSM559461     1   0.051      0.845 0.984 0.000 0.016 0.000 0.000
#&gt; GSM559462     1   0.218      0.832 0.888 0.000 0.000 0.112 0.000
#&gt; GSM559463     1   0.029      0.847 0.992 0.000 0.008 0.000 0.000
#&gt; GSM559464     1   0.051      0.845 0.984 0.000 0.016 0.000 0.000
#&gt; GSM559465     1   0.088      0.838 0.968 0.000 0.032 0.000 0.000
#&gt; GSM559467     4   0.432      0.187 0.396 0.004 0.000 0.600 0.000
#&gt; GSM559468     1   0.423      0.659 0.748 0.000 0.044 0.208 0.000
#&gt; GSM559469     4   0.560      0.338 0.376 0.000 0.080 0.544 0.000
#&gt; GSM559470     4   0.430      0.210 0.388 0.004 0.000 0.608 0.000
#&gt; GSM559471     1   0.262      0.827 0.872 0.000 0.012 0.116 0.000
#&gt; GSM559472     1   0.252      0.835 0.884 0.000 0.016 0.100 0.000
#&gt; GSM559473     2   0.420      0.348 0.000 0.592 0.000 0.408 0.000
#&gt; GSM559475     2   0.420      0.348 0.000 0.592 0.000 0.408 0.000
#&gt; GSM559477     2   0.000      0.766 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559478     2   0.399      0.678 0.000 0.720 0.012 0.268 0.000
#&gt; GSM559479     2   0.000      0.766 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559480     2   0.399      0.678 0.000 0.720 0.012 0.268 0.000
#&gt; GSM559481     2   0.399      0.678 0.000 0.720 0.012 0.268 0.000
#&gt; GSM559482     2   0.000      0.766 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559435     3   0.179      0.817 0.084 0.000 0.916 0.000 0.000
#&gt; GSM559439     3   0.185      0.817 0.088 0.000 0.912 0.000 0.000
#&gt; GSM559443     3   0.185      0.817 0.088 0.000 0.912 0.000 0.000
#&gt; GSM559447     3   0.196      0.814 0.076 0.000 0.916 0.000 0.008
#&gt; GSM559449     3   0.235      0.733 0.016 0.000 0.896 0.000 0.088
#&gt; GSM559453     3   0.403      0.362 0.000 0.000 0.648 0.000 0.352
#&gt; GSM559466     3   0.196      0.814 0.076 0.000 0.916 0.000 0.008
#&gt; GSM559474     5   0.000      1.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559476     3   0.185      0.817 0.088 0.000 0.912 0.000 0.000
#&gt; GSM559483     2   0.000      0.766 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559484     5   0.000      1.000 0.000 0.000 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-4-a').click(function(){
  $('#tab-MAD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-5'>
<p><a id='tab-MAD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; GSM559433     1  0.1753    0.72676 0.912 0.000 0.000 0.084 0.000 NA
#&gt; GSM559434     2  0.5671    0.32562 0.016 0.472 0.000 0.412 0.000 NA
#&gt; GSM559436     1  0.0547    0.73867 0.980 0.000 0.000 0.000 0.000 NA
#&gt; GSM559437     4  0.6367    0.18410 0.332 0.000 0.012 0.384 0.000 NA
#&gt; GSM559438     4  0.5486   -0.15228 0.020 0.332 0.000 0.560 0.000 NA
#&gt; GSM559440     4  0.5420   -0.16221 0.016 0.336 0.000 0.560 0.000 NA
#&gt; GSM559441     1  0.5579    0.33768 0.532 0.000 0.008 0.336 0.000 NA
#&gt; GSM559442     1  0.6232   -0.13094 0.356 0.000 0.004 0.348 0.000 NA
#&gt; GSM559444     2  0.2821    0.68641 0.000 0.832 0.000 0.152 0.000 NA
#&gt; GSM559445     4  0.6367    0.18410 0.332 0.000 0.012 0.384 0.000 NA
#&gt; GSM559446     4  0.6367    0.18410 0.332 0.000 0.012 0.384 0.000 NA
#&gt; GSM559448     1  0.0547    0.73867 0.980 0.000 0.000 0.000 0.000 NA
#&gt; GSM559450     2  0.2821    0.68641 0.000 0.832 0.000 0.152 0.000 NA
#&gt; GSM559451     1  0.3163    0.63510 0.764 0.000 0.000 0.232 0.000 NA
#&gt; GSM559452     4  0.6761   -0.11207 0.000 0.248 0.004 0.420 0.036 NA
#&gt; GSM559454     1  0.0146    0.74165 0.996 0.000 0.000 0.004 0.000 NA
#&gt; GSM559455     1  0.5579    0.33768 0.532 0.000 0.008 0.336 0.000 NA
#&gt; GSM559456     4  0.7143   -0.00682 0.124 0.000 0.156 0.400 0.000 NA
#&gt; GSM559457     1  0.2445    0.71628 0.872 0.000 0.000 0.108 0.000 NA
#&gt; GSM559458     1  0.6773    0.27877 0.500 0.000 0.024 0.208 0.032 NA
#&gt; GSM559459     1  0.0000    0.74088 1.000 0.000 0.000 0.000 0.000 NA
#&gt; GSM559460     1  0.0363    0.74001 0.988 0.000 0.000 0.000 0.000 NA
#&gt; GSM559461     1  0.0363    0.74001 0.988 0.000 0.000 0.000 0.000 NA
#&gt; GSM559462     1  0.3109    0.64181 0.772 0.000 0.000 0.224 0.000 NA
#&gt; GSM559463     1  0.0547    0.73867 0.980 0.000 0.000 0.000 0.000 NA
#&gt; GSM559464     1  0.0363    0.74001 0.988 0.000 0.000 0.000 0.000 NA
#&gt; GSM559465     1  0.0862    0.73590 0.972 0.000 0.004 0.008 0.000 NA
#&gt; GSM559467     4  0.3337    0.31954 0.260 0.000 0.000 0.736 0.000 NA
#&gt; GSM559468     1  0.4382    0.58000 0.720 0.000 0.000 0.156 0.000 NA
#&gt; GSM559469     1  0.6224   -0.10704 0.368 0.000 0.004 0.340 0.000 NA
#&gt; GSM559470     4  0.3290    0.32931 0.252 0.000 0.000 0.744 0.000 NA
#&gt; GSM559471     1  0.3699    0.64653 0.752 0.000 0.000 0.212 0.000 NA
#&gt; GSM559472     1  0.3572    0.65206 0.764 0.000 0.000 0.204 0.000 NA
#&gt; GSM559473     2  0.4228    0.49661 0.000 0.588 0.000 0.392 0.000 NA
#&gt; GSM559475     2  0.4228    0.49661 0.000 0.588 0.000 0.392 0.000 NA
#&gt; GSM559477     2  0.0000    0.73107 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM559478     2  0.4525    0.59109 0.000 0.664 0.004 0.056 0.000 NA
#&gt; GSM559479     2  0.0000    0.73107 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM559480     2  0.4525    0.59109 0.000 0.664 0.004 0.056 0.000 NA
#&gt; GSM559481     2  0.4525    0.59109 0.000 0.664 0.004 0.056 0.000 NA
#&gt; GSM559482     2  0.0000    0.73107 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM559435     3  0.1866    0.91726 0.084 0.000 0.908 0.000 0.000 NA
#&gt; GSM559439     3  0.1918    0.91654 0.088 0.000 0.904 0.000 0.000 NA
#&gt; GSM559443     3  0.1918    0.91654 0.088 0.000 0.904 0.000 0.000 NA
#&gt; GSM559447     3  0.1501    0.91467 0.076 0.000 0.924 0.000 0.000 NA
#&gt; GSM559449     3  0.2006    0.83641 0.016 0.000 0.904 0.000 0.080 NA
#&gt; GSM559453     3  0.3592    0.47605 0.000 0.000 0.656 0.000 0.344 NA
#&gt; GSM559466     3  0.1501    0.91467 0.076 0.000 0.924 0.000 0.000 NA
#&gt; GSM559474     5  0.0000    1.00000 0.000 0.000 0.000 0.000 1.000 NA
#&gt; GSM559476     3  0.1918    0.91654 0.088 0.000 0.904 0.000 0.000 NA
#&gt; GSM559483     2  0.0000    0.73107 0.000 1.000 0.000 0.000 0.000 NA
#&gt; GSM559484     5  0.0000    1.00000 0.000 0.000 0.000 0.000 1.000 NA
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-5-a').click(function(){
  $('#tab-MAD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-hclust-signature_compare](figure_cola/MAD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-hclust-collect-classes](figure_cola/MAD-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) k
#> MAD:hclust 48         2.95e-01 2
#> MAD:hclust 49         8.29e-03 3
#> MAD:hclust 46         8.29e-09 4
#> MAD:hclust 40         1.07e-07 5
#> MAD:hclust 34         1.58e-06 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:kmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "kmeans"]
# you can also extract it by
# res = res_list["MAD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-kmeans-collect-plots](figure_cola/MAD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-kmeans-select-partition-number](figure_cola/MAD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.471           0.889       0.897         0.4345 0.566   0.566
#> 3 3 0.742           0.902       0.932         0.4063 0.817   0.676
#> 4 4 0.747           0.732       0.870         0.1285 0.937   0.837
#> 5 5 0.691           0.635       0.807         0.1013 0.880   0.647
#> 6 6 0.688           0.592       0.757         0.0655 0.941   0.758
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-kmeans-get-classes-1'>
<p><a id='tab-MAD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559433     1   0.000      0.901 1.000 0.000
#&gt; GSM559434     2   0.680      0.985 0.180 0.820
#&gt; GSM559436     1   0.000      0.901 1.000 0.000
#&gt; GSM559437     1   0.295      0.888 0.948 0.052
#&gt; GSM559438     2   0.680      0.985 0.180 0.820
#&gt; GSM559440     2   0.680      0.985 0.180 0.820
#&gt; GSM559441     1   0.662      0.704 0.828 0.172
#&gt; GSM559442     1   0.000      0.901 1.000 0.000
#&gt; GSM559444     2   0.680      0.985 0.180 0.820
#&gt; GSM559445     1   0.760      0.620 0.780 0.220
#&gt; GSM559446     1   0.000      0.901 1.000 0.000
#&gt; GSM559448     1   0.118      0.898 0.984 0.016
#&gt; GSM559450     2   0.680      0.985 0.180 0.820
#&gt; GSM559451     1   0.000      0.901 1.000 0.000
#&gt; GSM559452     2   0.680      0.985 0.180 0.820
#&gt; GSM559454     1   0.000      0.901 1.000 0.000
#&gt; GSM559455     1   0.000      0.901 1.000 0.000
#&gt; GSM559456     1   0.634      0.843 0.840 0.160
#&gt; GSM559457     1   0.000      0.901 1.000 0.000
#&gt; GSM559458     1   0.662      0.838 0.828 0.172
#&gt; GSM559459     1   0.000      0.901 1.000 0.000
#&gt; GSM559460     1   0.000      0.901 1.000 0.000
#&gt; GSM559461     1   0.000      0.901 1.000 0.000
#&gt; GSM559462     1   0.000      0.901 1.000 0.000
#&gt; GSM559463     1   0.118      0.898 0.984 0.016
#&gt; GSM559464     1   0.000      0.901 1.000 0.000
#&gt; GSM559465     1   0.373      0.881 0.928 0.072
#&gt; GSM559467     1   0.000      0.901 1.000 0.000
#&gt; GSM559468     1   0.000      0.901 1.000 0.000
#&gt; GSM559469     1   0.000      0.901 1.000 0.000
#&gt; GSM559470     1   0.767      0.612 0.776 0.224
#&gt; GSM559471     1   0.000      0.901 1.000 0.000
#&gt; GSM559472     1   0.000      0.901 1.000 0.000
#&gt; GSM559473     2   0.680      0.985 0.180 0.820
#&gt; GSM559475     2   0.680      0.985 0.180 0.820
#&gt; GSM559477     2   0.680      0.985 0.180 0.820
#&gt; GSM559478     2   0.680      0.985 0.180 0.820
#&gt; GSM559479     2   0.680      0.985 0.180 0.820
#&gt; GSM559480     2   0.680      0.985 0.180 0.820
#&gt; GSM559481     2   0.680      0.985 0.180 0.820
#&gt; GSM559482     2   0.680      0.985 0.180 0.820
#&gt; GSM559435     1   0.680      0.833 0.820 0.180
#&gt; GSM559439     1   0.680      0.833 0.820 0.180
#&gt; GSM559443     1   0.680      0.833 0.820 0.180
#&gt; GSM559447     1   0.680      0.833 0.820 0.180
#&gt; GSM559449     1   0.680      0.833 0.820 0.180
#&gt; GSM559453     1   0.680      0.833 0.820 0.180
#&gt; GSM559466     1   0.680      0.833 0.820 0.180
#&gt; GSM559474     1   0.971      0.613 0.600 0.400
#&gt; GSM559476     1   0.680      0.833 0.820 0.180
#&gt; GSM559483     2   0.680      0.985 0.180 0.820
#&gt; GSM559484     2   0.000      0.777 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-2'>
<p><a id='tab-MAD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559433     1  0.0237      0.953 0.996 0.000 0.004
#&gt; GSM559434     2  0.3030      0.910 0.004 0.904 0.092
#&gt; GSM559436     1  0.0000      0.953 1.000 0.000 0.000
#&gt; GSM559437     1  0.3851      0.865 0.860 0.004 0.136
#&gt; GSM559438     2  0.7372      0.614 0.220 0.688 0.092
#&gt; GSM559440     2  0.3030      0.910 0.004 0.904 0.092
#&gt; GSM559441     1  0.3771      0.875 0.876 0.012 0.112
#&gt; GSM559442     1  0.0237      0.952 0.996 0.000 0.004
#&gt; GSM559444     2  0.2400      0.923 0.004 0.932 0.064
#&gt; GSM559445     1  0.6486      0.750 0.760 0.096 0.144
#&gt; GSM559446     1  0.3918      0.861 0.856 0.004 0.140
#&gt; GSM559448     1  0.0000      0.953 1.000 0.000 0.000
#&gt; GSM559450     2  0.2200      0.926 0.004 0.940 0.056
#&gt; GSM559451     1  0.0237      0.953 0.996 0.000 0.004
#&gt; GSM559452     2  0.3272      0.904 0.004 0.892 0.104
#&gt; GSM559454     1  0.0000      0.953 1.000 0.000 0.000
#&gt; GSM559455     1  0.1647      0.937 0.960 0.004 0.036
#&gt; GSM559456     1  0.1267      0.943 0.972 0.004 0.024
#&gt; GSM559457     1  0.0424      0.952 0.992 0.000 0.008
#&gt; GSM559458     1  0.0829      0.945 0.984 0.004 0.012
#&gt; GSM559459     1  0.0000      0.953 1.000 0.000 0.000
#&gt; GSM559460     1  0.0000      0.953 1.000 0.000 0.000
#&gt; GSM559461     1  0.0000      0.953 1.000 0.000 0.000
#&gt; GSM559462     1  0.0424      0.952 0.992 0.000 0.008
#&gt; GSM559463     1  0.0000      0.953 1.000 0.000 0.000
#&gt; GSM559464     1  0.0000      0.953 1.000 0.000 0.000
#&gt; GSM559465     1  0.0000      0.953 1.000 0.000 0.000
#&gt; GSM559467     1  0.3112      0.893 0.900 0.004 0.096
#&gt; GSM559468     1  0.0000      0.953 1.000 0.000 0.000
#&gt; GSM559469     1  0.1989      0.927 0.948 0.004 0.048
#&gt; GSM559470     1  0.4892      0.839 0.840 0.048 0.112
#&gt; GSM559471     1  0.0475      0.951 0.992 0.004 0.004
#&gt; GSM559472     1  0.0000      0.953 1.000 0.000 0.000
#&gt; GSM559473     2  0.0661      0.936 0.004 0.988 0.008
#&gt; GSM559475     2  0.0475      0.936 0.004 0.992 0.004
#&gt; GSM559477     2  0.0475      0.935 0.004 0.992 0.004
#&gt; GSM559478     2  0.0475      0.936 0.004 0.992 0.004
#&gt; GSM559479     2  0.0475      0.935 0.004 0.992 0.004
#&gt; GSM559480     2  0.0475      0.936 0.004 0.992 0.004
#&gt; GSM559481     2  0.0475      0.936 0.004 0.992 0.004
#&gt; GSM559482     2  0.0475      0.935 0.004 0.992 0.004
#&gt; GSM559435     3  0.3619      0.917 0.136 0.000 0.864
#&gt; GSM559439     3  0.3619      0.917 0.136 0.000 0.864
#&gt; GSM559443     3  0.3619      0.917 0.136 0.000 0.864
#&gt; GSM559447     3  0.3619      0.917 0.136 0.000 0.864
#&gt; GSM559449     3  0.3340      0.909 0.120 0.000 0.880
#&gt; GSM559453     3  0.3340      0.909 0.120 0.000 0.880
#&gt; GSM559466     3  0.3619      0.917 0.136 0.000 0.864
#&gt; GSM559474     3  0.1860      0.737 0.000 0.052 0.948
#&gt; GSM559476     3  0.6280      0.371 0.460 0.000 0.540
#&gt; GSM559483     2  0.0475      0.935 0.004 0.992 0.004
#&gt; GSM559484     2  0.5098      0.762 0.000 0.752 0.248
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-3'>
<p><a id='tab-MAD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559433     1  0.0188     0.8850 0.996 0.000 0.004 0.000
#&gt; GSM559434     2  0.1902     0.6498 0.004 0.932 0.000 0.064
#&gt; GSM559436     1  0.0524     0.8811 0.988 0.000 0.004 0.008
#&gt; GSM559437     4  0.7519     0.5040 0.364 0.136 0.012 0.488
#&gt; GSM559438     2  0.5861     0.2827 0.296 0.644 0.000 0.060
#&gt; GSM559440     2  0.1824     0.6525 0.004 0.936 0.000 0.060
#&gt; GSM559441     1  0.7054    -0.0399 0.536 0.144 0.000 0.320
#&gt; GSM559442     1  0.0376     0.8820 0.992 0.004 0.000 0.004
#&gt; GSM559444     2  0.0469     0.6666 0.000 0.988 0.000 0.012
#&gt; GSM559445     4  0.7810     0.7648 0.192 0.308 0.012 0.488
#&gt; GSM559446     4  0.7875     0.7693 0.216 0.284 0.012 0.488
#&gt; GSM559448     1  0.0524     0.8811 0.988 0.000 0.004 0.008
#&gt; GSM559450     2  0.0336     0.6694 0.000 0.992 0.000 0.008
#&gt; GSM559451     1  0.0469     0.8815 0.988 0.000 0.000 0.012
#&gt; GSM559452     2  0.2466     0.5791 0.004 0.900 0.000 0.096
#&gt; GSM559454     1  0.0188     0.8850 0.996 0.000 0.004 0.000
#&gt; GSM559455     1  0.4356     0.5490 0.708 0.000 0.000 0.292
#&gt; GSM559456     1  0.4584     0.5319 0.696 0.000 0.004 0.300
#&gt; GSM559457     1  0.1661     0.8532 0.944 0.000 0.004 0.052
#&gt; GSM559458     1  0.4868     0.5101 0.684 0.000 0.012 0.304
#&gt; GSM559459     1  0.0188     0.8850 0.996 0.000 0.004 0.000
#&gt; GSM559460     1  0.0188     0.8850 0.996 0.000 0.004 0.000
#&gt; GSM559461     1  0.0188     0.8850 0.996 0.000 0.004 0.000
#&gt; GSM559462     1  0.0188     0.8839 0.996 0.000 0.000 0.004
#&gt; GSM559463     1  0.0524     0.8811 0.988 0.000 0.004 0.008
#&gt; GSM559464     1  0.0188     0.8850 0.996 0.000 0.004 0.000
#&gt; GSM559465     1  0.0188     0.8850 0.996 0.000 0.004 0.000
#&gt; GSM559467     1  0.3870     0.6813 0.788 0.004 0.000 0.208
#&gt; GSM559468     1  0.0188     0.8835 0.996 0.000 0.000 0.004
#&gt; GSM559469     1  0.0657     0.8762 0.984 0.004 0.000 0.012
#&gt; GSM559470     1  0.5998     0.4802 0.680 0.108 0.000 0.212
#&gt; GSM559471     1  0.0376     0.8820 0.992 0.004 0.000 0.004
#&gt; GSM559472     1  0.0188     0.8850 0.996 0.000 0.004 0.000
#&gt; GSM559473     2  0.4605     0.7877 0.000 0.664 0.000 0.336
#&gt; GSM559475     2  0.4605     0.7877 0.000 0.664 0.000 0.336
#&gt; GSM559477     2  0.4454     0.7872 0.000 0.692 0.000 0.308
#&gt; GSM559478     2  0.4955     0.7856 0.000 0.648 0.008 0.344
#&gt; GSM559479     2  0.4454     0.7872 0.000 0.692 0.000 0.308
#&gt; GSM559480     2  0.4955     0.7856 0.000 0.648 0.008 0.344
#&gt; GSM559481     2  0.4955     0.7856 0.000 0.648 0.008 0.344
#&gt; GSM559482     2  0.4454     0.7872 0.000 0.692 0.000 0.308
#&gt; GSM559435     3  0.0707     0.8984 0.020 0.000 0.980 0.000
#&gt; GSM559439     3  0.0707     0.8984 0.020 0.000 0.980 0.000
#&gt; GSM559443     3  0.1042     0.8944 0.020 0.000 0.972 0.008
#&gt; GSM559447     3  0.0707     0.8984 0.020 0.000 0.980 0.000
#&gt; GSM559449     3  0.0336     0.8887 0.008 0.000 0.992 0.000
#&gt; GSM559453     3  0.0336     0.8887 0.008 0.000 0.992 0.000
#&gt; GSM559466     3  0.0707     0.8984 0.020 0.000 0.980 0.000
#&gt; GSM559474     4  0.6987     0.4768 0.004 0.276 0.140 0.580
#&gt; GSM559476     3  0.5244     0.1382 0.436 0.000 0.556 0.008
#&gt; GSM559483     2  0.4454     0.7872 0.000 0.692 0.000 0.308
#&gt; GSM559484     2  0.6446    -0.0202 0.000 0.584 0.088 0.328
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-4'>
<p><a id='tab-MAD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559433     1  0.1764    0.84577 0.928 0.000 0.000 0.064 0.008
#&gt; GSM559434     2  0.6620    0.01156 0.000 0.436 0.000 0.228 0.336
#&gt; GSM559436     1  0.1918    0.85407 0.928 0.000 0.000 0.036 0.036
#&gt; GSM559437     4  0.1774    0.43414 0.052 0.000 0.000 0.932 0.016
#&gt; GSM559438     2  0.8011   -0.04282 0.092 0.376 0.000 0.292 0.240
#&gt; GSM559440     2  0.6890   -0.01232 0.004 0.404 0.000 0.300 0.292
#&gt; GSM559441     4  0.4502    0.57339 0.180 0.000 0.000 0.744 0.076
#&gt; GSM559442     1  0.3462    0.77028 0.792 0.000 0.000 0.012 0.196
#&gt; GSM559444     2  0.5894    0.30227 0.000 0.532 0.000 0.112 0.356
#&gt; GSM559445     4  0.1661    0.36826 0.024 0.000 0.000 0.940 0.036
#&gt; GSM559446     4  0.1668    0.38486 0.028 0.000 0.000 0.940 0.032
#&gt; GSM559448     1  0.1579    0.85801 0.944 0.000 0.000 0.024 0.032
#&gt; GSM559450     2  0.5894    0.30227 0.000 0.532 0.000 0.112 0.356
#&gt; GSM559451     1  0.1943    0.85706 0.924 0.000 0.000 0.056 0.020
#&gt; GSM559452     5  0.5982    0.00162 0.000 0.312 0.000 0.136 0.552
#&gt; GSM559454     1  0.0798    0.87325 0.976 0.000 0.000 0.016 0.008
#&gt; GSM559455     4  0.4446    0.55462 0.400 0.000 0.000 0.592 0.008
#&gt; GSM559456     4  0.4884    0.52617 0.392 0.000 0.008 0.584 0.016
#&gt; GSM559457     1  0.3635    0.48117 0.748 0.000 0.000 0.248 0.004
#&gt; GSM559458     4  0.5750    0.40774 0.436 0.000 0.008 0.492 0.064
#&gt; GSM559459     1  0.1018    0.87465 0.968 0.000 0.000 0.016 0.016
#&gt; GSM559460     1  0.1608    0.87075 0.928 0.000 0.000 0.000 0.072
#&gt; GSM559461     1  0.1544    0.87137 0.932 0.000 0.000 0.000 0.068
#&gt; GSM559462     1  0.1579    0.87111 0.944 0.000 0.000 0.032 0.024
#&gt; GSM559463     1  0.1281    0.86608 0.956 0.000 0.000 0.012 0.032
#&gt; GSM559464     1  0.1671    0.87152 0.924 0.000 0.000 0.000 0.076
#&gt; GSM559465     1  0.1704    0.87317 0.928 0.000 0.000 0.004 0.068
#&gt; GSM559467     4  0.5872    0.44928 0.408 0.000 0.000 0.492 0.100
#&gt; GSM559468     1  0.3039    0.81647 0.836 0.000 0.000 0.012 0.152
#&gt; GSM559469     1  0.3745    0.75705 0.780 0.000 0.000 0.024 0.196
#&gt; GSM559470     4  0.5781    0.56913 0.308 0.000 0.000 0.576 0.116
#&gt; GSM559471     1  0.3319    0.79797 0.820 0.000 0.000 0.020 0.160
#&gt; GSM559472     1  0.0671    0.87357 0.980 0.000 0.000 0.016 0.004
#&gt; GSM559473     2  0.1430    0.67509 0.000 0.944 0.000 0.004 0.052
#&gt; GSM559475     2  0.1571    0.67607 0.000 0.936 0.000 0.004 0.060
#&gt; GSM559477     2  0.1608    0.68020 0.000 0.928 0.000 0.000 0.072
#&gt; GSM559478     2  0.0955    0.67769 0.000 0.968 0.000 0.004 0.028
#&gt; GSM559479     2  0.1608    0.68020 0.000 0.928 0.000 0.000 0.072
#&gt; GSM559480     2  0.0955    0.67769 0.000 0.968 0.000 0.004 0.028
#&gt; GSM559481     2  0.0955    0.67769 0.000 0.968 0.000 0.004 0.028
#&gt; GSM559482     2  0.1608    0.68020 0.000 0.928 0.000 0.000 0.072
#&gt; GSM559435     3  0.0000    0.89896 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559439     3  0.0000    0.89896 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559443     3  0.0162    0.89602 0.000 0.000 0.996 0.000 0.004
#&gt; GSM559447     3  0.0000    0.89896 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559449     3  0.0000    0.89896 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559453     3  0.0000    0.89896 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559466     3  0.0000    0.89896 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559474     5  0.5181    0.30447 0.000 0.004 0.032 0.452 0.512
#&gt; GSM559476     3  0.5149    0.11718 0.424 0.000 0.540 0.004 0.032
#&gt; GSM559483     2  0.1671    0.67991 0.000 0.924 0.000 0.000 0.076
#&gt; GSM559484     5  0.5944    0.49670 0.000 0.136 0.028 0.180 0.656
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-5'>
<p><a id='tab-MAD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559433     1  0.1588     0.7630 0.924 0.000 0.000 0.072 0.000 0.004
#&gt; GSM559434     6  0.6416     0.2914 0.000 0.208 0.000 0.088 0.148 0.556
#&gt; GSM559436     1  0.2403     0.7470 0.900 0.000 0.000 0.040 0.040 0.020
#&gt; GSM559437     4  0.2615     0.5336 0.004 0.000 0.000 0.852 0.136 0.008
#&gt; GSM559438     6  0.5666     0.3435 0.016 0.180 0.000 0.152 0.016 0.636
#&gt; GSM559440     6  0.5973     0.3448 0.000 0.196 0.000 0.124 0.072 0.608
#&gt; GSM559441     4  0.3501     0.6267 0.080 0.000 0.000 0.804 0.000 0.116
#&gt; GSM559442     6  0.5381    -0.3071 0.424 0.000 0.000 0.096 0.004 0.476
#&gt; GSM559444     2  0.6109     0.0722 0.000 0.424 0.000 0.008 0.204 0.364
#&gt; GSM559445     4  0.3027     0.5011 0.000 0.000 0.000 0.824 0.148 0.028
#&gt; GSM559446     4  0.2613     0.5241 0.000 0.000 0.000 0.848 0.140 0.012
#&gt; GSM559448     1  0.2259     0.7491 0.908 0.000 0.000 0.032 0.040 0.020
#&gt; GSM559450     2  0.6109     0.0722 0.000 0.424 0.000 0.008 0.204 0.364
#&gt; GSM559451     1  0.3535     0.7135 0.800 0.000 0.000 0.144 0.004 0.052
#&gt; GSM559452     6  0.5906     0.1425 0.000 0.108 0.000 0.044 0.284 0.564
#&gt; GSM559454     1  0.0692     0.7697 0.976 0.000 0.000 0.020 0.000 0.004
#&gt; GSM559455     4  0.2994     0.6215 0.208 0.000 0.000 0.788 0.000 0.004
#&gt; GSM559456     4  0.3705     0.5884 0.180 0.000 0.000 0.776 0.008 0.036
#&gt; GSM559457     1  0.3684     0.3596 0.664 0.000 0.000 0.332 0.000 0.004
#&gt; GSM559458     4  0.5481     0.4570 0.188 0.000 0.000 0.612 0.012 0.188
#&gt; GSM559459     1  0.1148     0.7705 0.960 0.000 0.000 0.020 0.004 0.016
#&gt; GSM559460     1  0.3943     0.6963 0.756 0.000 0.000 0.056 0.004 0.184
#&gt; GSM559461     1  0.3943     0.6963 0.756 0.000 0.000 0.056 0.004 0.184
#&gt; GSM559462     1  0.3229     0.7336 0.828 0.000 0.000 0.120 0.004 0.048
#&gt; GSM559463     1  0.1536     0.7565 0.940 0.000 0.000 0.004 0.040 0.016
#&gt; GSM559464     1  0.3884     0.6969 0.760 0.000 0.000 0.052 0.004 0.184
#&gt; GSM559465     1  0.3695     0.7025 0.776 0.000 0.000 0.044 0.004 0.176
#&gt; GSM559467     4  0.5504     0.4819 0.164 0.008 0.000 0.592 0.000 0.236
#&gt; GSM559468     1  0.5452     0.3736 0.520 0.000 0.000 0.100 0.008 0.372
#&gt; GSM559469     6  0.5424    -0.3469 0.444 0.000 0.000 0.100 0.004 0.452
#&gt; GSM559470     4  0.5553     0.4685 0.148 0.008 0.000 0.572 0.000 0.272
#&gt; GSM559471     1  0.5093     0.3222 0.528 0.000 0.000 0.084 0.000 0.388
#&gt; GSM559472     1  0.1644     0.7669 0.932 0.000 0.000 0.052 0.004 0.012
#&gt; GSM559473     2  0.2540     0.7560 0.000 0.872 0.000 0.004 0.020 0.104
#&gt; GSM559475     2  0.2492     0.7571 0.000 0.876 0.000 0.004 0.020 0.100
#&gt; GSM559477     2  0.0935     0.7726 0.000 0.964 0.000 0.000 0.032 0.004
#&gt; GSM559478     2  0.3341     0.7350 0.000 0.816 0.000 0.000 0.068 0.116
#&gt; GSM559479     2  0.1010     0.7717 0.000 0.960 0.000 0.000 0.036 0.004
#&gt; GSM559480     2  0.3341     0.7350 0.000 0.816 0.000 0.000 0.068 0.116
#&gt; GSM559481     2  0.3341     0.7350 0.000 0.816 0.000 0.000 0.068 0.116
#&gt; GSM559482     2  0.0935     0.7726 0.000 0.964 0.000 0.000 0.032 0.004
#&gt; GSM559435     3  0.0260     0.8962 0.000 0.000 0.992 0.000 0.000 0.008
#&gt; GSM559439     3  0.0260     0.8962 0.000 0.000 0.992 0.000 0.000 0.008
#&gt; GSM559443     3  0.0405     0.8943 0.000 0.000 0.988 0.000 0.004 0.008
#&gt; GSM559447     3  0.0000     0.8970 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559449     3  0.0146     0.8960 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM559453     3  0.0146     0.8960 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM559466     3  0.0000     0.8970 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559474     5  0.3052     0.7649 0.000 0.000 0.004 0.216 0.780 0.000
#&gt; GSM559476     3  0.6709     0.1096 0.312 0.000 0.480 0.044 0.016 0.148
#&gt; GSM559483     2  0.1010     0.7725 0.000 0.960 0.000 0.000 0.036 0.004
#&gt; GSM559484     5  0.3134     0.7816 0.000 0.044 0.004 0.068 0.860 0.024
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-kmeans-signature_compare](figure_cola/MAD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-kmeans-collect-classes](figure_cola/MAD-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) k
#> MAD:kmeans 52         5.15e-01 2
#> MAD:kmeans 51         2.17e-09 3
#> MAD:kmeans 46         1.34e-08 4
#> MAD:kmeans 37         5.89e-07 5
#> MAD:kmeans 37         1.52e-06 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "skmeans"]
# you can also extract it by
# res = res_list["MAD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-skmeans-collect-plots](figure_cola/MAD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-skmeans-select-partition-number](figure_cola/MAD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.980       0.991         0.5023 0.497   0.497
#> 3 3 0.825           0.874       0.944         0.3135 0.697   0.474
#> 4 4 0.961           0.942       0.975         0.1334 0.879   0.668
#> 5 5 0.799           0.778       0.882         0.0612 0.961   0.846
#> 6 6 0.772           0.638       0.819         0.0397 0.959   0.814
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-skmeans-get-classes-1'>
<p><a id='tab-MAD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559433     1  0.0000      0.995 1.000 0.000
#&gt; GSM559434     2  0.0000      0.984 0.000 1.000
#&gt; GSM559436     1  0.0000      0.995 1.000 0.000
#&gt; GSM559437     1  0.5408      0.856 0.876 0.124
#&gt; GSM559438     2  0.0000      0.984 0.000 1.000
#&gt; GSM559440     2  0.0000      0.984 0.000 1.000
#&gt; GSM559441     2  0.0672      0.978 0.008 0.992
#&gt; GSM559442     1  0.0000      0.995 1.000 0.000
#&gt; GSM559444     2  0.0000      0.984 0.000 1.000
#&gt; GSM559445     2  0.0000      0.984 0.000 1.000
#&gt; GSM559446     2  0.7950      0.684 0.240 0.760
#&gt; GSM559448     1  0.0000      0.995 1.000 0.000
#&gt; GSM559450     2  0.0000      0.984 0.000 1.000
#&gt; GSM559451     1  0.0000      0.995 1.000 0.000
#&gt; GSM559452     2  0.0000      0.984 0.000 1.000
#&gt; GSM559454     1  0.0000      0.995 1.000 0.000
#&gt; GSM559455     1  0.0000      0.995 1.000 0.000
#&gt; GSM559456     1  0.0000      0.995 1.000 0.000
#&gt; GSM559457     1  0.0000      0.995 1.000 0.000
#&gt; GSM559458     1  0.0000      0.995 1.000 0.000
#&gt; GSM559459     1  0.0000      0.995 1.000 0.000
#&gt; GSM559460     1  0.0000      0.995 1.000 0.000
#&gt; GSM559461     1  0.0000      0.995 1.000 0.000
#&gt; GSM559462     1  0.0000      0.995 1.000 0.000
#&gt; GSM559463     1  0.0000      0.995 1.000 0.000
#&gt; GSM559464     1  0.0000      0.995 1.000 0.000
#&gt; GSM559465     1  0.0000      0.995 1.000 0.000
#&gt; GSM559467     2  0.0000      0.984 0.000 1.000
#&gt; GSM559468     1  0.0000      0.995 1.000 0.000
#&gt; GSM559469     2  0.4431      0.894 0.092 0.908
#&gt; GSM559470     2  0.0000      0.984 0.000 1.000
#&gt; GSM559471     1  0.0000      0.995 1.000 0.000
#&gt; GSM559472     1  0.0000      0.995 1.000 0.000
#&gt; GSM559473     2  0.0000      0.984 0.000 1.000
#&gt; GSM559475     2  0.0000      0.984 0.000 1.000
#&gt; GSM559477     2  0.0000      0.984 0.000 1.000
#&gt; GSM559478     2  0.0000      0.984 0.000 1.000
#&gt; GSM559479     2  0.0000      0.984 0.000 1.000
#&gt; GSM559480     2  0.0000      0.984 0.000 1.000
#&gt; GSM559481     2  0.0000      0.984 0.000 1.000
#&gt; GSM559482     2  0.0000      0.984 0.000 1.000
#&gt; GSM559435     1  0.0000      0.995 1.000 0.000
#&gt; GSM559439     1  0.0000      0.995 1.000 0.000
#&gt; GSM559443     1  0.0000      0.995 1.000 0.000
#&gt; GSM559447     1  0.0000      0.995 1.000 0.000
#&gt; GSM559449     1  0.0000      0.995 1.000 0.000
#&gt; GSM559453     1  0.0000      0.995 1.000 0.000
#&gt; GSM559466     1  0.0000      0.995 1.000 0.000
#&gt; GSM559474     2  0.0000      0.984 0.000 1.000
#&gt; GSM559476     1  0.0000      0.995 1.000 0.000
#&gt; GSM559483     2  0.0000      0.984 0.000 1.000
#&gt; GSM559484     2  0.0000      0.984 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-2'>
<p><a id='tab-MAD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559433     1  0.0000      0.896 1.000 0.000 0.000
#&gt; GSM559434     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM559436     1  0.0000      0.896 1.000 0.000 0.000
#&gt; GSM559437     1  0.5465      0.623 0.712 0.000 0.288
#&gt; GSM559438     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM559440     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM559441     1  0.8869      0.270 0.496 0.380 0.124
#&gt; GSM559442     1  0.1753      0.864 0.952 0.048 0.000
#&gt; GSM559444     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM559445     2  0.4521      0.763 0.004 0.816 0.180
#&gt; GSM559446     1  0.5431      0.629 0.716 0.000 0.284
#&gt; GSM559448     3  0.5138      0.644 0.252 0.000 0.748
#&gt; GSM559450     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM559451     1  0.0000      0.896 1.000 0.000 0.000
#&gt; GSM559452     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM559454     1  0.0000      0.896 1.000 0.000 0.000
#&gt; GSM559455     1  0.3879      0.781 0.848 0.000 0.152
#&gt; GSM559456     1  0.5560      0.604 0.700 0.000 0.300
#&gt; GSM559457     1  0.0000      0.896 1.000 0.000 0.000
#&gt; GSM559458     3  0.0000      0.972 0.000 0.000 1.000
#&gt; GSM559459     1  0.0000      0.896 1.000 0.000 0.000
#&gt; GSM559460     1  0.0000      0.896 1.000 0.000 0.000
#&gt; GSM559461     1  0.0000      0.896 1.000 0.000 0.000
#&gt; GSM559462     1  0.0000      0.896 1.000 0.000 0.000
#&gt; GSM559463     1  0.0000      0.896 1.000 0.000 0.000
#&gt; GSM559464     1  0.0000      0.896 1.000 0.000 0.000
#&gt; GSM559465     1  0.0000      0.896 1.000 0.000 0.000
#&gt; GSM559467     1  0.0237      0.894 0.996 0.004 0.000
#&gt; GSM559468     1  0.0000      0.896 1.000 0.000 0.000
#&gt; GSM559469     1  0.4399      0.736 0.812 0.188 0.000
#&gt; GSM559470     1  0.6192      0.305 0.580 0.420 0.000
#&gt; GSM559471     1  0.0000      0.896 1.000 0.000 0.000
#&gt; GSM559472     1  0.0000      0.896 1.000 0.000 0.000
#&gt; GSM559473     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM559475     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM559477     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM559478     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM559479     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM559480     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM559481     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM559482     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM559435     3  0.0000      0.972 0.000 0.000 1.000
#&gt; GSM559439     3  0.0000      0.972 0.000 0.000 1.000
#&gt; GSM559443     3  0.0000      0.972 0.000 0.000 1.000
#&gt; GSM559447     3  0.0000      0.972 0.000 0.000 1.000
#&gt; GSM559449     3  0.0000      0.972 0.000 0.000 1.000
#&gt; GSM559453     3  0.0000      0.972 0.000 0.000 1.000
#&gt; GSM559466     3  0.0000      0.972 0.000 0.000 1.000
#&gt; GSM559474     3  0.0000      0.972 0.000 0.000 1.000
#&gt; GSM559476     3  0.0000      0.972 0.000 0.000 1.000
#&gt; GSM559483     2  0.0000      0.970 0.000 1.000 0.000
#&gt; GSM559484     2  0.5254      0.632 0.000 0.736 0.264
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-3'>
<p><a id='tab-MAD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559433     1  0.0921      0.950 0.972 0.000 0.000 0.028
#&gt; GSM559434     2  0.0000      0.987 0.000 1.000 0.000 0.000
#&gt; GSM559436     1  0.0000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM559437     4  0.0000      0.949 0.000 0.000 0.000 1.000
#&gt; GSM559438     2  0.0000      0.987 0.000 1.000 0.000 0.000
#&gt; GSM559440     2  0.0000      0.987 0.000 1.000 0.000 0.000
#&gt; GSM559441     4  0.0000      0.949 0.000 0.000 0.000 1.000
#&gt; GSM559442     1  0.0000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM559444     2  0.0000      0.987 0.000 1.000 0.000 0.000
#&gt; GSM559445     4  0.0000      0.949 0.000 0.000 0.000 1.000
#&gt; GSM559446     4  0.0000      0.949 0.000 0.000 0.000 1.000
#&gt; GSM559448     3  0.3486      0.745 0.188 0.000 0.812 0.000
#&gt; GSM559450     2  0.0000      0.987 0.000 1.000 0.000 0.000
#&gt; GSM559451     1  0.2345      0.887 0.900 0.000 0.000 0.100
#&gt; GSM559452     2  0.0000      0.987 0.000 1.000 0.000 0.000
#&gt; GSM559454     1  0.0000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM559455     4  0.0188      0.948 0.004 0.000 0.000 0.996
#&gt; GSM559456     4  0.1004      0.935 0.004 0.000 0.024 0.972
#&gt; GSM559457     1  0.4500      0.571 0.684 0.000 0.000 0.316
#&gt; GSM559458     3  0.0707      0.955 0.000 0.000 0.980 0.020
#&gt; GSM559459     1  0.0000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM559460     1  0.0000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM559461     1  0.0000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM559462     1  0.1716      0.921 0.936 0.000 0.000 0.064
#&gt; GSM559463     1  0.0000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM559464     1  0.0000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM559465     1  0.0000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM559467     4  0.0469      0.944 0.012 0.000 0.000 0.988
#&gt; GSM559468     1  0.0000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM559469     1  0.0817      0.947 0.976 0.024 0.000 0.000
#&gt; GSM559470     4  0.0707      0.938 0.020 0.000 0.000 0.980
#&gt; GSM559471     1  0.0000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM559472     1  0.0000      0.966 1.000 0.000 0.000 0.000
#&gt; GSM559473     2  0.0000      0.987 0.000 1.000 0.000 0.000
#&gt; GSM559475     2  0.0000      0.987 0.000 1.000 0.000 0.000
#&gt; GSM559477     2  0.0000      0.987 0.000 1.000 0.000 0.000
#&gt; GSM559478     2  0.0000      0.987 0.000 1.000 0.000 0.000
#&gt; GSM559479     2  0.0000      0.987 0.000 1.000 0.000 0.000
#&gt; GSM559480     2  0.0000      0.987 0.000 1.000 0.000 0.000
#&gt; GSM559481     2  0.0000      0.987 0.000 1.000 0.000 0.000
#&gt; GSM559482     2  0.0000      0.987 0.000 1.000 0.000 0.000
#&gt; GSM559435     3  0.0000      0.971 0.000 0.000 1.000 0.000
#&gt; GSM559439     3  0.0000      0.971 0.000 0.000 1.000 0.000
#&gt; GSM559443     3  0.0000      0.971 0.000 0.000 1.000 0.000
#&gt; GSM559447     3  0.0000      0.971 0.000 0.000 1.000 0.000
#&gt; GSM559449     3  0.0000      0.971 0.000 0.000 1.000 0.000
#&gt; GSM559453     3  0.0000      0.971 0.000 0.000 1.000 0.000
#&gt; GSM559466     3  0.0000      0.971 0.000 0.000 1.000 0.000
#&gt; GSM559474     4  0.4543      0.512 0.000 0.000 0.324 0.676
#&gt; GSM559476     3  0.0000      0.971 0.000 0.000 1.000 0.000
#&gt; GSM559483     2  0.0000      0.987 0.000 1.000 0.000 0.000
#&gt; GSM559484     2  0.3583      0.775 0.000 0.816 0.180 0.004
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-4'>
<p><a id='tab-MAD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559433     1  0.1310      0.720 0.956 0.000 0.000 0.024 0.020
#&gt; GSM559434     2  0.1478      0.919 0.000 0.936 0.000 0.000 0.064
#&gt; GSM559436     1  0.0000      0.740 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559437     4  0.0162      0.831 0.000 0.000 0.000 0.996 0.004
#&gt; GSM559438     2  0.3109      0.777 0.000 0.800 0.000 0.000 0.200
#&gt; GSM559440     2  0.2020      0.898 0.000 0.900 0.000 0.000 0.100
#&gt; GSM559441     4  0.0451      0.830 0.004 0.000 0.000 0.988 0.008
#&gt; GSM559442     5  0.3074      0.875 0.196 0.000 0.000 0.000 0.804
#&gt; GSM559444     2  0.1043      0.926 0.000 0.960 0.000 0.000 0.040
#&gt; GSM559445     4  0.0703      0.829 0.000 0.000 0.000 0.976 0.024
#&gt; GSM559446     4  0.0609      0.829 0.000 0.000 0.000 0.980 0.020
#&gt; GSM559448     3  0.4219      0.325 0.416 0.000 0.584 0.000 0.000
#&gt; GSM559450     2  0.0963      0.927 0.000 0.964 0.000 0.000 0.036
#&gt; GSM559451     1  0.2879      0.703 0.868 0.000 0.000 0.032 0.100
#&gt; GSM559452     2  0.3534      0.762 0.000 0.744 0.000 0.000 0.256
#&gt; GSM559454     1  0.0703      0.745 0.976 0.000 0.000 0.000 0.024
#&gt; GSM559455     4  0.2130      0.808 0.080 0.000 0.000 0.908 0.012
#&gt; GSM559456     4  0.4361      0.728 0.144 0.000 0.052 0.784 0.020
#&gt; GSM559457     1  0.2920      0.612 0.852 0.000 0.000 0.132 0.016
#&gt; GSM559458     3  0.4307      0.704 0.000 0.000 0.772 0.128 0.100
#&gt; GSM559459     1  0.1341      0.743 0.944 0.000 0.000 0.000 0.056
#&gt; GSM559460     1  0.4242      0.205 0.572 0.000 0.000 0.000 0.428
#&gt; GSM559461     1  0.4074      0.399 0.636 0.000 0.000 0.000 0.364
#&gt; GSM559462     1  0.3409      0.684 0.816 0.000 0.000 0.024 0.160
#&gt; GSM559463     1  0.2011      0.728 0.908 0.000 0.004 0.000 0.088
#&gt; GSM559464     1  0.4161      0.326 0.608 0.000 0.000 0.000 0.392
#&gt; GSM559465     1  0.3966      0.444 0.664 0.000 0.000 0.000 0.336
#&gt; GSM559467     4  0.4649      0.680 0.068 0.000 0.000 0.720 0.212
#&gt; GSM559468     5  0.3561      0.851 0.260 0.000 0.000 0.000 0.740
#&gt; GSM559469     5  0.2753      0.829 0.136 0.008 0.000 0.000 0.856
#&gt; GSM559470     4  0.5204      0.638 0.076 0.008 0.000 0.680 0.236
#&gt; GSM559471     5  0.3534      0.856 0.256 0.000 0.000 0.000 0.744
#&gt; GSM559472     1  0.0451      0.739 0.988 0.000 0.000 0.004 0.008
#&gt; GSM559473     2  0.0000      0.938 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559475     2  0.0000      0.938 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559477     2  0.0000      0.938 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559478     2  0.0000      0.938 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559479     2  0.0000      0.938 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.0000      0.938 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559481     2  0.0000      0.938 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559482     2  0.0000      0.938 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.0000      0.921 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559439     3  0.0000      0.921 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559443     3  0.0000      0.921 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559447     3  0.0000      0.921 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559449     3  0.0000      0.921 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559453     3  0.0000      0.921 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559466     3  0.0000      0.921 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559474     4  0.5847      0.446 0.000 0.000 0.264 0.592 0.144
#&gt; GSM559476     3  0.0162      0.918 0.000 0.000 0.996 0.000 0.004
#&gt; GSM559483     2  0.0000      0.938 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559484     2  0.5808      0.608 0.000 0.648 0.188 0.012 0.152
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-5'>
<p><a id='tab-MAD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559433     1  0.3699     0.6153 0.812 0.000 0.000 0.104 0.060 0.024
#&gt; GSM559434     2  0.3468     0.6628 0.000 0.728 0.000 0.000 0.264 0.008
#&gt; GSM559436     1  0.0862     0.6752 0.972 0.000 0.000 0.004 0.016 0.008
#&gt; GSM559437     4  0.0935     0.6924 0.004 0.000 0.000 0.964 0.032 0.000
#&gt; GSM559438     2  0.4672     0.5625 0.000 0.684 0.000 0.000 0.188 0.128
#&gt; GSM559440     2  0.3834     0.6738 0.000 0.732 0.000 0.000 0.232 0.036
#&gt; GSM559441     4  0.1297     0.6938 0.012 0.000 0.000 0.948 0.040 0.000
#&gt; GSM559442     6  0.2857     0.7087 0.072 0.000 0.000 0.000 0.072 0.856
#&gt; GSM559444     2  0.3265     0.6611 0.000 0.748 0.000 0.000 0.248 0.004
#&gt; GSM559445     4  0.2278     0.6524 0.000 0.000 0.000 0.868 0.128 0.004
#&gt; GSM559446     4  0.2278     0.6549 0.000 0.000 0.000 0.868 0.128 0.004
#&gt; GSM559448     3  0.4361     0.2285 0.436 0.000 0.544 0.000 0.016 0.004
#&gt; GSM559450     2  0.3265     0.6610 0.000 0.748 0.000 0.000 0.248 0.004
#&gt; GSM559451     1  0.5073     0.5302 0.680 0.000 0.000 0.020 0.160 0.140
#&gt; GSM559452     5  0.5115     0.3442 0.000 0.356 0.004 0.000 0.560 0.080
#&gt; GSM559454     1  0.0858     0.6786 0.968 0.000 0.000 0.000 0.004 0.028
#&gt; GSM559455     4  0.3261     0.6591 0.104 0.000 0.000 0.824 0.072 0.000
#&gt; GSM559456     4  0.5159     0.5585 0.172 0.000 0.044 0.700 0.076 0.008
#&gt; GSM559457     1  0.4600     0.5552 0.728 0.000 0.000 0.156 0.096 0.020
#&gt; GSM559458     3  0.5043     0.5622 0.000 0.000 0.692 0.124 0.156 0.028
#&gt; GSM559459     1  0.2147     0.6689 0.896 0.000 0.000 0.000 0.020 0.084
#&gt; GSM559460     6  0.3979    -0.0948 0.456 0.000 0.000 0.000 0.004 0.540
#&gt; GSM559461     1  0.3989     0.0803 0.528 0.000 0.000 0.000 0.004 0.468
#&gt; GSM559462     1  0.5436     0.4930 0.632 0.000 0.000 0.020 0.152 0.196
#&gt; GSM559463     1  0.2673     0.6392 0.852 0.000 0.004 0.000 0.012 0.132
#&gt; GSM559464     1  0.3993     0.0551 0.520 0.000 0.000 0.000 0.004 0.476
#&gt; GSM559465     1  0.4127     0.2441 0.588 0.000 0.000 0.008 0.004 0.400
#&gt; GSM559467     4  0.6883     0.4521 0.068 0.004 0.000 0.472 0.224 0.232
#&gt; GSM559468     6  0.1765     0.7314 0.096 0.000 0.000 0.000 0.000 0.904
#&gt; GSM559469     6  0.1644     0.7179 0.028 0.000 0.000 0.000 0.040 0.932
#&gt; GSM559470     4  0.6795     0.4370 0.032 0.016 0.000 0.468 0.228 0.256
#&gt; GSM559471     6  0.3806     0.6611 0.152 0.000 0.000 0.000 0.076 0.772
#&gt; GSM559472     1  0.2152     0.6747 0.912 0.000 0.000 0.012 0.040 0.036
#&gt; GSM559473     2  0.0146     0.8590 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559475     2  0.0547     0.8563 0.000 0.980 0.000 0.000 0.020 0.000
#&gt; GSM559477     2  0.0146     0.8591 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559478     2  0.0632     0.8534 0.000 0.976 0.000 0.000 0.024 0.000
#&gt; GSM559479     2  0.0363     0.8572 0.000 0.988 0.000 0.000 0.012 0.000
#&gt; GSM559480     2  0.0632     0.8534 0.000 0.976 0.000 0.000 0.024 0.000
#&gt; GSM559481     2  0.0632     0.8534 0.000 0.976 0.000 0.000 0.024 0.000
#&gt; GSM559482     2  0.0000     0.8595 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.0000     0.9014 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559439     3  0.0000     0.9014 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559443     3  0.0000     0.9014 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559447     3  0.0000     0.9014 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559449     3  0.0000     0.9014 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559453     3  0.0000     0.9014 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559466     3  0.0000     0.9014 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559474     5  0.5498    -0.0342 0.000 0.000 0.116 0.376 0.504 0.004
#&gt; GSM559476     3  0.0146     0.8984 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM559483     2  0.0000     0.8595 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559484     5  0.5526     0.5156 0.000 0.320 0.076 0.032 0.572 0.000
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-skmeans-signature_compare](figure_cola/MAD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-skmeans-collect-classes](figure_cola/MAD-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>              n disease.state(p) k
#> MAD:skmeans 52         3.51e-01 2
#> MAD:skmeans 50         2.80e-07 3
#> MAD:skmeans 52         7.19e-06 4
#> MAD:skmeans 46         3.89e-06 5
#> MAD:skmeans 42         5.99e-06 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "pam"]
# you can also extract it by
# res = res_list["MAD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-pam-collect-plots](figure_cola/MAD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-pam-select-partition-number](figure_cola/MAD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.999           0.960       0.983         0.3896 0.618   0.618
#> 3 3 0.999           0.956       0.981         0.5940 0.689   0.527
#> 4 4 0.787           0.752       0.884         0.2137 0.855   0.631
#> 5 5 0.766           0.671       0.825         0.0514 0.928   0.723
#> 6 6 0.854           0.801       0.911         0.0381 0.959   0.799
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-classes'>
<ul>
<li><a href='#tab-MAD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-pam-get-classes-1'>
<p><a id='tab-MAD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559433     2  0.0000      0.983 0.000 1.000
#&gt; GSM559434     2  0.0000      0.983 0.000 1.000
#&gt; GSM559436     2  0.0000      0.983 0.000 1.000
#&gt; GSM559437     2  0.0000      0.983 0.000 1.000
#&gt; GSM559438     2  0.0000      0.983 0.000 1.000
#&gt; GSM559440     2  0.0000      0.983 0.000 1.000
#&gt; GSM559441     2  0.0000      0.983 0.000 1.000
#&gt; GSM559442     2  0.0000      0.983 0.000 1.000
#&gt; GSM559444     2  0.0000      0.983 0.000 1.000
#&gt; GSM559445     2  0.0000      0.983 0.000 1.000
#&gt; GSM559446     2  0.0000      0.983 0.000 1.000
#&gt; GSM559448     1  0.0672      0.970 0.992 0.008
#&gt; GSM559450     2  0.0000      0.983 0.000 1.000
#&gt; GSM559451     2  0.0000      0.983 0.000 1.000
#&gt; GSM559452     2  0.2043      0.954 0.032 0.968
#&gt; GSM559454     2  0.0000      0.983 0.000 1.000
#&gt; GSM559455     2  0.0000      0.983 0.000 1.000
#&gt; GSM559456     1  0.8267      0.647 0.740 0.260
#&gt; GSM559457     2  0.0000      0.983 0.000 1.000
#&gt; GSM559458     1  0.0000      0.977 1.000 0.000
#&gt; GSM559459     2  0.0000      0.983 0.000 1.000
#&gt; GSM559460     2  0.0000      0.983 0.000 1.000
#&gt; GSM559461     2  0.3584      0.917 0.068 0.932
#&gt; GSM559462     2  0.0000      0.983 0.000 1.000
#&gt; GSM559463     2  0.8327      0.650 0.264 0.736
#&gt; GSM559464     2  0.0000      0.983 0.000 1.000
#&gt; GSM559465     2  0.8327      0.650 0.264 0.736
#&gt; GSM559467     2  0.0000      0.983 0.000 1.000
#&gt; GSM559468     2  0.0000      0.983 0.000 1.000
#&gt; GSM559469     2  0.0000      0.983 0.000 1.000
#&gt; GSM559470     2  0.0000      0.983 0.000 1.000
#&gt; GSM559471     2  0.0000      0.983 0.000 1.000
#&gt; GSM559472     2  0.0000      0.983 0.000 1.000
#&gt; GSM559473     2  0.0000      0.983 0.000 1.000
#&gt; GSM559475     2  0.0000      0.983 0.000 1.000
#&gt; GSM559477     2  0.0000      0.983 0.000 1.000
#&gt; GSM559478     2  0.0000      0.983 0.000 1.000
#&gt; GSM559479     2  0.0000      0.983 0.000 1.000
#&gt; GSM559480     2  0.0000      0.983 0.000 1.000
#&gt; GSM559481     2  0.0000      0.983 0.000 1.000
#&gt; GSM559482     2  0.0000      0.983 0.000 1.000
#&gt; GSM559435     1  0.0000      0.977 1.000 0.000
#&gt; GSM559439     1  0.0000      0.977 1.000 0.000
#&gt; GSM559443     1  0.0000      0.977 1.000 0.000
#&gt; GSM559447     1  0.0000      0.977 1.000 0.000
#&gt; GSM559449     1  0.0000      0.977 1.000 0.000
#&gt; GSM559453     1  0.0000      0.977 1.000 0.000
#&gt; GSM559466     1  0.0000      0.977 1.000 0.000
#&gt; GSM559474     1  0.0000      0.977 1.000 0.000
#&gt; GSM559476     1  0.0000      0.977 1.000 0.000
#&gt; GSM559483     2  0.0000      0.983 0.000 1.000
#&gt; GSM559484     1  0.0000      0.977 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-1-a').click(function(){
  $('#tab-MAD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-2'>
<p><a id='tab-MAD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559433     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559434     1  0.6192      0.266 0.580 0.420 0.000
#&gt; GSM559436     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559437     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559438     1  0.1411      0.942 0.964 0.036 0.000
#&gt; GSM559440     2  0.2878      0.874 0.096 0.904 0.000
#&gt; GSM559441     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559442     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559444     2  0.0000      0.976 0.000 1.000 0.000
#&gt; GSM559445     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559446     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559448     1  0.4452      0.760 0.808 0.000 0.192
#&gt; GSM559450     2  0.0000      0.976 0.000 1.000 0.000
#&gt; GSM559451     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559452     2  0.5000      0.825 0.044 0.832 0.124
#&gt; GSM559454     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559455     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559456     1  0.1753      0.930 0.952 0.000 0.048
#&gt; GSM559457     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559458     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559459     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559460     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559461     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559462     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559463     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559464     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559465     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559467     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559468     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559469     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559470     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559471     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559472     1  0.0000      0.974 1.000 0.000 0.000
#&gt; GSM559473     2  0.0592      0.966 0.012 0.988 0.000
#&gt; GSM559475     2  0.0000      0.976 0.000 1.000 0.000
#&gt; GSM559477     2  0.0000      0.976 0.000 1.000 0.000
#&gt; GSM559478     2  0.0000      0.976 0.000 1.000 0.000
#&gt; GSM559479     2  0.0000      0.976 0.000 1.000 0.000
#&gt; GSM559480     2  0.0000      0.976 0.000 1.000 0.000
#&gt; GSM559481     2  0.0000      0.976 0.000 1.000 0.000
#&gt; GSM559482     2  0.0000      0.976 0.000 1.000 0.000
#&gt; GSM559435     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559439     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559443     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559447     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559449     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559453     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559466     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559474     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559476     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559483     2  0.0000      0.976 0.000 1.000 0.000
#&gt; GSM559484     3  0.0000      1.000 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-2-a').click(function(){
  $('#tab-MAD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-3'>
<p><a id='tab-MAD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559433     1  0.0000     0.7043 1.000 0.000 0.000 0.000
#&gt; GSM559434     1  0.6837    -0.0143 0.504 0.392 0.000 0.104
#&gt; GSM559436     1  0.4222     0.6065 0.728 0.000 0.000 0.272
#&gt; GSM559437     1  0.0000     0.7043 1.000 0.000 0.000 0.000
#&gt; GSM559438     1  0.5376     0.1599 0.588 0.016 0.000 0.396
#&gt; GSM559440     2  0.6795     0.1373 0.432 0.472 0.000 0.096
#&gt; GSM559441     1  0.0000     0.7043 1.000 0.000 0.000 0.000
#&gt; GSM559442     4  0.2149     0.7781 0.088 0.000 0.000 0.912
#&gt; GSM559444     2  0.1302     0.9105 0.044 0.956 0.000 0.000
#&gt; GSM559445     1  0.0000     0.7043 1.000 0.000 0.000 0.000
#&gt; GSM559446     1  0.0000     0.7043 1.000 0.000 0.000 0.000
#&gt; GSM559448     1  0.7468     0.3315 0.464 0.000 0.352 0.184
#&gt; GSM559450     2  0.0000     0.9457 0.000 1.000 0.000 0.000
#&gt; GSM559451     1  0.4134     0.6497 0.740 0.000 0.000 0.260
#&gt; GSM559452     4  0.4872     0.3738 0.356 0.004 0.000 0.640
#&gt; GSM559454     1  0.3528     0.6693 0.808 0.000 0.000 0.192
#&gt; GSM559455     1  0.0000     0.7043 1.000 0.000 0.000 0.000
#&gt; GSM559456     1  0.4916     0.6531 0.760 0.000 0.056 0.184
#&gt; GSM559457     1  0.3444     0.6723 0.816 0.000 0.000 0.184
#&gt; GSM559458     3  0.1389     0.9496 0.000 0.000 0.952 0.048
#&gt; GSM559459     1  0.4916     0.3844 0.576 0.000 0.000 0.424
#&gt; GSM559460     4  0.2281     0.8268 0.096 0.000 0.000 0.904
#&gt; GSM559461     4  0.3569     0.6291 0.196 0.000 0.000 0.804
#&gt; GSM559462     1  0.4933     0.3767 0.568 0.000 0.000 0.432
#&gt; GSM559463     4  0.1118     0.8436 0.036 0.000 0.000 0.964
#&gt; GSM559464     4  0.2281     0.8268 0.096 0.000 0.000 0.904
#&gt; GSM559465     4  0.2281     0.8268 0.096 0.000 0.000 0.904
#&gt; GSM559467     1  0.4955     0.1945 0.556 0.000 0.000 0.444
#&gt; GSM559468     4  0.0000     0.8441 0.000 0.000 0.000 1.000
#&gt; GSM559469     4  0.0000     0.8441 0.000 0.000 0.000 1.000
#&gt; GSM559470     1  0.1474     0.6790 0.948 0.000 0.000 0.052
#&gt; GSM559471     4  0.0336     0.8437 0.008 0.000 0.000 0.992
#&gt; GSM559472     1  0.4817     0.4963 0.612 0.000 0.000 0.388
#&gt; GSM559473     2  0.0469     0.9369 0.000 0.988 0.000 0.012
#&gt; GSM559475     2  0.0000     0.9457 0.000 1.000 0.000 0.000
#&gt; GSM559477     2  0.0000     0.9457 0.000 1.000 0.000 0.000
#&gt; GSM559478     2  0.0000     0.9457 0.000 1.000 0.000 0.000
#&gt; GSM559479     2  0.0000     0.9457 0.000 1.000 0.000 0.000
#&gt; GSM559480     2  0.0000     0.9457 0.000 1.000 0.000 0.000
#&gt; GSM559481     2  0.0000     0.9457 0.000 1.000 0.000 0.000
#&gt; GSM559482     2  0.0000     0.9457 0.000 1.000 0.000 0.000
#&gt; GSM559435     3  0.0000     0.9952 0.000 0.000 1.000 0.000
#&gt; GSM559439     3  0.0000     0.9952 0.000 0.000 1.000 0.000
#&gt; GSM559443     3  0.0000     0.9952 0.000 0.000 1.000 0.000
#&gt; GSM559447     3  0.0000     0.9952 0.000 0.000 1.000 0.000
#&gt; GSM559449     3  0.0000     0.9952 0.000 0.000 1.000 0.000
#&gt; GSM559453     3  0.0000     0.9952 0.000 0.000 1.000 0.000
#&gt; GSM559466     3  0.0000     0.9952 0.000 0.000 1.000 0.000
#&gt; GSM559474     3  0.0000     0.9952 0.000 0.000 1.000 0.000
#&gt; GSM559476     3  0.0000     0.9952 0.000 0.000 1.000 0.000
#&gt; GSM559483     2  0.0000     0.9457 0.000 1.000 0.000 0.000
#&gt; GSM559484     3  0.0000     0.9952 0.000 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-3-a').click(function(){
  $('#tab-MAD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-4'>
<p><a id='tab-MAD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559433     1  0.3752   -0.00719 0.708 0.000 0.000 0.292 0.000
#&gt; GSM559434     4  0.6697    0.52385 0.108 0.212 0.000 0.600 0.080
#&gt; GSM559436     1  0.0162    0.60529 0.996 0.000 0.000 0.000 0.004
#&gt; GSM559437     4  0.4304    0.57297 0.484 0.000 0.000 0.516 0.000
#&gt; GSM559438     4  0.6118    0.57333 0.256 0.016 0.000 0.600 0.128
#&gt; GSM559440     4  0.6697    0.52385 0.108 0.212 0.000 0.600 0.080
#&gt; GSM559441     4  0.4304    0.57297 0.484 0.000 0.000 0.516 0.000
#&gt; GSM559442     5  0.0000    0.80265 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559444     2  0.2890    0.81220 0.004 0.836 0.000 0.160 0.000
#&gt; GSM559445     4  0.4304    0.57297 0.484 0.000 0.000 0.516 0.000
#&gt; GSM559446     1  0.4307   -0.61612 0.504 0.000 0.000 0.496 0.000
#&gt; GSM559448     1  0.3366    0.45892 0.768 0.000 0.232 0.000 0.000
#&gt; GSM559450     2  0.0404    0.97409 0.000 0.988 0.000 0.012 0.000
#&gt; GSM559451     1  0.1608    0.57789 0.928 0.000 0.000 0.000 0.072
#&gt; GSM559452     5  0.5289    0.30728 0.064 0.000 0.000 0.340 0.596
#&gt; GSM559454     1  0.0162    0.60529 0.996 0.000 0.000 0.000 0.004
#&gt; GSM559455     1  0.4297   -0.56294 0.528 0.000 0.000 0.472 0.000
#&gt; GSM559456     1  0.1168    0.60150 0.960 0.000 0.032 0.008 0.000
#&gt; GSM559457     1  0.0000    0.60251 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559458     3  0.1197    0.89591 0.000 0.000 0.952 0.000 0.048
#&gt; GSM559459     1  0.2377    0.58199 0.872 0.000 0.000 0.000 0.128
#&gt; GSM559460     5  0.1732    0.79630 0.080 0.000 0.000 0.000 0.920
#&gt; GSM559461     5  0.2891    0.65497 0.176 0.000 0.000 0.000 0.824
#&gt; GSM559462     1  0.4201    0.30633 0.592 0.000 0.000 0.000 0.408
#&gt; GSM559463     5  0.3837    0.58386 0.308 0.000 0.000 0.000 0.692
#&gt; GSM559464     5  0.1732    0.79630 0.080 0.000 0.000 0.000 0.920
#&gt; GSM559465     5  0.1732    0.79630 0.080 0.000 0.000 0.000 0.920
#&gt; GSM559467     1  0.6645    0.06612 0.400 0.000 0.000 0.224 0.376
#&gt; GSM559468     5  0.0000    0.80265 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559469     5  0.0000    0.80265 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559470     4  0.5381    0.57716 0.428 0.000 0.000 0.516 0.056
#&gt; GSM559471     5  0.3752    0.59399 0.292 0.000 0.000 0.000 0.708
#&gt; GSM559472     1  0.2732    0.56741 0.840 0.000 0.000 0.000 0.160
#&gt; GSM559473     2  0.0404    0.97130 0.000 0.988 0.000 0.000 0.012
#&gt; GSM559475     2  0.0000    0.98111 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559477     2  0.0000    0.98111 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559478     2  0.0000    0.98111 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559479     2  0.0000    0.98111 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.0000    0.98111 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559481     2  0.0000    0.98111 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559482     2  0.0000    0.98111 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.0000    0.93052 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559439     3  0.0000    0.93052 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559443     3  0.0000    0.93052 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559447     3  0.0000    0.93052 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559449     3  0.0000    0.93052 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559453     3  0.0000    0.93052 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559466     3  0.0000    0.93052 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559474     3  0.4182    0.66460 0.000 0.000 0.600 0.400 0.000
#&gt; GSM559476     3  0.0000    0.93052 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559483     2  0.0000    0.98111 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559484     3  0.4182    0.66460 0.000 0.000 0.600 0.400 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-4-a').click(function(){
  $('#tab-MAD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-5'>
<p><a id='tab-MAD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4 p5    p6
#&gt; GSM559433     1  0.3684      0.167 0.628 0.000 0.000 0.372  0 0.000
#&gt; GSM559434     4  0.0363      0.803 0.000 0.000 0.000 0.988  0 0.012
#&gt; GSM559436     1  0.0000      0.813 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM559437     4  0.2454      0.871 0.160 0.000 0.000 0.840  0 0.000
#&gt; GSM559438     4  0.0547      0.804 0.000 0.000 0.000 0.980  0 0.020
#&gt; GSM559440     4  0.0000      0.799 0.000 0.000 0.000 1.000  0 0.000
#&gt; GSM559441     4  0.2416      0.873 0.156 0.000 0.000 0.844  0 0.000
#&gt; GSM559442     6  0.0000      0.786 0.000 0.000 0.000 0.000  0 1.000
#&gt; GSM559444     2  0.3737      0.434 0.000 0.608 0.000 0.392  0 0.000
#&gt; GSM559445     4  0.2416      0.873 0.156 0.000 0.000 0.844  0 0.000
#&gt; GSM559446     4  0.3547      0.621 0.332 0.000 0.000 0.668  0 0.000
#&gt; GSM559448     1  0.2416      0.678 0.844 0.000 0.156 0.000  0 0.000
#&gt; GSM559450     2  0.1007      0.915 0.000 0.956 0.000 0.044  0 0.000
#&gt; GSM559451     1  0.0692      0.806 0.976 0.000 0.000 0.004  0 0.020
#&gt; GSM559452     6  0.3847      0.167 0.000 0.000 0.000 0.456  0 0.544
#&gt; GSM559454     1  0.0000      0.813 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM559455     4  0.2996      0.811 0.228 0.000 0.000 0.772  0 0.000
#&gt; GSM559456     1  0.0692      0.807 0.976 0.000 0.004 0.020  0 0.000
#&gt; GSM559457     1  0.0000      0.813 1.000 0.000 0.000 0.000  0 0.000
#&gt; GSM559458     3  0.1075      0.936 0.000 0.000 0.952 0.000  0 0.048
#&gt; GSM559459     1  0.0632      0.807 0.976 0.000 0.000 0.000  0 0.024
#&gt; GSM559460     6  0.0547      0.787 0.020 0.000 0.000 0.000  0 0.980
#&gt; GSM559461     6  0.2454      0.661 0.160 0.000 0.000 0.000  0 0.840
#&gt; GSM559462     1  0.3672      0.390 0.632 0.000 0.000 0.000  0 0.368
#&gt; GSM559463     6  0.3684      0.450 0.372 0.000 0.000 0.000  0 0.628
#&gt; GSM559464     6  0.0547      0.787 0.020 0.000 0.000 0.000  0 0.980
#&gt; GSM559465     6  0.0547      0.787 0.020 0.000 0.000 0.000  0 0.980
#&gt; GSM559467     1  0.5925      0.174 0.456 0.000 0.000 0.236  0 0.308
#&gt; GSM559468     6  0.0000      0.786 0.000 0.000 0.000 0.000  0 1.000
#&gt; GSM559469     6  0.0000      0.786 0.000 0.000 0.000 0.000  0 1.000
#&gt; GSM559470     4  0.2790      0.869 0.140 0.000 0.000 0.840  0 0.020
#&gt; GSM559471     6  0.3672      0.456 0.368 0.000 0.000 0.000  0 0.632
#&gt; GSM559472     1  0.0458      0.808 0.984 0.000 0.000 0.000  0 0.016
#&gt; GSM559473     2  0.0000      0.951 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM559475     2  0.0000      0.951 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM559477     2  0.0000      0.951 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM559478     2  0.0000      0.951 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM559479     2  0.0000      0.951 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM559480     2  0.0000      0.951 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM559481     2  0.0000      0.951 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM559482     2  0.0000      0.951 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM559435     3  0.0000      0.992 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM559439     3  0.0000      0.992 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM559443     3  0.0000      0.992 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM559447     3  0.0000      0.992 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM559449     3  0.0000      0.992 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM559453     3  0.0000      0.992 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM559466     3  0.0000      0.992 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM559474     5  0.0000      1.000 0.000 0.000 0.000 0.000  1 0.000
#&gt; GSM559476     3  0.0000      0.992 0.000 0.000 1.000 0.000  0 0.000
#&gt; GSM559483     2  0.0000      0.951 0.000 1.000 0.000 0.000  0 0.000
#&gt; GSM559484     5  0.0000      1.000 0.000 0.000 0.000 0.000  1 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-5-a').click(function(){
  $('#tab-MAD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-pam-signature_compare](figure_cola/MAD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-pam-collect-classes](figure_cola/MAD-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n disease.state(p) k
#> MAD:pam 52         1.20e-07 2
#> MAD:pam 51         1.89e-09 3
#> MAD:pam 43         2.59e-07 4
#> MAD:pam 45         4.32e-07 5
#> MAD:pam 45         1.30e-06 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "mclust"]
# you can also extract it by
# res = res_list["MAD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-mclust-collect-plots](figure_cola/MAD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-mclust-select-partition-number](figure_cola/MAD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.453           0.865       0.894         0.4122 0.599   0.599
#> 3 3 0.588           0.803       0.892         0.5632 0.602   0.399
#> 4 4 0.870           0.920       0.955        -0.0335 0.761   0.501
#> 5 5 0.786           0.774       0.893         0.1355 0.925   0.807
#> 6 6 0.749           0.701       0.807         0.0733 0.855   0.571
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 5
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-classes'>
<ul>
<li><a href='#tab-MAD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-mclust-get-classes-1'>
<p><a id='tab-MAD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559433     1  0.6438      0.992 0.836 0.164
#&gt; GSM559434     2  0.0000      0.893 0.000 1.000
#&gt; GSM559436     1  0.6343      0.994 0.840 0.160
#&gt; GSM559437     2  0.0000      0.893 0.000 1.000
#&gt; GSM559438     2  0.8763      0.467 0.296 0.704
#&gt; GSM559440     2  0.0000      0.893 0.000 1.000
#&gt; GSM559441     2  0.0000      0.893 0.000 1.000
#&gt; GSM559442     2  0.9608      0.202 0.384 0.616
#&gt; GSM559444     2  0.0000      0.893 0.000 1.000
#&gt; GSM559445     2  0.0000      0.893 0.000 1.000
#&gt; GSM559446     2  0.0000      0.893 0.000 1.000
#&gt; GSM559448     2  0.1184      0.889 0.016 0.984
#&gt; GSM559450     2  0.0000      0.893 0.000 1.000
#&gt; GSM559451     1  0.6438      0.992 0.836 0.164
#&gt; GSM559452     2  0.4939      0.852 0.108 0.892
#&gt; GSM559454     1  0.6343      0.994 0.840 0.160
#&gt; GSM559455     2  0.1633      0.877 0.024 0.976
#&gt; GSM559456     2  0.0000      0.893 0.000 1.000
#&gt; GSM559457     1  0.6712      0.983 0.824 0.176
#&gt; GSM559458     2  0.1184      0.889 0.016 0.984
#&gt; GSM559459     1  0.6343      0.994 0.840 0.160
#&gt; GSM559460     1  0.6343      0.994 0.840 0.160
#&gt; GSM559461     1  0.6343      0.994 0.840 0.160
#&gt; GSM559462     1  0.6973      0.966 0.812 0.188
#&gt; GSM559463     2  0.7139      0.677 0.196 0.804
#&gt; GSM559464     1  0.6343      0.994 0.840 0.160
#&gt; GSM559465     1  0.6531      0.989 0.832 0.168
#&gt; GSM559467     2  0.7299      0.658 0.204 0.796
#&gt; GSM559468     1  0.6343      0.994 0.840 0.160
#&gt; GSM559469     2  0.9209      0.360 0.336 0.664
#&gt; GSM559470     2  0.0672      0.888 0.008 0.992
#&gt; GSM559471     1  0.6531      0.990 0.832 0.168
#&gt; GSM559472     1  0.6343      0.994 0.840 0.160
#&gt; GSM559473     2  0.0000      0.893 0.000 1.000
#&gt; GSM559475     2  0.0000      0.893 0.000 1.000
#&gt; GSM559477     2  0.0000      0.893 0.000 1.000
#&gt; GSM559478     2  0.0000      0.893 0.000 1.000
#&gt; GSM559479     2  0.0000      0.893 0.000 1.000
#&gt; GSM559480     2  0.0000      0.893 0.000 1.000
#&gt; GSM559481     2  0.0000      0.893 0.000 1.000
#&gt; GSM559482     2  0.0000      0.893 0.000 1.000
#&gt; GSM559435     2  0.6438      0.826 0.164 0.836
#&gt; GSM559439     2  0.6438      0.826 0.164 0.836
#&gt; GSM559443     2  0.6438      0.826 0.164 0.836
#&gt; GSM559447     2  0.6438      0.826 0.164 0.836
#&gt; GSM559449     2  0.6438      0.826 0.164 0.836
#&gt; GSM559453     2  0.6438      0.826 0.164 0.836
#&gt; GSM559466     2  0.6438      0.826 0.164 0.836
#&gt; GSM559474     2  0.6438      0.826 0.164 0.836
#&gt; GSM559476     2  0.6438      0.826 0.164 0.836
#&gt; GSM559483     2  0.0000      0.893 0.000 1.000
#&gt; GSM559484     2  0.6438      0.826 0.164 0.836
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-1-a').click(function(){
  $('#tab-MAD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-2'>
<p><a id='tab-MAD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559433     1  0.0000     0.9169 1.000 0.000 0.000
#&gt; GSM559434     2  0.4968     0.6961 0.012 0.800 0.188
#&gt; GSM559436     1  0.0592     0.9114 0.988 0.000 0.012
#&gt; GSM559437     3  0.8392     0.6992 0.200 0.176 0.624
#&gt; GSM559438     2  0.6596     0.5714 0.256 0.704 0.040
#&gt; GSM559440     2  0.4178     0.7447 0.172 0.828 0.000
#&gt; GSM559441     3  0.9319     0.4724 0.340 0.176 0.484
#&gt; GSM559442     1  0.2550     0.8684 0.936 0.040 0.024
#&gt; GSM559444     2  0.0592     0.9335 0.012 0.988 0.000
#&gt; GSM559445     3  0.8392     0.6992 0.200 0.176 0.624
#&gt; GSM559446     3  0.8392     0.6992 0.200 0.176 0.624
#&gt; GSM559448     3  0.7564     0.6379 0.296 0.068 0.636
#&gt; GSM559450     2  0.0592     0.9335 0.012 0.988 0.000
#&gt; GSM559451     1  0.0000     0.9169 1.000 0.000 0.000
#&gt; GSM559452     3  0.7613     0.6147 0.064 0.316 0.620
#&gt; GSM559454     1  0.0000     0.9169 1.000 0.000 0.000
#&gt; GSM559455     1  0.5253     0.7541 0.828 0.076 0.096
#&gt; GSM559456     3  0.8009     0.6592 0.276 0.100 0.624
#&gt; GSM559457     1  0.0000     0.9169 1.000 0.000 0.000
#&gt; GSM559458     3  0.8013     0.6780 0.252 0.112 0.636
#&gt; GSM559459     1  0.0592     0.9114 0.988 0.000 0.012
#&gt; GSM559460     1  0.0000     0.9169 1.000 0.000 0.000
#&gt; GSM559461     1  0.0000     0.9169 1.000 0.000 0.000
#&gt; GSM559462     1  0.0592     0.9114 0.988 0.000 0.012
#&gt; GSM559463     1  0.0747     0.9106 0.984 0.000 0.016
#&gt; GSM559464     1  0.0000     0.9169 1.000 0.000 0.000
#&gt; GSM559465     1  0.0000     0.9169 1.000 0.000 0.000
#&gt; GSM559467     1  0.6438     0.6613 0.764 0.100 0.136
#&gt; GSM559468     1  0.0000     0.9169 1.000 0.000 0.000
#&gt; GSM559469     1  0.7036     0.5801 0.720 0.096 0.184
#&gt; GSM559470     1  0.8663    -0.0275 0.524 0.112 0.364
#&gt; GSM559471     1  0.0000     0.9169 1.000 0.000 0.000
#&gt; GSM559472     1  0.0592     0.9114 0.988 0.000 0.012
#&gt; GSM559473     2  0.0592     0.9335 0.012 0.988 0.000
#&gt; GSM559475     2  0.0592     0.9335 0.012 0.988 0.000
#&gt; GSM559477     2  0.0592     0.9335 0.012 0.988 0.000
#&gt; GSM559478     2  0.0592     0.9335 0.012 0.988 0.000
#&gt; GSM559479     2  0.0592     0.9335 0.012 0.988 0.000
#&gt; GSM559480     2  0.0000     0.9237 0.000 1.000 0.000
#&gt; GSM559481     2  0.0000     0.9237 0.000 1.000 0.000
#&gt; GSM559482     2  0.0592     0.9335 0.012 0.988 0.000
#&gt; GSM559435     3  0.0000     0.7770 0.000 0.000 1.000
#&gt; GSM559439     3  0.0000     0.7770 0.000 0.000 1.000
#&gt; GSM559443     3  0.0424     0.7791 0.000 0.008 0.992
#&gt; GSM559447     3  0.0000     0.7770 0.000 0.000 1.000
#&gt; GSM559449     3  0.0237     0.7782 0.000 0.004 0.996
#&gt; GSM559453     3  0.0424     0.7791 0.000 0.008 0.992
#&gt; GSM559466     3  0.0000     0.7770 0.000 0.000 1.000
#&gt; GSM559474     3  0.3412     0.7682 0.000 0.124 0.876
#&gt; GSM559476     3  0.5060     0.7766 0.064 0.100 0.836
#&gt; GSM559483     2  0.0592     0.9335 0.012 0.988 0.000
#&gt; GSM559484     3  0.3412     0.7682 0.000 0.124 0.876
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-2-a').click(function(){
  $('#tab-MAD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-3'>
<p><a id='tab-MAD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559433     1  0.0336      0.947 0.992 0.000 0.000 0.008
#&gt; GSM559434     2  0.5109      0.578 0.212 0.736 0.000 0.052
#&gt; GSM559436     1  0.0336      0.947 0.992 0.000 0.000 0.008
#&gt; GSM559437     1  0.2868      0.893 0.864 0.000 0.000 0.136
#&gt; GSM559438     1  0.3858      0.847 0.844 0.100 0.000 0.056
#&gt; GSM559440     2  0.4916      0.633 0.184 0.760 0.000 0.056
#&gt; GSM559441     1  0.2345      0.915 0.900 0.000 0.000 0.100
#&gt; GSM559442     1  0.0592      0.947 0.984 0.000 0.000 0.016
#&gt; GSM559444     2  0.1406      0.903 0.016 0.960 0.000 0.024
#&gt; GSM559445     1  0.2868      0.893 0.864 0.000 0.000 0.136
#&gt; GSM559446     1  0.2868      0.893 0.864 0.000 0.000 0.136
#&gt; GSM559448     1  0.1118      0.943 0.964 0.000 0.000 0.036
#&gt; GSM559450     2  0.0779      0.916 0.016 0.980 0.000 0.004
#&gt; GSM559451     1  0.0336      0.947 0.992 0.000 0.000 0.008
#&gt; GSM559452     2  0.2853      0.853 0.016 0.900 0.008 0.076
#&gt; GSM559454     1  0.0336      0.947 0.992 0.000 0.000 0.008
#&gt; GSM559455     1  0.1637      0.932 0.940 0.000 0.000 0.060
#&gt; GSM559456     1  0.2530      0.909 0.888 0.000 0.000 0.112
#&gt; GSM559457     1  0.0188      0.948 0.996 0.000 0.000 0.004
#&gt; GSM559458     1  0.3037      0.906 0.880 0.000 0.020 0.100
#&gt; GSM559459     1  0.0336      0.947 0.992 0.000 0.000 0.008
#&gt; GSM559460     1  0.0336      0.947 0.992 0.000 0.000 0.008
#&gt; GSM559461     1  0.0336      0.947 0.992 0.000 0.000 0.008
#&gt; GSM559462     1  0.0000      0.948 1.000 0.000 0.000 0.000
#&gt; GSM559463     1  0.0336      0.947 0.992 0.000 0.000 0.008
#&gt; GSM559464     1  0.0336      0.947 0.992 0.000 0.000 0.008
#&gt; GSM559465     1  0.0336      0.947 0.992 0.000 0.000 0.008
#&gt; GSM559467     1  0.1211      0.940 0.960 0.000 0.000 0.040
#&gt; GSM559468     1  0.0000      0.948 1.000 0.000 0.000 0.000
#&gt; GSM559469     1  0.1389      0.939 0.952 0.000 0.000 0.048
#&gt; GSM559470     1  0.1867      0.927 0.928 0.000 0.000 0.072
#&gt; GSM559471     1  0.0000      0.948 1.000 0.000 0.000 0.000
#&gt; GSM559472     1  0.0188      0.948 0.996 0.000 0.000 0.004
#&gt; GSM559473     2  0.0000      0.925 0.000 1.000 0.000 0.000
#&gt; GSM559475     2  0.0336      0.922 0.008 0.992 0.000 0.000
#&gt; GSM559477     2  0.0000      0.925 0.000 1.000 0.000 0.000
#&gt; GSM559478     2  0.0000      0.925 0.000 1.000 0.000 0.000
#&gt; GSM559479     2  0.0000      0.925 0.000 1.000 0.000 0.000
#&gt; GSM559480     2  0.0000      0.925 0.000 1.000 0.000 0.000
#&gt; GSM559481     2  0.0000      0.925 0.000 1.000 0.000 0.000
#&gt; GSM559482     2  0.0000      0.925 0.000 1.000 0.000 0.000
#&gt; GSM559435     3  0.0000      0.995 0.000 0.000 1.000 0.000
#&gt; GSM559439     3  0.0000      0.995 0.000 0.000 1.000 0.000
#&gt; GSM559443     3  0.0000      0.995 0.000 0.000 1.000 0.000
#&gt; GSM559447     3  0.0000      0.995 0.000 0.000 1.000 0.000
#&gt; GSM559449     3  0.0000      0.995 0.000 0.000 1.000 0.000
#&gt; GSM559453     3  0.0707      0.971 0.000 0.000 0.980 0.020
#&gt; GSM559466     3  0.0000      0.995 0.000 0.000 1.000 0.000
#&gt; GSM559474     4  0.1474      0.962 0.000 0.000 0.052 0.948
#&gt; GSM559476     1  0.5041      0.690 0.728 0.000 0.232 0.040
#&gt; GSM559483     2  0.0000      0.925 0.000 1.000 0.000 0.000
#&gt; GSM559484     4  0.2282      0.962 0.000 0.024 0.052 0.924
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-3-a').click(function(){
  $('#tab-MAD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-4'>
<p><a id='tab-MAD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559433     1  0.0000     0.8564 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559434     2  0.1960     0.8305 0.004 0.928 0.000 0.048 0.020
#&gt; GSM559436     1  0.0000     0.8564 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559437     4  0.2179     0.7019 0.112 0.000 0.000 0.888 0.000
#&gt; GSM559438     1  0.5907     0.0679 0.496 0.428 0.000 0.020 0.056
#&gt; GSM559440     2  0.3347     0.7529 0.056 0.864 0.000 0.024 0.056
#&gt; GSM559441     1  0.5338     0.0680 0.544 0.000 0.000 0.400 0.056
#&gt; GSM559442     1  0.1168     0.8383 0.960 0.000 0.000 0.008 0.032
#&gt; GSM559444     2  0.0451     0.8629 0.000 0.988 0.000 0.008 0.004
#&gt; GSM559445     4  0.2127     0.6940 0.108 0.000 0.000 0.892 0.000
#&gt; GSM559446     4  0.2179     0.7019 0.112 0.000 0.000 0.888 0.000
#&gt; GSM559448     1  0.4347     0.6487 0.780 0.000 0.076 0.136 0.008
#&gt; GSM559450     2  0.0324     0.8644 0.000 0.992 0.000 0.004 0.004
#&gt; GSM559451     1  0.0000     0.8564 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559452     2  0.2286     0.7917 0.000 0.888 0.004 0.108 0.000
#&gt; GSM559454     1  0.0000     0.8564 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559455     1  0.3055     0.7242 0.840 0.000 0.000 0.144 0.016
#&gt; GSM559456     4  0.4304     0.1398 0.484 0.000 0.000 0.516 0.000
#&gt; GSM559457     1  0.0000     0.8564 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559458     1  0.3741     0.5868 0.732 0.000 0.004 0.264 0.000
#&gt; GSM559459     1  0.0000     0.8564 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559460     1  0.0000     0.8564 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559461     1  0.0000     0.8564 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559462     1  0.0000     0.8564 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559463     1  0.0290     0.8516 0.992 0.000 0.000 0.008 0.000
#&gt; GSM559464     1  0.0000     0.8564 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559465     1  0.0000     0.8564 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559467     1  0.1845     0.8188 0.928 0.000 0.000 0.016 0.056
#&gt; GSM559468     1  0.0000     0.8564 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559469     1  0.1845     0.8188 0.928 0.000 0.000 0.016 0.056
#&gt; GSM559470     1  0.5274     0.1679 0.572 0.000 0.000 0.372 0.056
#&gt; GSM559471     1  0.0703     0.8463 0.976 0.000 0.000 0.000 0.024
#&gt; GSM559472     1  0.0000     0.8564 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559473     2  0.0162     0.8653 0.000 0.996 0.000 0.004 0.000
#&gt; GSM559475     2  0.0324     0.8644 0.000 0.992 0.000 0.004 0.004
#&gt; GSM559477     2  0.3123     0.8794 0.000 0.828 0.000 0.012 0.160
#&gt; GSM559478     2  0.3236     0.8804 0.000 0.828 0.000 0.020 0.152
#&gt; GSM559479     2  0.3123     0.8794 0.000 0.828 0.000 0.012 0.160
#&gt; GSM559480     2  0.3236     0.8804 0.000 0.828 0.000 0.020 0.152
#&gt; GSM559481     2  0.3236     0.8804 0.000 0.828 0.000 0.020 0.152
#&gt; GSM559482     2  0.3123     0.8794 0.000 0.828 0.000 0.012 0.160
#&gt; GSM559435     3  0.0162     0.9727 0.000 0.000 0.996 0.000 0.004
#&gt; GSM559439     3  0.0162     0.9727 0.000 0.000 0.996 0.000 0.004
#&gt; GSM559443     3  0.1942     0.9260 0.000 0.000 0.920 0.068 0.012
#&gt; GSM559447     3  0.0000     0.9732 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559449     3  0.0000     0.9732 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559453     3  0.1341     0.9258 0.000 0.000 0.944 0.056 0.000
#&gt; GSM559466     3  0.0000     0.9732 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559474     5  0.3491     0.7626 0.000 0.000 0.004 0.228 0.768
#&gt; GSM559476     1  0.6281     0.1817 0.532 0.000 0.324 0.136 0.008
#&gt; GSM559483     2  0.3123     0.8794 0.000 0.828 0.000 0.012 0.160
#&gt; GSM559484     5  0.4409     0.7828 0.000 0.148 0.004 0.080 0.768
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-4-a').click(function(){
  $('#tab-MAD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-5'>
<p><a id='tab-MAD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559433     1  0.0146    0.91529 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; GSM559434     6  0.3898    0.80398 0.000 0.336 0.000 0.012 0.000 0.652
#&gt; GSM559436     1  0.1387    0.86637 0.932 0.000 0.000 0.000 0.068 0.000
#&gt; GSM559437     4  0.0547    0.68149 0.020 0.000 0.000 0.980 0.000 0.000
#&gt; GSM559438     6  0.5630    0.05616 0.416 0.104 0.000 0.012 0.000 0.468
#&gt; GSM559440     6  0.3898    0.80398 0.000 0.336 0.000 0.012 0.000 0.652
#&gt; GSM559441     4  0.4504    0.30719 0.432 0.000 0.000 0.536 0.000 0.032
#&gt; GSM559442     1  0.2420    0.83636 0.888 0.000 0.000 0.004 0.076 0.032
#&gt; GSM559444     6  0.3847    0.80450 0.000 0.348 0.000 0.008 0.000 0.644
#&gt; GSM559445     4  0.0622    0.66857 0.008 0.000 0.000 0.980 0.000 0.012
#&gt; GSM559446     4  0.0547    0.68149 0.020 0.000 0.000 0.980 0.000 0.000
#&gt; GSM559448     5  0.4594    0.00530 0.480 0.000 0.000 0.000 0.484 0.036
#&gt; GSM559450     6  0.3756    0.80239 0.000 0.352 0.000 0.004 0.000 0.644
#&gt; GSM559451     1  0.0146    0.91529 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; GSM559452     6  0.3912    0.80478 0.000 0.340 0.000 0.012 0.000 0.648
#&gt; GSM559454     1  0.0000    0.91546 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559455     1  0.2597    0.70826 0.824 0.000 0.000 0.176 0.000 0.000
#&gt; GSM559456     4  0.2491    0.63431 0.164 0.000 0.000 0.836 0.000 0.000
#&gt; GSM559457     1  0.0146    0.91529 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; GSM559458     4  0.5310    0.31359 0.392 0.000 0.000 0.512 0.092 0.004
#&gt; GSM559459     1  0.0000    0.91546 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559460     1  0.0000    0.91546 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559461     1  0.0000    0.91546 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559462     1  0.0146    0.91529 0.996 0.000 0.000 0.004 0.000 0.000
#&gt; GSM559463     1  0.2915    0.72349 0.808 0.000 0.000 0.000 0.184 0.008
#&gt; GSM559464     1  0.0146    0.91462 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; GSM559465     1  0.1471    0.86757 0.932 0.000 0.000 0.000 0.064 0.004
#&gt; GSM559467     1  0.0622    0.90798 0.980 0.000 0.000 0.012 0.000 0.008
#&gt; GSM559468     1  0.0146    0.91506 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM559469     1  0.2649    0.83335 0.880 0.000 0.000 0.012 0.072 0.036
#&gt; GSM559470     1  0.4490    0.48040 0.700 0.000 0.000 0.104 0.000 0.196
#&gt; GSM559471     1  0.2039    0.85309 0.908 0.000 0.000 0.004 0.072 0.016
#&gt; GSM559472     1  0.0000    0.91546 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559473     6  0.3991    0.63812 0.000 0.472 0.000 0.004 0.000 0.524
#&gt; GSM559475     6  0.3944    0.71874 0.000 0.428 0.000 0.004 0.000 0.568
#&gt; GSM559477     2  0.0000    1.00000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559478     2  0.0000    1.00000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559479     2  0.0000    1.00000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.0000    1.00000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559481     2  0.0000    1.00000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559482     2  0.0000    1.00000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.3866    0.64743 0.000 0.000 0.516 0.000 0.484 0.000
#&gt; GSM559439     3  0.3868    0.64443 0.000 0.000 0.508 0.000 0.492 0.000
#&gt; GSM559443     5  0.3023   -0.53042 0.000 0.000 0.232 0.000 0.768 0.000
#&gt; GSM559447     3  0.3867    0.64606 0.000 0.000 0.512 0.000 0.488 0.000
#&gt; GSM559449     3  0.4096    0.64622 0.000 0.000 0.508 0.000 0.484 0.008
#&gt; GSM559453     3  0.4184    0.64449 0.000 0.000 0.504 0.000 0.484 0.012
#&gt; GSM559466     3  0.3866    0.64743 0.000 0.000 0.516 0.000 0.484 0.000
#&gt; GSM559474     3  0.5798   -0.09075 0.000 0.000 0.484 0.204 0.000 0.312
#&gt; GSM559476     5  0.3409    0.34432 0.300 0.000 0.000 0.000 0.700 0.000
#&gt; GSM559483     2  0.0000    1.00000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559484     3  0.4407    0.00871 0.000 0.000 0.492 0.024 0.000 0.484
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-5-a').click(function(){
  $('#tab-MAD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-mclust-signature_compare](figure_cola/MAD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-mclust-collect-classes](figure_cola/MAD-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) k
#> MAD:mclust 49         4.52e-02 2
#> MAD:mclust 50         3.37e-05 3
#> MAD:mclust 52         7.78e-09 4
#> MAD:mclust 47         2.16e-08 5
#> MAD:mclust 43         2.07e-07 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### MAD:NMF*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "NMF"]
# you can also extract it by
# res = res_list["MAD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-NMF-collect-plots](figure_cola/MAD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-NMF-select-partition-number](figure_cola/MAD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.920           0.945       0.974         0.4839 0.509   0.509
#> 3 3 0.770           0.796       0.922         0.3351 0.798   0.620
#> 4 4 0.771           0.771       0.883         0.1092 0.837   0.596
#> 5 5 0.690           0.641       0.745         0.0906 0.851   0.540
#> 6 6 0.738           0.675       0.826         0.0554 0.924   0.666
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-classes'>
<ul>
<li><a href='#tab-MAD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-NMF-get-classes-1'>
<p><a id='tab-MAD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559433     1  0.0000      0.987 1.000 0.000
#&gt; GSM559434     2  0.0000      0.950 0.000 1.000
#&gt; GSM559436     1  0.0000      0.987 1.000 0.000
#&gt; GSM559437     1  0.0938      0.979 0.988 0.012
#&gt; GSM559438     2  0.0000      0.950 0.000 1.000
#&gt; GSM559440     2  0.0000      0.950 0.000 1.000
#&gt; GSM559441     2  0.9491      0.459 0.368 0.632
#&gt; GSM559442     1  0.3733      0.923 0.928 0.072
#&gt; GSM559444     2  0.0000      0.950 0.000 1.000
#&gt; GSM559445     2  0.6148      0.821 0.152 0.848
#&gt; GSM559446     1  0.3879      0.921 0.924 0.076
#&gt; GSM559448     1  0.0000      0.987 1.000 0.000
#&gt; GSM559450     2  0.0000      0.950 0.000 1.000
#&gt; GSM559451     1  0.0376      0.984 0.996 0.004
#&gt; GSM559452     2  0.0000      0.950 0.000 1.000
#&gt; GSM559454     1  0.0000      0.987 1.000 0.000
#&gt; GSM559455     1  0.0672      0.982 0.992 0.008
#&gt; GSM559456     1  0.0000      0.987 1.000 0.000
#&gt; GSM559457     1  0.0000      0.987 1.000 0.000
#&gt; GSM559458     1  0.0000      0.987 1.000 0.000
#&gt; GSM559459     1  0.0000      0.987 1.000 0.000
#&gt; GSM559460     1  0.0000      0.987 1.000 0.000
#&gt; GSM559461     1  0.0000      0.987 1.000 0.000
#&gt; GSM559462     1  0.2423      0.956 0.960 0.040
#&gt; GSM559463     1  0.0000      0.987 1.000 0.000
#&gt; GSM559464     1  0.0000      0.987 1.000 0.000
#&gt; GSM559465     1  0.0000      0.987 1.000 0.000
#&gt; GSM559467     2  0.7219      0.765 0.200 0.800
#&gt; GSM559468     1  0.0000      0.987 1.000 0.000
#&gt; GSM559469     2  0.8081      0.697 0.248 0.752
#&gt; GSM559470     2  0.0672      0.945 0.008 0.992
#&gt; GSM559471     1  0.5737      0.845 0.864 0.136
#&gt; GSM559472     1  0.0000      0.987 1.000 0.000
#&gt; GSM559473     2  0.0000      0.950 0.000 1.000
#&gt; GSM559475     2  0.0000      0.950 0.000 1.000
#&gt; GSM559477     2  0.0000      0.950 0.000 1.000
#&gt; GSM559478     2  0.0000      0.950 0.000 1.000
#&gt; GSM559479     2  0.0000      0.950 0.000 1.000
#&gt; GSM559480     2  0.0000      0.950 0.000 1.000
#&gt; GSM559481     2  0.0000      0.950 0.000 1.000
#&gt; GSM559482     2  0.0000      0.950 0.000 1.000
#&gt; GSM559435     1  0.0000      0.987 1.000 0.000
#&gt; GSM559439     1  0.0000      0.987 1.000 0.000
#&gt; GSM559443     1  0.0000      0.987 1.000 0.000
#&gt; GSM559447     1  0.0000      0.987 1.000 0.000
#&gt; GSM559449     1  0.0000      0.987 1.000 0.000
#&gt; GSM559453     1  0.0000      0.987 1.000 0.000
#&gt; GSM559466     1  0.0000      0.987 1.000 0.000
#&gt; GSM559474     1  0.2236      0.960 0.964 0.036
#&gt; GSM559476     1  0.0000      0.987 1.000 0.000
#&gt; GSM559483     2  0.0000      0.950 0.000 1.000
#&gt; GSM559484     2  0.0000      0.950 0.000 1.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-1-a').click(function(){
  $('#tab-MAD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-2'>
<p><a id='tab-MAD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559433     1  0.0000     0.9147 1.000 0.000 0.000
#&gt; GSM559434     2  0.0000     0.8949 0.000 1.000 0.000
#&gt; GSM559436     1  0.0000     0.9147 1.000 0.000 0.000
#&gt; GSM559437     1  0.1529     0.8834 0.960 0.000 0.040
#&gt; GSM559438     2  0.0747     0.8845 0.016 0.984 0.000
#&gt; GSM559440     2  0.0000     0.8949 0.000 1.000 0.000
#&gt; GSM559441     1  0.6307    -0.0927 0.512 0.488 0.000
#&gt; GSM559442     1  0.1163     0.8933 0.972 0.028 0.000
#&gt; GSM559444     2  0.0000     0.8949 0.000 1.000 0.000
#&gt; GSM559445     2  0.7592     0.6068 0.112 0.680 0.208
#&gt; GSM559446     1  0.8332     0.3160 0.580 0.104 0.316
#&gt; GSM559448     1  0.3116     0.8034 0.892 0.000 0.108
#&gt; GSM559450     2  0.0000     0.8949 0.000 1.000 0.000
#&gt; GSM559451     1  0.0000     0.9147 1.000 0.000 0.000
#&gt; GSM559452     2  0.4842     0.6634 0.000 0.776 0.224
#&gt; GSM559454     1  0.0000     0.9147 1.000 0.000 0.000
#&gt; GSM559455     1  0.0000     0.9147 1.000 0.000 0.000
#&gt; GSM559456     1  0.0000     0.9147 1.000 0.000 0.000
#&gt; GSM559457     1  0.0000     0.9147 1.000 0.000 0.000
#&gt; GSM559458     3  0.5733     0.5233 0.324 0.000 0.676
#&gt; GSM559459     1  0.0000     0.9147 1.000 0.000 0.000
#&gt; GSM559460     1  0.0000     0.9147 1.000 0.000 0.000
#&gt; GSM559461     1  0.0000     0.9147 1.000 0.000 0.000
#&gt; GSM559462     1  0.0000     0.9147 1.000 0.000 0.000
#&gt; GSM559463     1  0.0000     0.9147 1.000 0.000 0.000
#&gt; GSM559464     1  0.0000     0.9147 1.000 0.000 0.000
#&gt; GSM559465     1  0.0000     0.9147 1.000 0.000 0.000
#&gt; GSM559467     2  0.6274     0.2095 0.456 0.544 0.000
#&gt; GSM559468     1  0.0000     0.9147 1.000 0.000 0.000
#&gt; GSM559469     2  0.5988     0.4514 0.368 0.632 0.000
#&gt; GSM559470     2  0.5098     0.6654 0.248 0.752 0.000
#&gt; GSM559471     1  0.1289     0.8897 0.968 0.032 0.000
#&gt; GSM559472     1  0.0000     0.9147 1.000 0.000 0.000
#&gt; GSM559473     2  0.0000     0.8949 0.000 1.000 0.000
#&gt; GSM559475     2  0.0000     0.8949 0.000 1.000 0.000
#&gt; GSM559477     2  0.0000     0.8949 0.000 1.000 0.000
#&gt; GSM559478     2  0.0000     0.8949 0.000 1.000 0.000
#&gt; GSM559479     2  0.0000     0.8949 0.000 1.000 0.000
#&gt; GSM559480     2  0.0000     0.8949 0.000 1.000 0.000
#&gt; GSM559481     2  0.0000     0.8949 0.000 1.000 0.000
#&gt; GSM559482     2  0.0000     0.8949 0.000 1.000 0.000
#&gt; GSM559435     3  0.1031     0.8903 0.024 0.000 0.976
#&gt; GSM559439     3  0.0237     0.8997 0.004 0.000 0.996
#&gt; GSM559443     1  0.6309    -0.1170 0.504 0.000 0.496
#&gt; GSM559447     3  0.0000     0.9006 0.000 0.000 1.000
#&gt; GSM559449     3  0.0000     0.9006 0.000 0.000 1.000
#&gt; GSM559453     3  0.0000     0.9006 0.000 0.000 1.000
#&gt; GSM559466     3  0.0000     0.9006 0.000 0.000 1.000
#&gt; GSM559474     3  0.0000     0.9006 0.000 0.000 1.000
#&gt; GSM559476     3  0.6045     0.3865 0.380 0.000 0.620
#&gt; GSM559483     2  0.0000     0.8949 0.000 1.000 0.000
#&gt; GSM559484     3  0.1964     0.8567 0.000 0.056 0.944
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-2-a').click(function(){
  $('#tab-MAD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-3'>
<p><a id='tab-MAD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559433     1  0.1118      0.851 0.964 0.000 0.036 0.000
#&gt; GSM559434     2  0.0817      0.945 0.000 0.976 0.000 0.024
#&gt; GSM559436     1  0.4955      0.390 0.556 0.000 0.444 0.000
#&gt; GSM559437     1  0.1792      0.799 0.932 0.000 0.000 0.068
#&gt; GSM559438     2  0.2275      0.899 0.048 0.928 0.004 0.020
#&gt; GSM559440     2  0.0895      0.946 0.004 0.976 0.000 0.020
#&gt; GSM559441     1  0.1174      0.832 0.968 0.012 0.000 0.020
#&gt; GSM559442     3  0.7560      0.331 0.172 0.216 0.584 0.028
#&gt; GSM559444     2  0.0921      0.944 0.000 0.972 0.000 0.028
#&gt; GSM559445     4  0.5663      0.292 0.440 0.024 0.000 0.536
#&gt; GSM559446     1  0.4961     -0.151 0.552 0.000 0.000 0.448
#&gt; GSM559448     3  0.1369      0.781 0.016 0.004 0.964 0.016
#&gt; GSM559450     2  0.1022      0.942 0.000 0.968 0.000 0.032
#&gt; GSM559451     1  0.0921      0.851 0.972 0.000 0.028 0.000
#&gt; GSM559452     2  0.2546      0.899 0.000 0.912 0.028 0.060
#&gt; GSM559454     1  0.2814      0.836 0.868 0.000 0.132 0.000
#&gt; GSM559455     1  0.0336      0.841 0.992 0.000 0.000 0.008
#&gt; GSM559456     1  0.0524      0.846 0.988 0.000 0.008 0.004
#&gt; GSM559457     1  0.0188      0.843 0.996 0.000 0.000 0.004
#&gt; GSM559458     4  0.6141      0.553 0.300 0.000 0.076 0.624
#&gt; GSM559459     1  0.2281      0.846 0.904 0.000 0.096 0.000
#&gt; GSM559460     1  0.2149      0.848 0.912 0.000 0.088 0.000
#&gt; GSM559461     1  0.2973      0.831 0.856 0.000 0.144 0.000
#&gt; GSM559462     1  0.0188      0.843 0.996 0.000 0.000 0.004
#&gt; GSM559463     3  0.1863      0.762 0.040 0.004 0.944 0.012
#&gt; GSM559464     1  0.3486      0.807 0.812 0.000 0.188 0.000
#&gt; GSM559465     1  0.3688      0.790 0.792 0.000 0.208 0.000
#&gt; GSM559467     1  0.0524      0.841 0.988 0.004 0.000 0.008
#&gt; GSM559468     1  0.3591      0.817 0.824 0.000 0.168 0.008
#&gt; GSM559469     2  0.6460      0.468 0.268 0.648 0.056 0.028
#&gt; GSM559470     1  0.1182      0.832 0.968 0.016 0.000 0.016
#&gt; GSM559471     1  0.5322      0.758 0.752 0.036 0.188 0.024
#&gt; GSM559472     1  0.3356      0.813 0.824 0.000 0.176 0.000
#&gt; GSM559473     2  0.0524      0.945 0.000 0.988 0.004 0.008
#&gt; GSM559475     2  0.0469      0.948 0.000 0.988 0.000 0.012
#&gt; GSM559477     2  0.0336      0.948 0.000 0.992 0.000 0.008
#&gt; GSM559478     2  0.0524      0.945 0.000 0.988 0.004 0.008
#&gt; GSM559479     2  0.0592      0.947 0.000 0.984 0.000 0.016
#&gt; GSM559480     2  0.1151      0.936 0.000 0.968 0.008 0.024
#&gt; GSM559481     2  0.1004      0.939 0.000 0.972 0.004 0.024
#&gt; GSM559482     2  0.0000      0.947 0.000 1.000 0.000 0.000
#&gt; GSM559435     3  0.2081      0.790 0.000 0.000 0.916 0.084
#&gt; GSM559439     3  0.1792      0.795 0.000 0.000 0.932 0.068
#&gt; GSM559443     3  0.0672      0.797 0.008 0.000 0.984 0.008
#&gt; GSM559447     3  0.3311      0.730 0.000 0.000 0.828 0.172
#&gt; GSM559449     3  0.4761      0.453 0.000 0.000 0.628 0.372
#&gt; GSM559453     4  0.3688      0.485 0.000 0.000 0.208 0.792
#&gt; GSM559466     3  0.3569      0.711 0.000 0.000 0.804 0.196
#&gt; GSM559474     4  0.1488      0.649 0.000 0.012 0.032 0.956
#&gt; GSM559476     3  0.0524      0.797 0.004 0.000 0.988 0.008
#&gt; GSM559483     2  0.0000      0.947 0.000 1.000 0.000 0.000
#&gt; GSM559484     4  0.1624      0.648 0.000 0.020 0.028 0.952
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-3-a').click(function(){
  $('#tab-MAD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-4'>
<p><a id='tab-MAD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559433     4  0.4074     0.6859 0.364 0.000 0.000 0.636 0.000
#&gt; GSM559434     2  0.4538     0.7601 0.004 0.724 0.000 0.044 0.228
#&gt; GSM559436     4  0.6908     0.2493 0.276 0.000 0.340 0.380 0.004
#&gt; GSM559437     4  0.3343     0.6885 0.172 0.000 0.000 0.812 0.016
#&gt; GSM559438     2  0.5944     0.6821 0.108 0.636 0.000 0.024 0.232
#&gt; GSM559440     2  0.5484     0.7269 0.016 0.664 0.000 0.080 0.240
#&gt; GSM559441     4  0.4095     0.7131 0.220 0.004 0.000 0.752 0.024
#&gt; GSM559442     1  0.5306     0.5004 0.748 0.064 0.096 0.004 0.088
#&gt; GSM559444     2  0.5389     0.7223 0.004 0.660 0.000 0.100 0.236
#&gt; GSM559445     4  0.2692     0.4512 0.016 0.008 0.000 0.884 0.092
#&gt; GSM559446     4  0.2438     0.5508 0.040 0.000 0.000 0.900 0.060
#&gt; GSM559448     3  0.1748     0.8815 0.028 0.016 0.944 0.008 0.004
#&gt; GSM559450     2  0.5013     0.7376 0.000 0.680 0.000 0.080 0.240
#&gt; GSM559451     4  0.4586     0.6967 0.336 0.000 0.016 0.644 0.004
#&gt; GSM559452     5  0.6576     0.2237 0.192 0.172 0.004 0.032 0.600
#&gt; GSM559454     1  0.5490     0.0320 0.592 0.000 0.084 0.324 0.000
#&gt; GSM559455     4  0.3752     0.7305 0.292 0.000 0.000 0.708 0.000
#&gt; GSM559456     4  0.3999     0.7115 0.344 0.000 0.000 0.656 0.000
#&gt; GSM559457     4  0.3932     0.7253 0.328 0.000 0.000 0.672 0.000
#&gt; GSM559458     1  0.7603    -0.3120 0.380 0.000 0.052 0.224 0.344
#&gt; GSM559459     1  0.4644     0.2524 0.680 0.000 0.040 0.280 0.000
#&gt; GSM559460     1  0.1502     0.6631 0.940 0.000 0.004 0.056 0.000
#&gt; GSM559461     1  0.2300     0.6556 0.904 0.000 0.024 0.072 0.000
#&gt; GSM559462     4  0.4273     0.5698 0.448 0.000 0.000 0.552 0.000
#&gt; GSM559463     3  0.3177     0.6812 0.208 0.000 0.792 0.000 0.000
#&gt; GSM559464     1  0.1549     0.6695 0.944 0.000 0.016 0.040 0.000
#&gt; GSM559465     1  0.2362     0.6532 0.900 0.000 0.024 0.076 0.000
#&gt; GSM559467     4  0.5924     0.4977 0.376 0.096 0.000 0.524 0.004
#&gt; GSM559468     1  0.0693     0.6663 0.980 0.000 0.008 0.000 0.012
#&gt; GSM559469     1  0.3885     0.5241 0.784 0.176 0.000 0.000 0.040
#&gt; GSM559470     4  0.4252     0.7171 0.340 0.008 0.000 0.652 0.000
#&gt; GSM559471     1  0.1243     0.6613 0.960 0.028 0.008 0.004 0.000
#&gt; GSM559472     1  0.5373     0.0934 0.620 0.000 0.084 0.296 0.000
#&gt; GSM559473     2  0.0671     0.8311 0.016 0.980 0.000 0.000 0.004
#&gt; GSM559475     2  0.1673     0.8177 0.032 0.944 0.000 0.008 0.016
#&gt; GSM559477     2  0.0566     0.8325 0.000 0.984 0.000 0.004 0.012
#&gt; GSM559478     2  0.1787     0.8167 0.032 0.940 0.000 0.012 0.016
#&gt; GSM559479     2  0.3628     0.7792 0.000 0.772 0.000 0.012 0.216
#&gt; GSM559480     2  0.1756     0.8165 0.036 0.940 0.000 0.008 0.016
#&gt; GSM559481     2  0.1588     0.8195 0.028 0.948 0.000 0.008 0.016
#&gt; GSM559482     2  0.0000     0.8322 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.0693     0.8904 0.012 0.000 0.980 0.000 0.008
#&gt; GSM559439     3  0.0671     0.8913 0.016 0.000 0.980 0.000 0.004
#&gt; GSM559443     3  0.1121     0.8839 0.044 0.000 0.956 0.000 0.000
#&gt; GSM559447     3  0.1282     0.8722 0.004 0.000 0.952 0.000 0.044
#&gt; GSM559449     3  0.3141     0.7381 0.000 0.000 0.832 0.016 0.152
#&gt; GSM559453     5  0.6350     0.1631 0.000 0.000 0.420 0.160 0.420
#&gt; GSM559466     3  0.1638     0.8522 0.000 0.000 0.932 0.004 0.064
#&gt; GSM559474     5  0.4302     0.6581 0.000 0.000 0.048 0.208 0.744
#&gt; GSM559476     3  0.1768     0.8721 0.072 0.000 0.924 0.000 0.004
#&gt; GSM559483     2  0.0324     0.8323 0.004 0.992 0.000 0.000 0.004
#&gt; GSM559484     5  0.4302     0.6581 0.000 0.000 0.048 0.208 0.744
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-4-a').click(function(){
  $('#tab-MAD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-5'>
<p><a id='tab-MAD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559433     4  0.4239    0.72331 0.092 0.000 0.024 0.796 0.028 0.060
#&gt; GSM559434     6  0.3014    0.77748 0.000 0.184 0.000 0.012 0.000 0.804
#&gt; GSM559436     4  0.7360    0.40477 0.184 0.024 0.236 0.488 0.024 0.044
#&gt; GSM559437     4  0.1492    0.75037 0.000 0.000 0.000 0.940 0.024 0.036
#&gt; GSM559438     6  0.4418    0.76759 0.072 0.144 0.000 0.024 0.004 0.756
#&gt; GSM559440     6  0.2320    0.80153 0.000 0.080 0.000 0.024 0.004 0.892
#&gt; GSM559441     4  0.2462    0.72305 0.000 0.004 0.000 0.860 0.004 0.132
#&gt; GSM559442     1  0.2462    0.70283 0.860 0.000 0.004 0.000 0.004 0.132
#&gt; GSM559444     6  0.2625    0.77539 0.000 0.072 0.000 0.056 0.000 0.872
#&gt; GSM559445     4  0.4290    0.64947 0.000 0.008 0.000 0.736 0.076 0.180
#&gt; GSM559446     4  0.4222    0.69512 0.004 0.000 0.000 0.748 0.116 0.132
#&gt; GSM559448     3  0.2169    0.79016 0.012 0.060 0.912 0.004 0.004 0.008
#&gt; GSM559450     6  0.2302    0.80201 0.000 0.120 0.000 0.008 0.000 0.872
#&gt; GSM559451     4  0.5227    0.57951 0.216 0.000 0.024 0.672 0.012 0.076
#&gt; GSM559452     6  0.5547    0.35604 0.200 0.004 0.004 0.000 0.196 0.596
#&gt; GSM559454     1  0.5814    0.32607 0.552 0.000 0.044 0.344 0.024 0.036
#&gt; GSM559455     4  0.0748    0.75649 0.004 0.004 0.000 0.976 0.000 0.016
#&gt; GSM559456     4  0.3219    0.71167 0.112 0.000 0.000 0.836 0.012 0.040
#&gt; GSM559457     4  0.1036    0.76174 0.024 0.000 0.000 0.964 0.004 0.008
#&gt; GSM559458     5  0.5709    0.49406 0.292 0.000 0.024 0.024 0.596 0.064
#&gt; GSM559459     1  0.5026    0.54462 0.664 0.000 0.028 0.260 0.020 0.028
#&gt; GSM559460     1  0.1405    0.80010 0.948 0.000 0.000 0.024 0.004 0.024
#&gt; GSM559461     1  0.2367    0.79590 0.900 0.000 0.012 0.064 0.004 0.020
#&gt; GSM559462     4  0.4569    0.21743 0.396 0.000 0.000 0.564 0.000 0.040
#&gt; GSM559463     3  0.5088   -0.00132 0.452 0.000 0.496 0.008 0.028 0.016
#&gt; GSM559464     1  0.1321    0.80008 0.952 0.000 0.000 0.024 0.004 0.020
#&gt; GSM559465     1  0.1745    0.79303 0.920 0.000 0.000 0.068 0.000 0.012
#&gt; GSM559467     2  0.6323    0.04642 0.140 0.492 0.000 0.332 0.012 0.024
#&gt; GSM559468     1  0.1410    0.77643 0.944 0.000 0.000 0.004 0.008 0.044
#&gt; GSM559469     1  0.0717    0.78569 0.976 0.000 0.000 0.000 0.008 0.016
#&gt; GSM559470     4  0.3494    0.75103 0.076 0.024 0.000 0.836 0.004 0.060
#&gt; GSM559471     1  0.1282    0.79981 0.956 0.004 0.000 0.024 0.012 0.004
#&gt; GSM559472     1  0.4860    0.35346 0.584 0.008 0.004 0.372 0.008 0.024
#&gt; GSM559473     2  0.2378    0.75041 0.000 0.848 0.000 0.000 0.000 0.152
#&gt; GSM559475     2  0.0363    0.78581 0.000 0.988 0.000 0.000 0.000 0.012
#&gt; GSM559477     2  0.2941    0.68582 0.000 0.780 0.000 0.000 0.000 0.220
#&gt; GSM559478     2  0.0146    0.78393 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; GSM559479     6  0.3428    0.62559 0.000 0.304 0.000 0.000 0.000 0.696
#&gt; GSM559480     2  0.0436    0.78436 0.004 0.988 0.000 0.000 0.004 0.004
#&gt; GSM559481     2  0.0436    0.78000 0.004 0.988 0.004 0.000 0.004 0.000
#&gt; GSM559482     2  0.2597    0.73460 0.000 0.824 0.000 0.000 0.000 0.176
#&gt; GSM559435     3  0.0146    0.82279 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; GSM559439     3  0.0291    0.82256 0.004 0.000 0.992 0.000 0.000 0.004
#&gt; GSM559443     3  0.1313    0.81019 0.016 0.000 0.952 0.000 0.028 0.004
#&gt; GSM559447     3  0.0790    0.81850 0.000 0.000 0.968 0.000 0.032 0.000
#&gt; GSM559449     3  0.2048    0.77177 0.000 0.000 0.880 0.000 0.120 0.000
#&gt; GSM559453     3  0.3659    0.42951 0.000 0.000 0.636 0.000 0.364 0.000
#&gt; GSM559466     3  0.1387    0.80577 0.000 0.000 0.932 0.000 0.068 0.000
#&gt; GSM559474     5  0.1007    0.78302 0.000 0.000 0.044 0.000 0.956 0.000
#&gt; GSM559476     3  0.2056    0.78915 0.080 0.004 0.904 0.000 0.012 0.000
#&gt; GSM559483     2  0.2854    0.70195 0.000 0.792 0.000 0.000 0.000 0.208
#&gt; GSM559484     5  0.1007    0.78302 0.000 0.000 0.044 0.000 0.956 0.000
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-5-a').click(function(){
  $('#tab-MAD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-NMF-signature_compare](figure_cola/MAD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-NMF-collect-classes](figure_cola/MAD-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n disease.state(p) k
#> MAD:NMF 51         2.06e-01 2
#> MAD:NMF 46         3.44e-08 3
#> MAD:NMF 45         1.14e-05 4
#> MAD:NMF 43         7.53e-06 5
#> MAD:NMF 43         3.44e-06 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:hclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "hclust"]
# you can also extract it by
# res = res_list["ATC:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-hclust-collect-plots](figure_cola/ATC-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-hclust-select-partition-number](figure_cola/ATC-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.481          0.0879       0.599         0.4131 0.683   0.683
#> 3 3 0.466          0.5902       0.808         0.2665 0.607   0.509
#> 4 4 0.484          0.7007       0.731         0.1734 0.756   0.534
#> 5 5 0.634          0.6947       0.858         0.1523 0.956   0.848
#> 6 6 0.639          0.6645       0.800         0.0725 0.989   0.957
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 5
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-classes'>
<ul>
<li><a href='#tab-ATC-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-hclust-get-classes-1'>
<p><a id='tab-ATC-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559433     1   0.998   -0.90186 0.524 0.476
#&gt; GSM559434     2   1.000    1.00000 0.488 0.512
#&gt; GSM559436     1   0.000    0.30476 1.000 0.000
#&gt; GSM559437     1   0.000    0.30476 1.000 0.000
#&gt; GSM559438     1   0.697    0.00519 0.812 0.188
#&gt; GSM559440     1   0.697    0.00519 0.812 0.188
#&gt; GSM559441     1   0.994   -0.86218 0.544 0.456
#&gt; GSM559442     1   0.000    0.30476 1.000 0.000
#&gt; GSM559444     1   0.141    0.28600 0.980 0.020
#&gt; GSM559445     2   1.000    1.00000 0.488 0.512
#&gt; GSM559446     1   0.697    0.00519 0.812 0.188
#&gt; GSM559448     2   1.000    1.00000 0.488 0.512
#&gt; GSM559450     1   0.141    0.28600 0.980 0.020
#&gt; GSM559451     1   0.697    0.00519 0.812 0.188
#&gt; GSM559452     1   0.697    0.00519 0.812 0.188
#&gt; GSM559454     1   0.998   -0.90186 0.524 0.476
#&gt; GSM559455     1   0.998   -0.90186 0.524 0.476
#&gt; GSM559456     2   1.000    1.00000 0.488 0.512
#&gt; GSM559457     2   1.000    1.00000 0.488 0.512
#&gt; GSM559458     2   1.000    1.00000 0.488 0.512
#&gt; GSM559459     1   0.996    0.37494 0.536 0.464
#&gt; GSM559460     1   0.998   -0.90186 0.524 0.476
#&gt; GSM559461     1   0.697    0.00519 0.812 0.188
#&gt; GSM559462     1   1.000    0.37583 0.512 0.488
#&gt; GSM559463     1   1.000    0.37583 0.512 0.488
#&gt; GSM559464     1   0.000    0.30476 1.000 0.000
#&gt; GSM559465     1   0.998   -0.90186 0.524 0.476
#&gt; GSM559467     1   0.000    0.30476 1.000 0.000
#&gt; GSM559468     1   0.644    0.05665 0.836 0.164
#&gt; GSM559469     2   1.000    1.00000 0.488 0.512
#&gt; GSM559470     1   0.998   -0.90186 0.524 0.476
#&gt; GSM559471     1   0.000    0.30476 1.000 0.000
#&gt; GSM559472     2   1.000    1.00000 0.488 0.512
#&gt; GSM559473     1   0.697    0.00519 0.812 0.188
#&gt; GSM559475     1   0.963   -0.67773 0.612 0.388
#&gt; GSM559477     1   1.000    0.37583 0.512 0.488
#&gt; GSM559478     1   0.999    0.37626 0.516 0.484
#&gt; GSM559479     1   1.000    0.37583 0.512 0.488
#&gt; GSM559480     1   1.000    0.37583 0.512 0.488
#&gt; GSM559481     1   0.999    0.37626 0.516 0.484
#&gt; GSM559482     1   1.000    0.37583 0.512 0.488
#&gt; GSM559435     1   0.998   -0.90186 0.524 0.476
#&gt; GSM559439     1   0.998   -0.90186 0.524 0.476
#&gt; GSM559443     1   0.000    0.30476 1.000 0.000
#&gt; GSM559447     1   0.998   -0.90186 0.524 0.476
#&gt; GSM559449     2   1.000    1.00000 0.488 0.512
#&gt; GSM559453     1   0.998   -0.90186 0.524 0.476
#&gt; GSM559466     1   0.998   -0.90186 0.524 0.476
#&gt; GSM559474     2   1.000    1.00000 0.488 0.512
#&gt; GSM559476     1   0.998   -0.90186 0.524 0.476
#&gt; GSM559483     1   1.000    0.37583 0.512 0.488
#&gt; GSM559484     1   0.999    0.37626 0.516 0.484
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-1-a').click(function(){
  $('#tab-ATC-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-2'>
<p><a id='tab-ATC-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559433     1  0.5465      0.560 0.712 0.000 0.288
#&gt; GSM559434     1  0.5948      0.429 0.640 0.000 0.360
#&gt; GSM559436     1  0.4897      0.502 0.812 0.172 0.016
#&gt; GSM559437     1  0.4897      0.502 0.812 0.172 0.016
#&gt; GSM559438     1  0.0000      0.609 1.000 0.000 0.000
#&gt; GSM559440     1  0.0000      0.609 1.000 0.000 0.000
#&gt; GSM559441     1  0.5291      0.566 0.732 0.000 0.268
#&gt; GSM559442     1  0.4897      0.502 0.812 0.172 0.016
#&gt; GSM559444     1  0.4121      0.514 0.832 0.168 0.000
#&gt; GSM559445     3  0.5835      0.578 0.340 0.000 0.660
#&gt; GSM559446     1  0.0000      0.609 1.000 0.000 0.000
#&gt; GSM559448     1  0.5948      0.429 0.640 0.000 0.360
#&gt; GSM559450     1  0.4121      0.514 0.832 0.168 0.000
#&gt; GSM559451     1  0.0000      0.609 1.000 0.000 0.000
#&gt; GSM559452     1  0.0000      0.609 1.000 0.000 0.000
#&gt; GSM559454     1  0.5465      0.560 0.712 0.000 0.288
#&gt; GSM559455     1  0.5465      0.560 0.712 0.000 0.288
#&gt; GSM559456     3  0.5835      0.578 0.340 0.000 0.660
#&gt; GSM559457     1  0.6308     -0.104 0.508 0.000 0.492
#&gt; GSM559458     3  0.5835      0.520 0.340 0.000 0.660
#&gt; GSM559459     2  0.2804      0.832 0.060 0.924 0.016
#&gt; GSM559460     1  0.5465      0.560 0.712 0.000 0.288
#&gt; GSM559461     1  0.0000      0.609 1.000 0.000 0.000
#&gt; GSM559462     2  0.0000      0.871 0.000 1.000 0.000
#&gt; GSM559463     2  0.0000      0.871 0.000 1.000 0.000
#&gt; GSM559464     1  0.4897      0.502 0.812 0.172 0.016
#&gt; GSM559465     1  0.5465      0.560 0.712 0.000 0.288
#&gt; GSM559467     1  0.4897      0.502 0.812 0.172 0.016
#&gt; GSM559468     1  0.1170      0.597 0.976 0.008 0.016
#&gt; GSM559469     1  0.5948      0.429 0.640 0.000 0.360
#&gt; GSM559470     1  0.5497      0.555 0.708 0.000 0.292
#&gt; GSM559471     1  0.4897      0.502 0.812 0.172 0.016
#&gt; GSM559472     1  0.5948      0.429 0.640 0.000 0.360
#&gt; GSM559473     1  0.0000      0.609 1.000 0.000 0.000
#&gt; GSM559475     1  0.4555      0.580 0.800 0.000 0.200
#&gt; GSM559477     2  0.0000      0.871 0.000 1.000 0.000
#&gt; GSM559478     2  0.5760      0.664 0.328 0.672 0.000
#&gt; GSM559479     2  0.0000      0.871 0.000 1.000 0.000
#&gt; GSM559480     2  0.0000      0.871 0.000 1.000 0.000
#&gt; GSM559481     2  0.5760      0.664 0.328 0.672 0.000
#&gt; GSM559482     2  0.0000      0.871 0.000 1.000 0.000
#&gt; GSM559435     1  0.5465      0.560 0.712 0.000 0.288
#&gt; GSM559439     1  0.5465      0.560 0.712 0.000 0.288
#&gt; GSM559443     1  0.4897      0.502 0.812 0.172 0.016
#&gt; GSM559447     1  0.5465      0.560 0.712 0.000 0.288
#&gt; GSM559449     3  0.0747      0.612 0.016 0.000 0.984
#&gt; GSM559453     1  0.5465      0.560 0.712 0.000 0.288
#&gt; GSM559466     1  0.5465      0.560 0.712 0.000 0.288
#&gt; GSM559474     3  0.0747      0.612 0.016 0.000 0.984
#&gt; GSM559476     1  0.5465      0.560 0.712 0.000 0.288
#&gt; GSM559483     2  0.0000      0.871 0.000 1.000 0.000
#&gt; GSM559484     2  0.5760      0.664 0.328 0.672 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-2-a').click(function(){
  $('#tab-ATC-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-3'>
<p><a id='tab-ATC-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559433     1   0.454     0.9144 0.676 0.000 0.000 0.324
#&gt; GSM559434     1   0.649     0.8337 0.604 0.000 0.104 0.292
#&gt; GSM559436     4   0.000     0.7641 0.000 0.000 0.000 1.000
#&gt; GSM559437     4   0.000     0.7641 0.000 0.000 0.000 1.000
#&gt; GSM559438     4   0.401     0.6780 0.244 0.000 0.000 0.756
#&gt; GSM559440     4   0.401     0.6780 0.244 0.000 0.000 0.756
#&gt; GSM559441     1   0.475     0.8332 0.632 0.000 0.000 0.368
#&gt; GSM559442     4   0.000     0.7641 0.000 0.000 0.000 1.000
#&gt; GSM559444     4   0.139     0.7629 0.048 0.000 0.000 0.952
#&gt; GSM559445     3   0.687     0.6015 0.264 0.000 0.584 0.152
#&gt; GSM559446     4   0.398     0.6829 0.240 0.000 0.000 0.760
#&gt; GSM559448     1   0.649     0.8337 0.604 0.000 0.104 0.292
#&gt; GSM559450     4   0.139     0.7629 0.048 0.000 0.000 0.952
#&gt; GSM559451     4   0.398     0.6829 0.240 0.000 0.000 0.760
#&gt; GSM559452     4   0.401     0.6780 0.244 0.000 0.000 0.756
#&gt; GSM559454     1   0.454     0.9144 0.676 0.000 0.000 0.324
#&gt; GSM559455     1   0.454     0.9144 0.676 0.000 0.000 0.324
#&gt; GSM559456     3   0.687     0.6015 0.264 0.000 0.584 0.152
#&gt; GSM559457     1   0.752    -0.0156 0.412 0.000 0.404 0.184
#&gt; GSM559458     3   0.701     0.5232 0.240 0.000 0.576 0.184
#&gt; GSM559459     2   0.736     0.5183 0.320 0.500 0.000 0.180
#&gt; GSM559460     1   0.454     0.9144 0.676 0.000 0.000 0.324
#&gt; GSM559461     4   0.398     0.6829 0.240 0.000 0.000 0.760
#&gt; GSM559462     2   0.454     0.5771 0.324 0.676 0.000 0.000
#&gt; GSM559463     2   0.454     0.5771 0.324 0.676 0.000 0.000
#&gt; GSM559464     4   0.000     0.7641 0.000 0.000 0.000 1.000
#&gt; GSM559465     1   0.454     0.9144 0.676 0.000 0.000 0.324
#&gt; GSM559467     4   0.000     0.7641 0.000 0.000 0.000 1.000
#&gt; GSM559468     4   0.349     0.7068 0.188 0.000 0.000 0.812
#&gt; GSM559469     1   0.649     0.8337 0.604 0.000 0.104 0.292
#&gt; GSM559470     1   0.470     0.9113 0.676 0.000 0.004 0.320
#&gt; GSM559471     4   0.000     0.7641 0.000 0.000 0.000 1.000
#&gt; GSM559472     1   0.649     0.8337 0.604 0.000 0.104 0.292
#&gt; GSM559473     4   0.401     0.6780 0.244 0.000 0.000 0.756
#&gt; GSM559475     4   0.497    -0.2120 0.452 0.000 0.000 0.548
#&gt; GSM559477     2   0.000     0.7039 0.000 1.000 0.000 0.000
#&gt; GSM559478     2   0.500     0.4208 0.000 0.504 0.000 0.496
#&gt; GSM559479     2   0.000     0.7039 0.000 1.000 0.000 0.000
#&gt; GSM559480     2   0.000     0.7039 0.000 1.000 0.000 0.000
#&gt; GSM559481     2   0.500     0.4208 0.000 0.504 0.000 0.496
#&gt; GSM559482     2   0.000     0.7039 0.000 1.000 0.000 0.000
#&gt; GSM559435     1   0.454     0.9144 0.676 0.000 0.000 0.324
#&gt; GSM559439     1   0.454     0.9144 0.676 0.000 0.000 0.324
#&gt; GSM559443     4   0.000     0.7641 0.000 0.000 0.000 1.000
#&gt; GSM559447     1   0.454     0.9144 0.676 0.000 0.000 0.324
#&gt; GSM559449     3   0.000     0.5028 0.000 0.000 1.000 0.000
#&gt; GSM559453     1   0.454     0.9144 0.676 0.000 0.000 0.324
#&gt; GSM559466     1   0.454     0.9144 0.676 0.000 0.000 0.324
#&gt; GSM559474     3   0.000     0.5028 0.000 0.000 1.000 0.000
#&gt; GSM559476     1   0.454     0.9144 0.676 0.000 0.000 0.324
#&gt; GSM559483     2   0.000     0.7039 0.000 1.000 0.000 0.000
#&gt; GSM559484     2   0.500     0.4208 0.000 0.504 0.000 0.496
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-3-a').click(function(){
  $('#tab-ATC-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-4'>
<p><a id='tab-ATC-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559433     3  0.0000     0.8589 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559434     3  0.2074     0.7865 0.000 0.000 0.896 0.000 0.104
#&gt; GSM559436     4  0.0000     0.7779 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559437     4  0.0000     0.7779 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559438     4  0.4163     0.7723 0.032 0.000 0.228 0.740 0.000
#&gt; GSM559440     4  0.4163     0.7723 0.032 0.000 0.228 0.740 0.000
#&gt; GSM559441     3  0.3577     0.6253 0.032 0.000 0.808 0.160 0.000
#&gt; GSM559442     4  0.0000     0.7779 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559444     4  0.1478     0.7887 0.000 0.000 0.064 0.936 0.000
#&gt; GSM559445     5  0.4219     0.5997 0.000 0.000 0.416 0.000 0.584
#&gt; GSM559446     4  0.4541     0.7208 0.032 0.000 0.288 0.680 0.000
#&gt; GSM559448     3  0.2074     0.7865 0.000 0.000 0.896 0.000 0.104
#&gt; GSM559450     4  0.1478     0.7887 0.000 0.000 0.064 0.936 0.000
#&gt; GSM559451     4  0.4541     0.7208 0.032 0.000 0.288 0.680 0.000
#&gt; GSM559452     4  0.4163     0.7723 0.032 0.000 0.228 0.740 0.000
#&gt; GSM559454     3  0.0000     0.8589 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559455     3  0.0000     0.8589 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559456     5  0.4219     0.5997 0.000 0.000 0.416 0.000 0.584
#&gt; GSM559457     3  0.4192    -0.0682 0.000 0.000 0.596 0.000 0.404
#&gt; GSM559458     5  0.4235     0.5243 0.000 0.000 0.424 0.000 0.576
#&gt; GSM559459     1  0.3368     0.7269 0.820 0.024 0.000 0.156 0.000
#&gt; GSM559460     3  0.0000     0.8589 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559461     4  0.4541     0.7208 0.032 0.000 0.288 0.680 0.000
#&gt; GSM559462     1  0.3336     0.7542 0.772 0.228 0.000 0.000 0.000
#&gt; GSM559463     1  0.1792     0.8136 0.916 0.084 0.000 0.000 0.000
#&gt; GSM559464     4  0.0000     0.7779 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559465     3  0.0880     0.8491 0.032 0.000 0.968 0.000 0.000
#&gt; GSM559467     4  0.0000     0.7779 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559468     4  0.3536     0.7799 0.032 0.000 0.156 0.812 0.000
#&gt; GSM559469     3  0.2074     0.7865 0.000 0.000 0.896 0.000 0.104
#&gt; GSM559470     3  0.0162     0.8578 0.000 0.000 0.996 0.000 0.004
#&gt; GSM559471     4  0.0000     0.7779 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559472     3  0.2074     0.7865 0.000 0.000 0.896 0.000 0.104
#&gt; GSM559473     4  0.4163     0.7723 0.032 0.000 0.228 0.740 0.000
#&gt; GSM559475     3  0.5028    -0.1439 0.032 0.000 0.524 0.444 0.000
#&gt; GSM559477     2  0.0000     0.6341 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559478     2  0.4904     0.4342 0.024 0.504 0.000 0.472 0.000
#&gt; GSM559479     2  0.0000     0.6341 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.0000     0.6341 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559481     2  0.4904     0.4342 0.024 0.504 0.000 0.472 0.000
#&gt; GSM559482     2  0.0000     0.6341 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.0000     0.8589 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559439     3  0.0880     0.8491 0.032 0.000 0.968 0.000 0.000
#&gt; GSM559443     4  0.0000     0.7779 0.000 0.000 0.000 1.000 0.000
#&gt; GSM559447     3  0.0880     0.8491 0.032 0.000 0.968 0.000 0.000
#&gt; GSM559449     5  0.0794     0.3684 0.028 0.000 0.000 0.000 0.972
#&gt; GSM559453     3  0.0880     0.8491 0.032 0.000 0.968 0.000 0.000
#&gt; GSM559466     3  0.0000     0.8589 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559474     5  0.0000     0.3902 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559476     3  0.0510     0.8554 0.016 0.000 0.984 0.000 0.000
#&gt; GSM559483     2  0.0000     0.6341 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559484     2  0.4904     0.4342 0.024 0.504 0.000 0.472 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-4-a').click(function(){
  $('#tab-ATC-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-5'>
<p><a id='tab-ATC-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559433     3  0.0260     0.7767 0.000 0.000 0.992 0.000 0.008 0.000
#&gt; GSM559434     3  0.2118     0.7088 0.000 0.000 0.888 0.104 0.008 0.000
#&gt; GSM559436     6  0.2416     0.6402 0.000 0.000 0.000 0.000 0.156 0.844
#&gt; GSM559437     6  0.2416     0.6402 0.000 0.000 0.000 0.000 0.156 0.844
#&gt; GSM559438     6  0.4873     0.5831 0.000 0.000 0.080 0.000 0.320 0.600
#&gt; GSM559440     6  0.4873     0.5831 0.000 0.000 0.080 0.000 0.320 0.600
#&gt; GSM559441     3  0.3482     0.5088 0.000 0.000 0.684 0.000 0.000 0.316
#&gt; GSM559442     6  0.2416     0.6402 0.000 0.000 0.000 0.000 0.156 0.844
#&gt; GSM559444     6  0.4651     0.5641 0.000 0.000 0.040 0.000 0.476 0.484
#&gt; GSM559445     4  0.4010     0.5740 0.000 0.000 0.408 0.584 0.008 0.000
#&gt; GSM559446     6  0.2859     0.6240 0.000 0.000 0.156 0.000 0.016 0.828
#&gt; GSM559448     3  0.2118     0.7088 0.000 0.000 0.888 0.104 0.008 0.000
#&gt; GSM559450     6  0.4651     0.5641 0.000 0.000 0.040 0.000 0.476 0.484
#&gt; GSM559451     6  0.2859     0.6240 0.000 0.000 0.156 0.000 0.016 0.828
#&gt; GSM559452     6  0.4873     0.5831 0.000 0.000 0.080 0.000 0.320 0.600
#&gt; GSM559454     3  0.0000     0.7809 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559455     3  0.0000     0.7809 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559456     4  0.4010     0.5740 0.000 0.000 0.408 0.584 0.008 0.000
#&gt; GSM559457     3  0.4002    -0.1910 0.000 0.000 0.588 0.404 0.008 0.000
#&gt; GSM559458     4  0.3804     0.4686 0.000 0.000 0.424 0.576 0.000 0.000
#&gt; GSM559459     1  0.3285     0.6942 0.820 0.000 0.000 0.000 0.116 0.064
#&gt; GSM559460     3  0.0000     0.7809 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559461     6  0.2859     0.6240 0.000 0.000 0.156 0.000 0.016 0.828
#&gt; GSM559462     1  0.2454     0.7397 0.840 0.160 0.000 0.000 0.000 0.000
#&gt; GSM559463     1  0.0260     0.8010 0.992 0.008 0.000 0.000 0.000 0.000
#&gt; GSM559464     6  0.2416     0.6402 0.000 0.000 0.000 0.000 0.156 0.844
#&gt; GSM559465     3  0.2416     0.7114 0.000 0.000 0.844 0.000 0.000 0.156
#&gt; GSM559467     6  0.2416     0.6402 0.000 0.000 0.000 0.000 0.156 0.844
#&gt; GSM559468     6  0.0790     0.6477 0.000 0.000 0.032 0.000 0.000 0.968
#&gt; GSM559469     3  0.2118     0.7088 0.000 0.000 0.888 0.104 0.008 0.000
#&gt; GSM559470     3  0.0146     0.7801 0.000 0.000 0.996 0.004 0.000 0.000
#&gt; GSM559471     6  0.2416     0.6402 0.000 0.000 0.000 0.000 0.156 0.844
#&gt; GSM559472     3  0.2118     0.7088 0.000 0.000 0.888 0.104 0.008 0.000
#&gt; GSM559473     6  0.4873     0.5831 0.000 0.000 0.080 0.000 0.320 0.600
#&gt; GSM559475     3  0.6108    -0.0826 0.000 0.000 0.376 0.000 0.320 0.304
#&gt; GSM559477     2  0.0000     0.9320 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559478     5  0.5461     1.0000 0.004 0.308 0.000 0.000 0.556 0.132
#&gt; GSM559479     2  0.0000     0.9320 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.2762     0.6617 0.000 0.804 0.000 0.000 0.196 0.000
#&gt; GSM559481     5  0.5461     1.0000 0.004 0.308 0.000 0.000 0.556 0.132
#&gt; GSM559482     2  0.0000     0.9320 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.0000     0.7809 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559439     3  0.2416     0.7114 0.000 0.000 0.844 0.000 0.000 0.156
#&gt; GSM559443     6  0.2416     0.6402 0.000 0.000 0.000 0.000 0.156 0.844
#&gt; GSM559447     3  0.2416     0.7114 0.000 0.000 0.844 0.000 0.000 0.156
#&gt; GSM559449     4  0.0858     0.3726 0.004 0.000 0.000 0.968 0.028 0.000
#&gt; GSM559453     3  0.2416     0.7114 0.000 0.000 0.844 0.000 0.000 0.156
#&gt; GSM559466     3  0.0000     0.7809 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559474     4  0.0000     0.3965 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559476     3  0.3125     0.7059 0.000 0.000 0.836 0.000 0.084 0.080
#&gt; GSM559483     2  0.0000     0.9320 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559484     5  0.5461     1.0000 0.004 0.308 0.000 0.000 0.556 0.132
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-5-a').click(function(){
  $('#tab-ATC-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-hclust-signature_compare](figure_cola/ATC-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-hclust-collect-classes](figure_cola/ATC-hclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) k
#> ATC:hclust 10               NA 2
#> ATC:hclust 47            0.623 3
#> ATC:hclust 47            0.121 4
#> ATC:hclust 45            0.163 5
#> ATC:hclust 47            0.272 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "kmeans"]
# you can also extract it by
# res = res_list["ATC:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-kmeans-collect-plots](figure_cola/ATC-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-kmeans-select-partition-number](figure_cola/ATC-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.950       0.981         0.3657 0.638   0.638
#> 3 3 1.000           0.968       0.988         0.7471 0.684   0.518
#> 4 4 0.586           0.599       0.721         0.1180 0.790   0.488
#> 5 5 0.588           0.614       0.755         0.0765 0.922   0.718
#> 6 6 0.619           0.588       0.713         0.0405 0.961   0.835
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 3
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-kmeans-get-classes-1'>
<p><a id='tab-ATC-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559433     1   0.000      0.984 1.000 0.000
#&gt; GSM559434     1   0.000      0.984 1.000 0.000
#&gt; GSM559436     1   0.000      0.984 1.000 0.000
#&gt; GSM559437     1   0.000      0.984 1.000 0.000
#&gt; GSM559438     1   0.000      0.984 1.000 0.000
#&gt; GSM559440     1   0.000      0.984 1.000 0.000
#&gt; GSM559441     1   0.000      0.984 1.000 0.000
#&gt; GSM559442     2   0.969      0.315 0.396 0.604
#&gt; GSM559444     1   0.000      0.984 1.000 0.000
#&gt; GSM559445     1   0.000      0.984 1.000 0.000
#&gt; GSM559446     1   0.000      0.984 1.000 0.000
#&gt; GSM559448     1   0.000      0.984 1.000 0.000
#&gt; GSM559450     1   0.000      0.984 1.000 0.000
#&gt; GSM559451     1   0.000      0.984 1.000 0.000
#&gt; GSM559452     1   0.000      0.984 1.000 0.000
#&gt; GSM559454     1   0.000      0.984 1.000 0.000
#&gt; GSM559455     1   0.000      0.984 1.000 0.000
#&gt; GSM559456     1   0.000      0.984 1.000 0.000
#&gt; GSM559457     1   0.000      0.984 1.000 0.000
#&gt; GSM559458     1   0.000      0.984 1.000 0.000
#&gt; GSM559459     2   0.000      0.963 0.000 1.000
#&gt; GSM559460     1   0.000      0.984 1.000 0.000
#&gt; GSM559461     1   0.000      0.984 1.000 0.000
#&gt; GSM559462     2   0.000      0.963 0.000 1.000
#&gt; GSM559463     2   0.000      0.963 0.000 1.000
#&gt; GSM559464     1   0.881      0.558 0.700 0.300
#&gt; GSM559465     1   0.000      0.984 1.000 0.000
#&gt; GSM559467     1   0.000      0.984 1.000 0.000
#&gt; GSM559468     1   0.000      0.984 1.000 0.000
#&gt; GSM559469     1   0.000      0.984 1.000 0.000
#&gt; GSM559470     1   0.000      0.984 1.000 0.000
#&gt; GSM559471     1   0.881      0.558 0.700 0.300
#&gt; GSM559472     1   0.000      0.984 1.000 0.000
#&gt; GSM559473     1   0.000      0.984 1.000 0.000
#&gt; GSM559475     1   0.000      0.984 1.000 0.000
#&gt; GSM559477     2   0.000      0.963 0.000 1.000
#&gt; GSM559478     2   0.000      0.963 0.000 1.000
#&gt; GSM559479     2   0.000      0.963 0.000 1.000
#&gt; GSM559480     2   0.000      0.963 0.000 1.000
#&gt; GSM559481     2   0.000      0.963 0.000 1.000
#&gt; GSM559482     2   0.000      0.963 0.000 1.000
#&gt; GSM559435     1   0.000      0.984 1.000 0.000
#&gt; GSM559439     1   0.000      0.984 1.000 0.000
#&gt; GSM559443     1   0.000      0.984 1.000 0.000
#&gt; GSM559447     1   0.000      0.984 1.000 0.000
#&gt; GSM559449     1   0.000      0.984 1.000 0.000
#&gt; GSM559453     1   0.000      0.984 1.000 0.000
#&gt; GSM559466     1   0.000      0.984 1.000 0.000
#&gt; GSM559474     1   0.000      0.984 1.000 0.000
#&gt; GSM559476     1   0.000      0.984 1.000 0.000
#&gt; GSM559483     2   0.000      0.963 0.000 1.000
#&gt; GSM559484     2   0.000      0.963 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-2'>
<p><a id='tab-ATC-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559433     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559434     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559436     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559437     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559438     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559440     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559441     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559442     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559444     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559445     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559446     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559448     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559450     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559451     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559452     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559454     3  0.0237      0.995 0.004 0.000 0.996
#&gt; GSM559455     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559456     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559457     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559458     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559459     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM559460     1  0.0424      0.983 0.992 0.000 0.008
#&gt; GSM559461     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559462     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM559463     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM559464     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559465     1  0.0424      0.983 0.992 0.000 0.008
#&gt; GSM559467     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559468     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559469     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559470     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559471     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559472     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559473     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559475     1  0.0424      0.983 0.992 0.000 0.008
#&gt; GSM559477     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM559478     2  0.0424      0.947 0.008 0.992 0.000
#&gt; GSM559479     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM559480     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM559481     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM559482     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM559435     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559439     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559443     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559447     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559449     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559453     1  0.0000      0.989 1.000 0.000 0.000
#&gt; GSM559466     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559474     3  0.0000      1.000 0.000 0.000 1.000
#&gt; GSM559476     1  0.4555      0.744 0.800 0.000 0.200
#&gt; GSM559483     2  0.0000      0.953 0.000 1.000 0.000
#&gt; GSM559484     2  0.6126      0.339 0.400 0.600 0.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-3'>
<p><a id='tab-ATC-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559433     1  0.3356    0.80647 0.824 0.000 0.176 0.000
#&gt; GSM559434     1  0.1557    0.91893 0.944 0.000 0.056 0.000
#&gt; GSM559436     4  0.3688    0.57521 0.000 0.000 0.208 0.792
#&gt; GSM559437     4  0.3688    0.57521 0.000 0.000 0.208 0.792
#&gt; GSM559438     4  0.4817    0.33531 0.000 0.000 0.388 0.612
#&gt; GSM559440     4  0.4972    0.14546 0.000 0.000 0.456 0.544
#&gt; GSM559441     3  0.4843    0.40784 0.000 0.000 0.604 0.396
#&gt; GSM559442     4  0.0592    0.59802 0.000 0.000 0.016 0.984
#&gt; GSM559444     4  0.2589    0.61303 0.000 0.000 0.116 0.884
#&gt; GSM559445     1  0.0592    0.91879 0.984 0.000 0.016 0.000
#&gt; GSM559446     4  0.4843    0.30342 0.000 0.000 0.396 0.604
#&gt; GSM559448     1  0.1474    0.91964 0.948 0.000 0.052 0.000
#&gt; GSM559450     4  0.2647    0.61136 0.000 0.000 0.120 0.880
#&gt; GSM559451     4  0.4941    0.21844 0.000 0.000 0.436 0.564
#&gt; GSM559452     3  0.5193    0.34086 0.008 0.000 0.580 0.412
#&gt; GSM559454     3  0.4697    0.47169 0.356 0.000 0.644 0.000
#&gt; GSM559455     3  0.4830    0.40916 0.392 0.000 0.608 0.000
#&gt; GSM559456     1  0.0921    0.91708 0.972 0.000 0.028 0.000
#&gt; GSM559457     1  0.0336    0.91805 0.992 0.000 0.008 0.000
#&gt; GSM559458     1  0.2149    0.87496 0.912 0.000 0.088 0.000
#&gt; GSM559459     2  0.7091    0.58680 0.000 0.508 0.136 0.356
#&gt; GSM559460     3  0.6084    0.60959 0.120 0.000 0.676 0.204
#&gt; GSM559461     4  0.4855    0.30453 0.000 0.000 0.400 0.600
#&gt; GSM559462     2  0.2760    0.85019 0.000 0.872 0.128 0.000
#&gt; GSM559463     2  0.4791    0.82685 0.000 0.784 0.136 0.080
#&gt; GSM559464     4  0.1118    0.60332 0.000 0.000 0.036 0.964
#&gt; GSM559465     3  0.6112    0.60275 0.096 0.000 0.656 0.248
#&gt; GSM559467     4  0.2011    0.61894 0.000 0.000 0.080 0.920
#&gt; GSM559468     4  0.4454    0.46111 0.000 0.000 0.308 0.692
#&gt; GSM559469     1  0.1557    0.91893 0.944 0.000 0.056 0.000
#&gt; GSM559470     1  0.3172    0.82481 0.840 0.000 0.160 0.000
#&gt; GSM559471     4  0.0188    0.60101 0.000 0.000 0.004 0.996
#&gt; GSM559472     1  0.1474    0.91964 0.948 0.000 0.052 0.000
#&gt; GSM559473     4  0.4941    0.20222 0.000 0.000 0.436 0.564
#&gt; GSM559475     3  0.5903    0.50491 0.052 0.000 0.616 0.332
#&gt; GSM559477     2  0.0000    0.88342 0.000 1.000 0.000 0.000
#&gt; GSM559478     4  0.5971   -0.32985 0.000 0.428 0.040 0.532
#&gt; GSM559479     2  0.0000    0.88342 0.000 1.000 0.000 0.000
#&gt; GSM559480     2  0.0000    0.88342 0.000 1.000 0.000 0.000
#&gt; GSM559481     2  0.5599    0.65560 0.000 0.644 0.040 0.316
#&gt; GSM559482     2  0.0000    0.88342 0.000 1.000 0.000 0.000
#&gt; GSM559435     3  0.4972    0.21750 0.456 0.000 0.544 0.000
#&gt; GSM559439     3  0.5298    0.47160 0.016 0.000 0.612 0.372
#&gt; GSM559443     4  0.1792    0.60059 0.000 0.000 0.068 0.932
#&gt; GSM559447     3  0.5746    0.51930 0.040 0.000 0.612 0.348
#&gt; GSM559449     1  0.2216    0.87265 0.908 0.000 0.092 0.000
#&gt; GSM559453     3  0.5615    0.50687 0.032 0.000 0.612 0.356
#&gt; GSM559466     3  0.4746    0.45764 0.368 0.000 0.632 0.000
#&gt; GSM559474     1  0.2216    0.87265 0.908 0.000 0.092 0.000
#&gt; GSM559476     3  0.5144    0.58384 0.052 0.000 0.732 0.216
#&gt; GSM559483     2  0.0000    0.88342 0.000 1.000 0.000 0.000
#&gt; GSM559484     4  0.5614   -0.00659 0.000 0.304 0.044 0.652
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-4'>
<p><a id='tab-ATC-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559433     1  0.3949      0.515 0.696 0.000 0.300 0.000 0.004
#&gt; GSM559434     1  0.1484      0.835 0.944 0.000 0.048 0.000 0.008
#&gt; GSM559436     4  0.4836      0.605 0.000 0.000 0.304 0.652 0.044
#&gt; GSM559437     4  0.4836      0.605 0.000 0.000 0.304 0.652 0.044
#&gt; GSM559438     4  0.2773      0.632 0.000 0.000 0.164 0.836 0.000
#&gt; GSM559440     4  0.3522      0.587 0.004 0.000 0.212 0.780 0.004
#&gt; GSM559441     3  0.2773      0.642 0.000 0.000 0.836 0.164 0.000
#&gt; GSM559442     4  0.5611     -0.157 0.000 0.000 0.076 0.516 0.408
#&gt; GSM559444     4  0.0912      0.566 0.000 0.000 0.012 0.972 0.016
#&gt; GSM559445     1  0.2790      0.815 0.880 0.000 0.052 0.000 0.068
#&gt; GSM559446     4  0.4511      0.589 0.000 0.000 0.356 0.628 0.016
#&gt; GSM559448     1  0.1197      0.837 0.952 0.000 0.048 0.000 0.000
#&gt; GSM559450     4  0.0912      0.571 0.000 0.000 0.016 0.972 0.012
#&gt; GSM559451     4  0.3636      0.619 0.000 0.000 0.272 0.728 0.000
#&gt; GSM559452     4  0.4936      0.141 0.012 0.000 0.412 0.564 0.012
#&gt; GSM559454     3  0.5169      0.595 0.304 0.000 0.640 0.048 0.008
#&gt; GSM559455     3  0.4440      0.564 0.324 0.000 0.660 0.012 0.004
#&gt; GSM559456     1  0.4078      0.750 0.784 0.000 0.148 0.000 0.068
#&gt; GSM559457     1  0.0912      0.836 0.972 0.000 0.012 0.000 0.016
#&gt; GSM559458     1  0.4049      0.767 0.780 0.000 0.056 0.000 0.164
#&gt; GSM559459     5  0.5864      0.589 0.000 0.220 0.016 0.124 0.640
#&gt; GSM559460     3  0.4779      0.712 0.144 0.000 0.740 0.112 0.004
#&gt; GSM559461     4  0.4482      0.593 0.000 0.000 0.348 0.636 0.016
#&gt; GSM559462     2  0.4350      0.629 0.000 0.704 0.028 0.000 0.268
#&gt; GSM559463     2  0.5083      0.323 0.000 0.540 0.028 0.004 0.428
#&gt; GSM559464     4  0.6262      0.155 0.000 0.000 0.164 0.504 0.332
#&gt; GSM559465     3  0.2914      0.719 0.052 0.000 0.872 0.076 0.000
#&gt; GSM559467     4  0.2074      0.573 0.000 0.000 0.044 0.920 0.036
#&gt; GSM559468     4  0.4400      0.623 0.000 0.000 0.308 0.672 0.020
#&gt; GSM559469     1  0.1484      0.835 0.944 0.000 0.048 0.000 0.008
#&gt; GSM559470     1  0.3093      0.721 0.824 0.000 0.168 0.000 0.008
#&gt; GSM559471     4  0.4576      0.198 0.000 0.000 0.040 0.692 0.268
#&gt; GSM559472     1  0.1197      0.837 0.952 0.000 0.048 0.000 0.000
#&gt; GSM559473     4  0.3242      0.612 0.000 0.000 0.172 0.816 0.012
#&gt; GSM559475     4  0.6178     -0.051 0.080 0.000 0.404 0.496 0.020
#&gt; GSM559477     2  0.0000      0.855 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559478     5  0.6553      0.713 0.000 0.236 0.000 0.292 0.472
#&gt; GSM559479     2  0.0000      0.855 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.0510      0.847 0.000 0.984 0.000 0.000 0.016
#&gt; GSM559481     5  0.6281      0.560 0.000 0.388 0.000 0.152 0.460
#&gt; GSM559482     2  0.0000      0.855 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.3932      0.541 0.328 0.000 0.672 0.000 0.000
#&gt; GSM559439     3  0.2732      0.649 0.000 0.000 0.840 0.160 0.000
#&gt; GSM559443     4  0.6066      0.444 0.000 0.000 0.240 0.572 0.188
#&gt; GSM559447     3  0.2886      0.665 0.008 0.000 0.844 0.148 0.000
#&gt; GSM559449     1  0.4333      0.749 0.752 0.000 0.060 0.000 0.188
#&gt; GSM559453     3  0.2806      0.661 0.004 0.000 0.844 0.152 0.000
#&gt; GSM559466     3  0.3266      0.707 0.200 0.000 0.796 0.004 0.000
#&gt; GSM559474     1  0.4333      0.749 0.752 0.000 0.060 0.000 0.188
#&gt; GSM559476     3  0.5232      0.666 0.060 0.000 0.744 0.084 0.112
#&gt; GSM559483     2  0.0000      0.855 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559484     5  0.5956      0.573 0.000 0.108 0.000 0.416 0.476
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-5'>
<p><a id='tab-ATC-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3 p4    p5    p6
#&gt; GSM559433     1  0.3547      0.459 0.696 0.000 0.300 NA 0.000 0.000
#&gt; GSM559434     1  0.0405      0.793 0.988 0.000 0.008 NA 0.000 0.000
#&gt; GSM559436     6  0.6362      0.491 0.000 0.000 0.292 NA 0.128 0.516
#&gt; GSM559437     6  0.6362      0.491 0.000 0.000 0.292 NA 0.128 0.516
#&gt; GSM559438     6  0.2907      0.619 0.000 0.000 0.152 NA 0.000 0.828
#&gt; GSM559440     6  0.3194      0.612 0.004 0.000 0.168 NA 0.000 0.808
#&gt; GSM559441     3  0.2222      0.682 0.000 0.000 0.896 NA 0.012 0.084
#&gt; GSM559442     5  0.5549      0.361 0.000 0.000 0.072 NA 0.584 0.304
#&gt; GSM559444     6  0.2624      0.532 0.000 0.000 0.028 NA 0.068 0.884
#&gt; GSM559445     1  0.4390      0.714 0.720 0.000 0.148 NA 0.000 0.000
#&gt; GSM559446     6  0.5053      0.585 0.000 0.000 0.368 NA 0.056 0.564
#&gt; GSM559448     1  0.0260      0.794 0.992 0.000 0.008 NA 0.000 0.000
#&gt; GSM559450     6  0.2255      0.544 0.000 0.000 0.024 NA 0.044 0.908
#&gt; GSM559451     6  0.3690      0.625 0.000 0.000 0.288 NA 0.012 0.700
#&gt; GSM559452     6  0.4868      0.359 0.016 0.000 0.332 NA 0.000 0.608
#&gt; GSM559454     3  0.4452      0.604 0.312 0.000 0.644 NA 0.000 0.040
#&gt; GSM559455     3  0.3351      0.635 0.288 0.000 0.712 NA 0.000 0.000
#&gt; GSM559456     1  0.4967      0.627 0.640 0.000 0.228 NA 0.000 0.000
#&gt; GSM559457     1  0.0790      0.791 0.968 0.000 0.000 NA 0.000 0.000
#&gt; GSM559458     1  0.3607      0.680 0.652 0.000 0.000 NA 0.000 0.000
#&gt; GSM559459     5  0.3263      0.542 0.000 0.024 0.000 NA 0.836 0.028
#&gt; GSM559460     3  0.3865      0.728 0.192 0.000 0.752 NA 0.000 0.056
#&gt; GSM559461     6  0.4943      0.592 0.000 0.000 0.360 NA 0.056 0.576
#&gt; GSM559462     2  0.6223      0.235 0.000 0.420 0.008 NA 0.252 0.000
#&gt; GSM559463     5  0.6291     -0.188 0.000 0.284 0.008 NA 0.392 0.000
#&gt; GSM559464     5  0.6367      0.103 0.000 0.000 0.172 NA 0.492 0.296
#&gt; GSM559465     3  0.1261      0.748 0.024 0.000 0.952 NA 0.000 0.024
#&gt; GSM559467     6  0.3820      0.495 0.000 0.000 0.064 NA 0.144 0.784
#&gt; GSM559468     6  0.5398      0.576 0.000 0.000 0.324 NA 0.064 0.580
#&gt; GSM559469     1  0.0405      0.793 0.988 0.000 0.008 NA 0.000 0.000
#&gt; GSM559470     1  0.2513      0.693 0.852 0.000 0.140 NA 0.000 0.000
#&gt; GSM559471     6  0.5503     -0.180 0.000 0.000 0.056 NA 0.428 0.484
#&gt; GSM559472     1  0.0260      0.794 0.992 0.000 0.008 NA 0.000 0.000
#&gt; GSM559473     6  0.3054      0.586 0.000 0.000 0.116 NA 0.004 0.840
#&gt; GSM559475     6  0.6011      0.278 0.096 0.000 0.292 NA 0.004 0.560
#&gt; GSM559477     2  0.0000      0.881 0.000 1.000 0.000 NA 0.000 0.000
#&gt; GSM559478     5  0.4388      0.607 0.000 0.092 0.000 NA 0.732 0.168
#&gt; GSM559479     2  0.0000      0.881 0.000 1.000 0.000 NA 0.000 0.000
#&gt; GSM559480     2  0.0858      0.863 0.000 0.968 0.004 NA 0.028 0.000
#&gt; GSM559481     5  0.4468      0.530 0.000 0.184 0.000 NA 0.720 0.088
#&gt; GSM559482     2  0.0000      0.881 0.000 1.000 0.000 NA 0.000 0.000
#&gt; GSM559435     3  0.3390      0.613 0.296 0.000 0.704 NA 0.000 0.000
#&gt; GSM559439     3  0.2013      0.709 0.008 0.000 0.908 NA 0.000 0.076
#&gt; GSM559443     6  0.6957      0.240 0.000 0.000 0.260 NA 0.272 0.404
#&gt; GSM559447     3  0.1655      0.733 0.008 0.000 0.932 NA 0.000 0.052
#&gt; GSM559449     1  0.4171      0.643 0.604 0.000 0.000 NA 0.012 0.004
#&gt; GSM559453     3  0.1655      0.733 0.008 0.000 0.932 NA 0.000 0.052
#&gt; GSM559466     3  0.2632      0.746 0.164 0.000 0.832 NA 0.000 0.000
#&gt; GSM559474     1  0.3747      0.645 0.604 0.000 0.000 NA 0.000 0.000
#&gt; GSM559476     3  0.5145      0.575 0.028 0.000 0.696 NA 0.032 0.044
#&gt; GSM559483     2  0.0000      0.881 0.000 1.000 0.000 NA 0.000 0.000
#&gt; GSM559484     5  0.3770      0.601 0.000 0.032 0.000 NA 0.752 0.212
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-kmeans-signature_compare](figure_cola/ATC-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-kmeans-collect-classes](figure_cola/ATC-kmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) k
#> ATC:kmeans 51           1.0000 2
#> ATC:kmeans 51           0.6430 3
#> ATC:kmeans 36           0.2183 4
#> ATC:kmeans 45           0.0340 5
#> ATC:kmeans 40           0.0761 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "skmeans"]
# you can also extract it by
# res = res_list["ATC:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-skmeans-collect-plots](figure_cola/ATC-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-skmeans-select-partition-number](figure_cola/ATC-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.959           0.964       0.983         0.4964 0.509   0.509
#> 3 3 0.953           0.928       0.956         0.2753 0.821   0.656
#> 4 4 0.923           0.890       0.954         0.1167 0.927   0.798
#> 5 5 0.775           0.778       0.875         0.0930 0.897   0.663
#> 6 6 0.813           0.747       0.857         0.0329 0.987   0.940
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
#> attr(,"optional")
#> [1] 2 3
```

There is also optional best $k$ = 2 3 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-skmeans-get-classes-1'>
<p><a id='tab-ATC-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559433     1   0.000      0.971 1.000 0.000
#&gt; GSM559434     1   0.000      0.971 1.000 0.000
#&gt; GSM559436     2   0.000      1.000 0.000 1.000
#&gt; GSM559437     2   0.000      1.000 0.000 1.000
#&gt; GSM559438     1   0.921      0.531 0.664 0.336
#&gt; GSM559440     1   0.000      0.971 1.000 0.000
#&gt; GSM559441     1   0.000      0.971 1.000 0.000
#&gt; GSM559442     2   0.000      1.000 0.000 1.000
#&gt; GSM559444     2   0.000      1.000 0.000 1.000
#&gt; GSM559445     1   0.000      0.971 1.000 0.000
#&gt; GSM559446     1   0.529      0.859 0.880 0.120
#&gt; GSM559448     1   0.000      0.971 1.000 0.000
#&gt; GSM559450     2   0.000      1.000 0.000 1.000
#&gt; GSM559451     1   0.000      0.971 1.000 0.000
#&gt; GSM559452     1   0.000      0.971 1.000 0.000
#&gt; GSM559454     1   0.000      0.971 1.000 0.000
#&gt; GSM559455     1   0.000      0.971 1.000 0.000
#&gt; GSM559456     1   0.000      0.971 1.000 0.000
#&gt; GSM559457     1   0.000      0.971 1.000 0.000
#&gt; GSM559458     1   0.000      0.971 1.000 0.000
#&gt; GSM559459     2   0.000      1.000 0.000 1.000
#&gt; GSM559460     1   0.000      0.971 1.000 0.000
#&gt; GSM559461     1   0.767      0.727 0.776 0.224
#&gt; GSM559462     2   0.000      1.000 0.000 1.000
#&gt; GSM559463     2   0.000      1.000 0.000 1.000
#&gt; GSM559464     2   0.000      1.000 0.000 1.000
#&gt; GSM559465     1   0.000      0.971 1.000 0.000
#&gt; GSM559467     2   0.000      1.000 0.000 1.000
#&gt; GSM559468     2   0.000      1.000 0.000 1.000
#&gt; GSM559469     1   0.000      0.971 1.000 0.000
#&gt; GSM559470     1   0.000      0.971 1.000 0.000
#&gt; GSM559471     2   0.000      1.000 0.000 1.000
#&gt; GSM559472     1   0.000      0.971 1.000 0.000
#&gt; GSM559473     1   0.680      0.785 0.820 0.180
#&gt; GSM559475     1   0.000      0.971 1.000 0.000
#&gt; GSM559477     2   0.000      1.000 0.000 1.000
#&gt; GSM559478     2   0.000      1.000 0.000 1.000
#&gt; GSM559479     2   0.000      1.000 0.000 1.000
#&gt; GSM559480     2   0.000      1.000 0.000 1.000
#&gt; GSM559481     2   0.000      1.000 0.000 1.000
#&gt; GSM559482     2   0.000      1.000 0.000 1.000
#&gt; GSM559435     1   0.000      0.971 1.000 0.000
#&gt; GSM559439     1   0.000      0.971 1.000 0.000
#&gt; GSM559443     2   0.000      1.000 0.000 1.000
#&gt; GSM559447     1   0.000      0.971 1.000 0.000
#&gt; GSM559449     1   0.000      0.971 1.000 0.000
#&gt; GSM559453     1   0.000      0.971 1.000 0.000
#&gt; GSM559466     1   0.000      0.971 1.000 0.000
#&gt; GSM559474     1   0.000      0.971 1.000 0.000
#&gt; GSM559476     1   0.000      0.971 1.000 0.000
#&gt; GSM559483     2   0.000      1.000 0.000 1.000
#&gt; GSM559484     2   0.000      1.000 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-2'>
<p><a id='tab-ATC-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559433     3  0.0000      0.985 0.000 0.000 1.000
#&gt; GSM559434     3  0.0000      0.985 0.000 0.000 1.000
#&gt; GSM559436     1  0.1643      0.954 0.956 0.044 0.000
#&gt; GSM559437     1  0.1529      0.956 0.960 0.040 0.000
#&gt; GSM559438     2  0.1289      0.822 0.000 0.968 0.032
#&gt; GSM559440     2  0.1964      0.827 0.000 0.944 0.056
#&gt; GSM559441     3  0.1529      0.965 0.000 0.040 0.960
#&gt; GSM559442     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM559444     2  0.1529      0.800 0.040 0.960 0.000
#&gt; GSM559445     3  0.0000      0.985 0.000 0.000 1.000
#&gt; GSM559446     2  0.6753      0.424 0.016 0.596 0.388
#&gt; GSM559448     3  0.0000      0.985 0.000 0.000 1.000
#&gt; GSM559450     2  0.1529      0.800 0.040 0.960 0.000
#&gt; GSM559451     2  0.1860      0.827 0.000 0.948 0.052
#&gt; GSM559452     2  0.5397      0.682 0.000 0.720 0.280
#&gt; GSM559454     3  0.0000      0.985 0.000 0.000 1.000
#&gt; GSM559455     3  0.0000      0.985 0.000 0.000 1.000
#&gt; GSM559456     3  0.1163      0.973 0.000 0.028 0.972
#&gt; GSM559457     3  0.0000      0.985 0.000 0.000 1.000
#&gt; GSM559458     3  0.0000      0.985 0.000 0.000 1.000
#&gt; GSM559459     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM559460     3  0.0000      0.985 0.000 0.000 1.000
#&gt; GSM559461     2  0.6753      0.424 0.016 0.596 0.388
#&gt; GSM559462     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM559463     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM559464     1  0.0000      0.984 1.000 0.000 0.000
#&gt; GSM559465     3  0.1289      0.970 0.000 0.032 0.968
#&gt; GSM559467     1  0.0747      0.986 0.984 0.016 0.000
#&gt; GSM559468     1  0.0237      0.982 0.996 0.004 0.000
#&gt; GSM559469     3  0.0000      0.985 0.000 0.000 1.000
#&gt; GSM559470     3  0.0000      0.985 0.000 0.000 1.000
#&gt; GSM559471     1  0.0747      0.986 0.984 0.016 0.000
#&gt; GSM559472     3  0.0000      0.985 0.000 0.000 1.000
#&gt; GSM559473     2  0.1529      0.824 0.000 0.960 0.040
#&gt; GSM559475     2  0.5397      0.682 0.000 0.720 0.280
#&gt; GSM559477     1  0.0747      0.986 0.984 0.016 0.000
#&gt; GSM559478     1  0.0747      0.986 0.984 0.016 0.000
#&gt; GSM559479     1  0.0747      0.986 0.984 0.016 0.000
#&gt; GSM559480     1  0.0747      0.986 0.984 0.016 0.000
#&gt; GSM559481     1  0.0747      0.986 0.984 0.016 0.000
#&gt; GSM559482     1  0.0747      0.986 0.984 0.016 0.000
#&gt; GSM559435     3  0.1163      0.973 0.000 0.028 0.972
#&gt; GSM559439     3  0.1529      0.965 0.000 0.040 0.960
#&gt; GSM559443     1  0.1529      0.956 0.960 0.040 0.000
#&gt; GSM559447     3  0.1529      0.965 0.000 0.040 0.960
#&gt; GSM559449     3  0.0000      0.985 0.000 0.000 1.000
#&gt; GSM559453     3  0.1529      0.965 0.000 0.040 0.960
#&gt; GSM559466     3  0.1163      0.973 0.000 0.028 0.972
#&gt; GSM559474     3  0.0000      0.985 0.000 0.000 1.000
#&gt; GSM559476     3  0.0000      0.985 0.000 0.000 1.000
#&gt; GSM559483     1  0.0747      0.986 0.984 0.016 0.000
#&gt; GSM559484     1  0.0747      0.986 0.984 0.016 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-3'>
<p><a id='tab-ATC-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559433     3  0.0188      0.944 0.000 0.004 0.996 0.000
#&gt; GSM559434     3  0.0000      0.945 0.000 0.000 1.000 0.000
#&gt; GSM559436     4  0.0336      0.847 0.008 0.000 0.000 0.992
#&gt; GSM559437     4  0.0336      0.847 0.008 0.000 0.000 0.992
#&gt; GSM559438     2  0.0188      0.909 0.000 0.996 0.000 0.004
#&gt; GSM559440     2  0.0188      0.910 0.000 0.996 0.004 0.000
#&gt; GSM559441     4  0.4819      0.387 0.000 0.004 0.344 0.652
#&gt; GSM559442     1  0.0000      0.980 1.000 0.000 0.000 0.000
#&gt; GSM559444     2  0.2973      0.771 0.144 0.856 0.000 0.000
#&gt; GSM559445     3  0.0376      0.942 0.000 0.004 0.992 0.004
#&gt; GSM559446     4  0.0817      0.840 0.000 0.024 0.000 0.976
#&gt; GSM559448     3  0.0000      0.945 0.000 0.000 1.000 0.000
#&gt; GSM559450     2  0.0188      0.909 0.004 0.996 0.000 0.000
#&gt; GSM559451     2  0.2647      0.812 0.000 0.880 0.000 0.120
#&gt; GSM559452     2  0.2149      0.858 0.000 0.912 0.088 0.000
#&gt; GSM559454     3  0.0000      0.945 0.000 0.000 1.000 0.000
#&gt; GSM559455     3  0.0000      0.945 0.000 0.000 1.000 0.000
#&gt; GSM559456     3  0.0524      0.940 0.000 0.004 0.988 0.008
#&gt; GSM559457     3  0.0000      0.945 0.000 0.000 1.000 0.000
#&gt; GSM559458     3  0.0000      0.945 0.000 0.000 1.000 0.000
#&gt; GSM559459     1  0.0000      0.980 1.000 0.000 0.000 0.000
#&gt; GSM559460     3  0.0000      0.945 0.000 0.000 1.000 0.000
#&gt; GSM559461     4  0.3400      0.705 0.000 0.180 0.000 0.820
#&gt; GSM559462     1  0.0000      0.980 1.000 0.000 0.000 0.000
#&gt; GSM559463     1  0.0000      0.980 1.000 0.000 0.000 0.000
#&gt; GSM559464     1  0.2281      0.892 0.904 0.000 0.000 0.096
#&gt; GSM559465     3  0.0657      0.938 0.000 0.004 0.984 0.012
#&gt; GSM559467     1  0.0000      0.980 1.000 0.000 0.000 0.000
#&gt; GSM559468     1  0.3688      0.756 0.792 0.000 0.000 0.208
#&gt; GSM559469     3  0.0000      0.945 0.000 0.000 1.000 0.000
#&gt; GSM559470     3  0.0000      0.945 0.000 0.000 1.000 0.000
#&gt; GSM559471     1  0.0000      0.980 1.000 0.000 0.000 0.000
#&gt; GSM559472     3  0.0000      0.945 0.000 0.000 1.000 0.000
#&gt; GSM559473     2  0.0188      0.910 0.000 0.996 0.004 0.000
#&gt; GSM559475     2  0.2149      0.858 0.000 0.912 0.088 0.000
#&gt; GSM559477     1  0.0000      0.980 1.000 0.000 0.000 0.000
#&gt; GSM559478     1  0.0000      0.980 1.000 0.000 0.000 0.000
#&gt; GSM559479     1  0.0000      0.980 1.000 0.000 0.000 0.000
#&gt; GSM559480     1  0.0000      0.980 1.000 0.000 0.000 0.000
#&gt; GSM559481     1  0.0000      0.980 1.000 0.000 0.000 0.000
#&gt; GSM559482     1  0.0000      0.980 1.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.0524      0.940 0.000 0.004 0.988 0.008
#&gt; GSM559439     3  0.4699      0.529 0.000 0.004 0.676 0.320
#&gt; GSM559443     4  0.0592      0.843 0.016 0.000 0.000 0.984
#&gt; GSM559447     3  0.4677      0.537 0.000 0.004 0.680 0.316
#&gt; GSM559449     3  0.0000      0.945 0.000 0.000 1.000 0.000
#&gt; GSM559453     3  0.4699      0.529 0.000 0.004 0.676 0.320
#&gt; GSM559466     3  0.0524      0.940 0.000 0.004 0.988 0.008
#&gt; GSM559474     3  0.0000      0.945 0.000 0.000 1.000 0.000
#&gt; GSM559476     3  0.0000      0.945 0.000 0.000 1.000 0.000
#&gt; GSM559483     1  0.0000      0.980 1.000 0.000 0.000 0.000
#&gt; GSM559484     1  0.0000      0.980 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-4'>
<p><a id='tab-ATC-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559433     1  0.1908     0.8480 0.908 0.000 0.092 0.000 0.000
#&gt; GSM559434     1  0.0609     0.9275 0.980 0.000 0.020 0.000 0.000
#&gt; GSM559436     5  0.1410     0.7005 0.000 0.000 0.060 0.000 0.940
#&gt; GSM559437     5  0.1270     0.7010 0.000 0.000 0.052 0.000 0.948
#&gt; GSM559438     4  0.0290     0.7472 0.000 0.000 0.008 0.992 0.000
#&gt; GSM559440     4  0.0290     0.7472 0.000 0.000 0.008 0.992 0.000
#&gt; GSM559441     3  0.3234     0.7662 0.084 0.000 0.852 0.000 0.064
#&gt; GSM559442     2  0.3177     0.7871 0.000 0.792 0.000 0.000 0.208
#&gt; GSM559444     4  0.0794     0.7372 0.000 0.028 0.000 0.972 0.000
#&gt; GSM559445     1  0.3876     0.3215 0.684 0.000 0.316 0.000 0.000
#&gt; GSM559446     5  0.5083     0.5591 0.000 0.000 0.280 0.068 0.652
#&gt; GSM559448     1  0.0000     0.9419 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559450     4  0.1041     0.7468 0.000 0.004 0.032 0.964 0.000
#&gt; GSM559451     4  0.4080     0.5757 0.012 0.000 0.136 0.800 0.052
#&gt; GSM559452     4  0.5721     0.3236 0.424 0.000 0.084 0.492 0.000
#&gt; GSM559454     1  0.0000     0.9419 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559455     1  0.0703     0.9297 0.976 0.000 0.024 0.000 0.000
#&gt; GSM559456     3  0.3876     0.8017 0.316 0.000 0.684 0.000 0.000
#&gt; GSM559457     1  0.0162     0.9428 0.996 0.000 0.004 0.000 0.000
#&gt; GSM559458     1  0.0290     0.9419 0.992 0.000 0.008 0.000 0.000
#&gt; GSM559459     2  0.3074     0.7979 0.000 0.804 0.000 0.000 0.196
#&gt; GSM559460     1  0.0000     0.9419 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559461     5  0.6721     0.2725 0.000 0.000 0.256 0.340 0.404
#&gt; GSM559462     2  0.3074     0.7979 0.000 0.804 0.000 0.000 0.196
#&gt; GSM559463     2  0.3109     0.7945 0.000 0.800 0.000 0.000 0.200
#&gt; GSM559464     2  0.4074     0.5383 0.000 0.636 0.000 0.000 0.364
#&gt; GSM559465     3  0.3684     0.8348 0.280 0.000 0.720 0.000 0.000
#&gt; GSM559467     2  0.0000     0.9062 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559468     5  0.4760    -0.0645 0.000 0.416 0.020 0.000 0.564
#&gt; GSM559469     1  0.0609     0.9275 0.980 0.000 0.020 0.000 0.000
#&gt; GSM559470     1  0.0162     0.9428 0.996 0.000 0.004 0.000 0.000
#&gt; GSM559471     2  0.0000     0.9062 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559472     1  0.0162     0.9428 0.996 0.000 0.004 0.000 0.000
#&gt; GSM559473     4  0.2248     0.7291 0.012 0.000 0.088 0.900 0.000
#&gt; GSM559475     4  0.5682     0.4354 0.372 0.000 0.088 0.540 0.000
#&gt; GSM559477     2  0.0000     0.9062 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559478     2  0.0000     0.9062 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559479     2  0.0000     0.9062 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.0000     0.9062 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559481     2  0.0000     0.9062 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559482     2  0.0000     0.9062 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.3730     0.8300 0.288 0.000 0.712 0.000 0.000
#&gt; GSM559439     3  0.2921     0.8258 0.124 0.000 0.856 0.000 0.020
#&gt; GSM559443     5  0.0609     0.6893 0.000 0.000 0.020 0.000 0.980
#&gt; GSM559447     3  0.2921     0.8258 0.124 0.000 0.856 0.000 0.020
#&gt; GSM559449     1  0.0290     0.9419 0.992 0.000 0.008 0.000 0.000
#&gt; GSM559453     3  0.2921     0.8258 0.124 0.000 0.856 0.000 0.020
#&gt; GSM559466     3  0.3895     0.7964 0.320 0.000 0.680 0.000 0.000
#&gt; GSM559474     1  0.0290     0.9419 0.992 0.000 0.008 0.000 0.000
#&gt; GSM559476     1  0.2669     0.8455 0.876 0.000 0.104 0.000 0.020
#&gt; GSM559483     2  0.0000     0.9062 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559484     2  0.0000     0.9062 0.000 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-5'>
<p><a id='tab-ATC-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559433     1  0.1082      0.925 0.956 0.000 0.040 0.000 0.004 0.000
#&gt; GSM559434     1  0.0458      0.940 0.984 0.000 0.000 0.000 0.000 0.016
#&gt; GSM559436     4  0.2350      0.658 0.000 0.000 0.036 0.888 0.076 0.000
#&gt; GSM559437     4  0.1863      0.694 0.000 0.000 0.036 0.920 0.044 0.000
#&gt; GSM559438     6  0.3907      0.425 0.000 0.000 0.004 0.000 0.408 0.588
#&gt; GSM559440     6  0.3907      0.425 0.000 0.000 0.004 0.000 0.408 0.588
#&gt; GSM559441     3  0.1138      0.872 0.024 0.000 0.960 0.012 0.004 0.000
#&gt; GSM559442     2  0.3835      0.609 0.000 0.668 0.000 0.320 0.012 0.000
#&gt; GSM559444     6  0.4289      0.462 0.000 0.024 0.004 0.000 0.340 0.632
#&gt; GSM559445     1  0.3337      0.597 0.736 0.000 0.260 0.000 0.004 0.000
#&gt; GSM559446     5  0.5160      0.416 0.000 0.000 0.108 0.320 0.572 0.000
#&gt; GSM559448     1  0.0000      0.949 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559450     6  0.1863      0.550 0.000 0.000 0.000 0.000 0.104 0.896
#&gt; GSM559451     5  0.4866      0.274 0.020 0.000 0.024 0.028 0.684 0.244
#&gt; GSM559452     6  0.4031      0.343 0.332 0.000 0.008 0.000 0.008 0.652
#&gt; GSM559454     1  0.0146      0.948 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; GSM559455     1  0.0458      0.946 0.984 0.000 0.016 0.000 0.000 0.000
#&gt; GSM559456     3  0.2668      0.865 0.168 0.000 0.828 0.000 0.004 0.000
#&gt; GSM559457     1  0.0291      0.949 0.992 0.000 0.004 0.000 0.004 0.000
#&gt; GSM559458     1  0.0405      0.948 0.988 0.000 0.008 0.000 0.004 0.000
#&gt; GSM559459     2  0.3729      0.639 0.000 0.692 0.000 0.296 0.012 0.000
#&gt; GSM559460     1  0.0000      0.949 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559461     5  0.4684      0.618 0.000 0.000 0.076 0.152 0.732 0.040
#&gt; GSM559462     2  0.3690      0.647 0.000 0.700 0.000 0.288 0.012 0.000
#&gt; GSM559463     2  0.3819      0.615 0.000 0.672 0.000 0.316 0.012 0.000
#&gt; GSM559464     2  0.4328      0.292 0.000 0.520 0.000 0.460 0.020 0.000
#&gt; GSM559465     3  0.2234      0.891 0.124 0.000 0.872 0.000 0.004 0.000
#&gt; GSM559467     2  0.0870      0.838 0.000 0.972 0.000 0.012 0.012 0.004
#&gt; GSM559468     4  0.6048      0.354 0.000 0.256 0.020 0.528 0.196 0.000
#&gt; GSM559469     1  0.0547      0.936 0.980 0.000 0.000 0.000 0.000 0.020
#&gt; GSM559470     1  0.0000      0.949 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559471     2  0.0603      0.845 0.000 0.980 0.000 0.016 0.004 0.000
#&gt; GSM559472     1  0.0000      0.949 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559473     6  0.0291      0.534 0.000 0.000 0.004 0.000 0.004 0.992
#&gt; GSM559475     6  0.3104      0.450 0.204 0.000 0.004 0.000 0.004 0.788
#&gt; GSM559477     2  0.0000      0.851 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559478     2  0.0000      0.851 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559479     2  0.0000      0.851 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.0000      0.851 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559481     2  0.0000      0.851 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559482     2  0.0000      0.851 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.2442      0.883 0.144 0.000 0.852 0.000 0.004 0.000
#&gt; GSM559439     3  0.0865      0.889 0.036 0.000 0.964 0.000 0.000 0.000
#&gt; GSM559443     4  0.0520      0.693 0.000 0.000 0.008 0.984 0.008 0.000
#&gt; GSM559447     3  0.0865      0.889 0.036 0.000 0.964 0.000 0.000 0.000
#&gt; GSM559449     1  0.0405      0.948 0.988 0.000 0.008 0.000 0.004 0.000
#&gt; GSM559453     3  0.0865      0.889 0.036 0.000 0.964 0.000 0.000 0.000
#&gt; GSM559466     3  0.2738      0.855 0.176 0.000 0.820 0.000 0.004 0.000
#&gt; GSM559474     1  0.0508      0.947 0.984 0.000 0.012 0.000 0.004 0.000
#&gt; GSM559476     1  0.4589      0.734 0.764 0.000 0.060 0.008 0.108 0.060
#&gt; GSM559483     2  0.0000      0.851 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559484     2  0.0000      0.851 0.000 1.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-skmeans-signature_compare](figure_cola/ATC-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-skmeans-collect-classes](figure_cola/ATC-skmeans-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>              n disease.state(p) k
#> ATC:skmeans 52           0.5143 2
#> ATC:skmeans 50           0.0874 3
#> ATC:skmeans 51           0.1201 4
#> ATC:skmeans 47           0.0489 5
#> ATC:skmeans 43           0.1542 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "pam"]
# you can also extract it by
# res = res_list["ATC:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-pam-collect-plots](figure_cola/ATC-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-pam-select-partition-number](figure_cola/ATC-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.997       0.999         0.3393 0.660   0.660
#> 3 3 0.629           0.705       0.882         0.7811 0.618   0.463
#> 4 4 0.572           0.708       0.830         0.1585 0.844   0.617
#> 5 5 0.746           0.793       0.876         0.1046 0.888   0.627
#> 6 6 0.814           0.825       0.902         0.0425 0.971   0.865
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-classes'>
<ul>
<li><a href='#tab-ATC-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-pam-get-classes-1'>
<p><a id='tab-ATC-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559433     1   0.000      1.000 1.000 0.000
#&gt; GSM559434     1   0.000      1.000 1.000 0.000
#&gt; GSM559436     1   0.000      1.000 1.000 0.000
#&gt; GSM559437     1   0.000      1.000 1.000 0.000
#&gt; GSM559438     1   0.000      1.000 1.000 0.000
#&gt; GSM559440     1   0.000      1.000 1.000 0.000
#&gt; GSM559441     1   0.000      1.000 1.000 0.000
#&gt; GSM559442     1   0.000      1.000 1.000 0.000
#&gt; GSM559444     1   0.000      1.000 1.000 0.000
#&gt; GSM559445     1   0.000      1.000 1.000 0.000
#&gt; GSM559446     1   0.000      1.000 1.000 0.000
#&gt; GSM559448     1   0.000      1.000 1.000 0.000
#&gt; GSM559450     1   0.000      1.000 1.000 0.000
#&gt; GSM559451     1   0.000      1.000 1.000 0.000
#&gt; GSM559452     1   0.000      1.000 1.000 0.000
#&gt; GSM559454     1   0.000      1.000 1.000 0.000
#&gt; GSM559455     1   0.000      1.000 1.000 0.000
#&gt; GSM559456     1   0.000      1.000 1.000 0.000
#&gt; GSM559457     1   0.000      1.000 1.000 0.000
#&gt; GSM559458     1   0.000      1.000 1.000 0.000
#&gt; GSM559459     2   0.000      0.994 0.000 1.000
#&gt; GSM559460     1   0.000      1.000 1.000 0.000
#&gt; GSM559461     1   0.000      1.000 1.000 0.000
#&gt; GSM559462     2   0.000      0.994 0.000 1.000
#&gt; GSM559463     2   0.000      0.994 0.000 1.000
#&gt; GSM559464     1   0.000      1.000 1.000 0.000
#&gt; GSM559465     1   0.000      1.000 1.000 0.000
#&gt; GSM559467     1   0.000      1.000 1.000 0.000
#&gt; GSM559468     1   0.000      1.000 1.000 0.000
#&gt; GSM559469     1   0.000      1.000 1.000 0.000
#&gt; GSM559470     1   0.000      1.000 1.000 0.000
#&gt; GSM559471     1   0.000      1.000 1.000 0.000
#&gt; GSM559472     1   0.000      1.000 1.000 0.000
#&gt; GSM559473     1   0.000      1.000 1.000 0.000
#&gt; GSM559475     1   0.000      1.000 1.000 0.000
#&gt; GSM559477     2   0.000      0.994 0.000 1.000
#&gt; GSM559478     2   0.000      0.994 0.000 1.000
#&gt; GSM559479     2   0.000      0.994 0.000 1.000
#&gt; GSM559480     2   0.000      0.994 0.000 1.000
#&gt; GSM559481     2   0.000      0.994 0.000 1.000
#&gt; GSM559482     2   0.000      0.994 0.000 1.000
#&gt; GSM559435     1   0.000      1.000 1.000 0.000
#&gt; GSM559439     1   0.000      1.000 1.000 0.000
#&gt; GSM559443     1   0.000      1.000 1.000 0.000
#&gt; GSM559447     1   0.000      1.000 1.000 0.000
#&gt; GSM559449     1   0.000      1.000 1.000 0.000
#&gt; GSM559453     1   0.000      1.000 1.000 0.000
#&gt; GSM559466     1   0.000      1.000 1.000 0.000
#&gt; GSM559474     1   0.000      1.000 1.000 0.000
#&gt; GSM559476     1   0.000      1.000 1.000 0.000
#&gt; GSM559483     2   0.000      0.994 0.000 1.000
#&gt; GSM559484     2   0.343      0.932 0.064 0.936
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-1-a').click(function(){
  $('#tab-ATC-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-2'>
<p><a id='tab-ATC-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559433     3   0.000     0.8879 0.000 0.000 1.000
#&gt; GSM559434     3   0.000     0.8879 0.000 0.000 1.000
#&gt; GSM559436     1   0.000     0.7865 1.000 0.000 0.000
#&gt; GSM559437     1   0.000     0.7865 1.000 0.000 0.000
#&gt; GSM559438     1   0.000     0.7865 1.000 0.000 0.000
#&gt; GSM559440     1   0.614     0.3794 0.596 0.000 0.404
#&gt; GSM559441     1   0.614     0.3794 0.596 0.000 0.404
#&gt; GSM559442     1   0.000     0.7865 1.000 0.000 0.000
#&gt; GSM559444     1   0.000     0.7865 1.000 0.000 0.000
#&gt; GSM559445     3   0.000     0.8879 0.000 0.000 1.000
#&gt; GSM559446     1   0.000     0.7865 1.000 0.000 0.000
#&gt; GSM559448     3   0.000     0.8879 0.000 0.000 1.000
#&gt; GSM559450     1   0.000     0.7865 1.000 0.000 0.000
#&gt; GSM559451     1   0.236     0.7538 0.928 0.000 0.072
#&gt; GSM559452     1   0.614     0.3794 0.596 0.000 0.404
#&gt; GSM559454     3   0.465     0.7119 0.208 0.000 0.792
#&gt; GSM559455     3   0.465     0.7119 0.208 0.000 0.792
#&gt; GSM559456     3   0.000     0.8879 0.000 0.000 1.000
#&gt; GSM559457     3   0.000     0.8879 0.000 0.000 1.000
#&gt; GSM559458     3   0.000     0.8879 0.000 0.000 1.000
#&gt; GSM559459     1   0.613    -0.0311 0.600 0.400 0.000
#&gt; GSM559460     1   0.614     0.3794 0.596 0.000 0.404
#&gt; GSM559461     1   0.000     0.7865 1.000 0.000 0.000
#&gt; GSM559462     2   0.000     0.8791 0.000 1.000 0.000
#&gt; GSM559463     2   0.614     0.5094 0.404 0.596 0.000
#&gt; GSM559464     1   0.000     0.7865 1.000 0.000 0.000
#&gt; GSM559465     3   0.613     0.2645 0.400 0.000 0.600
#&gt; GSM559467     1   0.000     0.7865 1.000 0.000 0.000
#&gt; GSM559468     1   0.000     0.7865 1.000 0.000 0.000
#&gt; GSM559469     3   0.000     0.8879 0.000 0.000 1.000
#&gt; GSM559470     3   0.000     0.8879 0.000 0.000 1.000
#&gt; GSM559471     1   0.000     0.7865 1.000 0.000 0.000
#&gt; GSM559472     3   0.000     0.8879 0.000 0.000 1.000
#&gt; GSM559473     1   0.000     0.7865 1.000 0.000 0.000
#&gt; GSM559475     1   0.630     0.1607 0.524 0.000 0.476
#&gt; GSM559477     2   0.000     0.8791 0.000 1.000 0.000
#&gt; GSM559478     1   0.164     0.7411 0.956 0.044 0.000
#&gt; GSM559479     2   0.000     0.8791 0.000 1.000 0.000
#&gt; GSM559480     2   0.000     0.8791 0.000 1.000 0.000
#&gt; GSM559481     2   0.614     0.5094 0.404 0.596 0.000
#&gt; GSM559482     2   0.000     0.8791 0.000 1.000 0.000
#&gt; GSM559435     3   0.296     0.8218 0.100 0.000 0.900
#&gt; GSM559439     1   0.614     0.3794 0.596 0.000 0.404
#&gt; GSM559443     1   0.000     0.7865 1.000 0.000 0.000
#&gt; GSM559447     1   0.614     0.3794 0.596 0.000 0.404
#&gt; GSM559449     3   0.000     0.8879 0.000 0.000 1.000
#&gt; GSM559453     1   0.614     0.3794 0.596 0.000 0.404
#&gt; GSM559466     3   0.460     0.7171 0.204 0.000 0.796
#&gt; GSM559474     3   0.000     0.8879 0.000 0.000 1.000
#&gt; GSM559476     3   0.599     0.4152 0.368 0.000 0.632
#&gt; GSM559483     2   0.000     0.8791 0.000 1.000 0.000
#&gt; GSM559484     1   0.000     0.7865 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-2-a').click(function(){
  $('#tab-ATC-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-3'>
<p><a id='tab-ATC-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559433     1  0.3257    0.82396 0.844 0.152 0.004 0.000
#&gt; GSM559434     1  0.1209    0.85208 0.964 0.032 0.004 0.000
#&gt; GSM559436     2  0.3074    0.60421 0.000 0.848 0.152 0.000
#&gt; GSM559437     2  0.3074    0.60421 0.000 0.848 0.152 0.000
#&gt; GSM559438     2  0.0000    0.75139 0.000 1.000 0.000 0.000
#&gt; GSM559440     2  0.1022    0.75543 0.032 0.968 0.000 0.000
#&gt; GSM559441     2  0.1022    0.75543 0.032 0.968 0.000 0.000
#&gt; GSM559442     3  0.4406    0.75329 0.000 0.300 0.700 0.000
#&gt; GSM559444     2  0.3837    0.50085 0.000 0.776 0.224 0.000
#&gt; GSM559445     1  0.0188    0.85269 0.996 0.000 0.004 0.000
#&gt; GSM559446     2  0.0000    0.75139 0.000 1.000 0.000 0.000
#&gt; GSM559448     1  0.0336    0.85266 0.992 0.000 0.008 0.000
#&gt; GSM559450     2  0.3726    0.52347 0.000 0.788 0.212 0.000
#&gt; GSM559451     2  0.0336    0.75345 0.008 0.992 0.000 0.000
#&gt; GSM559452     2  0.1209    0.75449 0.032 0.964 0.004 0.000
#&gt; GSM559454     1  0.4401    0.71072 0.724 0.272 0.004 0.000
#&gt; GSM559455     1  0.4401    0.71072 0.724 0.272 0.004 0.000
#&gt; GSM559456     1  0.0336    0.85266 0.992 0.000 0.008 0.000
#&gt; GSM559457     1  0.0336    0.85266 0.992 0.000 0.008 0.000
#&gt; GSM559458     1  0.0336    0.85266 0.992 0.000 0.008 0.000
#&gt; GSM559459     3  0.1902    0.71343 0.000 0.064 0.932 0.004
#&gt; GSM559460     2  0.5004    0.21158 0.392 0.604 0.004 0.000
#&gt; GSM559461     2  0.0000    0.75139 0.000 1.000 0.000 0.000
#&gt; GSM559462     4  0.2921    0.88390 0.000 0.000 0.140 0.860
#&gt; GSM559463     3  0.4713    0.00817 0.000 0.000 0.640 0.360
#&gt; GSM559464     3  0.4431    0.75160 0.000 0.304 0.696 0.000
#&gt; GSM559465     1  0.4898    0.40412 0.584 0.416 0.000 0.000
#&gt; GSM559467     3  0.4431    0.75160 0.000 0.304 0.696 0.000
#&gt; GSM559468     2  0.4830    0.02879 0.000 0.608 0.392 0.000
#&gt; GSM559469     1  0.3257    0.82396 0.844 0.152 0.004 0.000
#&gt; GSM559470     1  0.3257    0.82396 0.844 0.152 0.004 0.000
#&gt; GSM559471     3  0.4431    0.75160 0.000 0.304 0.696 0.000
#&gt; GSM559472     1  0.0336    0.85266 0.992 0.000 0.008 0.000
#&gt; GSM559473     2  0.1474    0.71901 0.000 0.948 0.052 0.000
#&gt; GSM559475     2  0.2593    0.71265 0.104 0.892 0.004 0.000
#&gt; GSM559477     4  0.0000    0.97766 0.000 0.000 0.000 1.000
#&gt; GSM559478     3  0.3501    0.76681 0.000 0.132 0.848 0.020
#&gt; GSM559479     4  0.0000    0.97766 0.000 0.000 0.000 1.000
#&gt; GSM559480     4  0.0000    0.97766 0.000 0.000 0.000 1.000
#&gt; GSM559481     3  0.3501    0.64803 0.000 0.020 0.848 0.132
#&gt; GSM559482     4  0.0000    0.97766 0.000 0.000 0.000 1.000
#&gt; GSM559435     1  0.3751    0.79360 0.800 0.196 0.004 0.000
#&gt; GSM559439     2  0.1211    0.75355 0.040 0.960 0.000 0.000
#&gt; GSM559443     3  0.4605    0.70866 0.000 0.336 0.664 0.000
#&gt; GSM559447     2  0.4776    0.26633 0.376 0.624 0.000 0.000
#&gt; GSM559449     1  0.0336    0.85266 0.992 0.000 0.008 0.000
#&gt; GSM559453     2  0.4776    0.26633 0.376 0.624 0.000 0.000
#&gt; GSM559466     1  0.4401    0.71072 0.724 0.272 0.004 0.000
#&gt; GSM559474     1  0.0336    0.85266 0.992 0.000 0.008 0.000
#&gt; GSM559476     2  0.4193    0.49571 0.268 0.732 0.000 0.000
#&gt; GSM559483     4  0.0000    0.97766 0.000 0.000 0.000 1.000
#&gt; GSM559484     3  0.3074    0.77001 0.000 0.152 0.848 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-3-a').click(function(){
  $('#tab-ATC-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-4'>
<p><a id='tab-ATC-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559433     3  0.1544      0.843 0.000 0.000 0.932 0.000 0.068
#&gt; GSM559434     3  0.3366      0.636 0.000 0.000 0.768 0.000 0.232
#&gt; GSM559436     4  0.0566      0.859 0.012 0.000 0.004 0.984 0.000
#&gt; GSM559437     4  0.0566      0.859 0.012 0.000 0.004 0.984 0.000
#&gt; GSM559438     4  0.0404      0.863 0.000 0.000 0.012 0.988 0.000
#&gt; GSM559440     4  0.1608      0.844 0.000 0.000 0.072 0.928 0.000
#&gt; GSM559441     4  0.1341      0.855 0.000 0.000 0.056 0.944 0.000
#&gt; GSM559442     1  0.3424      0.782 0.760 0.000 0.000 0.240 0.000
#&gt; GSM559444     4  0.1341      0.833 0.056 0.000 0.000 0.944 0.000
#&gt; GSM559445     3  0.4262     -0.155 0.000 0.000 0.560 0.000 0.440
#&gt; GSM559446     4  0.0510      0.864 0.000 0.000 0.016 0.984 0.000
#&gt; GSM559448     5  0.3109      0.982 0.000 0.000 0.200 0.000 0.800
#&gt; GSM559450     4  0.1043      0.846 0.040 0.000 0.000 0.960 0.000
#&gt; GSM559451     4  0.0510      0.864 0.000 0.000 0.016 0.984 0.000
#&gt; GSM559452     4  0.2929      0.779 0.000 0.000 0.180 0.820 0.000
#&gt; GSM559454     3  0.0000      0.887 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559455     3  0.0000      0.887 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559456     5  0.3508      0.916 0.000 0.000 0.252 0.000 0.748
#&gt; GSM559457     5  0.3074      0.986 0.000 0.000 0.196 0.000 0.804
#&gt; GSM559458     5  0.3074      0.986 0.000 0.000 0.196 0.000 0.804
#&gt; GSM559459     1  0.0880      0.762 0.968 0.000 0.000 0.032 0.000
#&gt; GSM559460     3  0.1341      0.836 0.000 0.000 0.944 0.056 0.000
#&gt; GSM559461     4  0.0510      0.864 0.000 0.000 0.016 0.984 0.000
#&gt; GSM559462     2  0.4654      0.785 0.052 0.740 0.000 0.012 0.196
#&gt; GSM559463     1  0.6824     -0.221 0.452 0.340 0.000 0.012 0.196
#&gt; GSM559464     1  0.3480      0.779 0.752 0.000 0.000 0.248 0.000
#&gt; GSM559465     3  0.0162      0.886 0.000 0.000 0.996 0.004 0.000
#&gt; GSM559467     1  0.3480      0.779 0.752 0.000 0.000 0.248 0.000
#&gt; GSM559468     4  0.4249     -0.118 0.432 0.000 0.000 0.568 0.000
#&gt; GSM559469     3  0.2516      0.789 0.000 0.000 0.860 0.000 0.140
#&gt; GSM559470     3  0.1410      0.857 0.000 0.000 0.940 0.000 0.060
#&gt; GSM559471     1  0.3480      0.779 0.752 0.000 0.000 0.248 0.000
#&gt; GSM559472     5  0.3074      0.986 0.000 0.000 0.196 0.000 0.804
#&gt; GSM559473     4  0.0451      0.862 0.004 0.000 0.008 0.988 0.000
#&gt; GSM559475     4  0.3707      0.661 0.000 0.000 0.284 0.716 0.000
#&gt; GSM559477     2  0.0000      0.960 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559478     1  0.1444      0.767 0.948 0.012 0.000 0.040 0.000
#&gt; GSM559479     2  0.0000      0.960 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.0000      0.960 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559481     1  0.1270      0.727 0.948 0.052 0.000 0.000 0.000
#&gt; GSM559482     2  0.0000      0.960 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.0000      0.887 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559439     4  0.3074      0.768 0.000 0.000 0.196 0.804 0.000
#&gt; GSM559443     1  0.3715      0.762 0.736 0.000 0.004 0.260 0.000
#&gt; GSM559447     3  0.0162      0.886 0.000 0.000 0.996 0.004 0.000
#&gt; GSM559449     5  0.3074      0.986 0.000 0.000 0.196 0.000 0.804
#&gt; GSM559453     3  0.0404      0.883 0.000 0.000 0.988 0.012 0.000
#&gt; GSM559466     3  0.0000      0.887 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559474     5  0.3074      0.986 0.000 0.000 0.196 0.000 0.804
#&gt; GSM559476     4  0.3966      0.598 0.000 0.000 0.336 0.664 0.000
#&gt; GSM559483     2  0.0000      0.960 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559484     1  0.1270      0.773 0.948 0.000 0.000 0.052 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-4-a').click(function(){
  $('#tab-ATC-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-5'>
<p><a id='tab-ATC-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559433     3  0.2664      0.775 0.000 0.000 0.816 0.000 0.184 0.000
#&gt; GSM559434     3  0.5149      0.603 0.000 0.000 0.624 0.192 0.184 0.000
#&gt; GSM559436     6  0.0000      0.879 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559437     6  0.0000      0.879 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559438     6  0.0000      0.879 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559440     6  0.0632      0.874 0.000 0.000 0.024 0.000 0.000 0.976
#&gt; GSM559441     6  0.2178      0.823 0.000 0.000 0.132 0.000 0.000 0.868
#&gt; GSM559442     1  0.1444      0.879 0.928 0.000 0.000 0.000 0.000 0.072
#&gt; GSM559444     6  0.0937      0.864 0.040 0.000 0.000 0.000 0.000 0.960
#&gt; GSM559445     3  0.4084      0.319 0.000 0.000 0.588 0.400 0.012 0.000
#&gt; GSM559446     6  0.0000      0.879 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559448     4  0.4222      0.694 0.000 0.000 0.088 0.728 0.184 0.000
#&gt; GSM559450     6  0.1556      0.836 0.080 0.000 0.000 0.000 0.000 0.920
#&gt; GSM559451     6  0.0000      0.879 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559452     6  0.2996      0.756 0.000 0.000 0.228 0.000 0.000 0.772
#&gt; GSM559454     3  0.2009      0.806 0.000 0.000 0.908 0.000 0.024 0.068
#&gt; GSM559455     3  0.0363      0.841 0.000 0.000 0.988 0.000 0.012 0.000
#&gt; GSM559456     4  0.2092      0.765 0.000 0.000 0.124 0.876 0.000 0.000
#&gt; GSM559457     4  0.0000      0.883 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559458     4  0.0000      0.883 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559459     1  0.0146      0.859 0.996 0.000 0.000 0.000 0.004 0.000
#&gt; GSM559460     3  0.2092      0.753 0.000 0.000 0.876 0.000 0.000 0.124
#&gt; GSM559461     6  0.0000      0.879 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559462     5  0.2664      1.000 0.000 0.184 0.000 0.000 0.816 0.000
#&gt; GSM559463     5  0.2664      1.000 0.000 0.184 0.000 0.000 0.816 0.000
#&gt; GSM559464     1  0.1556      0.879 0.920 0.000 0.000 0.000 0.000 0.080
#&gt; GSM559465     3  0.0146      0.841 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM559467     1  0.1556      0.879 0.920 0.000 0.000 0.000 0.000 0.080
#&gt; GSM559468     1  0.3867      0.244 0.512 0.000 0.000 0.000 0.000 0.488
#&gt; GSM559469     3  0.4643      0.692 0.000 0.000 0.688 0.128 0.184 0.000
#&gt; GSM559470     3  0.3453      0.781 0.000 0.000 0.804 0.064 0.132 0.000
#&gt; GSM559471     1  0.1556      0.879 0.920 0.000 0.000 0.000 0.000 0.080
#&gt; GSM559472     4  0.2664      0.775 0.000 0.000 0.000 0.816 0.184 0.000
#&gt; GSM559473     6  0.0000      0.879 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559475     6  0.3390      0.682 0.000 0.000 0.296 0.000 0.000 0.704
#&gt; GSM559477     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559478     1  0.0000      0.861 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559479     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559481     1  0.0000      0.861 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; GSM559482     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.0000      0.840 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559439     6  0.3607      0.629 0.000 0.000 0.348 0.000 0.000 0.652
#&gt; GSM559443     1  0.2135      0.842 0.872 0.000 0.000 0.000 0.000 0.128
#&gt; GSM559447     3  0.0146      0.841 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; GSM559449     4  0.0000      0.883 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559453     3  0.0260      0.839 0.000 0.000 0.992 0.000 0.000 0.008
#&gt; GSM559466     3  0.0363      0.841 0.000 0.000 0.988 0.000 0.012 0.000
#&gt; GSM559474     4  0.0000      0.883 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559476     6  0.3936      0.676 0.000 0.000 0.288 0.000 0.024 0.688
#&gt; GSM559483     2  0.0000      1.000 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559484     1  0.0000      0.861 1.000 0.000 0.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-5-a').click(function(){
  $('#tab-ATC-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-pam-signature_compare](figure_cola/ATC-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-pam-collect-classes](figure_cola/ATC-pam-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n disease.state(p) k
#> ATC:pam 52            1.000 2
#> ATC:pam 41            0.558 3
#> ATC:pam 45            0.619 4
#> ATC:pam 49            0.773 5
#> ATC:pam 50            0.805 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "mclust"]
# you can also extract it by
# res = res_list["ATC:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-mclust-collect-plots](figure_cola/ATC-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-mclust-select-partition-number](figure_cola/ATC-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.674           0.913       0.949          0.289 0.683   0.683
#> 3 3 0.455           0.657       0.837          0.608 0.855   0.799
#> 4 4 0.361           0.408       0.727          0.149 0.864   0.799
#> 5 5 0.538           0.780       0.854          0.251 0.652   0.465
#> 6 6 0.649           0.627       0.801          0.118 0.843   0.578
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 5
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-classes'>
<ul>
<li><a href='#tab-ATC-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-mclust-get-classes-1'>
<p><a id='tab-ATC-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559433     1  0.0000      0.971 1.000 0.000
#&gt; GSM559434     1  0.0000      0.971 1.000 0.000
#&gt; GSM559436     1  0.0000      0.971 1.000 0.000
#&gt; GSM559437     1  0.0000      0.971 1.000 0.000
#&gt; GSM559438     1  0.0000      0.971 1.000 0.000
#&gt; GSM559440     1  0.3733      0.899 0.928 0.072
#&gt; GSM559441     1  0.0000      0.971 1.000 0.000
#&gt; GSM559442     1  0.0000      0.971 1.000 0.000
#&gt; GSM559444     1  0.6623      0.747 0.828 0.172
#&gt; GSM559445     1  0.0000      0.971 1.000 0.000
#&gt; GSM559446     1  0.0000      0.971 1.000 0.000
#&gt; GSM559448     1  0.0000      0.971 1.000 0.000
#&gt; GSM559450     1  0.4815      0.859 0.896 0.104
#&gt; GSM559451     1  0.0000      0.971 1.000 0.000
#&gt; GSM559452     1  0.0000      0.971 1.000 0.000
#&gt; GSM559454     1  0.0000      0.971 1.000 0.000
#&gt; GSM559455     1  0.0000      0.971 1.000 0.000
#&gt; GSM559456     1  0.0000      0.971 1.000 0.000
#&gt; GSM559457     1  0.0376      0.968 0.996 0.004
#&gt; GSM559458     1  0.0000      0.971 1.000 0.000
#&gt; GSM559459     1  0.0000      0.971 1.000 0.000
#&gt; GSM559460     1  0.0000      0.971 1.000 0.000
#&gt; GSM559461     1  0.0000      0.971 1.000 0.000
#&gt; GSM559462     1  0.4690      0.865 0.900 0.100
#&gt; GSM559463     1  0.3431      0.909 0.936 0.064
#&gt; GSM559464     1  0.0000      0.971 1.000 0.000
#&gt; GSM559465     1  0.0000      0.971 1.000 0.000
#&gt; GSM559467     1  0.1184      0.958 0.984 0.016
#&gt; GSM559468     1  0.0000      0.971 1.000 0.000
#&gt; GSM559469     1  0.0000      0.971 1.000 0.000
#&gt; GSM559470     1  0.0000      0.971 1.000 0.000
#&gt; GSM559471     1  0.0000      0.971 1.000 0.000
#&gt; GSM559472     1  0.0000      0.971 1.000 0.000
#&gt; GSM559473     1  0.4815      0.859 0.896 0.104
#&gt; GSM559475     1  0.4815      0.859 0.896 0.104
#&gt; GSM559477     2  0.0000      0.788 0.000 1.000
#&gt; GSM559478     2  0.8955      0.752 0.312 0.688
#&gt; GSM559479     2  0.0000      0.788 0.000 1.000
#&gt; GSM559480     2  0.7745      0.800 0.228 0.772
#&gt; GSM559481     2  0.8499      0.788 0.276 0.724
#&gt; GSM559482     2  0.0000      0.788 0.000 1.000
#&gt; GSM559435     1  0.0000      0.971 1.000 0.000
#&gt; GSM559439     1  0.0000      0.971 1.000 0.000
#&gt; GSM559443     1  0.0000      0.971 1.000 0.000
#&gt; GSM559447     1  0.0000      0.971 1.000 0.000
#&gt; GSM559449     1  0.6801      0.732 0.820 0.180
#&gt; GSM559453     1  0.0000      0.971 1.000 0.000
#&gt; GSM559466     1  0.0000      0.971 1.000 0.000
#&gt; GSM559474     2  0.9323      0.693 0.348 0.652
#&gt; GSM559476     2  0.8661      0.781 0.288 0.712
#&gt; GSM559483     2  0.0000      0.788 0.000 1.000
#&gt; GSM559484     2  0.8661      0.781 0.288 0.712
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-1-a').click(function(){
  $('#tab-ATC-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-2'>
<p><a id='tab-ATC-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559433     1  0.0000     0.8354 1.000 0.000 0.000
#&gt; GSM559434     1  0.0237     0.8343 0.996 0.000 0.004
#&gt; GSM559436     1  0.0237     0.8343 0.996 0.004 0.000
#&gt; GSM559437     1  0.0000     0.8354 1.000 0.000 0.000
#&gt; GSM559438     1  0.0000     0.8354 1.000 0.000 0.000
#&gt; GSM559440     1  0.6274     0.4091 0.544 0.000 0.456
#&gt; GSM559441     1  0.0000     0.8354 1.000 0.000 0.000
#&gt; GSM559442     1  0.0000     0.8354 1.000 0.000 0.000
#&gt; GSM559444     1  0.6280     0.4027 0.540 0.000 0.460
#&gt; GSM559445     1  0.0000     0.8354 1.000 0.000 0.000
#&gt; GSM559446     1  0.0000     0.8354 1.000 0.000 0.000
#&gt; GSM559448     1  0.0000     0.8354 1.000 0.000 0.000
#&gt; GSM559450     1  0.5882     0.5965 0.652 0.000 0.348
#&gt; GSM559451     1  0.0000     0.8354 1.000 0.000 0.000
#&gt; GSM559452     1  0.5760     0.6147 0.672 0.000 0.328
#&gt; GSM559454     1  0.0000     0.8354 1.000 0.000 0.000
#&gt; GSM559455     1  0.0000     0.8354 1.000 0.000 0.000
#&gt; GSM559456     1  0.6189     0.1417 0.632 0.004 0.364
#&gt; GSM559457     1  0.3267     0.7839 0.884 0.000 0.116
#&gt; GSM559458     1  0.2878     0.7958 0.904 0.000 0.096
#&gt; GSM559459     1  0.0475     0.8324 0.992 0.004 0.004
#&gt; GSM559460     1  0.0000     0.8354 1.000 0.000 0.000
#&gt; GSM559461     1  0.0000     0.8354 1.000 0.000 0.000
#&gt; GSM559462     3  0.9090     0.4356 0.332 0.156 0.512
#&gt; GSM559463     3  0.9090     0.4356 0.332 0.156 0.512
#&gt; GSM559464     1  0.0237     0.8338 0.996 0.000 0.004
#&gt; GSM559465     1  0.0000     0.8354 1.000 0.000 0.000
#&gt; GSM559467     1  0.6579     0.4728 0.652 0.328 0.020
#&gt; GSM559468     1  0.4862     0.7114 0.820 0.160 0.020
#&gt; GSM559469     1  0.5706     0.6235 0.680 0.000 0.320
#&gt; GSM559470     1  0.4702     0.7181 0.788 0.000 0.212
#&gt; GSM559471     1  0.4575     0.7162 0.828 0.160 0.012
#&gt; GSM559472     1  0.5706     0.6238 0.680 0.000 0.320
#&gt; GSM559473     1  0.5905     0.5921 0.648 0.000 0.352
#&gt; GSM559475     1  0.5968     0.5778 0.636 0.000 0.364
#&gt; GSM559477     2  0.0000     0.7375 0.000 1.000 0.000
#&gt; GSM559478     1  0.8239     0.1854 0.532 0.388 0.080
#&gt; GSM559479     2  0.0000     0.7375 0.000 1.000 0.000
#&gt; GSM559480     2  0.4558     0.5786 0.044 0.856 0.100
#&gt; GSM559481     2  0.8402    -0.0435 0.376 0.532 0.092
#&gt; GSM559482     2  0.0000     0.7375 0.000 1.000 0.000
#&gt; GSM559435     1  0.0000     0.8354 1.000 0.000 0.000
#&gt; GSM559439     1  0.0000     0.8354 1.000 0.000 0.000
#&gt; GSM559443     1  0.3412     0.7244 0.876 0.000 0.124
#&gt; GSM559447     1  0.0000     0.8354 1.000 0.000 0.000
#&gt; GSM559449     3  0.6026     0.3981 0.244 0.024 0.732
#&gt; GSM559453     1  0.0237     0.8343 0.996 0.004 0.000
#&gt; GSM559466     1  0.4172     0.6785 0.840 0.004 0.156
#&gt; GSM559474     3  0.7874     0.3035 0.064 0.368 0.568
#&gt; GSM559476     3  0.6625     0.1754 0.008 0.440 0.552
#&gt; GSM559483     2  0.0000     0.7375 0.000 1.000 0.000
#&gt; GSM559484     3  0.7652     0.2121 0.044 0.444 0.512
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-2-a').click(function(){
  $('#tab-ATC-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-3'>
<p><a id='tab-ATC-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559433     1  0.4744     0.5090 0.704 0.000 0.012 0.284
#&gt; GSM559434     1  0.0524     0.5784 0.988 0.000 0.008 0.004
#&gt; GSM559436     1  0.5972     0.4709 0.632 0.000 0.064 0.304
#&gt; GSM559437     1  0.6187     0.4353 0.656 0.000 0.108 0.236
#&gt; GSM559438     1  0.0469     0.5782 0.988 0.000 0.000 0.012
#&gt; GSM559440     1  0.2216     0.5419 0.908 0.000 0.000 0.092
#&gt; GSM559441     1  0.4516     0.4919 0.736 0.000 0.012 0.252
#&gt; GSM559442     1  0.3942     0.5146 0.764 0.000 0.000 0.236
#&gt; GSM559444     1  0.2216     0.5419 0.908 0.000 0.000 0.092
#&gt; GSM559445     1  0.4744     0.5090 0.704 0.000 0.012 0.284
#&gt; GSM559446     1  0.3123     0.5516 0.844 0.000 0.000 0.156
#&gt; GSM559448     1  0.0817     0.5795 0.976 0.000 0.000 0.024
#&gt; GSM559450     1  0.4776     0.2352 0.624 0.000 0.000 0.376
#&gt; GSM559451     1  0.0000     0.5801 1.000 0.000 0.000 0.000
#&gt; GSM559452     1  0.3569     0.4881 0.804 0.000 0.000 0.196
#&gt; GSM559454     1  0.0592     0.5776 0.984 0.000 0.000 0.016
#&gt; GSM559455     1  0.1867     0.5808 0.928 0.000 0.000 0.072
#&gt; GSM559456     1  0.6048     0.3004 0.680 0.028 0.252 0.040
#&gt; GSM559457     1  0.4901     0.5143 0.780 0.000 0.108 0.112
#&gt; GSM559458     1  0.4827     0.5112 0.784 0.000 0.124 0.092
#&gt; GSM559459     1  0.4630     0.4874 0.732 0.000 0.016 0.252
#&gt; GSM559460     1  0.0469     0.5802 0.988 0.000 0.000 0.012
#&gt; GSM559461     1  0.1716     0.5747 0.936 0.000 0.000 0.064
#&gt; GSM559462     3  0.5713     0.3886 0.036 0.000 0.604 0.360
#&gt; GSM559463     4  0.7890    -0.2294 0.352 0.000 0.288 0.360
#&gt; GSM559464     1  0.3942     0.5146 0.764 0.000 0.000 0.236
#&gt; GSM559465     1  0.5178     0.4886 0.716 0.020 0.012 0.252
#&gt; GSM559467     1  0.6844     0.0934 0.588 0.152 0.000 0.260
#&gt; GSM559468     4  0.8778    -0.2534 0.380 0.128 0.092 0.400
#&gt; GSM559469     1  0.4746     0.2458 0.632 0.000 0.000 0.368
#&gt; GSM559470     1  0.3219     0.5375 0.836 0.000 0.000 0.164
#&gt; GSM559471     1  0.6844     0.0934 0.588 0.152 0.000 0.260
#&gt; GSM559472     1  0.4332     0.5101 0.792 0.000 0.032 0.176
#&gt; GSM559473     1  0.4776     0.2352 0.624 0.000 0.000 0.376
#&gt; GSM559475     1  0.4776     0.2352 0.624 0.000 0.000 0.376
#&gt; GSM559477     2  0.0000     0.8112 0.000 1.000 0.000 0.000
#&gt; GSM559478     1  0.6973     0.0554 0.584 0.220 0.000 0.196
#&gt; GSM559479     2  0.0000     0.8112 0.000 1.000 0.000 0.000
#&gt; GSM559480     2  0.1004     0.7848 0.024 0.972 0.000 0.004
#&gt; GSM559481     1  0.5360    -0.0673 0.552 0.436 0.000 0.012
#&gt; GSM559482     2  0.0000     0.8112 0.000 1.000 0.000 0.000
#&gt; GSM559435     1  0.4936     0.4918 0.672 0.000 0.012 0.316
#&gt; GSM559439     1  0.4477     0.5092 0.688 0.000 0.000 0.312
#&gt; GSM559443     1  0.4988     0.4834 0.728 0.000 0.036 0.236
#&gt; GSM559447     1  0.4857     0.4900 0.668 0.000 0.008 0.324
#&gt; GSM559449     1  0.5731    -0.0950 0.544 0.000 0.428 0.028
#&gt; GSM559453     1  0.5178     0.4886 0.716 0.020 0.012 0.252
#&gt; GSM559466     3  0.8377    -0.4709 0.376 0.032 0.400 0.192
#&gt; GSM559474     3  0.1543     0.5116 0.032 0.004 0.956 0.008
#&gt; GSM559476     3  0.1489     0.4952 0.000 0.044 0.952 0.004
#&gt; GSM559483     2  0.0000     0.8112 0.000 1.000 0.000 0.000
#&gt; GSM559484     2  0.7973    -0.3013 0.356 0.432 0.200 0.012
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-3-a').click(function(){
  $('#tab-ATC-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-4'>
<p><a id='tab-ATC-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559433     1  0.4358     0.5334 0.696 0.000 0.008 0.284 0.012
#&gt; GSM559434     4  0.1399     0.8291 0.028 0.000 0.020 0.952 0.000
#&gt; GSM559436     1  0.3868     0.8733 0.800 0.000 0.060 0.140 0.000
#&gt; GSM559437     1  0.3683     0.8789 0.832 0.004 0.032 0.120 0.012
#&gt; GSM559438     4  0.1569     0.8279 0.032 0.012 0.000 0.948 0.008
#&gt; GSM559440     4  0.1808     0.8276 0.044 0.012 0.000 0.936 0.008
#&gt; GSM559441     1  0.2513     0.8793 0.876 0.000 0.008 0.116 0.000
#&gt; GSM559442     1  0.3197     0.8716 0.836 0.000 0.024 0.140 0.000
#&gt; GSM559444     4  0.1969     0.8270 0.032 0.012 0.012 0.936 0.008
#&gt; GSM559445     1  0.2362     0.8447 0.900 0.000 0.008 0.084 0.008
#&gt; GSM559446     1  0.4542     0.7648 0.720 0.004 0.024 0.244 0.008
#&gt; GSM559448     4  0.2067     0.8226 0.048 0.000 0.032 0.920 0.000
#&gt; GSM559450     4  0.0290     0.8237 0.000 0.000 0.000 0.992 0.008
#&gt; GSM559451     4  0.2474     0.8059 0.084 0.012 0.000 0.896 0.008
#&gt; GSM559452     4  0.0960     0.8282 0.016 0.000 0.004 0.972 0.008
#&gt; GSM559454     4  0.1651     0.8281 0.036 0.000 0.012 0.944 0.008
#&gt; GSM559455     4  0.4942     0.5866 0.256 0.016 0.024 0.696 0.008
#&gt; GSM559456     1  0.2299     0.7893 0.916 0.012 0.004 0.012 0.056
#&gt; GSM559457     4  0.5416     0.6066 0.164 0.000 0.012 0.692 0.132
#&gt; GSM559458     4  0.5673     0.5795 0.188 0.000 0.016 0.668 0.128
#&gt; GSM559459     1  0.3688     0.8730 0.816 0.000 0.060 0.124 0.000
#&gt; GSM559460     4  0.2549     0.8189 0.060 0.004 0.024 0.904 0.008
#&gt; GSM559461     1  0.4948     0.5689 0.612 0.000 0.024 0.356 0.008
#&gt; GSM559462     3  0.1341     0.9601 0.000 0.000 0.944 0.000 0.056
#&gt; GSM559463     3  0.1469     0.9605 0.016 0.000 0.948 0.000 0.036
#&gt; GSM559464     1  0.3310     0.8733 0.836 0.004 0.024 0.136 0.000
#&gt; GSM559465     1  0.1408     0.8492 0.948 0.000 0.008 0.044 0.000
#&gt; GSM559467     4  0.2378     0.8054 0.064 0.016 0.012 0.908 0.000
#&gt; GSM559468     1  0.3777     0.8443 0.844 0.024 0.024 0.092 0.016
#&gt; GSM559469     4  0.0960     0.8231 0.016 0.000 0.004 0.972 0.008
#&gt; GSM559470     4  0.1579     0.8226 0.032 0.000 0.024 0.944 0.000
#&gt; GSM559471     4  0.4755     0.4419 0.292 0.028 0.000 0.672 0.008
#&gt; GSM559472     4  0.4101     0.7362 0.124 0.000 0.028 0.808 0.040
#&gt; GSM559473     4  0.0451     0.8236 0.000 0.004 0.000 0.988 0.008
#&gt; GSM559475     4  0.0451     0.8235 0.000 0.000 0.004 0.988 0.008
#&gt; GSM559477     2  0.0000     0.9419 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559478     4  0.3861     0.6129 0.000 0.264 0.000 0.728 0.008
#&gt; GSM559479     2  0.0404     0.9320 0.000 0.988 0.000 0.000 0.012
#&gt; GSM559480     2  0.2358     0.7796 0.000 0.888 0.000 0.104 0.008
#&gt; GSM559481     4  0.4489     0.3132 0.000 0.420 0.000 0.572 0.008
#&gt; GSM559482     2  0.0000     0.9419 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559435     1  0.2707     0.8800 0.860 0.000 0.008 0.132 0.000
#&gt; GSM559439     1  0.3197     0.8774 0.836 0.000 0.024 0.140 0.000
#&gt; GSM559443     1  0.2886     0.8609 0.884 0.000 0.036 0.068 0.012
#&gt; GSM559447     1  0.2707     0.8812 0.860 0.000 0.008 0.132 0.000
#&gt; GSM559449     4  0.5652     0.5251 0.140 0.000 0.004 0.644 0.212
#&gt; GSM559453     1  0.1956     0.8699 0.916 0.000 0.008 0.076 0.000
#&gt; GSM559466     1  0.2484     0.7889 0.912 0.012 0.008 0.020 0.048
#&gt; GSM559474     5  0.1701     0.8916 0.048 0.000 0.000 0.016 0.936
#&gt; GSM559476     5  0.0992     0.8888 0.000 0.000 0.024 0.008 0.968
#&gt; GSM559483     2  0.0000     0.9419 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559484     4  0.6722    -0.0257 0.000 0.408 0.020 0.432 0.140
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-4-a').click(function(){
  $('#tab-ATC-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-5'>
<p><a id='tab-ATC-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559433     3  0.3809     0.5443 0.304 0.000 0.684 0.000 0.004 0.008
#&gt; GSM559434     6  0.0653     0.6373 0.012 0.000 0.000 0.004 0.004 0.980
#&gt; GSM559436     3  0.0820     0.8566 0.012 0.000 0.972 0.016 0.000 0.000
#&gt; GSM559437     3  0.0748     0.8552 0.000 0.000 0.976 0.016 0.004 0.004
#&gt; GSM559438     6  0.4377     0.2817 0.436 0.000 0.024 0.000 0.000 0.540
#&gt; GSM559440     6  0.3098     0.6281 0.164 0.000 0.024 0.000 0.000 0.812
#&gt; GSM559441     3  0.0146     0.8569 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; GSM559442     3  0.0622     0.8578 0.012 0.000 0.980 0.008 0.000 0.000
#&gt; GSM559444     6  0.3017     0.6301 0.164 0.000 0.020 0.000 0.000 0.816
#&gt; GSM559445     3  0.3189     0.6347 0.236 0.000 0.760 0.000 0.004 0.000
#&gt; GSM559446     3  0.3189     0.7185 0.184 0.000 0.796 0.000 0.000 0.020
#&gt; GSM559448     6  0.4062     0.4350 0.344 0.000 0.012 0.004 0.000 0.640
#&gt; GSM559450     6  0.0146     0.6407 0.004 0.000 0.000 0.000 0.000 0.996
#&gt; GSM559451     1  0.5480    -0.0964 0.444 0.000 0.124 0.000 0.000 0.432
#&gt; GSM559452     6  0.3151     0.5667 0.252 0.000 0.000 0.000 0.000 0.748
#&gt; GSM559454     6  0.4139     0.4571 0.336 0.000 0.024 0.000 0.000 0.640
#&gt; GSM559455     1  0.6031     0.1277 0.420 0.000 0.268 0.000 0.000 0.312
#&gt; GSM559456     3  0.2902     0.7445 0.196 0.000 0.800 0.000 0.000 0.004
#&gt; GSM559457     1  0.5022     0.4453 0.680 0.000 0.016 0.004 0.096 0.204
#&gt; GSM559458     1  0.5042     0.4428 0.676 0.000 0.024 0.000 0.096 0.204
#&gt; GSM559459     3  0.0820     0.8570 0.016 0.000 0.972 0.012 0.000 0.000
#&gt; GSM559460     1  0.5531    -0.0626 0.452 0.000 0.132 0.000 0.000 0.416
#&gt; GSM559461     3  0.5275     0.3654 0.232 0.000 0.600 0.000 0.000 0.168
#&gt; GSM559462     4  0.0363     0.9767 0.000 0.000 0.000 0.988 0.012 0.000
#&gt; GSM559463     4  0.0405     0.9768 0.008 0.000 0.004 0.988 0.000 0.000
#&gt; GSM559464     3  0.0692     0.8577 0.020 0.000 0.976 0.000 0.000 0.004
#&gt; GSM559465     3  0.0405     0.8569 0.008 0.000 0.988 0.000 0.004 0.000
#&gt; GSM559467     6  0.5434     0.1552 0.148 0.004 0.268 0.000 0.000 0.580
#&gt; GSM559468     3  0.2815     0.7947 0.132 0.004 0.848 0.000 0.004 0.012
#&gt; GSM559469     6  0.0508     0.6377 0.012 0.000 0.000 0.004 0.000 0.984
#&gt; GSM559470     6  0.3975     0.3614 0.392 0.000 0.008 0.000 0.000 0.600
#&gt; GSM559471     3  0.5571     0.0425 0.148 0.000 0.496 0.000 0.000 0.356
#&gt; GSM559472     1  0.5122     0.3291 0.564 0.000 0.004 0.004 0.068 0.360
#&gt; GSM559473     6  0.0000     0.6400 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; GSM559475     6  0.0146     0.6407 0.004 0.000 0.000 0.000 0.000 0.996
#&gt; GSM559477     2  0.0000     0.8225 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559478     2  0.5128     0.6560 0.156 0.652 0.000 0.008 0.000 0.184
#&gt; GSM559479     2  0.0000     0.8225 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.2956     0.8041 0.088 0.848 0.000 0.000 0.000 0.064
#&gt; GSM559481     2  0.4445     0.7427 0.100 0.728 0.000 0.008 0.000 0.164
#&gt; GSM559482     2  0.0000     0.8225 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.0291     0.8574 0.004 0.000 0.992 0.000 0.004 0.000
#&gt; GSM559439     3  0.0508     0.8574 0.012 0.000 0.984 0.000 0.000 0.004
#&gt; GSM559443     3  0.0891     0.8531 0.008 0.000 0.968 0.024 0.000 0.000
#&gt; GSM559447     3  0.0508     0.8574 0.012 0.000 0.984 0.000 0.000 0.004
#&gt; GSM559449     1  0.5092     0.3968 0.660 0.000 0.012 0.000 0.128 0.200
#&gt; GSM559453     3  0.0405     0.8569 0.008 0.000 0.988 0.000 0.004 0.000
#&gt; GSM559466     3  0.3261     0.7382 0.192 0.004 0.792 0.000 0.008 0.004
#&gt; GSM559474     5  0.3629     0.6463 0.276 0.000 0.012 0.000 0.712 0.000
#&gt; GSM559476     5  0.0146     0.6195 0.004 0.000 0.000 0.000 0.996 0.000
#&gt; GSM559483     2  0.0000     0.8225 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559484     2  0.4744     0.7211 0.008 0.716 0.000 0.008 0.108 0.160
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-5-a').click(function(){
  $('#tab-ATC-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-mclust-signature_compare](figure_cola/ATC-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-mclust-collect-classes](figure_cola/ATC-mclust-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>             n disease.state(p) k
#> ATC:mclust 52           0.2329 2
#> ATC:mclust 40           1.0000 3
#> ATC:mclust 26           0.0119 4
#> ATC:mclust 49           0.0116 5
#> ATC:mclust 38           0.0470 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

---------------------------------------------------




### ATC:NMF






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "NMF"]
# you can also extract it by
# res = res_list["ATC:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 21512 rows and 52 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-NMF-collect-plots](figure_cola/ATC-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the _cola_
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-NMF-select-partition-number](figure_cola/ATC-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.662           0.931       0.947         0.2389 0.735   0.735
#> 3 3 0.895           0.931       0.972         0.6512 0.871   0.825
#> 4 4 0.645           0.785       0.904         0.7185 0.701   0.512
#> 5 5 0.568           0.597       0.802         0.0765 0.898   0.695
#> 6 6 0.526           0.484       0.746         0.0445 0.900   0.656
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because increasing
  $k$ does not provide enough extra information. If all $k$ are removed, it is
  marked as no subgroup is detected.
- For all $k$ with 1-PAC score larger than 0.9, the maximal $k$ is taken as
  the best $k$, and other $k$ are marked as optional $k$.
- If it does not fit the second rule. The $k$ with the maximal vote of the
  highest 1-PAC score, highest mean silhouette, and highest concordance is
  taken as the best $k$.

```r
suggest_best_k(res)
```

```
#> [1] 4
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-classes'>
<ul>
<li><a href='#tab-ATC-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-NMF-get-classes-1'>
<p><a id='tab-ATC-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2
#&gt; GSM559433     1  0.0000      0.972 1.000 0.000
#&gt; GSM559434     1  0.0000      0.972 1.000 0.000
#&gt; GSM559436     1  0.3733      0.894 0.928 0.072
#&gt; GSM559437     1  0.6247      0.787 0.844 0.156
#&gt; GSM559438     1  0.0000      0.972 1.000 0.000
#&gt; GSM559440     1  0.0000      0.972 1.000 0.000
#&gt; GSM559441     1  0.0376      0.968 0.996 0.004
#&gt; GSM559442     1  0.0000      0.972 1.000 0.000
#&gt; GSM559444     1  0.0000      0.972 1.000 0.000
#&gt; GSM559445     1  0.0000      0.972 1.000 0.000
#&gt; GSM559446     1  0.0000      0.972 1.000 0.000
#&gt; GSM559448     1  0.0000      0.972 1.000 0.000
#&gt; GSM559450     1  0.0000      0.972 1.000 0.000
#&gt; GSM559451     1  0.0000      0.972 1.000 0.000
#&gt; GSM559452     1  0.0000      0.972 1.000 0.000
#&gt; GSM559454     1  0.0000      0.972 1.000 0.000
#&gt; GSM559455     1  0.0000      0.972 1.000 0.000
#&gt; GSM559456     1  0.0000      0.972 1.000 0.000
#&gt; GSM559457     1  0.0000      0.972 1.000 0.000
#&gt; GSM559458     1  0.0000      0.972 1.000 0.000
#&gt; GSM559459     1  0.7056      0.734 0.808 0.192
#&gt; GSM559460     1  0.0000      0.972 1.000 0.000
#&gt; GSM559461     1  0.0000      0.972 1.000 0.000
#&gt; GSM559462     2  0.9815      0.308 0.420 0.580
#&gt; GSM559463     1  0.7219      0.721 0.800 0.200
#&gt; GSM559464     1  0.0000      0.972 1.000 0.000
#&gt; GSM559465     1  0.0000      0.972 1.000 0.000
#&gt; GSM559467     1  0.0000      0.972 1.000 0.000
#&gt; GSM559468     1  0.0000      0.972 1.000 0.000
#&gt; GSM559469     1  0.0000      0.972 1.000 0.000
#&gt; GSM559470     1  0.0000      0.972 1.000 0.000
#&gt; GSM559471     1  0.0000      0.972 1.000 0.000
#&gt; GSM559472     1  0.0000      0.972 1.000 0.000
#&gt; GSM559473     1  0.0000      0.972 1.000 0.000
#&gt; GSM559475     1  0.0000      0.972 1.000 0.000
#&gt; GSM559477     2  0.7219      0.930 0.200 0.800
#&gt; GSM559478     2  0.7674      0.911 0.224 0.776
#&gt; GSM559479     2  0.7219      0.930 0.200 0.800
#&gt; GSM559480     2  0.7219      0.930 0.200 0.800
#&gt; GSM559481     2  0.7299      0.928 0.204 0.796
#&gt; GSM559482     2  0.7219      0.930 0.200 0.800
#&gt; GSM559435     1  0.0000      0.972 1.000 0.000
#&gt; GSM559439     1  0.0000      0.972 1.000 0.000
#&gt; GSM559443     1  0.6438      0.775 0.836 0.164
#&gt; GSM559447     1  0.0000      0.972 1.000 0.000
#&gt; GSM559449     1  0.0000      0.972 1.000 0.000
#&gt; GSM559453     1  0.0000      0.972 1.000 0.000
#&gt; GSM559466     1  0.0000      0.972 1.000 0.000
#&gt; GSM559474     1  0.0000      0.972 1.000 0.000
#&gt; GSM559476     1  0.0000      0.972 1.000 0.000
#&gt; GSM559483     2  0.7219      0.930 0.200 0.800
#&gt; GSM559484     1  0.5629      0.787 0.868 0.132
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-1-a').click(function(){
  $('#tab-ATC-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-2'>
<p><a id='tab-ATC-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3
#&gt; GSM559433     1  0.0237      0.974 0.996 0.004 0.000
#&gt; GSM559434     1  0.0000      0.975 1.000 0.000 0.000
#&gt; GSM559436     1  0.4654      0.724 0.792 0.000 0.208
#&gt; GSM559437     3  0.5178      0.616 0.256 0.000 0.744
#&gt; GSM559438     1  0.0000      0.975 1.000 0.000 0.000
#&gt; GSM559440     1  0.0000      0.975 1.000 0.000 0.000
#&gt; GSM559441     1  0.0000      0.975 1.000 0.000 0.000
#&gt; GSM559442     1  0.5680      0.679 0.764 0.024 0.212
#&gt; GSM559444     1  0.1411      0.943 0.964 0.036 0.000
#&gt; GSM559445     1  0.0237      0.974 0.996 0.004 0.000
#&gt; GSM559446     1  0.0000      0.975 1.000 0.000 0.000
#&gt; GSM559448     1  0.0000      0.975 1.000 0.000 0.000
#&gt; GSM559450     1  0.0747      0.963 0.984 0.016 0.000
#&gt; GSM559451     1  0.0000      0.975 1.000 0.000 0.000
#&gt; GSM559452     1  0.0000      0.975 1.000 0.000 0.000
#&gt; GSM559454     1  0.0000      0.975 1.000 0.000 0.000
#&gt; GSM559455     1  0.0000      0.975 1.000 0.000 0.000
#&gt; GSM559456     1  0.0237      0.974 0.996 0.004 0.000
#&gt; GSM559457     1  0.0237      0.974 0.996 0.004 0.000
#&gt; GSM559458     1  0.0237      0.974 0.996 0.004 0.000
#&gt; GSM559459     3  0.2796      0.704 0.000 0.092 0.908
#&gt; GSM559460     1  0.0000      0.975 1.000 0.000 0.000
#&gt; GSM559461     1  0.0000      0.975 1.000 0.000 0.000
#&gt; GSM559462     3  0.0000      0.750 0.000 0.000 1.000
#&gt; GSM559463     3  0.0000      0.750 0.000 0.000 1.000
#&gt; GSM559464     1  0.3482      0.838 0.872 0.000 0.128
#&gt; GSM559465     1  0.0237      0.974 0.996 0.004 0.000
#&gt; GSM559467     1  0.0000      0.975 1.000 0.000 0.000
#&gt; GSM559468     1  0.0237      0.974 0.996 0.004 0.000
#&gt; GSM559469     1  0.0000      0.975 1.000 0.000 0.000
#&gt; GSM559470     1  0.0000      0.975 1.000 0.000 0.000
#&gt; GSM559471     1  0.0000      0.975 1.000 0.000 0.000
#&gt; GSM559472     1  0.0237      0.974 0.996 0.004 0.000
#&gt; GSM559473     1  0.0000      0.975 1.000 0.000 0.000
#&gt; GSM559475     1  0.0000      0.975 1.000 0.000 0.000
#&gt; GSM559477     2  0.0237      0.985 0.004 0.996 0.000
#&gt; GSM559478     2  0.1529      0.927 0.040 0.960 0.000
#&gt; GSM559479     2  0.0237      0.985 0.004 0.996 0.000
#&gt; GSM559480     2  0.0237      0.985 0.004 0.996 0.000
#&gt; GSM559481     2  0.0592      0.976 0.012 0.988 0.000
#&gt; GSM559482     2  0.0237      0.985 0.004 0.996 0.000
#&gt; GSM559435     1  0.0000      0.975 1.000 0.000 0.000
#&gt; GSM559439     1  0.0000      0.975 1.000 0.000 0.000
#&gt; GSM559443     3  0.4504      0.687 0.196 0.000 0.804
#&gt; GSM559447     1  0.0000      0.975 1.000 0.000 0.000
#&gt; GSM559449     1  0.0237      0.974 0.996 0.004 0.000
#&gt; GSM559453     1  0.0000      0.975 1.000 0.000 0.000
#&gt; GSM559466     1  0.0237      0.974 0.996 0.004 0.000
#&gt; GSM559474     1  0.0237      0.974 0.996 0.004 0.000
#&gt; GSM559476     1  0.0237      0.974 0.996 0.004 0.000
#&gt; GSM559483     2  0.0237      0.985 0.004 0.996 0.000
#&gt; GSM559484     1  0.4346      0.758 0.816 0.184 0.000
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-2-a').click(function(){
  $('#tab-ATC-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-3'>
<p><a id='tab-ATC-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4
#&gt; GSM559433     3  0.2345      0.843 0.100 0.000 0.900 0.000
#&gt; GSM559434     1  0.2704      0.785 0.876 0.000 0.124 0.000
#&gt; GSM559436     1  0.3569      0.680 0.804 0.000 0.000 0.196
#&gt; GSM559437     4  0.2345      0.846 0.100 0.000 0.000 0.900
#&gt; GSM559438     1  0.0000      0.832 1.000 0.000 0.000 0.000
#&gt; GSM559440     1  0.0000      0.832 1.000 0.000 0.000 0.000
#&gt; GSM559441     3  0.5558      0.389 0.364 0.000 0.608 0.028
#&gt; GSM559442     1  0.0469      0.824 0.988 0.000 0.000 0.012
#&gt; GSM559444     1  0.0000      0.832 1.000 0.000 0.000 0.000
#&gt; GSM559445     3  0.2149      0.852 0.088 0.000 0.912 0.000
#&gt; GSM559446     1  0.1302      0.825 0.956 0.000 0.044 0.000
#&gt; GSM559448     1  0.4985      0.121 0.532 0.000 0.468 0.000
#&gt; GSM559450     1  0.0000      0.832 1.000 0.000 0.000 0.000
#&gt; GSM559451     1  0.0000      0.832 1.000 0.000 0.000 0.000
#&gt; GSM559452     1  0.1118      0.827 0.964 0.000 0.036 0.000
#&gt; GSM559454     1  0.0000      0.832 1.000 0.000 0.000 0.000
#&gt; GSM559455     1  0.3024      0.772 0.852 0.000 0.148 0.000
#&gt; GSM559456     3  0.0000      0.869 0.000 0.000 1.000 0.000
#&gt; GSM559457     3  0.0592      0.875 0.016 0.000 0.984 0.000
#&gt; GSM559458     3  0.0336      0.874 0.008 0.000 0.992 0.000
#&gt; GSM559459     4  0.3837      0.721 0.224 0.000 0.000 0.776
#&gt; GSM559460     1  0.0188      0.832 0.996 0.000 0.004 0.000
#&gt; GSM559461     1  0.0000      0.832 1.000 0.000 0.000 0.000
#&gt; GSM559462     4  0.0000      0.883 0.000 0.000 0.000 1.000
#&gt; GSM559463     4  0.0000      0.883 0.000 0.000 0.000 1.000
#&gt; GSM559464     1  0.4382      0.522 0.704 0.000 0.000 0.296
#&gt; GSM559465     3  0.1118      0.872 0.036 0.000 0.964 0.000
#&gt; GSM559467     1  0.4382      0.588 0.704 0.000 0.296 0.000
#&gt; GSM559468     3  0.0336      0.874 0.008 0.000 0.992 0.000
#&gt; GSM559469     3  0.3873      0.709 0.228 0.000 0.772 0.000
#&gt; GSM559470     3  0.4103      0.667 0.256 0.000 0.744 0.000
#&gt; GSM559471     1  0.0336      0.832 0.992 0.000 0.008 0.000
#&gt; GSM559472     3  0.0707      0.875 0.020 0.000 0.980 0.000
#&gt; GSM559473     1  0.3123      0.757 0.844 0.000 0.156 0.000
#&gt; GSM559475     1  0.4843      0.338 0.604 0.000 0.396 0.000
#&gt; GSM559477     2  0.0000      0.980 0.000 1.000 0.000 0.000
#&gt; GSM559478     2  0.1867      0.885 0.072 0.928 0.000 0.000
#&gt; GSM559479     2  0.0000      0.980 0.000 1.000 0.000 0.000
#&gt; GSM559480     2  0.0000      0.980 0.000 1.000 0.000 0.000
#&gt; GSM559481     2  0.0188      0.977 0.004 0.996 0.000 0.000
#&gt; GSM559482     2  0.0000      0.980 0.000 1.000 0.000 0.000
#&gt; GSM559435     3  0.4697      0.440 0.356 0.000 0.644 0.000
#&gt; GSM559439     1  0.3837      0.689 0.776 0.000 0.224 0.000
#&gt; GSM559443     4  0.0188      0.882 0.000 0.000 0.004 0.996
#&gt; GSM559447     1  0.4730      0.430 0.636 0.000 0.364 0.000
#&gt; GSM559449     3  0.0188      0.873 0.004 0.000 0.996 0.000
#&gt; GSM559453     3  0.3266      0.778 0.168 0.000 0.832 0.000
#&gt; GSM559466     3  0.0188      0.873 0.004 0.000 0.996 0.000
#&gt; GSM559474     3  0.0188      0.873 0.004 0.000 0.996 0.000
#&gt; GSM559476     3  0.0188      0.873 0.004 0.000 0.996 0.000
#&gt; GSM559483     2  0.0000      0.980 0.000 1.000 0.000 0.000
#&gt; GSM559484     3  0.2654      0.804 0.004 0.108 0.888 0.000
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-3-a').click(function(){
  $('#tab-ATC-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-4'>
<p><a id='tab-ATC-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5
#&gt; GSM559433     1  0.4087      0.696 0.756 0.000 0.208 0.036 0.000
#&gt; GSM559434     4  0.2395      0.759 0.072 0.000 0.016 0.904 0.008
#&gt; GSM559436     3  0.5045      0.163 0.004 0.000 0.508 0.464 0.024
#&gt; GSM559437     3  0.1547      0.361 0.004 0.000 0.948 0.016 0.032
#&gt; GSM559438     4  0.1041      0.777 0.000 0.000 0.032 0.964 0.004
#&gt; GSM559440     4  0.0771      0.781 0.000 0.000 0.020 0.976 0.004
#&gt; GSM559441     3  0.5394      0.432 0.208 0.000 0.660 0.132 0.000
#&gt; GSM559442     4  0.4125      0.622 0.000 0.000 0.172 0.772 0.056
#&gt; GSM559444     4  0.1197      0.770 0.000 0.000 0.048 0.952 0.000
#&gt; GSM559445     1  0.5523      0.416 0.564 0.000 0.368 0.064 0.004
#&gt; GSM559446     4  0.4819      0.232 0.024 0.000 0.352 0.620 0.004
#&gt; GSM559448     4  0.4947      0.287 0.396 0.000 0.024 0.576 0.004
#&gt; GSM559450     4  0.0960      0.781 0.004 0.000 0.008 0.972 0.016
#&gt; GSM559451     4  0.0566      0.783 0.000 0.000 0.012 0.984 0.004
#&gt; GSM559452     4  0.1686      0.782 0.020 0.000 0.028 0.944 0.008
#&gt; GSM559454     4  0.0693      0.786 0.012 0.000 0.008 0.980 0.000
#&gt; GSM559455     4  0.6033      0.141 0.132 0.000 0.296 0.568 0.004
#&gt; GSM559456     1  0.3684      0.651 0.720 0.000 0.280 0.000 0.000
#&gt; GSM559457     1  0.2006      0.730 0.916 0.000 0.072 0.012 0.000
#&gt; GSM559458     1  0.3163      0.725 0.824 0.000 0.164 0.012 0.000
#&gt; GSM559459     5  0.4576      0.491 0.000 0.000 0.040 0.268 0.692
#&gt; GSM559460     4  0.1661      0.780 0.024 0.000 0.036 0.940 0.000
#&gt; GSM559461     4  0.0932      0.785 0.004 0.000 0.020 0.972 0.004
#&gt; GSM559462     5  0.2011      0.537 0.000 0.004 0.088 0.000 0.908
#&gt; GSM559463     3  0.4262     -0.393 0.000 0.000 0.560 0.000 0.440
#&gt; GSM559464     4  0.5523      0.279 0.000 0.000 0.088 0.592 0.320
#&gt; GSM559465     1  0.4063      0.645 0.708 0.000 0.280 0.012 0.000
#&gt; GSM559467     4  0.4217      0.618 0.232 0.000 0.008 0.740 0.020
#&gt; GSM559468     1  0.1059      0.710 0.968 0.000 0.020 0.008 0.004
#&gt; GSM559469     1  0.4697      0.325 0.620 0.000 0.008 0.360 0.012
#&gt; GSM559470     1  0.5013      0.360 0.612 0.000 0.028 0.352 0.008
#&gt; GSM559471     4  0.1948      0.771 0.024 0.000 0.008 0.932 0.036
#&gt; GSM559472     1  0.1628      0.678 0.936 0.000 0.008 0.056 0.000
#&gt; GSM559473     4  0.3320      0.719 0.124 0.000 0.016 0.844 0.016
#&gt; GSM559475     4  0.4524      0.535 0.280 0.000 0.008 0.692 0.020
#&gt; GSM559477     2  0.0162      0.976 0.000 0.996 0.000 0.000 0.004
#&gt; GSM559478     2  0.1544      0.880 0.000 0.932 0.000 0.068 0.000
#&gt; GSM559479     2  0.0000      0.977 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559480     2  0.0000      0.977 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559481     2  0.0290      0.971 0.000 0.992 0.000 0.008 0.000
#&gt; GSM559482     2  0.0000      0.977 0.000 1.000 0.000 0.000 0.000
#&gt; GSM559435     3  0.6439      0.212 0.332 0.000 0.476 0.192 0.000
#&gt; GSM559439     3  0.5028      0.219 0.032 0.000 0.524 0.444 0.000
#&gt; GSM559443     3  0.1996      0.356 0.036 0.000 0.928 0.004 0.032
#&gt; GSM559447     3  0.5122      0.464 0.060 0.000 0.628 0.312 0.000
#&gt; GSM559449     1  0.3421      0.712 0.788 0.000 0.204 0.008 0.000
#&gt; GSM559453     3  0.5268      0.199 0.320 0.000 0.612 0.068 0.000
#&gt; GSM559466     1  0.3305      0.700 0.776 0.000 0.224 0.000 0.000
#&gt; GSM559474     1  0.1892      0.725 0.916 0.000 0.080 0.000 0.004
#&gt; GSM559476     1  0.1012      0.693 0.968 0.000 0.012 0.000 0.020
#&gt; GSM559483     2  0.0162      0.976 0.000 0.996 0.000 0.000 0.004
#&gt; GSM559484     1  0.5202      0.533 0.696 0.240 0.016 0.032 0.016
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-4-a').click(function(){
  $('#tab-ATC-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-5'>
<p><a id='tab-ATC-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;           class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; GSM559433     1   0.547     0.1427 0.484 0.000 0.432 0.000 0.036 0.048
#&gt; GSM559434     6   0.352     0.6973 0.104 0.000 0.048 0.000 0.024 0.824
#&gt; GSM559436     3   0.490     0.3973 0.004 0.000 0.684 0.004 0.136 0.172
#&gt; GSM559437     3   0.228     0.3809 0.000 0.000 0.868 0.000 0.128 0.004
#&gt; GSM559438     6   0.169     0.6809 0.000 0.000 0.064 0.000 0.012 0.924
#&gt; GSM559440     6   0.180     0.6784 0.000 0.000 0.072 0.000 0.012 0.916
#&gt; GSM559441     3   0.382     0.5147 0.140 0.000 0.792 0.000 0.048 0.020
#&gt; GSM559442     6   0.580     0.0964 0.000 0.000 0.312 0.004 0.180 0.504
#&gt; GSM559444     6   0.325     0.5976 0.000 0.000 0.156 0.000 0.036 0.808
#&gt; GSM559445     3   0.555     0.0677 0.380 0.000 0.524 0.000 0.036 0.060
#&gt; GSM559446     3   0.565     0.2378 0.060 0.000 0.524 0.000 0.044 0.372
#&gt; GSM559448     6   0.639     0.1720 0.352 0.000 0.156 0.000 0.040 0.452
#&gt; GSM559450     6   0.108     0.6957 0.008 0.000 0.016 0.000 0.012 0.964
#&gt; GSM559451     6   0.231     0.6945 0.012 0.000 0.064 0.000 0.024 0.900
#&gt; GSM559452     6   0.189     0.7131 0.020 0.000 0.056 0.000 0.004 0.920
#&gt; GSM559454     6   0.245     0.7127 0.048 0.000 0.040 0.000 0.016 0.896
#&gt; GSM559455     6   0.608    -0.0502 0.148 0.000 0.400 0.000 0.020 0.432
#&gt; GSM559456     1   0.463     0.2132 0.520 0.000 0.440 0.000 0.040 0.000
#&gt; GSM559457     1   0.405     0.5864 0.768 0.000 0.164 0.000 0.024 0.044
#&gt; GSM559458     1   0.402     0.5542 0.720 0.000 0.244 0.000 0.008 0.028
#&gt; GSM559459     4   0.734     0.1553 0.000 0.000 0.212 0.408 0.144 0.236
#&gt; GSM559460     6   0.442     0.6456 0.080 0.000 0.148 0.000 0.024 0.748
#&gt; GSM559461     6   0.402     0.6501 0.032 0.000 0.144 0.000 0.044 0.780
#&gt; GSM559462     4   0.000    -0.0153 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; GSM559463     5   0.495     0.0000 0.000 0.000 0.244 0.120 0.636 0.000
#&gt; GSM559464     3   0.763    -0.1338 0.020 0.000 0.360 0.136 0.148 0.336
#&gt; GSM559465     3   0.487    -0.1657 0.468 0.000 0.488 0.000 0.028 0.016
#&gt; GSM559467     6   0.431     0.6410 0.184 0.000 0.012 0.000 0.068 0.736
#&gt; GSM559468     1   0.314     0.5968 0.856 0.000 0.060 0.000 0.056 0.028
#&gt; GSM559469     6   0.499     0.2025 0.472 0.000 0.016 0.000 0.036 0.476
#&gt; GSM559470     1   0.582    -0.0285 0.484 0.000 0.104 0.000 0.024 0.388
#&gt; GSM559471     6   0.186     0.6799 0.016 0.000 0.004 0.000 0.060 0.920
#&gt; GSM559472     1   0.344     0.5493 0.828 0.000 0.036 0.000 0.028 0.108
#&gt; GSM559473     6   0.352     0.6763 0.136 0.000 0.024 0.000 0.028 0.812
#&gt; GSM559475     6   0.449     0.6095 0.228 0.000 0.016 0.000 0.052 0.704
#&gt; GSM559477     2   0.000     0.9745 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559478     2   0.161     0.8633 0.000 0.916 0.000 0.000 0.000 0.084
#&gt; GSM559479     2   0.000     0.9745 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559480     2   0.052     0.9636 0.000 0.984 0.000 0.000 0.008 0.008
#&gt; GSM559481     2   0.000     0.9745 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559482     2   0.000     0.9745 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559435     3   0.452     0.4833 0.196 0.000 0.720 0.000 0.020 0.064
#&gt; GSM559439     3   0.397     0.5433 0.044 0.000 0.772 0.000 0.020 0.164
#&gt; GSM559443     3   0.417     0.2537 0.056 0.000 0.736 0.008 0.200 0.000
#&gt; GSM559447     3   0.293     0.5660 0.040 0.000 0.860 0.000 0.012 0.088
#&gt; GSM559449     1   0.401     0.5383 0.704 0.000 0.268 0.000 0.016 0.012
#&gt; GSM559453     3   0.446     0.4748 0.160 0.000 0.736 0.000 0.088 0.016
#&gt; GSM559466     1   0.421     0.4444 0.636 0.000 0.336 0.000 0.028 0.000
#&gt; GSM559474     1   0.366     0.5665 0.800 0.000 0.100 0.000 0.096 0.004
#&gt; GSM559476     1   0.292     0.4922 0.828 0.000 0.008 0.000 0.156 0.008
#&gt; GSM559483     2   0.000     0.9745 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; GSM559484     1   0.628     0.2827 0.548 0.276 0.004 0.000 0.104 0.068
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-5-a').click(function(){
  $('#tab-ATC-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-NMF-signature_compare](figure_cola/ATC-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-NMF-collect-classes](figure_cola/ATC-NMF-collect-classes-1.png)



Test correlation between subgroups and known annotations. If the known
annotation is numeric, one-way ANOVA test is applied, and if the known
annotation is discrete, chi-squared contingency table test is applied.

```r
test_to_known_factors(res)
```

```
#>          n disease.state(p) k
#> ATC:NMF 51           0.9923 2
#> ATC:NMF 52           0.8845 3
#> ATC:NMF 47           0.1138 4
#> ATC:NMF 35           0.0379 5
#> ATC:NMF 30           0.0241 6
```



If matrix rows can be associated to genes, consider to use `functional_enrichment(res,
...)` to perform function enrichment for the signature genes. See [this vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/functional_enrichment.html) for more detailed explanations.


 

## Session info


```r
sessionInfo()
```

```
#> R version 3.6.0 (2019-04-26)
#> Platform: x86_64-pc-linux-gnu (64-bit)
#> Running under: CentOS Linux 7 (Core)
#> 
#> Matrix products: default
#> BLAS:   /usr/lib64/libblas.so.3.4.2
#> LAPACK: /usr/lib64/liblapack.so.3.4.2
#> 
#> locale:
#>  [1] LC_CTYPE=en_GB.UTF-8       LC_NUMERIC=C               LC_TIME=en_GB.UTF-8       
#>  [4] LC_COLLATE=en_GB.UTF-8     LC_MONETARY=en_GB.UTF-8    LC_MESSAGES=en_GB.UTF-8   
#>  [7] LC_PAPER=en_GB.UTF-8       LC_NAME=C                  LC_ADDRESS=C              
#> [10] LC_TELEPHONE=C             LC_MEASUREMENT=en_GB.UTF-8 LC_IDENTIFICATION=C       
#> 
#> attached base packages:
#> [1] grid      stats     graphics  grDevices utils     datasets  methods   base     
#> 
#> other attached packages:
#> [1] genefilter_1.66.0    ComplexHeatmap_2.3.1 markdown_1.1         knitr_1.26          
#> [5] GetoptLong_0.1.7     cola_1.3.2          
#> 
#> loaded via a namespace (and not attached):
#>  [1] circlize_0.4.8       shape_1.4.4          xfun_0.11            slam_0.1-46         
#>  [5] lattice_0.20-38      splines_3.6.0        colorspace_1.4-1     vctrs_0.2.0         
#>  [9] stats4_3.6.0         blob_1.2.0           XML_3.98-1.20        survival_2.44-1.1   
#> [13] rlang_0.4.2          pillar_1.4.2         DBI_1.0.0            BiocGenerics_0.30.0 
#> [17] bit64_0.9-7          RColorBrewer_1.1-2   matrixStats_0.55.0   stringr_1.4.0       
#> [21] GlobalOptions_0.1.1  evaluate_0.14        memoise_1.1.0        Biobase_2.44.0      
#> [25] IRanges_2.18.3       parallel_3.6.0       AnnotationDbi_1.46.1 highr_0.8           
#> [29] Rcpp_1.0.3           xtable_1.8-4         backports_1.1.5      S4Vectors_0.22.1    
#> [33] annotate_1.62.0      skmeans_0.2-11       bit_1.1-14           microbenchmark_1.4-7
#> [37] brew_1.0-6           impute_1.58.0        rjson_0.2.20         png_0.1-7           
#> [41] digest_0.6.23        stringi_1.4.3        polyclip_1.10-0      clue_0.3-57         
#> [45] tools_3.6.0          bitops_1.0-6         magrittr_1.5         eulerr_6.0.0        
#> [49] RCurl_1.95-4.12      RSQLite_2.1.4        tibble_2.1.3         cluster_2.1.0       
#> [53] crayon_1.3.4         pkgconfig_2.0.3      zeallot_0.1.0        Matrix_1.2-17       
#> [57] xml2_1.2.2           httr_1.4.1           R6_2.4.1             mclust_5.4.5        
#> [61] compiler_3.6.0
```


