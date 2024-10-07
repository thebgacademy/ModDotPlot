# ModDotPlot tutorial

Get a "Regular" instance: 4 cores, 8 G RAM, 30 GB Storage

## Learning goals

 - Understand main ModDotPlot parameters
 - Learn how to interpret results
 - Learn how to customize plots to create publication quality figures

 ## Where's the data?

 - Included in the GitPod when launched
 - Can be downloaded from https://cs.jhu.edu/~asweeten/bga24/bga24_data.tar.gz 

## Generating a basic plot

Once GitPod has finished downloading external dependencies and data, make sure to verify that ModDotPlot has installed correctly.
```bash
moddotplot -h
```
ModDotPlot can run either in **static** mode which produces simple image files, or **interactive** mode which launches an interactive web browser. For now, let's produce some static plots! (Don't worry about understanding the parameters yet, we'll go over them later)
```bash
 moddotplot static -f bga24_data/acro_short_arms/*.fa -o short_arms --compare --grid
```

 This command should take ~10 minutes to run. While its running, we'll go through the [presentation](moddotplot.pdf) explaining how ModDotPlot works and how to interpret the plots it produces.

### Let’s take a look at the outputs
```bash
ll short_arms/
---------------
seq1.bed    seq2.bed    seq3.bed
seq1_FULL.pdf   seq2_FULL.pdf   seq3_FULL.pdf
seq1_FULL.png   seq2_FULL.png   seq3_FULL.png
seq1_TRI.pdf   seq2_TRI.pdf   seq3_TRI.pdf
seq1_TRI.png    seq2_TRI.png   seq3_TRI.png
seq1_HIST.pdf   seq2_HIST.pdf   seq3_HIST.pdf
seq1_HIST.png   seq2_HIST.png   seq3_HIST.png
seq1_seq2_COMPARE.bed   seq2_seq3_COMPARE.bed
seq1_seq2_COMPARE.pdf   seq2_seq3_COMPARE.pdf
seq1_seq2_COMPARE.png   seq2_seq3_COMPARE.png
seq1_seq3_COMPARE.bed
seq1_seq3_COMPARE.pdf
seq1_seq3_COMPARE.png
```
#### Some of the important outputs:
<dl>
<dt>seq*_FULL*</dt>
<dd>Standard dotplot output. Will produce a pdf and a png.</dd>
<dt>seq*_TRI*</dt>
<dd>Upper triangle portion of _FULL. Used for self identity plots</dd>
<dt>seq*_HIST*</dt>
<dd>A histogram of values, colors partioned at each boundary site.</dd>
<dt>seq*.bed</dt>
<dd>Paired-end bedfile, with raw Average Nucleotide Identity values for each pairwise set of intervals </dd>
<dt>*_COMPARE*</dt>
<dd>For each pairwise combination of sequences. Only shown when multiple inputs are used with the `--compare` or `--compare-only` flag.</dd>
<dt>*_GRID*</dt>
<dd>An N x N grid of plots, with self-identity on the diagonal, and matching comparative plots orthogonal. Only produced with `--compare` and </dd>
</dl>

Sequences 1, 2, and 3 represent the short arms of Chr13, Chr14, and Chr21. Can you figure out which of these sequences is Chr14? Hint: It contains a inversion in the comparative dotplot, relative to 13 and 21!

### Exploring rDNA missassembly

rDNA is, arguably, one of the most difficult regions of the human genome to assemble. This is due to their highly repetitive tandem repeats often spanning megabases in length.

```bash
 moddotplot static -f bga24_data/acro_short_arms/*.fa -o short_arms --compare --grid
```

Unlike with the short arms, this command should run in under a minute! One of these haplotypes contains a missassembly. Can you figure out which one?

### Using interactive mode

ModDotPlot is able to produce a hierarchy of matrices, thanks to the hierarchical sketching approach it uses. These matrices can be saved using the `--save` command when running ModDotPlot in interactive mode, and accessed without expensive re-computation using `-l/--load`. Let's open a pre-computed view of an Arabadopsis chromosome:
```bash
moddotplot interactive --load data/arabadopsis/interactive_matrices
```
The above command launches an application on your machine's localhost, on port `8050` (this can be changed using the `--port` command). To view, simply go to your web browser of choice and `localhost:8050`. This might not be possible if you're running ModDotPlot on an HPC environment. Fortunately for our purposes, VSCode and GitPod support automatic port forwarding! This creates an [SSH tunnel](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling) between GitPod and your local machine, allowing you to view `localhost:8050`.

Note: Expect some latency when port forwarding! Using localhost on your own machine is the fastest way to explore ModDotPlot's interactive mode!

Playing around with Chromosome 2 of the Arabadopsis Col-Cen reference genome, we can see that we can enhance our plots resolution pretty significantly!

### Comparative X Centromere

As our final dataset for the day, we want to look at the centromeres of two human Chromosome X's: One from [CHM13](https://github.com/marbl/CHM13), the other from [HG002](https://github.com/marbl/HG002). 

```bash
moddotplot interactive -f bga24_data/cenX/*.fa --quick --compare
```

Adding `--quick` prevents the creation of hierarchical matrices. While this loses out in interactivity, this is nice to quickly explore a genome & ply with other paramaters.

#### Issues

For troubleshooting, bug reports, or general questions, please message me on the BGA24 Discord's #moddotplot-2024 channel, or anytime afterwards at alex ~dot~ sweeten ~at~ nih ~dot~ gov. Thanks for using ModDotPlot! :)
