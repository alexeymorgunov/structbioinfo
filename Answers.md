# Structural bioinformatics practical - ANSWERS

---
**1. Finding the right structures.**

a) [Q05506](http://www.uniprot.org/uniprot/Q05506).

b) [Q05506.fasta](http://www.uniprot.org/uniprot/Q05506.fasta).

c) With tRNA: 59-60, 106-111, 484-498. With Arg: 148-153.

d) They are all quite similar in quality and it seems there are only two independent experiments (1F7U and 1F7V are two structure models from the same experiment with and without the arginine). All structures seem to be of average quality but 1F7U seems marginally better than the other two across all indicators, let's choose this one. [1F7U](files/1f7u.pdb).

e) The unit cell contains four molecules. You can check out the space group [here](http://img.chem.ucl.ac.uk/sgp/large/018az1.htm). You can also see in the PDB file how this information is encoded - check `REMARK 290` entries. Some outer regions of the structural model have lower quality - this is quite common as the surface regions are usually more dynamic. [PDBsum](https://www.ebi.ac.uk/pdbsum/1F7U).

f)
  ```bash
cat 1f7u.pdb | grep ^ATOM | grep -E '^.{21}A' | cut -c18-26 | grep " A " | uniq | cut -d" " -f1 > sequence.txt
cat 1f7u.pdb | grep ^SEQRES | grep -E '^.{11}A' | cut -c20- | tr " " "\n" | sed '/^$/d' > sequence2.txt
diff sequence.txt sequence2.txt
```
The N-terminal methionine is missing from the structural model but is present in the SEQRES field. That is because the full gene sequence was used in protein expression (that information goes in the SEQRES field), but the methionine is subsequently cleaved in the yeast cell, and so the protein that actually crystallises doesn't have it. So UniProt listing the PDB model as starting at position 1 is actually wrong! Care should be taken because most databases process the SEQRES entries when figuring out what part of the protein the structural model contains. But it is not the correct method - ATOM entries should be checked instead.

---
**2. Viewing the structure - PyMOL.**

No questions here.

---
**3. Interesting databases.**

a) [Results](http://consurf.tau.ac.il/results/1551476465/output.php). (Link will expire by end of March.) Full [results archive](files/consurf_results.zip). Core of the protein is more conserved, surfaces involved in interacting with the tRNA are also very conserved but outer regions are more variable - exactly what we'd expect. Functional parts and the core of a protein are typically more conserved than outer regions or functionally less important parts. Therefore, evolutionary conservation can help localise the active site or interacting surfaces.

b) [InterPro](https://www.ebi.ac.uk/interpro/protein/Q05506).
- 3 domains - ArgRS NTD, catalytic core and DALR anticodon binding. Some motifs including HIGH and KMSK. Multiple databases agree on the predictions, good support.
- Yes, we should see three domains in the structure quite well.
- It is part of [IPR001278](http://www.ebi.ac.uk/interpro/entry/IPR001278) with over 37,000 proteins. (It was just 28,000 in 2018!) Our domain architecture is the most common.

c) The two databases disagree. SCOP entries are manually annotated and thus are more detailed/less error-prone. CATH is mostly automated so covers more proteins but is less precise, as you can see from the comparison.
- N-terminal domain: [CATH](http://www.cathdb.info/version/v4_2_0/superfamily/3.30.1360.70/classification) and [SCOP](http://scop.mrc-lmb.cam.ac.uk/scop/data/scop.b.e.bfi.c.b.html) - note the different hierarchical levels.
- Catalytic domain: [CATH](http://www.cathdb.info/version/v4_2_0/superfamily/1.10.730.10/classification) and [SCOP](http://scop.mrc-lmb.cam.ac.uk/scop/data/scop.b.d.da.b.b.html) - here, SCOP correctly classifies the Class I tRNA synthetase catalytic domains, but CATH shows a group for Ile tRNA synthetase catalytic domains even though our protein is an Arg tRNA synthetase! Also worth noting the "mainly alpha" and "alpha and beta" discrepancy at the highest level.
- tRNA binding domain: [CATH](http://www.cathdb.info/version/v4_2_0/superfamily/3.40.50.620/classification) and [SCOP](http://scop.mrc-lmb.cam.ac.uk/scop/data/scop.b.b.ea.b.b.html) - completely different classification levels, especially noteworthy is the top level "alpha beta" vs "all alpha" discrepancy.

d) [tRNA-synt_1d](http://pfam.xfam.org/family/tRNA-synt_1d).
- [PF00750_uniprot.txt](files/PF00750_uniprot.txt).

---
**4. What if we don't have a structure?**

a) [Alignment](https://www.uniprot.org/align/A201903106746803381A1F0E0DB47453E0216320D3911E0O). (Link will expire on 17 Mar 2019.) Very similar sequences, 64.4% identity means this is definitely the same protein. Notice a small insertion in the _C. albicans_ version. Downloaded [alignment](files/alignment.fasta).

b) [Results](https://swissmodel.expasy.org/interactive/TZS3Bm/). (Link may expire.) Full [results archive](files/homology_modelling_results.zip) and the [PDB model](model.pdb) only.

c) The disordered part corresponds to the insertion. Our homology model shows this region to be on the surface of the protein, a loop insertion. This makes perfect sense structurally. Clearly, we shouldn't trust the model in this region - we don't know how this loop folds in reality or whether it even has a stable conformation.

d) RMSD should be around 0.32. It is not surprising at all - we modelled an almost identical sequence with only a single insertion into a known structure. Any larger RMSD would in fact be worrying.

e) The human protein is [P54136](http://www.uniprot.org/uniprot/P54136) with PDB structural model [4ZAJ](https://www.rcsb.org/structure/4ZAJ). PDB file is [here](files/4zaj.pdb). (You may need to reload `1F7U` if you have changed things in PyMOL after the previous exercise - follow the commands until loading the model.)
```
fetch 4zaj
select human, resi 1-588 in chain A in 4zaj
deselect
hide everything
show cartoon, cerev
center
show cartoon, human
color pink, human
cealign human, cerev
center
```
The RMSD should be around 3.88. Unsurprisingly, this is worse than the previous alignment but still a very good score. Visual assessment should confirm that the proteins are very similar to each other. Baker's yeast and humans are quite far apart on the evolutionary tree, with sequence identity for this essential protein of only 24% ([alignment](https://www.uniprot.org/align/A201903106746803381A1F0E0DB47453E0216320D3911EFZ) - link will expire on 17 Mar 2019) but with very strong structural homology. Structure is indeed much better conserved than sequence!

---
**5. Coevolutionary analysis**

[Completed job](http://gremlin.bakerlab.org/sub.php?id=1520023781) and precomputed [results](files/gremlin.zip).

a) Possible command sequence:
```
reinitialize
fetch 1f7u
hide everything
select protein, chain A
deselect
show ribbon, protein
color green, protein
distance dist1, i. 554 and n. CA, i. 595 and n. CA
select contact, chain A and resi 127 or resi 531
show sticks, contact
distance dist2, i. 127, i. 531, cutoff=5
distance dist3, i. 102 and n. CA, i. 113 and n. CA
distance dist4, i. 144 and n. CA, i. 182 and n. CA
distance dist5, i. 177, i. 185, cutoff=5
select contact2, chain A and resi 177 or resi 185
show sticks, contact2
```

Looks like all top 5 predictions are spot on! Good job, GREMLIN!



---
### License

This material is released under a
[CC-BY-NC-SA license](https://creativecommons.org/licenses/by-nc-sa/4.0/) ![license](https://licensebuttons.net/l/by-nc-sa/3.0/88x31.png).
