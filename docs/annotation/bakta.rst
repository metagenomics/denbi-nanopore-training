Annotating the genome with Bakta
=================================

The multiplex capability and high yield of current day DNA-sequencing instruments has made bacterial whole genome sequencing a routine affair. The subsequent de novo assembly of reads into contigs has been well addressed. The final step of annotating all relevant genomic features on those contigs can be achieved slowly using existing web- and email-based systems, but these are not applicable for sensitive data or integrating into computational pipelines. The Bakta command line application is widely used and one of the most established tools for bacterial genome annotation. It balances comprehensive annotation with computational efficiency via alignment-free sequence identifications. However, the usage of command line software tools and the interpretation of result files in various formats might be challenging and pose technical barriers. Here, we present the recent updates on the Bakta web server, a user-friendly web interface for conducting and visualizing annotations using Bakta without requiring command line expertise or local computing resources. Key features include interactive visualizations through circular genome plots, linear genome browsers, and searchable data tables facilitating the interpretation of complex annotation results. The web server generates standard bioinformatics outputs (GFF3, GenBank, EMBL) and annotates diverse genomic features, including coding sequences, non-coding RNAs, small open reading frames (sORFs), and many more. The development of an auto-scaling cloud-native architecture and improved database integration led to substantially faster processing times and higher throughputs. The system supports FAIR principles via extensive cross-reference links to external databases, including RefSeq, UniRef, and Gene Ontology. Also, novel features have been implemented to foster sharing and collaborative interpretation of results. The web server is freely available at https://bakta.computational.bio.

References
^^^^^^^^^^

**Bakta Web** https://doi.org/10.1093/nar/gkaf335

**Bakta Command Line** https://doi.org/10.1099/mgen.0.000685
