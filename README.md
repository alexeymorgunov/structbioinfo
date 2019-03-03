# Structural bioinformatics practical

**Part II BBS Bioinformatics** - 15:00-17:00, 11 Mar 2019

Instructions for the practical are below. [Answers](Answers.md) are available for reference.

Just in case, I've made available all the [files](files) that you are otherwise supposed to download throughout the practical from their original sources. This may be useful if some analyses take too long to run on the servers, the servers are down (happens more often than you'd think!) or if they fail to run for some unexplained reason. Check the relevant section in [Answers](Answers.md) for the exact links.

[Lecture slides](StructBioInfo2019.pdf) from the lecture on 5 Mar 2019 are also available for reference.

---
**1. Finding the right structures - UniProt and PDB.**

We are interested in analysing the structure of a protein called arginine-tRNA ligase (synthetase) from the yeast _Saccharomyces cerevisiae_.

a) Go to [UniProt](http://www.uniprot.org/) and find the entry corresponding to this protein using the search bar. Take a look around at the available information. Feel free to click through to various databases to familiarise yourself with what information UniProt aggregates.

b) Download the `.fasta` file containing the sequence of our protein. We'll need it later. Also add it to the basket on the website.

c) Note down the residue numbers interacting with ligands. We'll visualise them later.

d) Find the table listing available 3D structure models. Choose the link destination to be RCSB PDB. Click through to all available structure models and check their quality indicators. Which one do you think we should choose to look at? Download the `.pdb` file.

e) While you're on the PDB database, take a look around at the available information. There is a tab for viewing the structural model in browser. You can use that to check if the unit cell contains one or more molecules. There are quick settings to colour the molecule by properties such as the B factor. Are there any patterns regarding which regions contain lower quality residues? You can also visit the PDBsum entry (follow the link on UniProt in the PDB table) to familiarise yourself with the structural model's layout.

