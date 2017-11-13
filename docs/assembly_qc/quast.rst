Assembly evaluation with QUAST
==============================

(TODO: Anpasse)

QUAST stands for QUality ASsessment Tool. The tool evaluates genome
assemblies by computing various metrics.  You can find all project
news and the latest version of the tool at `sourceforge
<http://sourceforge.net/projects/quast>`_.  QUAST utilizes MUMmer,
GeneMarkS, GeneMark-ES, GlimmerHMM, and GAGE. 

To call ``quast.py`` we have to provide a reference genome and an assembly. The reference is usually
not available, of course::

  cd
  
  quast.py -t 16 -o ~/quast_canu_assembly -R ~/Reference/CXERO_10272017.fna ~/canu_assembly/canuAssembly.contigs.fasta

QUAST generates HTML reports including a number of interactive graphics. To access these reports, copy the
quast directory to your `www` folder::

  cp -r quast_canu_assembly ~/www/

Open port 80 in your VM, after that, you can load the reports in your web browser::

  http://YOUR_OPENSTACK_INSTANCE_IP/quast_canu_assembly/summary/report.html

