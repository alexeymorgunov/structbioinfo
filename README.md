# Structural bioinformatics practical

**Part II BBS Bioinformatics** - 16:00-18:00, 5 Mar 2018

Instructions for the practical are below. [Answers](Answers.md) are available for reference.

Just in case, I've made available all the [files](files) that you are otherwise supposed to download throughout the practical from their original sources.

---
**1. Finding the right structures - UniProt and PDB.**

We are interested in analysing the structure of a protein called arginine-tRNA ligase (synthetase) from the yeast _Saccharomyces cerevisiae_.

a) Go to [UniProt](http://www.uniprot.org/) and find the entry corresponding to this protein using the search bar. Take a look around at the available information. Feel free to click through to various databases to familiarise yourself with what information UniProt aggregates.

b) Download the `.fasta` file containing the sequence of our protein. We'll need it later. Also add it to the basket on the website.

c) Note down the residue numbers interacting with ligands. We'll visualise them later.

d) Find the table listing available 3D structure models. Choose the link destination to be RCSB PDB. Click through to all available structure models and check their quality indicators. Which one do you think we should choose to look at? Download the `.pdb` file.

e) While you're on the PDB database, take a look around at the available information. There is a tab for viewing the structural model in browser. You can use that to check if the unit cell contains one or more molecules. There are quick settings to colour the molecule by properties such as the B factor. Are there any patterns as to which regions contain lower quality residues? You can also visit the PDBsum entry (follow the link on UniProt in the PDB table) to familiarise yourself with the structural model's layout.

f) Open and explore the PDB file in a text editor. If curious about the information contained in some fields, you can look them up [here](http://www.wwpdb.org/documentation/file-format-content/format33/v3.3.html) or more briefly for the coordinate entries [here](https://www.cgl.ucsf.edu/chimera/docs/UsersGuide/tutorials/pdbintro.html).
_optional_
- Check if you still remember how to process text files in [Unix shell](https://github.com/alexeymorgunov/unixshellcourse)! Check if the sequence from ATOM instances matches the one in SEQRES in our structural model. (Make sure you choose the correct chain!)

---
**2. Viewing the structure - PyMOL.**

Start the PyMOL software. Take a quick look at [this tutorial](https://pymolwiki.org/index.php/Practical_Pymol_for_Beginners) (ignore scripting!) - skim it through to understand what's possible.

Enter the following commands (one per line) and explore what they do.
 ```pymol
fetch 1f7u
select heteroatoms, hetatm
select protein, chain A
select tRNA, chain B
hide everything, (heteroatoms, RNA, protein)
show sticks, tRNA
show cartoon, protein
spectrum count, rainbow, protein, byres=1
show_as spheres, heteroatoms
preset.publication(selection='all')
hide all
select contact_tRNA, (i. 59-60, i. 106-111, i. 484-498)
show sticks, contact_tRNA
deselect
show lines, tRNA
color white, tRNA
distance mydist, i. 59-60 and n. CA, i. 484-498 and n. CA
hide everything, mydist
distance mydist2, i. 59-60, i. 484-498, mode=4
```

We are not doing a PyMOL practical today so the above is just to quickly familiarise you with the syntax. You can always refer to [command reference](http://pymol.org/pymol-command-ref.html) for more useful commands.

---
**3. Interesting databases.**

Go back to the UniProt entry for our protein.

a) Click through to the InterPro database entry (`CTRL-F` for "View protein in InterPro").
- What domains and motifs does our protein contain, and are they well supported?
- Does it match what you see in the structure?
- What InterPro family is it part of? Here you can read a bit about this protein, if you're interested. How many members does it have and is our protein similar to the most common domain architecture in this family or is it part of a smaller subgroup?

b) From the "Structures" tab in InterPro, follow the links to SCOP and CATH database entries. Do these databases agree on the classification of the constituent domains?

Go back to the UniProt entry for our protein.

c) Click through to the Pfam database entry (`CTRL-F` for "View protein in Pfam"). Find the Pfam family corresponding to the core domain. Familiarise yourself with the available information. You can even find the HMM profile for this Pfam family here.
- Let's download the family sequence alignment. We'll need it later for predicting contacts using coevolution. Go to the "Alignments" tab and in "format an alignment" choose UniProt, FASTA, tree, all upper case, gaps as dashes, download.

d) Go to [ConSurf](http://consurf.tau.ac.il/) to map evolutionary conservation onto protein structure. Choose amino acids, known protein structure YES, enter our PDB ID, NO MSA, keep everything else default or automatic. Submit the job. It will take some time to run.

---
**4. What if we don't have a structure?**

a) What if we are not interested in the (arguably) overstudied baker's yeast? Let's go for a nastier yeast, _Candida albicans_ (don't google photos). [A0A1D8PCI5](http://www.uniprot.org/uniprot/A0A1D8PCI5) is the UniProt entry for its arginine-tRNA synthetase. Download the `.fasta` file. Add the protein sequence to the basket, select both A0A1D8PCI5 and Q05506 in your basket and click "Align". How similar are the two protein sequences? Download the alignment as an uncompressed `.fasta` file.

b) Let's build ourselves a structural model of the _C. albicans_ protein by homology modelling. Go to [SWISS-MODEL](https://swissmodel.expasy.org/). Select "Target-Template Alignment" and upload our alignment `.fasta` file. Choose the correct PDB file to build with. Let it run, get some coffee and try the next question.

c) Check the _C. albicans_ protein for disorder. Go to [IUPred](http://iupred.enzim.hu/) and submit the A0A1D8PCI5 sequence. Which region in the alignment does the most disordered part correspond to? Does this make sense when you look at the homology model?

d) Let's see how similar our homology model is to the original structural model. Go to PyMOL again and do the following:
 ```pymol
reinitialize
fetch 1f7u
hide everything
select protein, chain A
deselect
show cartoon, protein
color green, protein
load ~/Downloads/model.pdb #check location!
select mymodel, model_
deselect
show cartoon, mymodel
color yellow, mymodel
#fun begins
cealign mymodel, protein
```

What's our RMSD? Is it surprising that the alignment is so good?

alignment
consurf
coevolution
