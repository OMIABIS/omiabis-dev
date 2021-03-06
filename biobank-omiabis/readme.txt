1. Make BFO 2 classes only based OMIABIS
===========================================

a. Separate terms defined in OMIABIS and those defined in the external resources (OBO Foundry Ontologies) in different OWL files

b. Convert the OWL files which used multiple versions of  BFO (1.1, pre-Graz, and 2.0) to BFO 2.0 only  OWL files using the script implemented using OWL-API

c. BFO 2 classes only based OMIABIS ontology contains following OWL files:
- omiabis.owl: OMIABIS specific terms
- import_OBI_subset.owl: OBI subset that OMIABIS built on, it contains both needed IAO and OBI terms
- import_OBO.owl: terms defined in external OBO Foundry ontologies and retrieved using OntoFox
- externalByhand.owl: terms defined in external OBO Foundry ontologies added manually

import BFO 2 classes only OWL file:
http://purl.obolibrary.org/obo/bfo/2014-05-03/classes-only.owl


NOTE: OMIABIS contains one BFO 1.1 class:
http://www.ifomis.org/bfo/1.1/span#ConnectedTemporalRegion
Because no mapped BFO 2.0 class was found.


2. Make BFO 2 classes only based BIOBANK
===========================================

a. Convert the OWL files which used BFO pre-Graz version to BFO 2.0 version OWL files using the script implemented using OWL-API

b. merged several imported OWL files for merging purpose

c. BFO 2 classes only based BIOBANK ontology contains following OWL files:
- biobank.owl: BIOBANK specific terms
- OBI_subset.owl: OBI subset that BIOBANK built on, it contains both needed IAO and OBI terms
- import_OBO.owl: terms defined in external OBO Foundry ontologies and retrieved using OntoFox
(Notes:import_OBO.owl was generated by merging import_CHEBI.owl and import_PATO.owl to import_OBO.owl)
- externalByhand.owl: terms defined in external OBO Foundry ontologies added manually

Terms imported manually in externalByhand.owl
	http://purl.obolibrary.org/obo/RO_0002222
	http://purl.obolibrary.org/obo/RO_0002223
Better to be added in OntoFox input file and imported using OntoFox


3. Merge two ontologies
===========================================

- Merged OBI subset OWL files used by OMIABIS and BIOBANK using Protege
	import_OBI_subset.owl

- Merged imported external terms using MIREOT mechanism OWL files used by OMIABIS and BIOBANK using Protege 
	import_OBO.owl
(Note: Protege has bugs in merging, it automatically add some objectProperties as annotationProperties during this process. Need to be manually cleaned up)

- Merged manually imported external terms OWL files used by OMIABIS and BIOBANK using Protege 
	externalByhand.owl
	
- Merged omiabis.owl and biobank.owl using Protege (both owl don't import any external owl files)
	biobank-omiabis.owl

- Add import statements to biobank-omiabis.owl, which are
	<owl:imports rdf:resource="http://purl.obolibrary.org/obo/bfo/2014-05-03/classes-only.owl"/>
	<owl:imports rdf:resource="http://purl.obolibrary.org/obo/biobank-omiabis/externalByhand.owl"/>
	<owl:imports rdf:resource="http://purl.obolibrary.org/obo/biobank-omiabis/import_OBI_subset.owl"/>
	<owl:imports rdf:resource="http://purl.obolibrary.org/obo/biobank-omiabis/import_OBO.owl"/>

- Made minor edits on merged ontology metadata, such as change all people as dc:contributor and indicated the ontology is a merged OMIABIS and BIOBANK ontology

- Deal with the left BFO 1.1 class (ConnectedTemporalRegion) 
http://www.ifomis.org/bfo/1.1/span#ConnectedTemporalRegion is defined as equivalent to union of its two subclasses (temporal_instant, temporal_interval)

Its two subclasses have mapped BFO 2.0 classes
temporal_instant -> zero-dimensional temporal region
temporal_interval -> one-dimensional temporal region

So replaced BFO 1.1 ConnectedTemporalRegion as (zero-dimensional temporal region or one-dimensional temporal region) in the axiom and removed it.

Only one term in OMIABIS used BFO1.1:ConnectedTemporalRegion, which is:
time in formalin measurement: A time measurement datum that states the duration during which a specimen was submerged in formalin. Based on the definiation, may consider to use 'one-dimensional temporal region' only.
In addition, IAO has relation 'is duration of' which relates a process to a time-measurement-datum that represents the duration of the process. It can consider to use 'is duration of' to simplify the axiom.


===========================================
The consistency was checked using Hermit 1.3.4. No inconsistency was found.

Note: OMIABIS:specimen ID is similar to OBI:specimen identifier, need to think how to merge the term.