f) Open and explore the PDB file in a text editor. If curious about the information contained in some fields, you can look them up [here](http://www.wwpdb.org/documentation/file-format-content/format33/v3.3.html) or more briefly for the coordinate entries [here](https://www.cgl.ucsf.edu/chimera/docs/UsersGuide/tutorials/pdbintro.html).
- _Optional._ Check if you still remember how to process text files in [Unix shell](https://github.com/alexeymorgunov/unixshellcourse)! Check if the sequence from ATOM instances matches the one in SEQRES in our structural model. (Make sure you choose the correct chain!)

---
**2. Viewing the structure - PyMOL.**

Start the PyMOL software. (If you don't have it installed, you can download it [here](https://pymol.org/2/).) Take a quick look at [this tutorial](https://pymolwiki.org/index.php/Practical_Pymol_for_Beginners) (ignore scripting!) - skim through to understand what's possible.

Let's play around with our structure to familiarise ourselves with it. Enter the following commands (one per line) and explore what they do.

If at any point things start looking weird and you don't know what's going on, `reinitialize` will get you back to the starting point but all progress will be lost.

 ```
fetch 1f7u
select heteroatoms, hetatm
select protein, chain A
select tRNA, chain B
hide everything, (heteroatoms, tRNA, protein)
deselect
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

We are not doing a PyMOL practical today so the above is just to quickly familiarise you with the syntax. You can always refer to [documentation](https://pymol.org/dokuwiki/doku.php) for more useful commands.

---
**3. Interesting databases.**

a) Go to [ConSurf](http://consurf.tau.ac.il/) to map evolutionary conservation onto protein structure. Choose the following settings: amino acids, known protein structure YES, enter our PDB ID, NO MSA, keep everything else default or automatic. Submit the job. It will take some time to run. Move on with questions and come back to analyse the output when it is done (10-15 min). It is best to view the results in browser with NGL viewer, and set the style to "spacefill". Are the results unexpected?

Go back to the UniProt entry for our protein.

b) Click through to the InterPro database entry (`CTRL-F` for "View protein in InterPro").
- What domains and motifs does our protein contain, and are they well supported? (What's the evidence for them?)
- Does it match what you see in the structure?
- What InterPro family is it part of? Here you can read a bit about this protein, if you're interested. How many members does the family have? Is our protein similar to the most common domain architecture in this family or is it part of a smaller subgroup?

c) From the "Structures" tab in InterPro, follow the links to SCOP and CATH database entries. Do these databases agree on the classification of the constituent domains?

Go back to the UniProt entry for our protein.

d) Click through to the Pfam database entry (`CTRL-F` for "View protein in Pfam"). Find the Pfam family corresponding to the core domain. Familiarise yourself with the available information. You can even find the HMM profile for this Pfam family here.
- Let's download the family sequence alignment. You'll need it later for predicting contacts using coevolution (but only if you choose to use your own alignment). Go to the "Alignments" tab and in "format an alignment" choose the following settings: UniProt, FASTA, tree, all upper case, gaps as dashes, download.

---
**4. What if we don't have a structure?**

a) What if we are not interested in the (arguably) overstudied baker's yeast? Let's go for a nastier yeast, _Candida albicans_ (don't google photos). [A0A1D8PCI5](http://www.uniprot.org/uniprot/A0A1D8PCI5) is the UniProt entry for its arginine-tRNA synthetase. Download the `.fasta` file. Add the protein sequence to the basket, select both A0A1D8PCI5 and Q05506 in your basket and click "Align". How similar are the two protein sequences? Download the alignment as an uncompressed `.fasta` file.

b) Let's build ourselves a structural model of the _C. albicans_ protein by homology modelling. Go to [SWISS-MODEL](https://swissmodel.expasy.org/). Select "Target-Template Alignment" and upload our alignment `.fasta` file. Choose the correct PDB file to build with. Let it run, get some coffee and try the next question.

c) Check the _C. albicans_ protein for disorder. Go to [IUPred](http://iupred.enzim.hu/) and submit the A0A1D8PCI5 sequence (from the `.fasta` file). Which region in the alignment does the most disordered part correspond to? Does this make sense when you look at the homology model?

d) Let's see how similar our homology model is to the original structural model. Go to PyMOL again and enter the following commands:
 ```
reinitialize
fetch 1f7u
hide everything
select protein, chain A
deselect
show cartoon, protein
color green, protein
load ~/Downloads/model.pdb    #check your location!
select mymodel, model_
deselect
show cartoon, mymodel
color yellow, mymodel
cealign mymodel, protein
```

What's our RMSD? Is that a good alignment? Is it surprising to find that?

e) Let's compare this alignment to an alignment between two proteins, both with experimentally derived structural models. Find the human arginine-tRNA synthetase and align it to the _S. cerevisiae_ protein. You could also run a sequence alignment to compare to.

---
**5. Coevolutionary analysis**

To run a good coevolutionary analysis, it is best to have the software running on your own machine but some servers exist for quick and dirty analysis. We'll do one such analysis - don't expect the very best results!

Go to [GREMLIN](http://gremlin.bakerlab.org/submit.php) and enter the sequence for the _S. cerevisiae_ protein. Keep all the parameters as default. Enter your email to get notified when it completes. Let it run, it will take a while (or if your settings exactly match mine, you'll get the precomputed results immediately).

If the job hasn't finished running, view [online](http://gremlin.bakerlab.org/sub.php?id=1520023781) or download the precomputed [results](files/gremlin.zip).

a) Let's see if some of the top predictions are actually close in space in our structural model. Go back to PyMOL and load the _S. cerevisiae_ structure. Use the commands similar to those in section 2 above to visualise and measure distances between the top five pairs of predicted contacts. The common convention is to consider two residues in contact if any of their non-hydrogen atoms are within 5 Å of each other. A less stringent (and easier to compute) way is to consider if their alpha carbons are within 8 Å of each other. Also, consider only pairs that are more than 5 residues apart in sequence. So, how well is GREMLIN doing?

---
Great job on getting to the end of this practical! I hope you enjoyed it and learned something. If you still have time remaining, feel free to repeat some of the analysis with your own favourite protein, or to explore more options available in these databases and servers.

If you have any questions or comments, don't hesitate to email me: asm63[at]cam.ac.uk.


---
### License

This material is released under a
[CC-BY-NC-SA license](https://creativecommons.org/licenses/by-nc-sa/4.0/) ![license](https://licensebuttons.net/l/by-nc-sa/3.0/88x31.png).
