# Structural bioinformatics practical - ANSWERS

---
**1. Finding the right structures.**

a) [Q05506](http://www.uniprot.org/uniprot/Q05506).

b) [Q05506.fasta](http://www.uniprot.org/uniprot/Q05506.fasta).

c) With tRNA: 59-60, 106-111, 484-498. With Arg: 148-153.

d) They are all quite similar in quality and it seems there are only two independent experiments (1F7U and 1F7V are two structure models from the same experiment with and without the arginine). 1F7U seems marginally better than the other two, let's go for that. [1F7U](files/1f7u.pdb).

e) The unit cell contains eight molecules. Some outer regions have lower quality, unsurprisingly. [PDBsum](https://www.ebi.ac.uk/pdbsum/1F7U).

f)
  ```bash
cat 1f7u.pdb | grep ^ATOM | grep -E '^.{21}A' | cut -c18-26 | grep " A " | uniq | cut -d" " -f1 > sequence.txt
cat 1f7u.pdb | grep ^SEQRES | grep -E '^.{11}A' | cut -c20- | tr " " "\n" | sed '/^$/d' > sequence2.txt
diff sequence.txt sequence2.txt
```
The N-terminal methionine is missing from the actual structural model but is present in the SEQRES field. That is because the full gene sequence was used in protein expression, but the methionine is subsequently cleaved in the yeast cell, and so the protein that actually crystallises doesn't have it. So UniProt listing the PDB model as starting at position 1 is actually wrong!

---
**2. Viewing the structure - PyMOL.**

No questions here.

---
**3. Interesting databases.**

a) [InterPro](https://www.ebi.ac.uk/interpro/protein/Q05506).
- 3 domains - ArgRS NTD, catalytic core and DALR anticodon binding. Some motifs including HIGH and KMSK. Multiple databases agree on the predictions, good support.
- Yes, we should see three domains in the structure quite well.
- It is part of [IPR001278](http://www.ebi.ac.uk/interpro/entry/IPR001278) with over 28,000 proteins. Our domain architecture is the most common.

b) They don't. SCOP entries are manually annotated and thus are more detailed/less error-prone. CATH is mostly automated so covers more proteins but is less precise, as you can see from the comparison.

c) [tRNA-synt_1d](http://pfam.xfam.org/family/tRNA-synt_1d).
- [PF00750_uniprot.txt](files/PF00750_uniprot.txt).

---
**4. What if we don't have a structure?**

a) [Alignment](http://www.uniprot.org/align/A2018030283C3DD8CE55183C76102DC5D3A26728B0A99479). (Link will expire on 9 Mar 2018.) Very similar sequences, 64.4% of identity means this is definitely the same protein. Notice a small insertion in the _C. albicans_ version. Downloaded [alignment](files/alignment.fasta).

b) [Results](https://swissmodel.expasy.org/interactive/fQ9e8F/). (Link may expire). Full [results archive](homology_modelling_results.zip) and [PDB model](C_albicans.pdb) only.

c) The disordered part corresponds to the insertion. Our homology model shows this region to be on the surface of the protein, a loop insertion. This makes perfect sense structurally.

d) RMSD should be around 1.46. It is not surprising at all - we modelled an almost identical sequence with only a single insertion into a known structure. Any larger RMSD would in fact be worrying.
