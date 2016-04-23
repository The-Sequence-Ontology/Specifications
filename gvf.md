### Genome Variation Format 1.07

#### Table of Contents

[Summary](#summary)
-   [A Quick Explanation and Example of GVF Content](#quick_gvf_examples)
-   [GVF Specification Archive](#gvf_archive)
-   [GVF Wiki Pages](#gvf_wiki)

[Data Sets](#data_sets)
-   [10Gen Data set](#10gen_dataset)
-   [Data from ENSEMBL](#ensembl_data)
-   [Data from NCBI](#ncbi_data)

[Column Descriptions](#first_eight_columns)
[Character Escaping](#character_escaping)
[GVF Column 9 Attributes](#column_9_attributes)
-   [Attribute Summary](#attribute_summary)
-   [Community Attributes Collection](#community_attributes_collection)
-   [Attribute Definitions](#attribute_definitions)

[Pragmas](#pragmas)
-   [GFF3 Pragmas](#gff3_pragmas)
-   [GVF Pragmas](#gvf_pragmas)
-   [Simple Pragmas](#simple_pragmas)
-   [Structured Pragmas Summary](#structured_pragmas_summary)
-   [Structured Pragmas](#structured_pragmas)

[Comments](#comments)
[Database Cross References](#database_cross_references)
[Multi-individual GVF Files](#multi_individual_files)
[GVF Feature Examples](#gvf_feature_examples)
[Acknowledgments](#Acknowledgements)
[Change Log](#change_log)
#### Summary

*Version 1.07
28 April 2014*

The Genome Variation Format (GVF) is a very simple file format for describing sequence\_alteration features at nucleotide resolution relative to a reference genome. The GVF format was published in *Reese et al., Genome Biol., 2010;11(8):R88* [A standard variation file format for human genome sequences](http://genomebiology.com/2010/11/8/R88/abstract). We would like to acknowledge the contributing groups for their support.
Karen Eilbeck
Biomedical Informatics
University of Utah
Paul Flicek
ENSEMBL
EBI
Gabor Marth
Boston University
1000 Genomes Project
Martin Reese
Omicia Inc.
Lincoln Stein
Ontario Cancer Institute
Mark Yandell
Department of Human Genetics
University of Utah
GVF is a type of [GFF3](/resources/gff3.html) file with additional pragmas and attributes specified. The GVF format has the same nine-column tab-delimited format as GFF3. All of the requirements and restrictions specified for GFF3 apply to the GVF specification as well and thus a GVF file should be fully compatible with code used for processing and displaying GFF3 files. In addition, GVF adds additional constraints to some of these columns as described below.

See the [GFF3 Specification](/resources/gff3.html) for more details about GFF3.

##### A Quick Explanation and Example of GVF Content

The text in this blue box will give a very brief description of the GVF format for the impatient and those who only need to understand the simplest GVF content. This "blue box" description of GVF will likely suffice for most cases where describing simple SNV/indel features for a single genome in GVF format is the goal. Users seeking to achieve that goal do not need to read the entire specification. After this "blue box", the remainder of the document will explain the complete GVF format in detail.
GVF has two types of lines (empty lines and lines beginning with a single '\#' are allowed but ignored). Lines beginning with '\#\#' are *pragmas* which contain meta-data. All other lines are *feature lines*.

**Pragmas:**
-   Begin with '\#\#'
-   Contain meta-data.
-   Only this one is required:
    \#\#gvf-version 1.07

**Feature Lines**:
-   Nine tab-delimited columns.
-   seqid: The chromosome or contig on which the sequence\_alteration is located (text).
-   source: The source (i.e. an algorithm or database) of the sequence\_alteration (text.)
-   type: A SO term describing the type of sequence\_alteration (child term of [SO sequence\_alteration](http://www.sequenceontology.org/browser/current_release/term/SO:0001059)) or a [gap](http://www.sequenceontology.org/browser/current_svn/term/SO:0000730).
-   start: A 1-based integer for the begining of the sequence\_alteration locus on the plus strand (integer).
-   end: A 1-based integer of the end of the sequence\_alteration on plus strand (integer).
-   score: A ([Phred scaled](http://en.wikipedia.org/wiki/Phred_quality_score) probability that the sequence\_alteration call is incorrect (real number).
-   strand: The strand of the feature (+/-).
-   phase: This column allows compatibility with GFF3 (.).
-   attributes: Tag/value pairs describing attributes of the sequence\_alteration (tag1=value1,value2;tag2=value1;).
    -   ID: A file-wide unique identifier (required).
    -   Variant\_seq: All unique sequences seen in the individual described in the file at the features locus - including the reference sequence if appropriate (required). Any IUPAC nucleotide symbol is allowed, but usually just A, T, G, C. Plus any of the following:
        -   . (period): Unknown/missing value.
        -   - (hyphen): No sequence (i.e. for a homozygous deletion Variant\_seq=-;).
        -   ~ (tilde): Place holder for a sequence too long to show. The tilde can be followed by an integer that describes the length of the omitted sequence.
        -   @ (at): An alias for the sequence found in the Reference\_seq attribute.
        -   ! (exclamation): Place holder for the missing sequence at a hemizygous locus.
        -   ^ (caret): Place holder for the missing sequence at a location without enough data to make an accurate call (no-call locus).
    -   Reference\_seq: The reference sequence. Nucleotide characters as well as '-' and '~' as described above.

The following characters must be [URL-encoded](http://en.wikipedia.org/wiki/Percent-encoding) throughout the GVF document except as noted:

| Char                    | Exception          | Escape       |
|-------------------------|--------------------|--------------|
| tab                     | separating columns | %09          |
| newline                 | end of lines       | %0A          |
| carriage return         | end of lines       | %0D          |
| All control charachters | None               | %00-%1F, %7F |

The following characters have special meaning and need to be [URL-encoded](http://en.wikipedia.org/wiki/Percent-encoding) throughout column 9 when not used as specified:

| Char            | Escape |
|-----------------|--------|
| ';' (semicolon) | %3B    |
| '=' (equals)    | %3D    |
| '%' (percent)   | %25    |
| '&' (ampersand) | %26    |
| ',' (comma)     | %2C    |

A few lines of single nucleotide variants (SNV) are shown below as an example of a very simple GVF file. Scroll right to see the complete lines..

\#\#gvf-version 1.07 \#\#genome-build NCBI B36.3 \#\#sequence-region chr16 1 88827254 chr16 samtools SNV 49291141 49291141 . + . ID=ID\_1;Variant\_seq=A,G;Reference\_seq=G; chr16 samtools SNV 49291360 49291360 . + . ID=ID\_2;Variant\_seq=G;Reference\_seq=C; chr16 samtools SNV 49302125 49302125 . + . ID=ID\_3;Variant\_seq=T,C;Reference\_seq=C; chr16 samtools SNV 49302365 49302365 . + . ID=ID\_4;Variant\_seq=G,C;Reference\_seq=C; chr16 samtools SNV 49302700 49302700 . + . ID=ID\_5;Variant\_seq=T;Reference\_seq=C; chr16 samtools SNV 49303084 49303084 . + . ID=ID\_6;Variant\_seq=G,T;Reference\_seq=T; chr16 samtools SNV 49303156 49303156 . + . ID=ID\_7;Variant\_seq=T,C;Reference\_seq=C; chr16 samtools SNV 49303427 49303427 . + . ID=ID\_8;Variant\_seq=T,C;Reference\_seq=C; chr16 samtools SNV 49303596 49303596 . + . ID=ID\_9;Variant\_seq=T,C;Reference\_seq=C;

##### GVF Specification Archive

Previous versions of the GVF specification are archived. Please see the [GVF Specification Archive](gvf_archives.html) to find these older versions of the specification.

##### GVF Wiki Pages

While this page provides the official description of the GVF format with some simple examples, several [GVF wiki pages](/so_wiki/index.php/Main_Page#GVF_Documentation) have been set up to describe the GVF format in a more tutorial manner, to provide additional examples and to document implementations of GVF files by the GVF community.

#### Data Sets

##### 10Gen Data set

The SNVs from ten of the first sequenced individual human genomes have been converted to GVF format and are made available as the [10Gen Data set](/resources/10Gen.html).

##### Data from ENSEMBL

ENSEMBL is providing GVF files for their sequence\_alteration data sets at:

-   <ftp://ftp.ensembl.org/pub/current_variation/gvf/>

##### Data from NCBI

The dbVar database at NCBI is providing GVF files for their structural variant data at:

-   <ftp://ftp.ncbi.nlm.nih.gov/pub/dbVar/data/Homo_sapiens/by_assembly/NCBI36/gvf/>
-   <ftp://ftp.ncbi.nlm.nih.gov/pub/dbVar/data/Homo_sapiens/by_assembly/GRCh37/gvf/>

#### Column Descriptions

Sequence alterations are described in a GVF file with 9 tab-delimited columns. The columns of a GVF file are inherited from GFF3 with additional constraints placed on some columns. A brief descriptions of each column follows:

Column 1 "seqid":  
The ID of the landmark used to establish the coordinate system for the current feature. IDs may contain any characters, but must escape any characters not in the set \[a-zA-Z0-9.:^\*$@!+\_?-|\]. In particular, IDs may not contain unescaped white space and must not begin with an unescaped "&gt;". Although the GVF format allows colons within the seqid values, be aware that some applications disallow it and thus it is recommended that colons be avoided in seqid.

Column 2: "source"  
The source is a free text qualifier intended to describe the algorithm or operating procedure that generated this feature. Typically this is the name of a piece of software, such as "MAQ" or a database name, such as "dbSNP". Although the value of source is not constrained, the \#\#source-method pragma may be used to describe the source in more detail.

Column 3: "type"  
The type of the feature. This is constrained to be either: (a) the SO term sequence\_alteration ([SO:0001059](http://www.sequenceontology.org/browser/current_svn/term/SO:0001059)), (b) a child term of sequence\_alteration, (c) the SO term gap ([SO:0000730](http://www.sequenceontology.org/browser/current_svn/term/SO:0000730)), or (d) the SO accession number for any of the previous terms. The gap feature, while not a sequence\_alteration, provides a way to annotate gaps in the individuals genome assembly where sequence\_alteration information is unknown (low-coverage, no-call regions).

Columns 4 & 5: "start" and "end"  
The start and end of the feature, in 1-based integer coordinates, relative to the landmark given in column 1. Start is always less than or equal to end. For features that cross the origin of a circular feature (e.g. most bacterial genomes, plasmids, and some viral genomes), the requirement for start to be less than or equal to end is satisfied by making end = the position of the end + the length of the landmark feature. For zero-length features, such as an insertion, start equals end and the implied site is to the three-prime of the indicated base in the direction of the landmark.

Column 6: "score"  
The score of the feature, an integer or floating point number. The semantics of the score are not defined, however it is strongly recommended that a [Phred scaled quality score](http://en.wikipedia.org/wiki/Phred_quality_score) be used whenever possible. While the semantics of score are not defined, the \#\#score-method pragma may be used to describe how the score was calculated.

Column 7: "strand"  
The strand of the feature. The '+' (plus sign) for positive strand (relative to the landmark), and the '-' (minus sign) for minus strand, and '.' (period) for features that are not stranded. In addition, '?' (question mark) can be used for features whose strandedness is relevant, but unknown.

Column 8: "phase"  
The phase column is not used in GVF, but is maintained with the placeholder '.' (period) for compatibility with GFF3 and tools that conform to the GFF3 specification.

Column 9: "attributes"  
The ninth column in GFF3/GVF contains one or more tag/value pairs that describe attributes of the feature. Tags are separated from values by an '=' (equal sign). Multiple values for a given tag are separated by a ',' (comma). Sets of tag/value pairs are separated from each other by a ';' (semicolon). Thus any use of any of the charachters ';=,' must be [URL-encoded](http://en.wikipedia.org/wiki/Percent-encoding).

#### Character Escaping

The following characters must be [URL-encoded](http://en.wikipedia.org/wiki/Percent-encoding) throughout the GVF document except as noted:

| Char                    | Exception          | Escape       |
|-------------------------|--------------------|--------------|
| tab                     | separating columns | %09          |
| newline                 | end of lines       | %0A          |
| carriage return         | end of lines       | %0D          |
| All control charachters | None               | %00-%1F, %7F |

The following characters have special meaning and need to be [URL-encoded](http://en.wikipedia.org/wiki/Percent-encoding) throughout column 9 when not used as specified:

| Char            | Escape |
|-----------------|--------|
| ';' (semicolon) | %3B    |
| '=' (equals)    | %3D    |
| '%' (percent)   | %25    |
| '&' (ampersand) | %26    |
| ',' (comma)     | %2C    |

#### GVF Column 9 Attributes

##### Attribute Summary

Column 9 in GFF3/GVF allows use of tag-value pairs to define attributes of a feature. All of the GFF3 attribute tag-value pairs are supported in GVF as well. Note in particular that the GFF3 attribute tags ID is required and Alias and Dbxref are often useful in GVF. GVF specifies the additional attribute definitions given below. As specified by GFF3, all attribute tags beginning with upper-case letters are reserved for future use by the GFF3 and GVF specifications, however applications are free to use attributes beginning with lower-case letters to provide application specific data.

##### Community Attributes Collection

The Sequence Ontology provides a collection of descriptions of attributes in use by the GVF community. These descriptions are of organization specific attributes (those attributes beginning with a lower case letter that are specific to that organizations data sets) or they provide clarification about how the organizations data sets fit within the existing attribute structure. Note that existing attributes should not be redefined here. This information is provided as a wiki. Please refer to [GVF Community Supported Attributes](/so_wiki/index.php?title=GVF_Community_Supported_Attributes) for more information. If you would like to maintain a description of attributes in use by your organization please [contact the Sequence Ontology group](/about/contacts.html) to set up an account on the wiki.

##### Attribute Definitions

Attributes may describe information about the locus of the sequence\_alteration (i.e. it's ID, a Dbxref or the Reference\_seq) or they may describe information about a particular individual's data at that locus (i.e. Total\_read, Genotype). When a GVF file contains data for [multiple individuals]() the second type of attribute needs to specify different values for each individual. In the attribute definitions below a "Scope" is described for each attribute with a value of "Individual" or "Locus". This distinction is relevant to multi-individual files where attributes that describe individual data contain a comma separated list of values for each individual that is variant at the locus. The details of this are described below in the section [Multipe Individual GVF files]().

ID  
-   **Required:** Yes
-   **Scope:** Locus
-   **Description:**

    The ID attribute is used to assign an ID to a feature. As in GFF3 this ID must be unique within the file and is not required to have meaning outside of the file.

-   **Valid Values:** [Any valid character(s)](#character_escaping).
-   **Format:** Single value.
-   **Example:**

ID=SNV\_00123;

Alias  
-   **Required:** Optional
-   **Scope:** Locus
-   **Description:**

    A secondary name for the feature. This attribute can be used to provide, among other things, [HGVS nomenclature](http://www.hgvs.org/mutnomen/), or [ISCN nomenclature](http://content.karger.com/ProdukteDB/produkte.asp?Aktion=showproducts&searchWhat=books&ProduktNr=244102) names for the sequence\_alteration as described below in the Format section. Note that cross-references to sequence\_alterations in established databases (i.e. dbSNP, OMIM) should use the Dbxref attribute described below.

-   **Valid Values:** [Any valid character(s)](#character_escaping)
-   **Format:**

    The format for the Alias attribute is a single free text value. However if the text begins with HGVS: or ISCN:, then the name given should comply with the given nomenclature system.

-   **Example:**

An alias for the 3 three base-pair deletion that is the most common cause of Cystic Fibrosis.

Alias=delta-F508

A sequence\_alteration described in [HGVS nomenclature](http://www.hgvs.org/mutnomen/).

Alias=HGVS:NM\_004006.2:c.3G&gt;T

A sequence\_alteration described in [ISCN nomenclature](http://content.karger.com/ProdukteDB/produkte.asp?Aktion=showproducts&searchWhat=books&ProduktNr=244102). Shown here unescaped, but note the URL escaping rules described below.

Alias=ISCN:45,XY,t(13q,14q)

Note that some characters in HGVS and ISCN nomenclature (for example ';=%&,') will need to be [escaped in GVF](#character_escaping). For example the ISCN alias above should appear as:

Alias=ISCN:45%2CXY%2Ct(13q%2C14q)

Dbxref  
-   **Required:** Optional
-   **Scope:** Locus
-   **Description:**

    A database cross reference. This can be useful to associate this sequence\_alteration with the same sequence\_alteration previously described in databases such as dbSNP or OMIM. See the [Database Cross References](#database_cross_references) section below for more details.

-   **Valid Values:** Any valid combination of DB and ID. The DB abbreviations are described [below](#database_cross_references).
-   **Format:** A colon separated database name and database ID pair.
-   **Example:**

A dbSNP database cross-reference.

Dbxref=dbSNP:rs3131969;

An OMIM database cross-reference.

Dbxref=OMIM:605956.0004;

Variant\_seq  
-   **Required:** Required
-   **Scope:** Locus
-   **Description:**

    All sequences (alleles) found in an individual (or group of individuals) at a variable locus are given with the Variant\_seq attribute as a comma separated list. Note that the reference sequence is included here as well, when appropriate (for example when the locus is heterozygous). The sequence should be given for the strand described (in column 7) for this feature. If the feature is on the minus strand then the sequence given here is the reverse-compliment of the reference genome for these coordinates.

-   **Valid Values:**

    Any nucleic acid sequence using [IUPAC ambiguity codes](http://en.wikipedia.org/wiki/Nucleic_acid_notation#IUPAC_notation). Note, however, that ambiguity codes are used to represent uncertainty of a given sequence at a locus, and should never be used to represent the multiple altered sequences present at a given locus. For example if both an A and a T are seen at a locus then both the A and the T should be listed, not a W which is the ambiguity code for A or T. If a W is used, it means that only one sequence is seen at this locus and that it is unclear whether that sequence is an A or a T.

    In addition to the observed nucleic acid sequence, several other characters (.-~@!^) are valid values in the Variant\_seq attribute. Use of these characters is described below with examples.

-   **Format:** A comma separated list of the valid values.
-   **Nucleotide Examples:**

    A homozygous SNV locus

    Variant\_seq=A;Reference\_seq=T;

    A heterozygous SNV locus

    Variant\_seq=A,T;Reference\_seq=T;

    A triallelic locus from a multi-individual GVF file where three nucleotides have been seen within the population.

    Variant\_seq=A,C,T;Reference\_seq=T;

-   **Other Examples:**
    . (period)  
    The '.' character is used to represent missing data. There are more specific ways to represent specific kinds of missing data (hemizygous and no-call loci) with other characters described below, and those more specific characters should always be used preferentially where appropriate.

    The site is known to be altered, but it is not known whether the site is homozygous, heterozygous, hemizygous or could not be called due to missing data, so the '.' character is used to represent this ambiguity.

    Variant\_seq=A,.;Reference\_seq=T

    - (minus sign/hyphen)  
    The '-' character denotes a lack of sequence (e.g. in the case of an insertion or deletion) in either the Variant\_seq or Reference\_seq attribute. For a deletion feature there is no corresponding sequence to list in the Variant\_seq attribute and likewise there is no corresponding Reference\_seq attribute sequence for an insertion.

    A heterozygous deletion.

    Variant\_seq=ATC,-;Reference\_seq=ATC;

    ~\[INT\] (equivalency sign/tilde) optionally followed by an integer  
    The '~' character can be used as a place holder for a sequence that is too long to include in the Variant\_seq or Reference\_seq attribute. The '~' character may be optionally followed by an integer which gives the length of the sequence represented by the '~'.

    A large heterozygous deletion. In this case the length and sequence of the region deleted can be inferred from the coordinates of the feature, so the optional integer representing the length of the deletion is not included, although it could have been included as shown below for the insertion.

    Variant\_seq=-,~;Reference\_seq=~;

    A large heterozygous insertion 837 bp long. With an insertion, the length of the inserted sequence can not be determined if the '~' character is used, so we include the length as an integer after the '~'.

    Variant\_seq=~837,-;Reference\_seq=-;

    ! (exclamation point/bang)  
    The '!' character is use to denote that the site is hemizygous as it would otherwise be interpreted as a homozygous site.

    A hemizygous SNV on chrY.

    Variant\_seq=A,!;Reference\_seq=T;

    ^ (caret/circumflex)  
    The '^' character is used to denote a site where a sequence\_alteration could not be called (no-call) because of a lack of sufficient underlying data (lack of coverage or poor quality base-calls or alignments at the locus). This allows a distinction to be made between loci that are homozygous reference from those where there simply isn't any (or enough) data to know what the sequence at the locus is.

    An SNV where there is enough sequence coverage to call the SNV, but not enough to determine if the site is heterozygous and homozygous.

    Variant\_seq=A,^;Reference\_seq=T;

    A locus where there is no data (or insufficient data) to determine if the site is variant or not. Note that a gap feature can be used to describe a region of missing data.

    Variant\_seq=^;Reference\_seq=T;

Reference\_seq  
-   **Required:** Required
-   **Scope:** Locus
-   **Description:**

    The sequence corresponding to the seqid, start and end coordinates of this feature in the associated genomic sequence file. A single value should be given for the Reference\_seq. In the case where the sequence\_alteration represents an insertion relative to the reference, the Reference\_seq is given as '-'. The sequence should be given relative to the strand described (in column 7) for this feature. If the feature is on the minus strand then the sequence given for the Reference\_seq attribute is the reverse-compliment of the reference genome for this feature's coordinates.

-   **Valid Values:**

    Any nucleic acid sequence using [IUPAC ambiguity codes](http://www.ncbi.nlm.nih.gov/pubmed/2582368).

    In addition to the observed nucleic acid sequence, two other characters (-~) are valid values in the Reference\_seq attribute. Use of these characters is described below.

-   **Nucleotide Examples:**
    Reference\_seq=A;

-   **Other Examples:**
    - (minus sign/hyphen)  
    The '-' character denotes a lack of sequence (e.g. in the case of an insertion) in the Reference\_seq attribute. For an insertion feature there is no corresponding sequence to list in the Reference\_seq attribute.

    A homozygous insertion.

    Variant\_seq=ATC;Reference\_seq=-'

    ~\[INT\] (equivalency sign/tilde) optionally followed by an integer  
    The '~' character can be used as a place holder for a sequence that is too long to include in the Reference\_seq attribute. The '~' character may be optionally followed by an integer which gives the length of the sequence represented by the '~' although this integer is allowed, it is unnecessary in the case of the Reference\_seq attribute since this can be inferred from the start and end coordinates.

    A large heterozygous deletion. In this case the length and sequence of the region deleted can be inferred from the coordinates of the feature, so the optional integer representing the length of the deletion is not included.

    Variant\_seq=-,~;Reference\_seq=~;

Variant\_reads  
-   **Required:** Optional
-   **Scope:** Individual
-   **Description:**

    The number of reads supporting each altered sequence at this location given in the form Variant\_reads=integer. Read counts for sequence\_alterations longer than one nucleotide should be average read counts for each position over the length of the sequence\_alteration. The order of the reads must be in the same order that the corresponding sequences in the Variant\_seq attribute. A '.' character may be used as a placeholder to signify missing data. Multiple values which correspond to the number of reads for each Variant\_seq are separated by colons (:). Multiple sets of colon separated values (when specifying multi-individual GVF files) are separated from each other by commas.

-   **Valid Values:** Colon separated integer or '.' values.
-   **Format:** Comma separated list of colon separated integer sets.
-   **Example:**

An SNV where the number of reads supporting an A is 13, but the number of reads supporting a T is not known.

Variant\_seq=A,T;Variant\_reads=13:.;

An SNV from a multi-individual GVF file. The first individual has 13 and 10 variants respectively for the A, and T variant sequences and the second individual has 23 reads supporting the A variant sequence and no reads supporting T variant sequence.

Variant\_seq=A,T;Variant\_reads=13:10,23:0;Individual=3,8;

Total\_reads  
-   **Required:** Optional
-   **Scope:** Individual
-   **Description:**

    The total number of reads covering a sequence\_alteration. Total read counts for sequence\_alterations longer than one nucleotide should be average read counts for each position over the length of the sequence\_alteration. The sum of reads given in Variant\_reads is not required to be the same as Total\_reads. A '.' character may be used to signify missing data. Multiple values (when specifying multi-individual GVF files) are separated from each other by commas.

-   **Valid Values:** Integer or '.'
-   **Format:** A single value.
-   **Example:**

The total reads covering a locus in a single-individual GVF file.

Total\_reads=35;

The total reads covering a locus in a multi-individual GVF file. The first individual variant at this locus has 35 total read supporting the locus and the second individual has 75.

Total\_reads=35,75;Individual=3,8;

Zygosity  
-   **Required:** Optional
-   **Scope:** Individual
-   **Description:**

    The zygosity of this locus where zygosity is heterozygous, homozygous or hemizygous. This attribute is optional as the zygosity can be inferred from the value of the Variant\_seq attribute. A '.' character may be used as a placeholder to signify missing data. Multiple values (when specifying multi-individual GVF files) are separated from each other by commas.

-   **Valid Values:** The values heterozygous, homozygous, hemizygous, or the '.' character.
-   **Format:** A single value.
-   **Example:**

A heterozygous locus

Variant\_seq=A,T;Zygosity=heterozygous;

A homozygous locus

Variant\_seq=A;Zygosity=homozygous;

A hemizygous locus

Variant\_seq=A,!;Zygosity=hemizygous;

Zygosity in a multi-individual GVF file. The first individual variant at this locus is homozygous and the second individual is heterozygous.

Variant\_seq=A;Zygosity=homozygous,heterozygous;

Variant\_freq  
-   **Required:** Optional
-   **Scope:** Locus
-   **Description:**

    A real number describing the frequency of the sequence\_alteration in a population. The details of the source of the frequency should be described in an attribute-method pragma as described below. The order of the values given must be in the same order that the corresponding sequences occur in the Variant\_seq attribute. A '.' character may be used as a placeholder to signify missing data.

-   **Valid Values:** Real numbers or the '.' character.
-   **Format:** Comma separated list
-   **Example:**

Variant\_seq=A,T;Variant\_freq=0.05,0.95;

Variant\_effect  
-   **Required:** Optional
-   **Scope:** Locus
-   **Description:**

    The effect that a sequence\_alteration has on a sequence\_feature that overlaps it.

-   **Format:**

    Four white-space delimited fields:

    Variant\_effect=sequence\_variant index feature\_type feature\_ID feature\_ID

-   **Valid Values:**

    -   The first field is a term that describes the effect of the sequence\_alteration on a sequence feature and must be the SO term [sequence\_variant](http://www.sequenceontology.org/browser/current_release/term/SO:0001060) or one of its children (a term with an is\_a relationship to sequence\_variant).
    -   The second field is a 0-based index value that identifies which Variant\_seq the effect is being described for. For example, if this Variant\_effect attribute is describing the effect of the first sequence in the Variant\_seq attribute then the index value would be 0, for a Variant\_effect attribute describing the second sequence in the Variant\_seq attribute the index value would be 1 and so on (see the examples below). The index is necessary because multiple Variant\_effect values may refer to the same Variant\_seq.
    -   The third field is a term describing the sequence feature that is being affected. This term must be the SO term sequence\_feature or one of its children (a term with an is\_a reltaionship to sequence\_feature).
    -   The fourth field (and all additional feilds) is/are feature ID(s). These feature IDs correspond to ID attributes in a GFF3 file that describe the sequence features (for example genes or mRNAs) annotated for the genome that are affected by this variant locus. Applications are optionally allowed to append a matched pair of parenthesis to the end of each ID. These parenthesis can contain additional application specific detail about the effect on this feature (for example the start and end of the altered seueqnce in the coordiantes of the feature). This additional detail is currently not defined by the GVF specification. It is the responsibility of the application to describe this additional data for the user. Allowing this additional data within the parenthesis is not however intended to support free text descriptions and must not violate the escaping conventions of the GVF specification or contain whitespace.

    White space is not allowed within any of the four fields. Both of the SO terms can be given either as a valid SO term name or the SO ID. If a single Variant\_seq has different effects on different sequence features (for example it causes a non\_synonymous\_codon change in one mRNA, but a synonymous\_codon change in another mRNA) then multiple Variant\_effect values are given separated by commas.

-   **Format:** Four space separated values.
-   **Examples:**

An SNV who's sequence 'A' causes a non-synonymous codon change in the mRNAs NM\_001160184 and NM\_032129.

Variant\_seq=A,T;Variant\_effect=non\_synonymous\_codon 0 mRNA NM\_001160184 NM\_032129;

An SNV who's sequence 'A' causes a non-synonymous codon change in the mRNAs NM\_001160184 and NM\_032129. Parentheses contain application specific data. In this case the additional data represents the location, of the altered sequence in the coordinates of the feature; the reference and alternate codons and the reference and altered amino acids (start:end|ref\_codon:alt\_codon|ref\_aa:alt\_aa). The location of the altered sequences differs between the two mRNA because they are different splice forms.

Variant\_seq=C,T;Variant\_effect=non\_synonymous\_codon 0 mRNA NM\_001160184(794:794|TTC:TCC|F:S) NM\_032129(758:758|TTC:TCC|F:S);

Start\_range End\_range  
-   **Required:** Optional
-   **Scope:** Locus
-   **Description:**

    The Start\_range and End\_range attributes describe ambiguity in the start and end coordinates given in column 4 and 5. The description below is given for Start\_range, but the same rules apply for End\_range. The value is required to be two integers (or a '.' for unknown values) separated by a comma. The first value defines a range of ambiguity less-than the value given in column 4 and must be less-than or equal-to that coordinate (or '.') The second value defines a range of ambiguity greater-than the coordinate specified in column 4 and must be greater-than or equal-to the value of that coordinate (or '.'). If either value is equal-to the coordinate in column 4 then there is no ambiguity in that direction. The values given for these attributes are always relative to the landmark feature just as they are for the coordinates given in columns 4 and 5.

-   **Valid Values:** Two integers
-   **Format:** Comma separated
-   **Example:**

A sequence\_alteration that has a start coordinate of 2000 and an end coordinate of 5000, but for which there is 500 bp of ambiguity for both coordinates in either direction.

Start\_range=1500,2500;End\_range=4500,5500

Phased  
-   **Required:** Optional
-   **Scope:** Individual
-   **Description:**

    The Phased attribute can be used in conjuction with the Genotype attribute to define a set of sequence\_alterations whose genotypes are phased (on the same chromosome). A phased genotype indicates that for heterozygous loci it is known which chromosome each sequence\_alteration sequence belongs to. The ordering of the integer pairs in the Genotype attributes for a phased set of sequence\_alterations is constrained for phased loci such that all sequences that occur together on one landmark feature (chromosome) are referenced by the same position in the Genotype tag. For example, if an individual has a sequence\_alteration with 'Variant\_seq=A,C;Genotype=0:1' attributes and another sequence\_alteration on the same landmark feature (chromosome) has 'Variant\_seq=G,T;Genotype=1:0' attributes then the A (0) and T (1) occur together on one copy of the chromosome because the both occupy first position of the integer pair in the Genotype attributes. Likewise, the C (1) and G (0) occur together on the other copy of that chromosome. If an entire GVF file (or a defined subset of it - for example all SNVs) is phased, then the '\#\#phased-genotypes' pragma described below is used. However, if only regions of the genome are phased; the Phased attribute allows you to specify those features and to describe which features are phased together if mutliple phased regions exist. The Phased attribute only has meaning for loci that are heterozygous, but may be given for any sequence\_alteration. The value of the Phased attribute can be any alphanumeric value. This value will serve as an ID for (and will be shared by all) features that are phased together on the given landmark feature and is not required to have meaning outside the file or in other contexts within the file. If multiple regions of the chromosome are phased then separate values should be used to group each set of phased features. To avoid ambiguity the same value should not be used for more than one phased region even if they lie on different landmark features (chromosomes). Multiple values (when specifying multi-individual GVF files) are separated from each other by commas.

-   **Valid Values:** An alphanumeric string.
-   **Format:** A single value or comma separated list for multi-individual files.
-   **Example:**

This sequence\_alteration is in phase with all other sequence\_alterations with the Phased=A13 attribute set.

Phased=A13;

Genotype  
-   **Required:** Required if the \#\#multi-individual pragma is in use, optional otherwise.
-   **Scope:** Individual
-   **Description:**

    The Genotype attribute describes which sequence(s) in the Variant\_seq attribute are present in an individual. This attribute is useful (and in fact is required) when the \#\#multi-individual pragma (see below) is in use. Two colon separated integers are given as the value, and multiple comma separated integer pairs can be given. The integers are a 0-based index to the values given in the Variant\_seq attribute.

-   **Valid Values:** Colon separated integers (one value for each chromosome).
-   **Format:** Comma separated list of colon separated integer sets.
-   **Example:**

The individual has an A on one chromosome and a T on the other at this locus.

Variant\_seq=A,T;Genotype=0:1

There are two individuals in this multi-individual GVF file. The first individual has an A,T genotype and the second has a T,T genotype.

Variant\_seq=A,T;Genotype=0:1,1:1

Individual  
-   **Required:** Optional
-   **Scope:** Locus
-   **Description:**

    The Individual attribute is used in conjunction with the \#\#multi-individual pragma to implement multi-individual GVF files. The value for this attribute is a comma separated list of integers. These integers are a 0-based index into the IDs given in the \#\#multi-individual pragma and indicate which individuals have sequence\_alterations at this locus. In addition, the order of values given in other individual specific attributes corresponds to the order of the individuals specified here.

    See the section on multi-individual GVF files below.

-   **Valid Values:** Integer
-   **Format:** Comma separated list.
-   **Example:**

Individual NA18507 and NA19240 both have sequence\_alteration at this locus in the multi-individual GVF file with data for three individuals.

\#\#gvf-version 1.07 \#\#multi-individual NA18507, NA12878, NA19240 chr1 MAQ SNV 1023489 1023489 129 + . ID=chr1:maq\_cns2snp:SNV:1023489;Variant\_seq=C,G;Reference\_seq=C;Individual=0,2;Genotype=1:1,0,1;

Variant\_codon  
-   **Required:** Optional
-   **Scope:** Locus
-   **Description:**

    The Variant\_codon attribute can be used to describe the codon(s) that overlap a feature as modified by the sequence(s) in the Variant\_seq attribute. All codons are given in their entirety, so if the feature partially overlaps a codon, the codon is still given in it's entirety. If the feature overlaps some coding region and some non-coding region then the codons are given for the overlapped portion of the coding sequence. All codons are given as a single string of sequence the length of which must be divisible by three. One codon sequence string must be given for each sequence in the Variant\_seq attribute separated by commas. This attribute can be provided as a convinience for users, however, there is potential for ambiguity if the sequence\_alteration at this locus causes different codons on different mRNAs. Please consider using the Variant\_effect attribute as shown in the example above for a more detailed way to describe codons on multiple mRNAs.

-   **Valid Values:** Nucleic acid sequence (the length of which is divisible by 3).
-   **Format:** A comma separated list of nucleic acid sequences.
-   **Example:**

An SNV overlaps one nucleotide in the given codon of the mRNA.

Variant\_seq=A,G;Reference\_seq=A;Variant\_codon=GAG,GGG;

Reference\_codon  
-   **Required:** Optional
-   **Scope:** Locus
-   **Description:**

    The Reference\_codon attribute can be used to describe the codon(s) that overlap a feature on the reference genome. All codons are given in their entirety, so if the feature partially overlaps a codon, the codon is still given in it's entirety. If the feature overlaps some coding region and some non-coding region then the codons are given for the overlapped portion of the coding sequence. All codons are given as a single string of sequence the length of which must be divisible by three. Only one codon sequence string may be given since the reference genome sequence is represented as haploid.

-   **Valid Values:** Nucleic acid sequence (the length of which is divisible by 3).
-   **Format:** A nucleic acid sequence.
-   **Example:**

An SNV overlaps one nucleotide in the given codon (on the reference genome sequence) of the mRNA.

Variant\_seq=A,G;Reference\_seq=A;Reference\_codon=GAG;

Variant\_aa  
-   **Required:** Optional
-   **Scope:** Locus
-   **Description:**

    The Variant\_aa attribute can be used to describe the amino acid(s) that overlap a feature as modified by the sequence(s) in the Variant\_seq attribute. All overlapping amino acids are given, so if the feature partially overlaps an underlying codon, the amino acid is still given. If the feature overlaps some coding region and some non-coding region then the amino acids are given for the overlapping portion of the coding sequence. All amino acids are given as a single string of sequence. One amino acid sequence string must be given for each sequence in the Variant\_seq attribute separated by commas. This attribute can be provided as a convinience for users, however, there is potential for ambiguity if the sequence\_alteration at this locus causes different codons on different mRNAs. Please consider using the Variant\_effect attribute as shown in the example above for a more detailed way to describe codons on multiple mRNAs.

-   **Valid Values:** Amino acid sequence (single letter code).
-   **Format:** A comma separated list of amino acid sequences.
-   **Example:**

An SNV overlaps one nucleotide in the given amino acid of the mRNA

Variant\_seq=A,G;Reference\_seq=A;Variant\_aa=E,G;

Reference\_aa  
-   **Required:** Optional
-   **Scope:** Locus
-   **Description:**

    The Reference\_aa attribute can be used to describe the amino acid(s) that overlap a feature on the reference genome. All amino acids are given, so if the feature partially overlaps a portion of a codon, the amino acid is still given in it's entirety. If the feature overlaps some coding region and some non-coding region then the amino acids are given for the overlapping portion of the coding sequence. All amino acids are given as a single string of sequence. Only one amino acid sequence string may be given.

-   **Valid Values:** Amino acid sequence.
-   **Format:** A single amino acid sequence.
-   **Example:**

An SNV overlaps one nucleotide of the codon for the given amino acid (reference genome sequence) of the mRNA.

Variant\_seq=A,G;Reference\_seq=A;Reference\_aa=E;

Breakpoint\_detail  
-   **Required:** Optional
-   **Scope:** Locus
-   **Description:**

    The Breakpoint\_detail attribute can be used to describe the source or destination of a zero-length sequence\_alteration. Examples of these sequence\_alterations are insertion, duplication, and translocation. The seqeunce correspoding to an insertion can be given in the Variant\_seq attribute, but in cases where this sequence is very long the inserted sequence can be included with it's own seqid in the fasta file (or fasta portion of the GVF file) and referenced with the Breakpoint\_detail attribute. In the case of a duplication, the source sequence of the duplication within the associated reference genome sequence can be referenced. When specifying a translocation, the destination of the translocated sequence within the genome can be specified. In general the format is seqid:start-end:strand (e.g. chr1:12345-67890:+). The seqid describes the landmark feature (chromosome) where the source/destination sequence is located. Note that the seqid is allowed to contain a colon, and thus care must be taken in parsing this field. The start and end values describe the coordinates of the source/destination sequence. The start value given alone specifies another break point in the genome (i.e. the destination of a translocation). The strand specifies the strand from which the source/destination sequence should be read. In the case of a translocation the strand specifies the strand on which the sequence continues at the destination.

-   **Valid Values:** The seqid, start, end and strand.
-   **Format:** seqid:start-end:strand
-   **Example:**

A duplication that originated from a region on the plus strand of chr1 begining at 12345 and ending at 67890.

Breakpoint\_detail=chr1:12345-67890:+;

A translocation that causes the sequence of the forward strand at this breakpoint to continue on the minus strand at chr1:12345.

Breakpoint\_detail=chr1:12345:-;

Breakpoint\_range  
-   **Required:** Optional
-   **Scope:** Locus
-   **Description:**

    The Breakpoint\_range attribute describes ambiguity in the coordinate given in Breakpoint\_detail. The value is required to be two integers (or a '.' for unknown values) separated by a comma. The first value defines a range of ambiguity less-than the value given in Breakpoint\_detail and must be less-than or equal-to that coordinate (or '.') The second value defines a range of ambiguity greater-than the coordinate specified in Breakpoint\_detail and must be greater-than or equal-to the value of that coordinate (or '.'). If either value is equal-to the coordinate in Breakpoint\_detail then there is no ambiguity in that direction.

    *The Breakpoint\_range attribute describes ambiguity in the coordinate(s) given in Breakpoint\_detail. The value is required to be two comma separated integers (or a '.' for unknown values) for each coordinate given in the Breakpoint\_detail attribute. The Breakpoint\_detail attribute can descirbe a single coordinate (in the case of a translocation, e.g. chr1:67890 or coordinate range using two coordinates (in the case of insertions and duplications, e.g. chr1:12345-67890). Thus if two coordinates are given in Breakpoint\_detail, four integers (or unknown values) must be given in Breakpoint\_range and the first two values of Breakpoint\_range are associated with the first coordinate in Breakpoint\_deail while the second two values of Breakpoint\_range are associated with the second coordinate of Breakpoint\_detail. The first value of each pair of values in Breakpoint\_range defines a range of ambiguity less-than the associated coordinate in Breakpoint\_detail and must be less-than or equal-to that coordinate (or '.') The second value of each pair defines a range of ambiguity greater-than the associated coordinate in Breakpoint\_detail and must be greater-than or equal-to the value of that coordinate (or '.'). If either value is equal-to the associated coordinate in Breakpoint\_detail then there is no ambiguity in that direction.*

-   **Valid Values:** Two *or four* integers
-   **Format:** Comma separated
-   **Example:**

An interchromosomal\_breakpoint with a Breakpoint\_detail coordinate of chr16:15000:+. There is 500 bp of ambiguity in either direction for both the location of the interchromosomal\_breakpoint and the associated coordinate located on chr16 described by Breakpoint\_detail.

chr4 . interchromosomal\_breakpoint 2000 2000 . . . ID=ICB\_001;Start\_range=1500,2500;Breakpoint\_detail=16:15000:+;Breakpoint\_range=14500,15500

A duplication that originated from a region on the plus strand of chr1 begining at 10000 and ending at 60000.

Breakpoint\_detail=chr1:10000-60000:+;Breakpoint\_range=9500,15000,55000,65000

Sequence\_context  
-   **Required:** Optional
-   **Scope:** Locus
-   **Description:**

    The Sequence\_context attribute can be used to provide a region of sequence context from the reference genome around a sequence\_alteration. This can be particularly useful for a feature like an insertion whose location on the genome can not be validated with sequence by the user of a GVF file. Two sequences are given, separated by a comma. Either sequence can be omitted and replaced by a '.'. The two sequences given are always in the order 5' of the sequence\_alteration then 3' of the sequence\_alteration on the plus strand of the reference genome. The sequences themselves are also always given on the plus strand.

-   **Valid Values:** Two nucleotide sequences or the '.' character.
-   **Format:** Comma separated pair of nucleotide sequences.
-   **Example:**

An insertion that is flanked by the 5' sequence 'ATCTGAGCTAGACTT' and the 3' sequence 'ATCCTATATCGGTAT'.

Sequence\_context=ATCTGAGCTAGACTT,ATCCTATATCGGTAT;

#### Pragmas

##### GFF3 Pragmas

GFF3 provides pragmas that define file-wide directives to processing software. Pragmas should be given at the top of a GVF file before any feature lines are given. All pragmas from GFF3 are allowed in GVF. Note especially the following GFF3 pragmas which are encouraged for use with GVF:

\#\#sequence-region \#\#feature-ontology \#\#genome-build

##### GVF Pragmas

In addition to GFF3 pragmas, GVF specifies an additional set of pragmas for describing the meta data associated with sequence\_alterations. There are two basic type of pragmas specified by GVF simple and structured.

Simple GVF pragmas take single values in the form:

\#\#pragma value

These simple pragmas take a single value and the value applies to all sequence\_alterations in the file.

Structured pragmas have additional structure that allow more complex data to be described and these are described below.

##### Simple Pragmas

###### \#\#gvf-version

-   **Type:** Simple
-   **Description:** The version of the GVF specification that this file conforms to. This is the only required pragma for GVF and is the first (or second) line of the file. Note that some GFF3 parsers will require a \#\#gff-version pragma at the top of the file as required by the GFF3 spec. GVF specific parsers should thus tolerate this pragma as the first line of the file and the \#\#gvf-version pragma as the second line.
-   **Supported Values:** Any valid GVF specification value (i.e. 1.07).
-   **Example:**

\#\#gvf-version 1.07

###### \#\#reference-fasta

-   **Type:** Simple
-   **Description:** A fasta file which provides the reference assembly for this genome.
-   **Supported Values: A path, filename or URL giving the location of a fasta file.**
-   **Example:**

\#\#reference-fasta /data/genome\_assembly.fa

###### \#\#feature-gff3

-   **Type:** Simple
-   **Description:** If the sequence\_alterations in this GVF file are annotated with Variant\_effect attributes, this pragma provides the GFF3 formatted file that contains the features that the variant effects are annotated relative to.
-   **Supported Values:**
-   **Example:**

\#\#feature-gff3 /data/features.gff3

###### \#\#file-version

-   **Type:** Simple
-   **Description:** The file-version pragma allows specification of the version of a file.
-   **Supported Values:** The format and interpretation of the version is left to the application, although a scheme of an incrementing decimal number where the fractional part of the number represents minor changes to formatting and documentation while changes to the integral part of the number represent actual changes to the sequence\_alteration data is suggested.
-   **Example:** This is version 1.01 of data represented in this file.

\#\#file-version 1.01

###### \#\#file-date

-   **Type:** Simple
-   **Description:** The file-date pragma is included as a method to describe the date when the file was created or last modified.
-   **Supported Values:** The ISO 8601 standard for dates in the form YYYY-MM-DD is required for the value.
-   **Examples:** The data in this file was created/modified on Feb. 8th 2012.

\#\#file-date 2012-02-08

###### \#\#individual-id

-   **Type:** Simple
-   **Description:** This pragma provides an ID for the individual represented in the file. This ID would typically be unique within a working set of GVF files.
-   **Supported Values:**Any alphanumeric value.
-   **Example:** A male of Yoruba origin whose ID in the Coriell database is NA18507

\#\#individual-id NA18507

###### \#\#population

-   **Type:** Simple
-   **Description:** This pragma described the population that an individual belongs to.
-   **Supported Values:** It is recommended that, where possible, the codes used by HAPMAP and 1000 Genomes project for populations be used: <ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/README.populations>
-   **Examples:** An individual from Nigeria of Yoruban descent.

\#\#population YRI

###### \#\#sex

-   **Type:** Simple
-   **Description:** The genetic sex of the individual.
-   **Supported Values:**female, male
-   **Examples:** A female individual.

\#\#sex female

###### \#\#technology-platform-class

-   **Type:** Simple
-   **Description:** The general type of technology platform used to generate the data supporting the sequence\_alterations annotated in this file.
-   **Supported Values:**
    -   SRS: Short read sequencing (i.e. Illumina, SOLiD, 454, Ion\_Torrent)
    -   SMS: Single molecule sequencing (i.e. Helicos, PacBio)
    -   Capillary: Capillary sequencing (i.e. ABI 3700)
    -   DNA\_Chip: DNA microarray SNP detection
-   **Example:** The sequence\_alterations in the file were discovered by short-read sequencing.

\#\#technology-platform-class SRS

###### \#\#technology-platform-name

**Type:** Simple
**Description:** The name of the specific platform on which the data supporting the sequence\_alteration calls in the file were based.
**Supported Values:**
Illumina: Any Illumina platform or use one of the following:
-   Illumina\_GA
-   Illumina\_GAII
-   Illumina\_GAIIx
-   Illumina\_HiSeq
-   Illumina\_MiSeq

SOLiD: ABI SOLiD
Complete\_Genomics: Complete Genomics
Ion\_Torrent: Ion Torrent
454\_LS: 454 Life Sciences Pyrosequencing
Helicos: Helicos BioSciences tSMS
PACBIO\_RS: Pacific Biosciences PACBIO RS
Affy\_HS\_6.0: Affymetrix Human SNP Array 6.0
Affy\_HS\_5.0: Human SNP Array 5.0
HumanOmni2.5-8: Illumina BeadChip HumanOmni2.5-8
HumanOmni1S-8: Illumina BeadChip HumanOmni1S-8
HumanOmni1-Quad: Illumina BeadChip HumanOmni1-Quad
HumanExome\_1.0: Illumina BeadChip HumanExome 1.0
Please help us keep this list up to date by sending e-mail to the [SO developers](mailto:song-devel@lists.sourceforge.net) with new suggested values.

**Examples:** The sequence\_alterations in the file were discovered by sequencing on Illumina GA.
\#\#technology-platform-name Illumina

###### \#\#technology-platform-version

-   **Type:** Simple
-   **Description:** The version of a given sequencing technology.
-   **Supported Values:**Any valid alphanumeric string.
-   **Examples:**

\#\#technology-platform-version 3

###### \#\#technology-platform-machine-id

-   **Type:** Simple
-   **Description:** The ID or serial number for the machine that generated the data supporting the sequence\_alterations in this file
-   **Supported Values:**Any valid alphanumeric string.
-   **Examples:** The ID or serial number of the machine used to generate the data from which the sequence\_alterations were discovered is DA39-A3EE-5E6B-4B0D325.

\#\#technology-platform-machine-id DA39-A3EE-5E6B-4B0D325

###### \#\#technology-platform-read-length

-   **Type:** Simple
-   **Description:** For sequencing based technologies, this pragma provides a simple method to describe the read length for the sequencing run.
-   **Supported Values:** Integer
-   **Examples:** The length of sequencing reads for the data used in this file is 100 bp.

\#\#technology-platform-read-length 100

###### \#\#technology-platform-read-type

**Type:** Simple
**Description:** For sequencing based technologies, this pragma provides a simple method to describe the layout of the sequence reads - such as fragment or paired end reads.
**Supported Values:**
-   fragment: Single-end sequenced fragments
-   pair: Standard paired-end library
-   **Examples:**The sequencing library used for sequencing this individual was sequenced from both ends generating paired reads from a paired-end library.

\#\#technology-platform-read-type pair

###### \#\#technology-platform-read-pair-span

-   **Type:** Simple
-   **Description:** For paired-end sequencing based technologies, this pragma provides a simple method to describe the average span of the paired sequence reads. The value describes the average genomic length (including the insert) from the beginning of a read to the end of it's mate.
-   **Supported Values:** Integer
-   **Example:** The average read pair span for this library was 600 bp.

\#\#technology-platform-read-pair-span 600

###### \#\#technology-platform-average-coverage

-   **Type:** Simple
-   **Description:** For sequencing based technologies, this pragma provides a simple method to describe the average depth of coverage for all supporting sequence reads combined.
-   **Supported Values:** Integer
-   **Examples:** The average depth of coverage for this entire genome was 27.

\#\#technology-platform-average-coverage 27

###### \#\#sequencing-scope

-   **Type:** Simple
-   **Description:** This pragma can used to describe extent of the genome sequencing for sequence based sequence\_alteration calls.
-   **Supported Values:**

    -   whole\_genome
    -   whole\_exome
    -   targeted\_capture

    Please help us keep this list up to date by sending e-mail to the [SO developers](mailto:song-devel@lists.sourceforge.net) with new suggested values.

-   **Examples:** The data from which the sequence\_alterations in this file were generated came from whole genome sequencing.

###### \#\#capture-method

-   **Type:** Simple
-   **Description:** For sequence that derived from exome or other targeted capture techniques this pragma a method to describe the capture technique.
-   **Supported Values:** One of the following values or free text.

    -   TruSeq\_exome
    -   TargetSeq
    -   SureSelect\_exome
    -   SureSelect\_custom
    -   NimbleGen\_exome
    -   NimbleGen\_custom
    -   Other\_exome
    -   Other\_custom

    Please help us keep this list up to date by sending e-mail to the [SO developers](mailto:song-devel@lists.sourceforge.net) with new suggested values.

-   **Examples:** The DNA from which the sequence\_alterations in this file was enriched for exon regions using the NimbleGen exome capture kit.

\#\#capture-method NimbleGen\_exome

###### \#\#capture-regions

-   **Type:** Simple
-   **Description:** For sequence that derived from exome or other targeted capture techniques this pragma provides a method to describe a file (BED format suggested) that contains the targeted regions.
-   **Supported Values:** A path, file or URL

    Please help us keep this list up to date by sending e-mail to the [SO developers](mailto:song-devel@lists.sourceforge.net) with new suggested values.

-   **Examples:**

\#\#capture-regions /path/to/exon\_regions.bed

###### \#\#sequence-alignment

-   **Type:** Simple
-   **Description:** This pragma describes the sequence alignment algorithm/pipline used.
-   **Supported Values:** Free text
-   **Examples:**

\#\#sequence-alignment Reads were aligned with bwa using default parameters

###### \#\#variant-calling

-   **Type:** Simple
-   **Description:** This pragma describes the variant calling pipeline used.
-   **Supported Values:** Free text
-   **Examples:**

\#\#variant-calling Variants were called with samtools mpileup.

###### \#\#sample-description

-   **Type:** Simple
-   **Description:** This pragma describes the sample from which the sequence\_alterations were derived.
-   **Supported Values: Free text**
-   **Examples:**

\#\#sample-description Sample obtained from peripheral blood.

###### \#\#genomic-source

-   **Type:** Simple
-   **Description:** This pragma uses LOINC (<http://loinc.org/>) codes to describe the genomic source from which the sequence\_alterations were derived.
-   **Supported Values:** prenatal, somatic, germline
-   **Examples:**

\#\#genomic-source somatic

###### \#\#multi-individual

-   **Type:** Simple
-   **Description:**

    GVF files with data sets of multiple individuals are handled in GVF by specifying the \#\#multi-individual pragma. The value of this pragma is two or more unique IDs for the individuals whose data is represented in the file. When this pragma is given additional attribute tags are required, and additional constraints on existing attribute tags are required. These additional constraints are described [below](#multi_individual_files).

-   **Supported Values:** A comma separated list of individual IDs.
-   **Examples:**

\#\#multi-individual NA18507,NA19240,NA19238,NA12878

##### Structured Pragmas Summary

Structured pragmas have additional structure that allow more complex data to be described and in most cases allow the pragma to apply only to a subset of the sequence\_alterations in a file. These structured pragmas can also take the simple form having only a single value as shown in the example below.

\#\#pragma free text value

If there is no '=' (equal sign) contained in the value, then the pragma's value is interpreted as simple and the value is treated as free text. The default tag to which the value should be assigned when a structured pragma is given in simple form is designated for each pragma below.

Usually however, structured pragmas are used because the information being described is more complex than allow by simple pragmas, or because the scope of the pragma is being limited to a subset of the sequence\_alterations in the file. These pragmas take on the following form:

\#\#pragma tag=value;tag=value1,value2;

The structured form above has tag/value pairs. The tags are separated from the values by the equal '=' sign. Multiple tag=value pairs are separated from each other by a semicolon ';' and multiple values for a given tag are separated by a comma ','. This is the same tag/value structure used in column 9 of GFF3/GVF files. The supported tags for each structured pragma are specified below.

Tags beginning with upper case letters are reserved for future use within the GVF spec, but additional tags can be used on an application specific basis if they begin with lower case letters.

##### Common Tags for Structured Pragmas

The following tags are allowed in many structured pragmas and thus are described separately here:

 Seqid   
The Seqid tag can be used to limit the scope of a pragma. The value can be any valid seqid(s) found in column 1 of the GVF file, given as a comma separated list. The pragma then applies only to those seqids listed. If the tag is omitted or has no value, then the pragma applies to sequence\_alterations on all seqids.

 Source   
The Source tag can be used to limit the scope of a pragma. The value can be any valid source(s) found in column 2 of the GVF file, given as a comma separated list. The pragma then applies only to those sequence\_alterations with a specified source value in column 2. If the tag is omitted or has no value, then the pragma applies to all sequence\_alterations.

 Type   
The Type tag can be used to limit the scope of a pragma. The value can be any valid type(s) found in column 3 of the GVF file, given as a comma separated list. The pragma then applies only to those sequence\_alterations with a specified type value in column 3. If the tag is omitted or has no value, then the pragma applies to all sequence\_alterations.

 Dbxref   
When a Dbxref tag is specified it's value takes the form DB\_Name:ID as described below in the [Database Cross References](#database_cross_references) section, such as the location of sequence files or a link to a paper describing a method.

 Comment   
The Comment tag is allowed for all structured pragmas to provide a more human readable description.

For example if a pragma only applies to SNVs, nucleotide insertions and nucleotide deletions that were called by mpileup on chromosome 13 then the following would indicate the scope for the given pragma:

Seqid=chr13;Source=mpileup;Type=SNV,nucleotide\_insertion,nucleotide\_deletion;

##### Structured Pragmas

###### \#\#technology-platform

-   **Type:** Structured
-   **Description:**

    This pragma provides details about the technologies (i.e. sequencing or micro array) used to generate the primary data.

-   **Supported Tags:** Seqid, Source, Type, Dbxref, Comment
-   **Default Tag:** Comment
-   **Additional Tag/Value Pairs:**
    -   **Tag:** Platform\_class
    -   **Values:**

        -   SRS: Short read sequencing (i.e. Illumina, SOLiD, 454, Ion\_Torrent)
        -   SMS: Single molecule sequencing (i.e. Helicos, PacBio)
        -   Capillary: Capillary sequencing (i.e. ABI 3700)
        -   DNA\_Chip: DNA microarray SNP detection

        Please help us keep this list up to date by sending e-mail to the [SO developers](mailto:song-devel@lists.sourceforge.net) with new suggested values.

    -   **Tag: Platform\_name**
    -   **Values:**

        -   Illumina: Illumina GA, GAII and GAIIx
        -   SOLiD: ABI SOLiD
        -   Complete\_Genomics: Complete Genomics
        -   Ion\_Torrent: Ion Torrent
        -   454\_LS: 454 Life Sciences Pyrosequencing
        -   Helicos: Helicos BioSciences tSMS
        -   PACBIO\_RS: Pacific Biosciences PACBIO RS
        -   Affy\_HS\_6.0: Affymetrix Human SNP Array 6.0
        -   Affy\_HS\_5.0: Human SNP Array 5.0
        -   HumanOmni2.5-8: Illumina BeadChip HumanOmni2.5-8
        -   HumanOmni1S-8: Illumina BeadChip HumanOmni1S-8
        -   HumanOmni1-Quad: Illumina BeadChip HumanOmni1-Quad

        Please help us keep this list up to date by sending e-mail to the [SO developers](mailto:song-devel@lists.sourceforge.net) with new suggested values.

    -   **Tag:** Read\_length
    -   **Value:**Integer
    -   **Tag:** Read\_type
    -   **Values:** fragment, pair
    -   **Tag:** Read\_pair\_span
    -   **Value:** Integer
    -   **Tag:** Average\_coverage
    -   **Value:** Integer

-   **Examples:**

All features with source SOAP and type SNV were called from data that was generated by an Illumina GA short read sequencer. There was both a single read (fragment) library and two paired-end libraries. The read length for both libraries is 35 base pairs, and the paired-end libraries had an average read span of 135 and 440 from beginning of the first read to the end of the second. The average depth of coverage for sequencing from all libraries combined was 36x haploid coverage against the reference genome.

\#\#technology-platform Source=SOAP;Type=SNV;Dbxref=URI:http://www.illumina.com;Platform\_class=SRS;Platform\_name=Illumina;Read\_type=fragment,pair;Read\_length=35;Read\_pair\_span=135,440;Average\_coverage=36;

All features with source AFFY\_SNP\_6 and type SNV are called from data that was generated by SNP Array genotyping with an Affymetrix Human SNP Array 6.0.

\#\#technology-platform Seqid=chr1;Source=AFFY\_SNP\_6;Type=SNV;Dbxref=URI:http://www.affymetrix.com;Platform\_class=DNA\_Chip;Platform\_name=Affymetrix Human SNP Array 6.0;

###### \#\#data-source

**Type:** Structured
**Description:**
This pragma provides details about the source data for the sequence\_alterations contained in this file. This may include links to the actual sequence reads in a trace archive, or links to a sequence\_alteration file in another format that have been converted to GVF.

**Supported Tags:** Seqid, Source, Type, Dbxref, Comment
**Default Tag:** Comment
**Additional Tag/Value Pairs:**
Tag: Data\_type
Values:
-   DNA\_sequence
-   RNA\_sequence
-   DNA\_microarray
-   Array\_CGH
-   

Please help us keep this list up to date by sending e-mail to the [SO developers](mailto:song-devel@lists.sourceforge.net) with new suggested values.

**Examples:**
The data for all features with source MAQ and type SNV are based on DNA sequence data from NCBI Short Read Archive ID SRA008175.

\#\#data-source Source=MAQ;Type=SNV;Dbxref=SRA:SRA008175;Data\_type=DNA sequence;Comment=NCBI Short Read Archive (http://www.ncbi.nlm.nih.gov/Traces/sra);

The data for all features with source MAQ and type SNV were converted to GVF format from sequence\_alteration information that can be downloaded from the given URI.

\#\#data-source Source=MAQ;Type=SNV;Dbxref=URI:ftp://ftp.kobic.kr/pub/KOBIC-KoreanGenome/genetic\_variations/KOREF-solexa-snp-X30\_Q40d4D100.gff;Data\_type=sequence\_alteration;Comment=Korean Bioinformation Center (KOBIC) FTP site;

###### \#\#score-method

-   **Type:** Structured
-   **Description:** This pragma provides details about the algorithms or methodologies used to generate the score of a feature. A typical use would be to provide a Dbxref link to a journal article or website describing the software or algorithm used to calculate the score.
-   **Supported Tags:** Seqid, Source, Type, Dbxref, Comment
-   **Default Tag:** Comment
-   **Examples:**

\#\#score-method Comment=Scores are Phred scaled probabilities of an incorrect sequence\_alteration call

###### \#\#source-method

-   **Type:** Structured
-   **Description:** This pragma provides details about the algorithms or methodologies used to generate data for a given source in the file. This is used for example to document how a particular type of sequence\_alteration was called. A typical use would be to provide a Dbxref link to a journal article describing software used for calling the sequence\_alteration data with the given source tag.
-   **Supported Tags:** Seqid, Source, Type, Dbxref, Comment
-   **Default Tag:** Comment
-   **Examples:**

Features on chromosome 1 whose source is MAQ and type is SNV had SNVs called by MAQ which is referenced with PubMed ID 18714091.

\#\#source-method Seqid=chr1;Source=MAQ;Type=SNV;Dbxref=PMID:18714091;Comment=MAQ SNV calls;

Features on all chromosomes with source SOAP and type SNV were called by SOAPsnp which is referenced with PubMed IDs 18227114 and 18987735.

\#\#source-method Source=SOAP;Type=SNV;Dbxref=PMID:18227114,PMID:18987735;Comment=Short Elongated Alignment Program (SOAP);

###### \#\#attribute-method

-   **Type:** Structured
-   **Description:** This pragma provides details about algorithms or methodologies for a given attribute tag in the file. This is used to document how a particular type of attribute value (i.e. Zygosity, Variant\_effect) was calculated.
-   **Supported Tags:** Seqid, Source, Type, Dbxref, Comment
-   **Default Tag:** Comment
-   **Examples:**

The value for the Zygosity attributes for all features with a source SOLiD and a type SNV were passed through into the GVF file 'as-is' from the original data file. A Dbxref could have been included as well to reference the paper of the original study.

\#\#attribute-method Source=SOLiD;Type=SNV;Attribute=Zygosity;Comment=Zygosity is reported here as determined in the original study.

###### \#\#phenotype-description

-   **Type:** Structured
-   **Description:** A description of the phenotype of the individual. This pragma can contain either ontology constrained terms, or a free text description of the individual's phenotype or both. In the first case the constraining ontology is given with the Ontology tag which is a URI pointing to an ontology file. The IDs or terms given with the Term tag are then required to be in the given ontology. Multiple terms may included as a comma separated list. A free text description of the phenotype may be given in the Comment tag to supplement or replace the ontology description. Support for phenotype annotation in GVF is a work in progress. A [wiki page](http://www.sequenceontology.org/so_wiki/index.php/Using_Phenotype_Ontologies_in_GVF) has been created to foster a discussion about best practices for using phenotype ontologies in GVF.
-   **Supported Tags:** Seqid, Source, Type, Dbxref, Comment
-   **Default Tag:** Comment
-   **Examples:**

A text description of a 50 year old individual with AML.

\#\#phenotype-description Comment=Individual presented at 50 years with AML;

An ontology description of an individual with AML.

\#\#phenotype-description Term=acute myloid leukemia;Ontology=http://www.human-phenotype-ontology.org/human-phenotype-ontology.obo.gz;

A HPO term used to describe an individual with AML.

\#\#phenotype-description Term=HP:0004808;Ontology=http://www.human-phenotype-ontology.org/human-phenotype-ontology.obo.gz;

A mature, sterile fly. Note that multiple terms are included in the Term value as a comma separated list.

\#\#phenotype-description Term=sterile,mature;Ontology=http://www.berkeleybop.org/ontologies/pato.owl;

###### \#\#phased-genotypes

-   **Type:** Structured
-   **Description:**

    This pragma indicates that the genotypes in the file are phased. Tag-value pairs can be used to limit the scope of the pragma. For example, you may want the pragma to define only SNV features or only features with a given source value as phased. Phased genotypes indicate that for heterozygous loci it is known which chromosome each sequence\_alteration sequence belongs to. That is, if one sequence\_alteration has 'Variant\_seq=A,C;Genotype=0:1;' attributes and another sequence\_alteration on the same landmark feature (chromosome) has 'Variant\_seq=T,G;Genotype=1:0;' attributes then the A (0) and G (1) occur together on one copy of the chromosome because their both correspond to the Genotype value in the first position. Likewise the C and T occur together on the other copy of that chromosome. This information can come from pedigree data, population data or complete or partial chromosome assembly. When the '\#\#phased-genotypes' pragma is given for a file, the sequences given in the Variant\_seq attributes for all features (except as limited by the tag-value pairs described above) are required to be ordered in this way. That is, the first sequence given (with the Genotype attribute) is on the same copy of the chromosome as the first sequence given in all other Genotype attributes covered and likewise, the second sequence given is always on the other copy of the chromosome. Note that you can use the Phased attribute (see above) to indicate that individual features are phased. The phased attribute would be used when only particular regions of the genome are phased.

-   **Supported Tags:** Seqid, Source, Type, Dbxref, Comment
-   **Examples:**

All sequence\_alterations are phased in this file.

\#\#phased-genotypes

Only the SNVs in the file are phased.

\#\#phased-genotypes Type=SNV;

#### Comments

Other less structured details can be included in the form of free text comments. And since they are for human reading only they can scroll over multiple lines with a comment identifier '\#' at the beginning of each line. GVF parsers are allowed to ignore comments.

\# laboratory-description \# This is individual X035 that was originally mislabeled as X053. As of 03/08/10 all labels are up to date.

#### Database Cross References

For all attributes and pragmas in GVF that have a Dbxref tag, the value is in the form database:ID. For example an SNV might have a cross reference to dbSNP and OMIM as 'Dbxref=dbSNP:rs113993958,OMIM:602421.0004'. The format of each type of ID varies from database to database. An authoritative list of databases, their DBTAGs, and the URI transformation rules that can be used to fetch the objects given their IDs can be found at this location:

<ftp://ftp.geneontology.org/pub/go/doc/GO.xrf_abbs>

Further details can be found here:

<ftp://ftp.geneontology.org/pub/go/doc/GO.xrf_abbs_spec>

In addition, database:ID pairs can point to a stable URN, URL or URI with Dbxref=URL:http//www.example.com for example.

#### Multi-individual GVF Files

While the GVF format was designed primarily for personal genomics - containing rich information about the effects of sequence alterations for a single individual per file, it works equally well to convey genotype information for mutliple individuals in addition to the rich Variant\_effect information that it conveys for each locus. GVF provides a way to handle the genotype information for multiple individuals on a single feature line in two steps. First, the \#\#multi-individual pragma is given at the top of the file which specifies that the file will contain data for multiple individuals and gives a unique ID for each individual contained in the file. Second, on each of the feature lines in the file an "Individual" attribute is given. This attribute contains a list of individuals that have sequence\_alterations at this locus represented as a list of integers that provide a 0-based index to the IDs in the \#\#individual-id pragma.

1.  The Variant\_seq attribute captures all the sequences seen at this locus in any individual contained within the file.
2.  An "Individual" attribute is required for each sequence\_alteration feature given in a multi-individual GVF file. This attribute contains a comma separated list of integers which provide a 0-based index into the \#\#multi-individual pragma ID list. This list of indexes identifies which individuals have sequence\_alterations at this locus. Individuals who are homozygous reference at this locus are not included in the "Individual" attribute list.
3.  A Genotype attribute is required for each sequence\_alteration feature which is a comma separated list of integer pairs. The integers within the pairs are separated by a colon. These integers are a 0-based index into the Variant\_seq attribute and identify the genotype of each individual as described above. This list of pairs gives genotype information about each individual identified in the "Individual" attribute and must contain an entry (or use '.:.' for unknown) for each of those individuals.
4.  Other attributes such as Variant\_reads, and Phased may also contain comma separated lists which give information for each individual in a similar fashion to that described above for Genotype. See those attribute descriptions for more details.

\#\#gvf-version 1.07 \#\#feature-ontology http://www.sequenceontology.org/resources/obo\_files/current\_release.obo \#\#multi-individual NA19240,NA18507,NA12878,NA19238 \#\#genome-build NCBI B36.3 \#\#sequence-region chr16 1 88827254 chr16 dbSNP SNV 49291360 49291360 . + . ID=ID\_2;Variant\_seq=C,G;Individual=0,1,2,3;Genotype=0:1,0:0,1:1,0:1; chr16 dbSNP SNV 49302125 49302125 . + . ID=ID\_3;Variant\_seq=C,T;Individual=0,1,3;Genotype=0:1,2:2,0:2; chr16 dbSNP SNV 49302365 49302365 . + . ID=ID\_4;Variant\_seq=G;Individual=0,1;Genotype=0:0,0:0; chr16 dbSNP SNV 49302700 49302700 . + . ID=ID\_5;Variant\_seq=C,T;Individual=2,3;Genotype=0:1,0:0; chr16 dbSNP SNV 49303084 49303084 . + . ID=ID\_6;Variant\_seq=T,G,A;Individual=3;Genotype=1,2:; chr16 dbSNP SNV 49303427 49303427 . + . ID=ID\_8;Variant\_seq=T;Individual=0;Genotype=0:0; chr16 dbSNP SNV 49303596 49303596 . + . ID=ID\_9;Variant\_seq=A,G,T;Individual=0,1,3;Genotype=1:2,3:3,1:3;

#### GVF Feature Examples

A heterozygous SNV with sequences G and C having 17 and 16 reads respectively. This SNV falls within the CDS of the RefSeq mRNAs NM\_012345 and NM\_543210 and the sequence\_alteration causes a variant\_effect of nonsynonymous\_codon with respect to the mRNAs NM\_012345 and NM\_543210. The second variant sequence 'C' is the same as the reference and thus has no functional effect relative to the reference.

chr1 SOAP SNV 15883 15883 36.5 + . ID=chr1:SOAP:SNV:15883;Variant\_seq=G,C;Zygosity=heterozygous;Variant\_reads=17,16;Total\_reads=33;Reference\_seq=C;Variant\_effect=nonsynonymous\_codon 0 mRNA NM\_012345,NM\_543210;

A homozygous deletion in the individual genome relative to the reference genome. The region deleted is longer than 50 nucleotides and thus the GVF simply has a'~' as the Reference\_sequence value. There are a total of 27 reads on average spanning this region, of which 26 on average supported the deletion.

chr1 Celera nucleotide\_deletion 8834426 8834497 . + . ID=ABC\_98765;Variant\_seq=-;Reference\_seq=~;Variant\_reads=26;Total\_reads=27;

A Copy number variant from [NCBI dbVar](http://www.ncbi.nlm.nih.gov/dbvar/) for which there is some ambiguity in the start and end coordinates that is described with the Start\_range and End\_range attributes.

NC\_000010.9 dbVar copy\_number\_variation 51580055 51580298 . . . ID=nssv8537;Name=nssv8537(Loss);Parent=nsv5710;Start\_range=51580055,51580132;End\_range=51580242,51580298;

Another copy number variant as above. In this example the outer boundaries of the ranges are given with nucleotide resolution, but the inner boundaries are unknown.

NC\_000024.7 dbVar copy\_number\_variation 9188753 9995409 . . . ID=nssv27813;Name=nssv27813(Gain);Parent=nsv10015;Start\_range=9188753,.;End\_range=.,9995409;

Another copy number variant as above. In this example the inner boundaries of the ranges are given with nucleotide resolution, but the outer boundaries are unknown.

NC\_000001.9 dbVar copy\_number\_variation 213446726 220244033 . . . ID=nsv491682;Name=nsv491682(CNV);Start\_range=.,213446726;End\_range=220244033,;

A complex example where a t(8;21)(q22;q22.3) translocation, an inversion and an SNV overlap. These are actually three separate sequence\_alterations each mapping to a common region of the reference genome and thus are described as three separated records in the GVF file - each relative to it's location on the reference genome.

chr21 BreakDancer translocation 41400000 46944323 . + . ID=t8-21\_q22-q22.3;Dbxref=PMID:2052570 chr21 DGV inversion 42061144 42083169 . + . ID=Variation\_37237; chr21 samtools SNV 42071394 42071394 . + . ID=rs2989342;Variant\_seq=C,T;Reference\_seq=T

The two SNVs below are phased. In this case the individual is a compound heterozygote with the Variant\_seq G on one copy of the chromosome (first value given in the Variant\_seq attribute) and the other Variant\_seq A on the other copy of the chromosome (second value given in the Variant\_seq attribute).

chr1 GATK SNV 15883 15883 . + . ID=SNV001;Reference\_seq=C;Variant\_seq=G,C;Zygosity=heterozygous;Phased=001; chr1 GATK SNV 15765 15765 . + . ID=SNV002;Reference\_seq=G;Variant\_seq=G,A;Zygosity=heterozygous;Phased=001;

A region of the genome for which there is no sequence alignment data and thus is designated as no-call by using a gap feature. Without the gap feature this region would be assumed to be homozygous reference.

chr1 CG gap 108873 1109456 . + . ID=GAP001;

A few lines from a more complete GVF file are shown below with the functional consequence of each sequence\_alteration annotated relative to their effect on coding for mRNA/protein annotations (scroll right to see complete lines):

\#\#gvf-version 1.07 \#\#feature-ontology http://www.sequenceontology.org/resources/obo\_files/current\_release.obo \#\#genome-build NCBI B36.3 \#\#sequence-region chr16 1 88827254 chr16 samtools SNV 49291141 49291141 . + . ID=ID\_1;Variant\_seq=A,G;Reference\_seq=G;Zygosity=heterozygous;Variant\_effect=synonymous\_codon 0 mRNA NM\_022162; chr16 samtools SNV 49302125 49302125 . + . ID=ID\_3;Variant\_seq=T,C;Reference\_seq=C;Zygosity=heterozygous;Variant\_effect=nonsynonymous\_codon 0 mRNA NM\_022162;Alias=NP\_071445.1:p.P45S; chr16 samtools SNV 49302365 49302365 . + . ID=ID\_4;Variant\_seq=G,C;Reference\_seq=C;Zygosity=heterozygous;Variant\_effect=non\_synonymous\_codon 0 mRNA NM\_022162;Alias=NP\_071445.1:p.L125V; chr16 samtools SNV 49302700 49302700 . + . ID=ID\_5;Variant\_seq=T;Reference\_seq=C;Zygosity=homozygous;Variant\_effect=synonymous\_codon 0 mRNA NM\_022162; chr16 samtools SNV 49303084 49303084 . + . ID=ID\_6;Variant\_seq=G,T;Reference\_seq=T;Zygosity=heterozygous;Variant\_effect=synonymous\_codon 0 mRNA NM\_022162; chr16 samtools SNV 49303156 49303156 . + . ID=ID\_7;Variant\_seq=T,C;Reference\_seq=C;Zygosity=heterozygous;Variant\_effect=synonymous\_codon 0 mRNA NM\_022162; chr16 samtools SNV 49303427 49303427 . + . ID=ID\_8;Variant\_seq=T,C;Reference\_seq=C;Zygosity=heterozygous;Variant\_effect=non\_synonymous\_codon 0 mRNA NM\_022162;Alias=NP\_071445.1:p.R479W; chr16 samtools SNV 49303596 49303596 . + . ID=ID\_9;Variant\_seq=T,C;Reference\_seq=C;Zygosity=heterozygous;Variant\_effect=non\_synonymous\_codon 0 mRNA NM\_022162;Alias=NP\_071445.1:p.A535V;

#### Acknowledgments

We would like to thank the NHGRI for funding this work (R44HG2991, R44HG3667). We would like to thank the following people for feedback and contributions to the specification (in alphabetical order):

-   Steve Chervitz
-   Deanna Church
-   Fiona Cunningham
-   Tim Heffron
-   Hao Hu
-   Chad Huff
-   Edward Kirluata
-   Bob Kuhn
-   John Lopez
-   Graham Ritchie
-   Archie Russel
-   Mark Singleton
-   Lauro Sumoy

The GVF Specification is maintained by [Barry Moore](mailto:barry.moore@genetics.utah.edu).

#### Change Log

1.07 Mon Apr 28 13:44:47 MDT 2014  
-   Updated the \#\#technology-platform-name pragma.
-   Added the Breakpoint\_range attribute and made minor updates to Breakpoint\_detail to support those changes.
-   Updated wording for the Reference\_seq attribute and changed it's status to required.

<!-- -->

1.06 Tue Jan 24 14:46:17 MST 2012  
-   Added "Blue Box" description of a very simple implimentation of GVF.
-   Reformatted attribute definitions to provide more detail and a more consistent structure
-   Added additional valid characters for Varaint\_seq and Reference\_seq \[!^\] to support hemizygous and no-called loci.
-   Added the Zygosity attribute.
-   Modified the format of the Genotype attribute.
-   Added the Individual attribute to support population based GVF files.
-   Added the Breakpoint\_detail attribute.
-   Added the Sequence\_context attribute.
-   Added a distinction between simple and structured pragmas. Updated the format of all pragmas to provide more detail and more consistent structure. Added several new simple pragmas including: population, sex, technology-platform-class, technology-platform-name, technology-platform-machine-id technology-platform-read-length, technology-platform-read-type, technology-platform-read-pair-span, technology-platform-average-coverage, sequencing-scope, sequence-alignment, variant-calling, sample-description, genomic-source multi-individual.
-   Added the attribute-method pragma to describe methods used to calculate attribute values.
-   Added the multi-individual pragma and a description of support for multi-individual GVF.
-   Added/modified several examples.
-   Removed the Variant\_copy\_number and Reference\_copy\_number attributes.
-   The values for attributes which provide data that corresponds to each copy of a chromosome (Genotype and Variant\_reads) have colon separated values for each chromosome present and comma separated lists of these value sets for multiple individuals in a multi-individual GVF.

<!-- -->

1.05 Wed Jan 19 16:26:14 MST 2011  
-   Modified the description and examples for the Start\_range and End\_range tag slightly as per discussions with NCBI, Ensembl and UCSC.
-   Modified the wording under Community Attributes Collection slightly.
-   Several typo fixes that were caught by Bob Kuhn and John Lopez

<!-- -->

1.04 Tue Dec 14 14:51:41 MST 2010  
-   Added Display\_id attribute to the Individual-id pragma.

<!-- -->

1.03 Wed Nov 24 10:45:18 MST 2010  
-   Specified constraint on column 3 to be a sequence\_alteration, one of it's children or gap.
-   Added \#\#phased-genotype pragma and example.
-   Added Start\_range and End\_range attributes and examples.
-   Cleaned up the 'Quick Example of GVF Content' section to make it simpler.
-   Replaced Nomenclature attributes with Alias attributes in the 'Quick Example of GVF Content' section.
-   Added the \#\#score-method pragma as a method to describe the score calculated in column 6.
-   Added links to the GVF wiki pages.

<!-- -->

1.02 Sat Jul 24 08:17:39 MDT 2010  
-   Updated format of Variant\_effect tag and updated the examples to comply.
-   Minor updates to the homozygous deletion example .
-   Added link to the 10Gen Data set.

<!-- -->

1.01 Fri Mar 19 16:56:20 MDT 2010  
-   Added documentation to, and fixed inconsistencies with pragma examples.
-   Added hemizygous as a valid Genotype tag value.
-   Removed Intersected\_feature tag.
-   Modified Variant\_effect tag to incorporate the function of the Intersected\_feature tag.
-   Clarified the definition of the Variant\_copy\_number and Reference\_copy\_number tags.
-   Modified the GVF examples to reflect the changes in Variant\_effect and Intersected\_feature tags.
