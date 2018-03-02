# Structural bioinformatics practical - ANSWERS

**1. Finding the right structures.**

a) [Q05506](http://www.uniprot.org/uniprot/Q05506).

b) [Q05506.fasta](http://www.uniprot.org/uniprot/Q05506.fasta).

c) With tRNA: 59-60, 106-111, 484-498. With Arg: 148-153.

d) They are all quite similar in quality and it seems there are only two independent experiments (1F7U and 1F7V are two structure models from the same experiment with and without the arginine). 1F7U seems marginally better than the other two, let's go for that. [1F7U](files/1f7u.pdb).

e) The unit cell contains eight molecules. Some outer regions have lower quality, unsurprisingly.

**2. Viewing the structure - PyMOL.**

**3. Interesting databases.**

a) [InterPro](https://www.ebi.ac.uk/interpro/protein/Q05506).
- 3 domains - ArgRS NTD, catalytic core and DALR anticodon binding. Some motifs including HIGH and KMSK. Multiple databases agree on the predictions, good support.
- Yes, we should see three domains in the structure quite well.
- It is part of [IPR001278](http://www.ebi.ac.uk/interpro/entry/IPR001278) with over 28,000 proteins. Our domain architecture is the most common.

b) They don't. SCOP entries are manually annotated and thus are more detailed/less error-prone. CATH is mostly automated so covers more proteins but is less precise, as you can see from the comparison.

c) [tRNA-synt_1d](http://pfam.xfam.org/family/tRNA-synt_1d).
- [PF00750_uniprot.txt](files/PF00750_uniprot.txt).
