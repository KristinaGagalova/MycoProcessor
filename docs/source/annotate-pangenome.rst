Annotate-pangenome workflow
===================================

**Annotate-pangenome** is a gene annotation workflow which produces
representative pan-gene sets from multiple isolate genomes. The 
workflow accounts for the possibility that some isolates may have more
available evidence for annotation (here termed 'reference isolates') than others
('alternate isolates'), and is executed dynamically depending on provided inputs.

The prediction of genes is largely powered by ``funannotate`` modules. First, all genome assemblies
are preprocessed with ``funannotate clean`` and ``sort`` modules. Genomes that have corresponding RNA reads
go through ``funannotate train``, which will produce an RNA-bam, PASA gff, and transcript fasta. These are
used to annotate the 'reference isolate' genomes with ``funannotate predict``. For genomes without RNA reads, the transcripts from running ``funannotate train``
on 'reference isolates' will be used (and concatenated with input transcripts if provided) to perform ``PASA`` annotation
on 'alternate isolates'. The produced PASA gff will be used first as input evidence to ``Coding Quarry``, which is not supported in funannotate predict unless an RNA-bam is provided. 
The PASA gff and Coding Quarry annotations will then be used as evidence in ``funannotate predict``. 

Optionally, ``Coding Quarry pathogen mode`` (CQPM) can be run after funannotate predict. This module will mine intergenic regions for effector-like genes that may have been missed by ``funannotate``.

Finally, ``proteinortho self-blast`` is run to cluster predicted proteins into ortho-groups, and custom scripts are used
to filter and extract representative sequences, yeilding a pan-gene set representative of multiple isolate genomes. 


.. figure:: images/Annotate-pangenome.drawio.png
  :width: 600
  :alt: Basic schematic of annotate-pangenome workflow

Basic schematic of annotate-pangenome workflow

.. _commands:

Annotate-pangenome Commands
===========================
A description for all Annotate-pangenome commands.

.. code-block:: None

	Usage:	nextflow run MycoProcessor/main.nf --tool annotate-pangenome <arguments>

	Required:
		--genomes           Genome fasta files
		--outputdir         Output directory
		--gmark_db          "/scratch/y95/kgagalova/HGT/funannotate/genemark/gmes_linux_64" 
		--augustus_config   "/scratch/y95/kgagalova/software/Augustus/config" 
		--evm_home          "/scratch/y95/kgagalova/software/EVidenceModeler-v2.1.0" 

	Optional:
		--transcripts       Transcript file/s, multiple files will be concatenated
		--rnareads          RNA reads for reference isolates, must have same basename as corresponding genome
		-profile            Nextflow profile to use for execution
		--funannotate_db    Path for funannotate database (will be installed/used from MycoProcessor directory by default)
		--cqpm              Perform additional annotation with Coding Quarry pathogen mode
		--signalp_path      Path for SignalP v4.1 (required if running cqpm)


.. note::
   If running CQPM, singularity must be enabled, and a path to SignalP v4.1 must be provided.




