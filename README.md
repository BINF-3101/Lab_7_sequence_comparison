# Lab_7_sequence_comparison

This lab has **two parts**. 
**Part 1** The first part you will analyze the PGM sequences generated by everyone in the class last week. 
**Part 2** You will find a gene involved in the pathway of your choice

&ensp;

# Outline
[PART 1](#part-1)

[Step 1 - Setup Folders](#step-1---setup-your-lab-7-folder)

[Step 2 - Copy files](#step-2---copy-the-pgm1-sequences-into-your-lab-7-folder)

[Step 3 - Align Sequences](#step-3---align-the-pgm1-sequences-using-muscle)

[Step 4 - Consensus Sequence](#step-4---create-a-consensus-sequence)

[LQ 1](#lq-1)

[LQ 2](#lq-2)

[LQ 3](#lq-3)

[Step 5 - Compare to consensus](#step-5---compare-your-sequence-to-the-consensus)

[LQ 4](#lq-4)

[PART 2](#part-2)

[Step 1 - File setup](#step-1---copy-your-gene-of-interest-file)

[Step 2 - Align sequences](#step-2---align-your-reference-sequences)

[Step 3 - Build HMM](#step-3---build-your-hmm)

[Step 4 - Search with HMM](#step-4---search-your-genome-annotation-with-hmmsearch)

[Step 5 - Examine HMM search results](#step-5---look-at-your-hmm-output)

[LQ 5](#lq-5)

[LQ 6](#lq-6)

&ensp;
# PART 1
&ensp;
## Step 1 - Setup your lab 7 folder

As usual, you will need to set up your new folder for lab 7. 
Make a directory called "lab_7"

&ensp;
&ensp;

## Step 2 - Copy the pgm1 sequences into your lab 7 folder

The sequences for lab are in a file called "all_pgm1.fasta".

Move into your lab 7 folder and copy ```/projects/class/binf3101_001/all_pgm1.fasta``` into your lab 7 folder

You will also need to copy in your PGM1 file from lab_6. 

At this point, you should have the following files in your lab_7 folder:
- all_pgm1.fasta
- SRR00000.pgm1.fasta

&ensp;

## Step 3 - Align the pgm1 sequences using MUSCLE
&ensp;
### Step 3a - load MUSCLE 
Muscle is already installed on the cluster. To load it you can use

```bash
module load muscle
```
&ensp;

### Step 3b - Align the whole class's PGM1 sequences

You will use MUSCLE to align the sequences in the file ```all_pgm1.fasta``` and save it to a new file called ```all_pgm1.align.fasta```

To learn how to use MUSCLE you can use the help documentation. There are two mandatory things you need to include, and they are found under the **Basic usage** section of the help documentation. 

To access the help documentation for MUSCLE type

```bash
muscle --help
```

Once you generate the protein alignment, you can take a look at it using the ```cat``` command 

&ensp;
&ensp;

## Step 4 - Create a consensus sequence

A consensus sequence looks at the alignment and reports the most common amino acid (or base pair) at that position. 

For example, if we had the alignment below

PLANK
PRANK
CRANK
PLANS

We could generate a consensus sequence that reports the consensus amino acid at a threshold of >50%
This would be:

PxANK

There is no consensus (x) for that second position because no amino acid reaches >50%

To generate our consensus sequence, we are going to use the EMBOSS program **cons**

&ensp;

### Step 4a - Load emboss

To load emboss we will use

```ml emboss``

### Step 4b - Generate consensus Sequence

To generate the consensus sequence for all the PGM1 files (all_pgm1.align.fasta) we want to use the format 

```cons inputfile outputfile```

Fill in the input and output files to generate the consensus sequence. 

&ensp;

### Step 4c - Look at the consensus sequence

Take a look at the consensus sequence we generated and answer the questions below. 

Note - Some of the letters are upper-case and some are lower-case.

UPPERCASE letters = **high conservation** wherease 
lowercase letters = **low conservation**

&ensp;
&ensp;

# LQ 1

How many positions in the sequence alignment did not have a consensus amino acid?

&ensp;
&ensp;

# LQ 2

How many positions in the sequence alignment had low conservation for Threonine?

**_TIP_** In this case when we use grep we **do not use the -c command**. This is because it only counts **lines**.
If we want to match just one character (for example, z) we would use grep with the ```-o``` option. For example, the command would be
```bash 
grep -o "z" file.txt
z
z
z
z
```

That will print out every time there is a "z" in the file. 

If you don't want to count the number of times that is printed out, we can add " | wc -l" to the end of our command. This calls the **w**ord **c**ount command and the ```-l``` option says to count the lines

```bash
grep -o "z" file.txt | wc -l
4
```

&ensp;
&ensp;

# LQ 3

How many positions in the sequence alignment had high conservation for Threonine?

&ensp;
&ensp;

## Step 5 - Compare your sequence to the consensus

To compare your PGM1 sequence (generated in lab 6) to the consensus sequence, we are going to use **blastp**

In this case, each file is on 1 sequence - so it doesn't matter which file we put as the query and which we put as the sequence. 

Save the blastp output to a new file so you can look at it and answer the questions below

&ensp;
&ensp;

# LQ 4 
Report the following statistic for your PGM1 vs the consensus
1. Score (bits)
2. E-value
3. Percent Identities
4. Percent Gaps

&ensp;
&ensp;

# PART 2
In this part, you will identify your metabolic gene. 

**_Note_** If you picked sucrose last week I had to change your gene because I couldn't find the sucrose metabolic gene in every genome! Might be worth looking into in my research.

Head to the google doc here to find which **gene** you will be analyzing. Link: https://docs.google.com/spreadsheets/d/1smi0H0h7qz-APVstP750x553q-NDQG7KeRtBs8uF-Fg/edit?usp=sharing 

&ensp;
&ensp;

## Step 1 - Copy your gene of interest file

I have found reference sequences (aka known sequences) for each of the genes. They are in files called ```gene.fasta``` in the shared class space. 

Now you will need to copy that file into your current lab_7 directory

```bash
cp /projects/class/binf3101_001/lab_7_ref_seqs/yourprot.fasta .
```

You will also need to copy over your protein annotations from last lab. So if you are still in the lab 7 folder you can use the following command

```bash
cp ../lab_6/SRR00000.prot.fasta .
```

Now you should also have those two files in your directory.

&ensp;
&ensp;

## HMM SEARCH 

Instead of using blast, this time we are going to use **hmmer**. The major steps of doing a hmmer search are

1) Align reference sequences
2) Build an hmm from the reference sequence alignment
3) Search your sequences using the hmm

## Step 2 - Align your reference sequences

We are going to use a different alignment software, MAFFT, so you can practice. 

Mafft has an even easier command-line application 

```bash
module load mafft

mafft input > output
```

In this case, your input file will be your reference sequences. The outout will be a new file called someting line ```gene.align.fasta```

Take a look at your alignment using the ```cat``` command.

&ensp;
&ensp;

## Step 3 - Build your HMM

To build your HMM it's very easy! 
```bash
#load hmmer
module load hmmer

#run hmmbuild - replace gene with your gene of interest
hmmbuild gene.hmm gene.align.fasta
```
Take a look at your new Hidden Markov Model profile file using head

&ensp;
&ensp;

## Step 4 - Search your genome annotation with hmmsearch 

If you look in the hmmsearch help ```hmmsearch -h``` you'll see that the usage is in the format below:
```bash
hmmsearch -h

Usage: hmmsearch [options] <hmmfile> <seqdb>
```

We are going to use two options:

```-E 1e-10``` - that sets the cutoff to a very small number! So we only get good matches
```-o output.txt``` - this will send the output to the file output.txt

Our hmm file <hmmfile> is the one you created above in the format ```gene.hmm```

The "seqdb " is just our SRR00000.prot.fasta file. 

Put all those pieces together and run the command! 

&ensp;
&ensp;

## Step 5 - look at your hmm output

Open up your hmmer output file and answer the questions below

&ensp;
&ensp;

# LQ 5 
How many complete sequences passed the E-value cutoff?

&ensp;
&ensp;

# LQ 6 
What sequence (or sequences) have the most significant E-value? There may be more than one if there is a tie. 






