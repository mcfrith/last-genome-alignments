# last-genome-alignments

Here are some pair-wise genome alignments made with
[LAST](http://last.cbrc.jp/).

## 2017 human-ape alignments

The human genome (hg38) was aligned to chimp (panTro5) and gorilla
(gorGor5), as follows.  This alignment recipe is very
accurate-but-slow.  A faster recipe would [mask repeats during
alignment](https://github.com/mcfrith/last-rna/blob/master/last-long-reads.md),
and/or omit `-m50`.

First, an "index" of the human genome was prepared, suitable for
comparing it to highly-similar sequences:

    lastdb -P0 -uNEAR -R01 hg38-NEAR hg38_no_alt_analysis_set.fa

Then, substitution and gap frequencies were determined:

    last-train -P0 --revsym --matsym --gapsym -E0.05 -C2 hg38-NEAR panTro5.fa > hg38-panTro5.mat

* Human-chimp parameters:
  [hg38-panTro5.mat](https://drive.google.com/open?id=0Bw_yRzJW8ZA_MDNYR0ptQkYyZGc)
* Human-gorilla parameters:
  [hg38-gorGor5.mat](https://drive.google.com/open?id=0Bw_yRzJW8ZA_Q3pIOTlVVjZ3bzA)

Next, many-to-one ape-to-human alignments were made:

    lastal -m50 -E0.05 -C2 -p hg38-panTro5.mat hg38-NEAR panTro5.fa | last-split -m1 > hg38-panTro5-1.maf

The above command was the slowest step (3 CPU-weeks).  You can
"easily" parallelize it, by processing each sequence within panTro5.fa
separately (in parallel).  But each process uses quite a lot of
memory, so take care that multiple parallel runs don't exceed your
memory.

* Human-chimp many-to-one alignments:
  [hg38-panTro5-1.maf.gz](https://drive.google.com/open?id=0Bw_yRzJW8ZA_dGNiOTBzU1NTcE0)
* Human-gorilla many-to-one alignments:
  [hg38-gorGor5-1.maf.gz](https://drive.google.com/open?id=0Bw_yRzJW8ZA_SVNabUhiZmJiamM)

Next, one-to-one ape-to-human alignments were made:

    maf-swap hg38-panTro5-1.maf |
    awk '/^s/ {$2 = (++s % 2 ? "panTro5." : "hg38.") $2} 1' |
    last-split -m1 |
    maf-swap > hg38-panTro5-2.maf

The awk command prepends the assembly name to each chromosome name
(e.g. chr7 -> hg38.chr7).

* Human-chimp one-to-one alignments:
  [hg38-panTro5-2.maf.gz](https://drive.google.com/open?id=0Bw_yRzJW8ZA_aGNZWUdBLUFLNWs)
* Human-gorilla one-to-one alignments:
  [hg38-gorGor5-2.maf.gz](https://drive.google.com/open?id=0Bw_yRzJW8ZA_VXVIdk8ycm5GZFk)

Finally, simple-sequence alignments were discarded, the alignments
were converted to tabular format, and alignments with error
probability > 10^-5 were discarded:

    last-postmask hg38-panTro5-2.maf |
    maf-convert -n tab |
    awk -F'=' '$2 <= 1e-5' > hg38-panTro5.tab

* Human-chimp tabular alignments:
  [hg38-panTro5.tab.gz](https://drive.google.com/open?id=0Bw_yRzJW8ZA_bEFySHpLQ1BiMnc)
  ([dotplot](https://drive.google.com/open?id=0Bw_yRzJW8ZA_Z21ZbGpXelo3Z0E))
* Human-gorilla tabular alignments:
  [hg38-gorGor5.tab.gz](https://drive.google.com/open?id=0Bw_yRzJW8ZA_V0Z2RkhncEZINzQ)
  ([dotplot](https://drive.google.com/open?id=0Bw_yRzJW8ZA_eVU2a1RHM1VIdUE))

## 2017 human-mouse alignments

The human genome (hg38) was aligned to mouse (mm10).  This alignment
recipe is even more slow-and-sensitive.

First, an "index" of the human genome was prepared, suitable for
comparing it to less-similar sequences:

    lastdb -P0 -uMAM4 -R01 hg38-MAM4 hg38_no_alt_analysis_set.fa

Then, substitution and gap frequencies were determined:

    last-train -P0 --revsym --matsym --gapsym -E0.05 -C2 hg38-MAM4 mm10.fa > hg38-mm10.mat

* Human-mouse parameters:
  [hg38-mm10.mat](https://drive.google.com/open?id=0Bw_yRzJW8ZA_WUxrcVZRSlZoWW8)
* Human-dog parameters:
  [hg38-canFam3.mat](https://drive.google.com/open?id=0Bw_yRzJW8ZA_eG9TOEtMbWswVkU)

Next, many-to-one mouse-to-human alignments were made:

    lastal -m100 -E0.05 -C2 -p hg38-mm10.mat hg38-MAM4 mm10.fa | last-split -m1 > hg38-mm10-1.maf

* Human-mouse many-to-one alignments:
  [hg38-mm10-1.maf.gz](https://drive.google.com/open?id=0Bw_yRzJW8ZA_bjRQVllSbmNVdHM)

Finally, one-to-one MAF alignments, and high-confidence tabular
alignments, were made in the same way as above.

* Human-mouse one-to-one alignments:
  [hg38-mm10-2.maf.gz](https://drive.google.com/open?id=0Bw_yRzJW8ZA_ZVlna0lveGJKSmc)

* Human-mouse tabular alignments:
  [hg38-mm10.tab.gz](https://drive.google.com/open?id=0Bw_yRzJW8ZA_V0RnVlk5NGtlR00)
  ([dotplot](https://drive.google.com/open?id=0Bw_yRzJW8ZA_NmJWRF90Rm1wVm8))
