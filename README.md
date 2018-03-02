# Structural bioinformatics practical

**Part II BBS Bioinformatics** - 16:00-18:00, 5 Mar 2018

Instructions for the practical are below. [Answers](Answers.md) are available for reference.

Just in case, I've made available all the [files](files) that you are otherwise supposed to download throughout the practical from their original sources.

---
**1. Finding the right structures - UniProt and PDB.**

We are interested in analysing the structure of a protein called arginine-tRNA ligase (synthetase) from the yeast _Saccharomyces cerevisiae_.

a) Go to [UniProt](http://www.uniprot.org/) and find the entry corresponding to this protein using the search bar. Take a look around at the available information. Feel free to click through to various databases to familiarise yourself with what information UniProt aggregates.

b) Download the `.fasta` file containing the sequence of our protein. We'll need it later to compare with others.

c) Note down the residue numbers interacting with ligands. We'll visualise them later.

d) Find the table listing available 3D structure models. Choose the link destination to be RCSB PDB. Click through to all available structure models and check their quality indicators. Which one do you think we should choose to look at? Download the `.pdb` file.

e) While you're on the PDB database, take a look around at the available information. There is a tab for viewing the structural model in browser. You can use that to check if the unit cell contains one or more molecules. There are quick settings to colour the molecule by properties such as the B factor. Are there any patterns as to which regions contain lower quality residues?

f) Open and explore the PDB file in a text editor. If curious about the information contained in some fields, you can look them up [here](http://www.wwpdb.org/documentation/file-format-content/format33/v3.3.html) or more briefly for the coordinate entries [here](https://www.cgl.ucsf.edu/chimera/docs/UsersGuide/tutorials/pdbintro.html).
_optional_
- Check if you still remember how to process text files in [Unix shell](https://github.com/alexeymorgunov/unixshellcourse)! Check if the sequence from ATOM instances matches the one in SEQRES in our structural model. (Make sure you choose the correct chain!)

---
**2. Viewing the structure - PyMOL.**

Start the PyMOL software. Take a quick look at [this tutorial](https://pymolwiki.org/index.php/Practical_Pymol_for_Beginners) (ignore scripting!) - skim it through to understand what's possible.

Enter the following commands (one per line) and explore what they do.
```
fetch 1f7u
select heteroatoms, hetatm
select protein, chain A
select RNA, chain B
hide everything, (heteroatoms, RNA, protein)
show sticks, RNA
show cartoon, protein
spectrum count, rainbow, protein, byres=1
show_as spheres, heteroatoms
preset.publication(selection='all')
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
