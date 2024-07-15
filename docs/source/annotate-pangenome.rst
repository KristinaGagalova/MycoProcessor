Annotate-pangenome workflow
===================================

**Annotate-pangenome** is a gene annotation workflow which produces
representative pan-gene sets from multiple isolate genomes. The 
workflow accounts for the possibility that some isolates may have more
available evidence for annotation (here termed 'reference isolates') than others
('alternative isolates'), and is executed dynamically depending on provided inputs.


.. _commands:

Annotate-pangenome Commands
===========================
A description for all Annotate-pangenome commands.

.. code-block:: None

	Usage:	nextflow run MycoProcessor --tool annotate pangenome <arguments>

	Required:
		--genomes           Genome fasta files
		--outputdir         Output directory

	Optional:
		--transcripts       Transcript file/s, multiple files will be concatenated
		--rnareads          RNA reads for reference isolates, must have same basename as corresponding genome
		-profile            Nextflow profile to use for execution
		--gmark_db          "/scratch/y95/kgagalova/HGT/funannotate/genemark/gmes_linux_64" 
		--augustus_config   "/scratch/y95/kgagalova/software/Augustus/config" 
		--evm_home          "/scratch/y95/kgagalova/software/EVidenceModeler-v2.1.0" 
		--funannotate_db    Path for funannotate database (will be installed/used from MycoProcessor directory by default)
		--cqpm              Perform additional annotation with Coding Quarry pathogen mode
		--signalp_path      Path for SignalP v4.1 (required if running cqpm)


.. note::
   If running CQPM, singularity must be enabled, and a path to SignalP v4.1 must be provided.


