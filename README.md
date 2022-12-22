# last-genome-alignments

Here are some pair-wise genome alignments made with
[LAST](https://gitlab.com/mcfrith/last).

## 2022 alignments

The `2022` directory has various alignments of these genomes:

| Genome name     | Animal     | Source   | Assembly name (if different) |
|-----------------|------------|----------|------------------------------|
| allMis28112v4   | alligator  | [NCBI][] | ASM28112v4                   |
| cerSim1         | rhinoceros | [UCSC][] |                              |
| chrPic3.0.3     | turtle     | [NCBI][] | Chrysemys_picta_bellii-3.0.3 |
| equCab3         | horse      | [UCSC][] |                              |
| hg38            | human      | [UCSC][] | hg38.analysisSet             |
| mOrnAna1.pri.v4 | platypus   | [NCBI][] |                              |

They were made with LAST version 1411, using the recipe below under
"2021 alignments".

## 2021 alignments

The `2021` directory has various alignments of these genomes:

| Genome name   | Animal     | Source   | Assembly name (if different) |
|---------------|------------|----------|------------------------------|
| allMis28112v4 | alligator  | [NCBI][] | ASM28112v4                   |
| Bfl_VNyyK     | lancelet   | [NCBI][] |                              |
| calMil1       | chimaera   | [UCSC][] |                              |
| chrPic3.0.3   | turtle     | [NCBI][] | Chrysemys_picta_bellii-3.0.3 |
| hg38          | human      | [UCSC][] | hg38.analysisSet             |
| kPetMar1      | lamprey    | [NCBI][] | kPetMar1.pri                 |
| latCha1       | coelacanth | [UCSC][] |                              |
| lepOcu1       | gar        | [NCBI][] | LepOcu1                      |
| xenTro10      | frog       | [NCBI][] | UCB_Xtro_10.0                |

They can be replicated by running LAST version >= 1387 like this:

    lastdb -P8 -uMAM8 myDB genome1.fa

    last-train -P8 --revsym -D1e9 --sample-number=5000 myDB genome2.fa > my.train

    lastal -P8 -D1e9 -m100 --split-f=MAF+ -p my.train myDB genome2.fa > many-to-one.maf

    last-split -r many-to-one.maf | last-postmask > out.maf

* The `-P8` option makes it faster by using 8 threads: adjust as
  appropriate for your computer.  This has no effect on the results.

* The `-uMAM8` and `-m100` strive for high sensitivity, but make the
  `lastal` command use much time and memory, e.g. several days and
  hundreds of gigabytes.

* You can trade off multi-threading and memory use (with no effect on
  results), see
  [here](https://gitlab.com/mcfrith/last/-/blob/main/doc/last-parallel.rst).

## 2017 alignments

**Warning:** these recipes were for an older version of LAST.

* Since LAST version 1205, `-R01` has no effect and can be omitted
  (because it's the default).

* For LAST version >= 1180, it's best to add option `-fMAF+` to the
  first (many-to-one) `last-split`.  (In older versions, `-fMAF+` was
  the default.)

* Since LAST version 983, `last-split` option `-m1` has no effect and
  can be omitted (because it's the default).

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

[NCBI]: https://www.ncbi.nlm.nih.gov/genome
[UCSC]: https://genome.ucsc.edu/
