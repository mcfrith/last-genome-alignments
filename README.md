# last-genome-alignments

Here are some pair-wise genome alignments made with
[LAST](https://gitlab.com/mcfrith/last).

## 2023 alignments

The `2023` directory has alignments of these genomes:

| Code   | Phylum       | Animal              | Scientific name                 | Genome                   |
|--------|--------------|---------------------|---------------------------------|--------------------------|
| helRob | annelid      | jawless leech       | *Helobdella robusta*            | GCF_000326865.1          |
| hirMed | annelid      | medicinal leech     | *Hirudo medicinalis*            | GCA_011800805.1          |
| lamSat | annelid      | Satsuma tubeworm    | *Lamellibrachia satsuma*        | GCA_022478865.1          |
| oweFus | annelid      | shingle tubeworm    | *Owenia fusiformis*             | GCA_903813345.2          |
| armNas | arthropod    | woodlouse           | *Armadillidium nasatum*         | GCA_009176605.1          |
| cenScu | arthropod    | scorpion            | *Centruroides sculpturatus*     | GCF_000671375.1          |
| droMel | arthropod    | fruit fly           | *Drosophila melanogaster*       | GCF_000001215.4          |
| gloMae | arthropod    | millipede           | *Glomeris maerens*              | GCA_023279145.1          |
| homAme | arthropod    | lobster             | *Homarus americanus*            | GCF_018991925.1          |
| limPol | arthropod    | horseshoe crab      | *Limulus polyphemus*            | GCF_000517525.1          |
| macAtr | arthropod    | robber fly          | *Machimus atricapillus*         | GCA_933228815.1          |
| strMar | arthropod    | centipede           | *Strigamia maritima*            | GCA_000239455.1          |
| linAna | brachiopod   | shamisen shell      | *Lingula anatina*               | GCF_001039355.2          |
| asyLuc | chordate     | Bahama lancelet     | *Asymmetron lucayanum*          | GCA_001663935.1          |
| braFlo | chordate     | lancelet            | *Branchiostoma floridae*        | GCF_000003815.2          |
| calMil | chordate     | chimaera            | *Callorhinchus milii*           | GCF_018977255.1          |
| eptBur | chordate     | hagfish             | *Eptatretus burgeri*            | GCA_024346535.1          |
| homSap | chordate     | human               | *Homo sapiens*                  | hg38_no_alt_analysis_set |
| petMar | chordate     | lamprey             | *Petromyzon marinus*            | GCF_010993605.1          |
| acrMil | cnidaria     | stony coral         | *Acropora millepora*            | GCF_013753865.1          |
| actTen | cnidaria     | waratah anemone     | *Actinia tenebrosa*             | GCF_009602425.1          |
| epiPla | cnidaria     | zoanthid            | *Epizoanthus planus*            | GCA_025388665.1          |
| nemVec | cnidaria     | starlet sea anemone | *Nematostella vectensis*        | GCF_932526225.1          |
| horCal | ctenophore   | sea gooseberry      | *Hormiphora californensis*      | GCA_020137815.1          |
| mneLei | ctenophore   | sea walnut          | *Mnemiopsis leidyi*             | GCA_000226015.1          |
| apoJap | echinoderm   | sea cucumber        | *Apostichopus japonicus*        | GCA_002754855.1          |
| strPur | echinoderm   | sea urchin          | *Strongylocentrotus purpuratus* | GCF_000002235.5          |
| ptyFla | hemichordate | Hawaiian acorn worm | *Ptychodera flava*              | GCA_001465055.1          |
| sacKow | hemichordate | acorn worm          | *Saccoglossus kowalevskii*      | GCF_000003605.2          |
| aplCal | mollusc      | sea hare            | *Aplysia californica*           | GCF_000002075.1          |
| craGig | mollusc      | oyster              | *Crassostrea gigas*             | GCF_902806645.1          |
| halRuf | mollusc      | abalone             | *Haliotis rufescens*            | GCF_023055435.1          |
| limBul | mollusc      | sea butterfly       | *Limacina bulimoides*           | GCA_009866985.1          |
| mizYes | mollusc      | scallop             | *Mizuhopecten yessoensis*       | GCF_002113885.1          |
| octBim | mollusc      | octopus             | *Octopus bimaculoides*          | GCF_001194135.2          |
| phoLin | mollusc      | top snail           | *Phorcus lineatus*              | GCA_921293015.1          |
| watSci | mollusc      | firefly squid       | *Watasenia scintillans*         | GCA_015471945.1          |
| phoOva | phoronid     | horseshoe worm      | *Phoronis ovalis*               | GCA_028565635.1          |

The alignments were made with LAST version 1453, like this:

    lastdb -P8 -uMAM8 -c myDB genome1.fa

    last-train -P8 --revsym -D1e9 --sample-number=5000 myDB genome2.fa > my.train

    lastal -P8 -D1e9 -m100 --split-f=MAF+ -p my.train myDB genome2.fa > many-to-one.maf

    last-split -r many-to-one.maf > one-to-one.maf

This is currently the recommended way to compare distantly-related
genomes, where most of the DNA lacks similarity.

* `-P8` makes it faster by using 8 threads: adjust as suitable for
  your computer.  This has no effect on the results.

* `-uMAM8` and `-m100` strive for high sensitivity, but use a lot of
  memory and run time.  To go much faster, omit `-m100`.  To halve the
  memory use and run time, change `MAM8` to `MAM4`.

* `--sample-number=5000` makes `last-train` use more samples of
  genome2, for fear that most of genome2 lacks similarity to genome1.
  For the same reason, `-D1e9` is used with `last-train`, to avoid
  weak chance similarities more strictly.

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
