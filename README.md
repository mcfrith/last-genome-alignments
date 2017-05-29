# last-genome-alignments

Here are some pair-wise genome alignments made with
[LAST](http://last.cbrc.jp/).

## 2017 human-ape alignments

The human genome (hg38) was aligned to chimp (panTro5) and gorilla
(gorGor5), as follows.  This alignment recipe is very
accurate-but-slow.  A faster recipe would mask repeats during
alignment, and/or omit `-m50`.

First, an "index" of the human genome was prepared, suitable for
comparing it to highly-similar sequences:

    lastdb -P0 -uNEAR -R01 hg38-NEAR hg38_no_alt_analysis_set.fa

Then, substitution and gap frequencies were determined:

    last-train -P0 --revsym --matsym --gapsym -E0.05 -C2 hg38-NEAR panTro5.fa > hg38-panTro5.mat

* Human-chimp parameters:
  [hg38-panTro5.mat](http://last.cbrc.jp/genome-alignments/hg38-panTro5.mat)
* Human-gorilla parameters:
  [hg38-gorGor5.mat](http://last.cbrc.jp/genome-alignments/hg38-gorGor5.mat)

Next, many-to-one ape-to-human alignments were made:

    lastal -m50 -E0.05 -C2 -p hg38-panTro5.mat hg38-NEAR panTro5.fa | last-split -m1 > hg38-panTro5-1.maf

The above command was the slowest step (3 CPU-weeks).  You can
"easily" parallelize it, by processing each sequence within panTro5.fa
separately (in parallel).  But each process uses quite a lot of
memory, so take care that multiple parallel runs don't exceed your
memory.

* Human-chimp many-to-one alignments:
  [hg38-panTro5-1.maf.gz](http://last.cbrc.jp/genome-alignments/hg38-panTro5-1.maf.gz)
* Human-gorilla many-to-one alignments:
  [hg38-gorGor5-1.maf.gz](http://last.cbrc.jp/genome-alignments/hg38-panTro5-1.maf.gz)

Next, one-to-one ape-to-human alignments were made:

    maf-swap hg38-panTro5-1.maf |
    awk '/^s/ {$2 = (++s % 2 ? "panTro5." : "hg38.") $2} 1' |
    last-split -m1 |
    maf-swap > hg38-panTro5-2.maf

The awk command prepends the assembly name to each chromosome name
(e.g. chr7 -> hg38.chr7).

Finally, simple-sequence alignments were discarded, the alignments
were converted to tabular format, and alignments with error
probability > 10^-5 were discarded:

    last-postmask hg38-panTro5-2.maf |
    maf-convert -n tab |
    awk -F'=' '$2 <= 1e-5' > hg38-panTro5.tab

* Human-chimp tabular alignments:
  [hg38-panTro5.tab.gz](http://last.cbrc.jp/genome-alignments/hg38-panTro5.tab.gz)
* Human-gorilla tabular alignments:
  [hg38-gorGor5.tab.gz](http://last.cbrc.jp/genome-alignments/hg38-gorGor5.tab.gz)

## 2017 human-mammal parameters

Alignment parameters were determined for the human genome (hg38)
versus mouse (mm10) and dog (canFam3).

First, an "index" of the human genome was prepared, suitable for
comparing it to less-similar sequences:

    lastdb -P0 -uMAM4 -R01 hg38-MAM4 hg38_no_alt_analysis_set.fa

Then, substitution and gap frequencies were determined:

    last-train -P0 --revsym --matsym --gapsym -E0.05 -C2 hg38-MAM4 mm10.fa > hg38-mm10.mat

* Human-mouse parameters:
  [hg38-mm10.mat](http://last.cbrc.jp/genome-alignments/hg38-mm10.mat)
* Human-dog parameters:
  [hg38-canFam3.mat](http://last.cbrc.jp/genome-alignments/hg38-canFam3.mat)
