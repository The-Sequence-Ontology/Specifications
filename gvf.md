### Genome Variation Format 1.10

#### Table of Contents

-   [Summary](#summary)
    -   [A Quick Explanation and Example of GVF Content](#a-quick-explanation-and-example-of-gvf-content)
    -   [GVF Specification Archive](#gvf-specification-archive)
    -   [GVF Wiki Pages](#gvf-wiki-pages)

-   [Data Sets](#data-sets)
    -   [10Gen Data set](#10gen-data-set)
    -   [Data from ENSEMBL](#data-from-ensembl)
    -   [Data from NCBI](#data-from-ncbi)

-   [Column Descriptions](#column-descriptions)
-   [Character Escaping](#character-escaping)
-   [GVF Column 9 Attributes](#gvf-column-9-attributes)
    -   [Attribute Summary](#attribute-summary)
    -   [Community Attributes Collection](#community-attributes-collection)
    -   [Attribute Definitions](#attribute-definitions)

-   [Pragmas](#pragmas)
    -   [GFF3 Pragmas](#gff3-pragmas)
    -   [GVF Pragmas](#gvf-pragmas)
    -   [Simple Pragmas](#simple-pragmas)
    -   [Structured Pragmas Summary](#structured-pragmas-summary)
    -   [Structured Pragmas](#structured-pragmas)

-   [Comments](#comments)
-   [Database Cross References](#database-cross-references)
-   [Multi-individual GVF Files](#multi-individual-gvf-files)
-   [GVF Feature Examples](#gvf-feature-examples)
-   [Acknowledgments](#acknowledgments)
-   [Change Log](#change-log)

#### Summary

*Version 1.10*
*7 October 2016*

The Genome Variation Format (GVF) is a very simple file format for describing sequence_alteration features at nucleotide resolution relative to a reference genome. The GVF format was published in *Reese et al., Genome Biol., 2010;11(8):R88* [A standard variation file format for human genome sequences](http://genomebiology.biomedcentral.com/articles/10.1186/gb-2010-11-8-r88). We would like to acknowledge the contributing groups for their support.

<table>
    <tr>
        <td>Karen Eilbeck</td>
        <td>Biomedical Informatics<br/>University of Utah</td>
    </tr>
    <tr>
        <td>Paul Flicek</td>
        <td>ENSEMBL<br/>EBI</td>
    </tr>
    <tr>
        <td>Gabor Marth</td>
        <td>Boston University<br/>1000 Genomes Project</td>
    </tr>
    <tr>
        <td>Martin Reese</td>
        <td>Omicia Inc.</td>
    </tr>
    <tr>
        <td>Lincoln Stein</td>
        <td>Ontario Cancer Institute</td>
    </tr>
    <tr>
        <td>Mark Yandell</td>
        <td>Department of Human Genetics<br/>University of Utah</td>
    </tr>
</table>

GVF is a type of [GFF3](/gff3.md) file with additional pragmas and attributes specified. The GVF format has the same nine-column tab-delimited format as GFF3. All of the requirements and restrictions specified for GFF3 apply to the GVF specification as well and thus a GVF file should be fully compatible with code used for processing and displaying GFF3 files. In addition, GVF adds additional constraints to some of these columns as described below.

See the [GFF3 Specification](/gff3.md) for more details about GFF3.

##### A Quick Explanation and Example of GVF Content

<table>
    <tr>
        <td>
            <p>The text in this box will give a very brief description of the GVF format for the impatient and those who only need to understand the simplest GVF content. This "blue box" description of GVF will likely suffice for most cases where describing simple SNV/indel features for a single genome in GVF format is the goal. Users seeking to achieve that goal do not need to read the entire specification. After this box, the remainder of the document will explain the complete GVF format in detail.
            <p>GVF has two types of lines (empty lines and lines beginning with a single '#' are allowed but ignored). Lines beginning with '##' are <i>pragmas</i> which contain meta-data. All other lines are <i>feature lines</i>.</p>
            <ul>
                <li>
                    <strong>Pragmas:</strong>
                    <ul>
                        <li>Begin with '##'</li>
                        <li>Contain meta-data.</li>
                        <li>
                            Only this one is required:
                            <pre>##gvf-version 1.10</pre>
                        </li>
                    </ul>
                </li>
                <li>
                    <strong>Feature Lines:</strong>
                    <ul>
                        <li>
                            Nine tab-delimited columns.
                            <ul>
                                <li>seqid: The chromosome or contig on which the sequence_alteration is located (text).</li>
                                <li>source: The source (i.e. an algorithm or database) of the sequence_alteration (text.)</li>
                                <li>type: An SO term describing the type of sequence_alteration (child term of <a href="http://www.sequenceontology.org/browser/current_release/term/SO:0001059">SO sequence_alteration</a>), no_sequence_alteration (<a href="http://www.sequenceontology.org/browser/current_svn/term/SO:0002073">SO no sequence alteration</a>), or a <a href="http://www.sequenceontology.org/browser/current_svn/term/SO:0000730">gap</a>.</li>
                                <li>start: A 1-based integer for the begining of the sequence_alteration locus on the plus strand (integer).</li>
                                <li>end: A 1-based integer of the end of the sequence_alteration on plus strand (integer).</li>
                                <li>score: A (<a href="http://en.wikipedia.org/wiki/Phred_quality_score">Phred scaled</a>) probability that the sequence_alteration call is incorrect (real number).</li>
                                <li>strand: The strand of the feature (+/-).</li>
                                <li>phase: This column allows compatibility with GFF3 (.).</li>
                                <li>
                                    attributes: Tag/value pairs describing attributes of the sequence_alteration (tag1=value1,value2;tag2=value1;).
                                    <ul>
                                        <li>ID: A file-wide unique identifier (required).</li>
                                        <li>
                                            Variant_seq: All unique sequences seen in the individual described in the file at the features locus - including the reference sequence if appropriate (required). Any IUPAC nucleotide symbol is allowed, but usually just A, T, G, C. Plus any of the following:
                                            <ul>
                                                <li>. (period): Unknown/missing value.</li>
                                                <li>- (hyphen): No sequence (e.g. for a homozygous deletion Variant_seq=-;).</li>
                                                <li>~ (tilde): Place holder for a sequence too long to show. The tilde can be followed by an integer that describes the length of the omitted sequence.</li>
                                                <li>@ (at): An alias for the sequence found in the Reference_seq attribute.</li>
                                                <li>! (exclamation): Place holder for the missing sequence at a hemizygous locus.</li>
                                                <li>^ (caret): Place holder for the missing sequence at a location without enough data to make an accurate call (no-call locus).</li>
                                            </ul>
                                        </li>
                                        <li>Reference_seq: The reference sequence. Nucleotide characters as well as '-' and '~' as described above.</li>
                                    </ul>
                                </li>
                            </ul>
                        </li>
                    </ul>
                 </li>
             </ul>
            <p>The following characters must be <a href="http://en.wikipedia.org/wiki/Percent-encoding">URL-encoded</a> throughout the GVF document except as noted:</p>
            <table>
                <tr>
                    <th>Char</th>
                    <th>Exception</th>
                    <th>Escape</th>
                </tr>
                <tr>
                    <td>tab</td>
                    <td>separating columns</td>
                    <td>%09</td>
                </tr>
                <tr>
                    <td>newline</td>
                    <td>end of lines</td>
                    <td>%0A</td>
                </tr>
                <tr>
                    <td>carriage return</td>
                    <td>end of lines</td>
                    <td>%0D</td>
                </tr>
                <tr>
                    <td>All control charachters</td>
                    <td>None</td>
                    <td>%00-%1F, %7F</td>
                </tr>
            </table>
            <p>The following characters have special meaning and need to be <a href="http://en.wikipedia.org/wiki/Percent-encoding">URL-encoded</a> throughout column 9 when not used as specified:</p>
            <table>
                <tr>
                    <th>Char</th>
                    <th>Escape</th>
                </tr>
                <tr>
                    <td>';' (semicolon)</td>
                    <td>%3B</td>
                </tr>
                <tr>
                    <td>'=' (equals)</td>
                    <td>%3D</td>
                </tr>
                <tr>
                    <td>'%' (percent)</td>
                    <td>%25</td>
                </tr>
                <tr>
                    <td>'&' (ampersand)</td>
                    <td>%26</td>
                </tr>
                <tr>
                    <td>',' (comma)</td>
                    <td>%2C</td>
                </tr>
            </table>
            <p>A few lines of single nucleotide variants (SNV) are shown below as an example of a very simple GVF file. Scroll right to see the complete lines.</p>
            <pre>
##gvf-version 1.10
##genome-build NCBI B36.3
##sequence-region chr16 1 88827254

chr16 samtools SNV 49291141 49291141 . + . ID=ID_1;Variant_seq=A,G;Reference_seq=G;
chr16 samtools SNV 49291360 49291360 . + . ID=ID_2;Variant_seq=G;Reference_seq=C;
chr16 samtools SNV 49302125 49302125 . + . ID=ID_3;Variant_seq=T,C;Reference_seq=C;
chr16 samtools SNV 49302365 49302365 . + . ID=ID_4;Variant_seq=G,C;Reference_seq=C;
chr16 samtools SNV 49302700 49302700 . + . ID=ID_5;Variant_seq=T;Reference_seq=C;
chr16 samtools SNV 49303084 49303084 . + . ID=ID_6;Variant_seq=G,T;Reference_seq=T;
chr16 samtools SNV 49303156 49303156 . + . ID=ID_7;Variant_seq=T,C;Reference_seq=C;
chr16 samtools SNV 49303427 49303427 . + . ID=ID_8;Variant_seq=T,C;Reference_seq=C;
chr16 samtools SNV 49303596 49303596 . + . ID=ID_9;Variant_seq=T,C;Reference_seq=C;</pre>
        </td>
    </tr>
</table>

##### GVF Specification Archive

Previous versions of the GVF specification are archived. Please see the [GVF Specification Archive](http://www.sequenceontology.org/resources/gvf_archives.html) to find these older versions of the specification.

##### GVF Wiki Pages

While this page provides the official description of the GVF format with some simple examples, several [GVF wiki pages](http://www.sequenceontology.org/so_wiki/index.php/Main_Page#Sequence_Ontology_based_file_formats) have been set up to describe the GVF format in a more tutorial manner, to provide additional examples and to document implementations of GVF files by the GVF community.

#### Data Sets

##### 10Gen Data set

The SNVs from ten of the first sequenced individual human genomes have been converted to GVF format and are made available as the [10Gen Data set](http://www.sequenceontology.org/resources/10Gen.html).

##### Data from ENSEMBL

ENSEMBL is providing GVF files for their sequence_alteration data sets at:

-   ftp://ftp.ensembl.org/pub/current_variation/gvf/

##### Data from NCBI

The dbVar database at NCBI is providing GVF files for their structural variant data at:

-   ftp://ftp.ncbi.nlm.nih.gov/pub/dbVar/data/Homo_sapiens/by_assembly/NCBI36/gvf/
-   ftp://ftp.ncbi.nlm.nih.gov/pub/dbVar/data/Homo_sapiens/by_assembly/GRCh37/gvf/
-   ftp://ftp.ncbi.nlm.nih.gov/pub/dbVar/data/Homo_sapiens/by_assembly/GRCh38/gvf/

#### Column Descriptions

Sequence alterations are described in a GVF file with 9 tab-delimited columns. The columns of a GVF file are inherited from GFF3 with additional constraints placed on some columns. A brief descriptions of each column follows:

<dl>
    <dt>Column 1 "seqid"</dt>
    <dd>The ID of the landmark used to establish the coordinate system for the current feature. IDs may contain any characters, but must escape any characters not in the set [a-zA-Z0-9.:^*$@!+_?-|]. In particular, IDs may not contain unescaped white space and must not begin with an unescaped "&gt;". Although the GVF format allows colons within the seqid values, be aware that some applications disallow it and thus it is recommended that colons be avoided in seqid.</dd>
    <dt>Column 2: "source"</dt>
    <dd>The source is a free text qualifier intended to describe the algorithm or operating procedure that generated this feature. Typically this is the name of a piece of software, such as "MAQ" or a database name, such as "dbSNP". Although the value of source is not constrained, the ##source-method pragma may be used to describe the source in more detail.</dd>
    <dt>Column 3: "type"</dt>
    <dd>The type of the feature. This is constrained to be either: (a) the SO term sequence_alteration <a href="http://www.sequenceontology.org/browser/current_svn/term/SO:0001059">SO:0001059</a>, (b) a child term of sequence_alteration, (c) the SO term no_sequence_alteration <a href="http://www.sequenceontology.org/browser/current_svn/term/SO:0002073">SO:0002073</a>, (d) the SO term gap <a href="http://www.sequenceontology.org/browser/current_svn/term/SO:0000730">SO:0000730</a>, or (e) the SO accession number for any of the previous terms. The gap feature, while not a sequence_alteration, provides a way to annotate gaps in the individuals genome assembly where sequence_alteration information is unknown (low-coverage, no-call regions).</dd>
    <dt>Columns 4 & 5: "start" and "end"</dt>
    <dd>The start and end of the feature, in 1-based integer coordinates, relative to the landmark given in column 1. Start is always less than or equal to end. For features that cross the origin of a circular feature (e.g. most bacterial genomes, plasmids, and some viral genomes), the requirement for start to be less than or equal to end is satisfied by making end = the position of the end + the length of the landmark feature. For zero-length features, such as an insertion, start equals end and the implied site is to the three-prime of the indicated base in the direction of the landmark.</dd>
    <dt>Column 6: "score"</dt>
    <dd>The score of the feature, an integer or floating point number. The semantics of the score are not defined, however it is strongly recommended that a <a href="http://en.wikipedia.org/wiki/Phred_quality_score">Phred scaled quality score</a> be used whenever possible. While the semantics of score are not defined, the ##score-method pragma may be used to describe how the score was calculated.</dd>
    <dt>Column 7: "strand"</dt>
    <dd>The strand of the feature. The '+' (plus sign) for positive strand (relative to the landmark), and the '-' (minus sign) for minus strand, and '.' (period) for features that are not stranded. In addition, '?' (question mark) can be used for features whose strandedness is relevant, but unknown.</dd>
    <dt>Column 8: "phase"</dt>
    <dd>The phase column is not used in GVF, but is maintained with the placeholder '.' (period) for compatibility with GFF3 and tools that conform to the GFF3 specification.</dd>
    <dt>Column 9: "attributes"</dt>
    <dd>The ninth column in GFF3/GVF contains one or more tag/value pairs that describe attributes of the feature. Tags are separated from values by an '=' (equal sign). Multiple values for a given tag are separated by a ',' (comma). Sets of tag/value pairs are separated from each other by a ';' (semicolon). Thus any use of any of the charachters ';=,' must be <a href="http://en.wikipedia.org/wiki/Percent-encoding">URL-encoded</a>.</dd>
</dl>

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

The Sequence Ontology provides a collection of descriptions of attributes in use by the GVF community. These descriptions are of organization specific attributes (those attributes beginning with a lower case letter that are specific to that organizations data sets) or they provide clarification about how the organizations data sets fit within the existing attribute structure. Note that existing attributes should not be redefined here. This information is provided as a wiki. Please refer to [GVF Community Supported Attributes](http://www.sequenceontology.org/so_wiki/index.php?title=GVF_Community_Supported_Attributes) for more information. If you would like to maintain a description of attributes in use by your organization please [contact the Sequence Ontology group](http://www.sequenceontology.org/about/contacts.html) to set up an account on the wiki.

##### Attribute Definitions

Attributes may describe information about the locus of the sequence_alteration (e.g. its ID, a Dbxref or the Reference_seq) or they may describe information about a particular individual's data at that locus (e.g. Total_read, Genotype). When a GVF file contains data for multiple individuals the second type of attribute needs to specify different values for each individual. In the attribute definitions below a "Scope" is described for each attribute with a value of "Individual" or "Locus". This distinction is relevant to multi-individual files where attributes that describe individual data contain a comma separated list of values for each individual that is variant at the locus. The details of this are described below in the section [Multi-individual GVF Files](#multi-individual-gvf-files).

<dl>
    <dt>ID</dt>
    <dd>
        <ul>
            <li><strong>Required:</strong> Yes</li>
            <li><strong>Scope:</strong> Locus</li>
            <li>
                <strong>Description:</strong>
                <p>The ID attribute is used to assign an ID to a feature. As in GFF3 this ID must be unique within the file and is not required to have meaning outside of the file.</p>
            </li>
            <li><strong>Valid Values:</strong> <a href="#character-escaping">Any valid character(s)</a>.</li>
            <li><strong>Format:</strong> Single value.</li>
            <li>
                <strong>Example:</strong>
                <pre>ID=SNV_00123;</pre>
            </li>
        </ul>
    </dd>
    <dt>Alias</dt>
    <dd>
        <ul>
            <li><strong>Required:</strong> Optional</li>
            <li><strong>Scope:</strong> Locus</li>
            <li>
                <strong>Description:</strong>
                <p>A secondary name for the feature. This attribute can be used to provide, among other things, <a href="http://www.hgvs.org/mutnomen/">HGVS nomenclature</a> or <a href="http://content.karger.com/Book/Home/244102">ISCN nomenclature</a> names for the sequence_alteration as described below in the Format section. Note that cross-references to sequence_alterations in established databases (e.g. dbSNP, OMIM) should use the Dbxref attribute described below.</p>
            </li>
            <li><strong>Valid Values:</strong> <a href="#character-escaping">Any valid character(s)</a></li>
            <li>
                <strong>Format:</strong>
                <p>The format for the Alias attribute is a single free text value. However if the text begins with HGVS: or ISCN:, then the name given should comply with the given nomenclature system.</p>
            </li>
            <li>
                <strong>Example:</strong>
                <p>An alias for the 3 three base-pair deletion that is the most common cause of Cystic Fibrosis.</p>
                <pre>Alias=delta-F508</pre>
                <p>A sequence_alteration described in <a href="http://www.hgvs.org/mutnomen/">HGVS nomenclature</a>.</p>
                <pre>Alias=HGVS:NM_004006.2:c.3G&gt;T</pre>
                <p>A sequence_alteration described in <a href="http://content.karger.com/Book/Home/244102">ISCN nomenclature</a>. Shown here unescaped, but note the URL escaping rules described below.</p>
                <pre>Alias=ISCN:45,XY,t(13q,14q)</pre>
                <p>Note that some characters in HGVS and ISCN nomenclature (for example ';=%&,') will need to be <a href="#character-escaping">escaped in GVF</a>. For example the ISCN alias above should appear as:</p>
                <pre>Alias=ISCN:45%2CXY%2Ct(13q%2C14q)</pre>
            </li>
        </ul>
    </dd>
    <dt>Dbxref</dt>
    <dd>
        <ul>
            <li><strong>Required:</strong> Optional</li>
            <li><strong>Scope:</strong> Locus</li>
            <li>
                <strong>Description:</strong>
                <p>A database cross reference. This can be useful to associate this sequence_alteration with the same sequence_alteration previously described in databases such as dbSNP or OMIM. See the <a href="#database-cross-references">Database Cross References</a> section below for more details.</p>
            </li>
            <li><strong>Valid Values:</strong> Any valid combination of DB and ID. The DB abbreviations are described <a href="#database-cross-references">below</a>.</li>
            <li><strong>Format:</strong> A colon separated database name and database ID pair.</li>
            <li>
                <strong>Example:</strong>
                <p>A dbSNP database cross-reference.</p>
                <pre>Dbxref=dbSNP:rs3131969;</pre>
                <p>An OMIM database cross-reference.</p>
                <pre>Dbxref=OMIM:605956.0004;</pre>
            </li>
        </ul>
    </dd>
    <dt>Variant_seq</dt>
    <dd>
        <ul>
            <li><strong>Required:</strong> Required</li>
            <li><strong>Scope:</strong> Locus</li>
            <li>
                <strong>Description:</strong>
                <p>All sequences (alleles) found in an individual (or group of individuals) at a variable locus are given with the Variant_seq attribute as a comma separated list. Note that the reference sequence is included here as well, when appropriate (for example when the locus is heterozygous). The sequence should be given for the strand described (in column 7) for this feature. If the feature is on the minus strand then the sequence given here is the reverse-compliment of the reference genome for these coordinates.</p>
            </li>
            <li>
                <strong>Valid Values:</strong>
                <p>Any nucleic acid sequence using <a href="http://en.wikipedia.org/wiki/Nucleic_acid_notation#IUPAC_notation">IUPAC ambiguity codes</a>. Note, however, that ambiguity codes are used to represent uncertainty of a given sequence at a locus, and should never be used to represent the multiple altered sequences present at a given locus. For example if both an A and a T are seen at a locus then both the A and the T should be listed, not a W which is the ambiguity code for A or T. If a W is used, it means that only one sequence is seen at this locus and that it is unclear whether that sequence is an A or a T.</p>
                <p>In addition to the observed nucleic acid sequence, several other characters (.-~@!^) are valid values in the Variant_seq attribute. Use of these characters is described below with examples.</p>
            </li>
            <li><strong>Format:</strong> A comma separated list of the valid values.</li>
            <li>
                <strong>Nucleotide Examples:</strong>
                <p>A homozygous SNV locus</p>
                <pre>Variant_seq=A;Reference_seq=T;</pre>
                <p>A heterozygous SNV locus</p>
                <pre>Variant_seq=A,T;Reference_seq=T;</pre>
                <p>A triallelic locus from a multi-individual GVF file where three nucleotides have been seen within the population.</p>
                <pre>Variant_seq=A,C,T;Reference_seq=T;</pre>
            </li>
            <li>
                <strong>Other Examples:</strong>
                <dl>
                    <dt>. (period)</dt>
                    <dd>
                        <p>The '.' character is used to represent missing data. There are more specific ways to represent specific kinds of missing data (hemizygous and no-call loci) with other characters described below, and those more specific characters should always be used preferentially where appropriate.</p>
                        <p>The site is known to be altered, but it is not known whether the site is homozygous, heterozygous, hemizygous or could not be called due to missing data, so the '.' character is used to represent this ambiguity.</p>
                        <pre>Variant_seq=A,.;Reference_seq=T</pre>
                    </dd>
                    <dt>- (minus sign/hyphen)</dt>
                    <dd>
                        <p>The '-' character denotes a lack of sequence (e.g. in the case of an insertion or deletion) in either the Variant_seq or Reference_seq attribute. For a deletion feature there is no corresponding sequence to list in the Variant_seq attribute and likewise there is no corresponding Reference_seq attribute sequence for an insertion.</p>
                        <p>A heterozygous deletion.</p>
                        <pre>Variant_seq=ATC,-;Reference_seq=ATC;</pre>
                    </dd>
                    <dt>~[INT] (equivalency sign/tilde) optionally followed by an integer</dt>
                    <dd>
                        <p>The '~' character can be used as a place holder for a sequence that is too long to include in the Variant_seq or Reference_seq attribute. The '~' character may be optionally followed by an integer which gives the length of the sequence represented by the '~'.</p>
                        <p>A large heterozygous deletion. In this case the length and sequence of the region deleted can be inferred from the coordinates of the feature, so the optional integer representing the length of the deletion is not included, although it could have been included as shown below for the insertion.</p>
                        <pre>Variant_seq=-,~;Reference_seq=~;</pre>
                        <p>A large heterozygous insertion 837 bp long. With an insertion, the length of the inserted sequence can not be determined if the '~' character is used, so we include the length as an integer after the '~'.</p>
                        <pre>Variant_seq=~837,-;Reference_seq=-;</pre>
                    </dd>
                    <dt>! (exclamation point/bang)</dt>
                    <dd>
                        <p>The '!' character is use to denote that the site is hemizygous as it would otherwise be interpreted as a homozygous site.</p>
                        <p>A hemizygous SNV on chrY.</p>
                        <pre>Variant_seq=A,!;Reference_seq=T;</pre>
                    </dd>
                    <dt>^ (caret/circumflex)</dt>
                    <dd>
                        <p>The '^' character is used to denote a site where a sequence_alteration could not be called (no-call) because of a lack of sufficient underlying data (lack of coverage or poor quality base-calls or alignments at the locus). This allows a distinction to be made between loci that are homozygous reference from those where there simply isn't any (or enough) data to know what the sequence at the locus is.</p>
                        <p>An SNV where there is enough sequence coverage to call the SNV, but not enough to determine if the site is heterozygous and homozygous.</p>
                        <pre>Variant_seq=A,^;Reference_seq=T;</pre>
                        <p>A locus where there is no data (or insufficient data) to determine if the site is variant or not. Note that a gap feature can be used to describe a region of missing data.</p>
                        <pre>Variant_seq=^;Reference_seq=T;</pre>
                    </dd>
                </dl>
            </li>
        </ul>
    </dd>
    <dt>Reference_seq</dt>
    <dd>
        <ul>
            <li><strong>Required:</strong> Required</li>
            <li><strong>Scope:</strong> Locus</li>
            <li>
                <strong>Description:</strong>
                <p>The sequence corresponding to the seqid, start and end coordinates of this feature in the associated genomic sequence file. A single value should be given for the Reference_seq. In the case where the sequence_alteration represents an insertion relative to the reference, the Reference_seq is given as '-'. The sequence should be given relative to the strand described (in column 7) for this feature. If the feature is on the minus strand then the sequence given for the Reference_seq attribute is the reverse-compliment of the reference genome for this feature's coordinates.</p>
            </li>
            <li>
                <strong>Valid Values:</strong>
                <p>Any nucleic acid sequence using <a href="http://www.ncbi.nlm.nih.gov/pubmed/2582368">IUPAC ambiguity codes</a>.</p>
                <p>In addition to the observed nucleic acid sequence, two other characters (-~) are valid values in the Reference_seq attribute. Use of these characters is described below.</p>
            </li>
            <li>
                <strong>Nucleotide Examples:</strong>
                <pre>Reference_seq=A;</pre>
            </li>
            <li>
                <strong>Other Examples:</strong>
                <dl>
                    <dt>- (minus sign/hyphen)</dt>
                    <dd>
                        <p>The '-' character denotes a lack of sequence (e.g. in the case of an insertion) in the Reference_seq attribute. For an insertion feature there is no corresponding sequence to list in the Reference_seq attribute.</p>
                        <p>A homozygous insertion.</p>
                        <pre>Variant_seq=ATC;Reference_seq=-'</pre>
                    </dd>
                    <dt>~[INT] (equivalency sign/tilde) optionally followed by an integer</dt>
                    <dd>
                        <p>The '~' character can be used as a place holder for a sequence that is too long to include in the Reference_seq attribute. The '~' character may be optionally followed by an integer which gives the length of the sequence represented by the '~' although this integer is allowed, it is unnecessary in the case of the Reference_seq attribute since this can be inferred from the start and end coordinates.</p>
                        <p>A large heterozygous deletion. In this case the length and sequence of the region deleted can be inferred from the coordinates of the feature, so the optional integer representing the length of the deletion is not included.</p>
                        <pre>Variant_seq=-,~;Reference_seq=~;</pre>
                    </dd>
                </dl>
            </li>
        </ul>
    </dd>
    <dt>Variant_reads</dt>
    <dd>
        <ul>
            <li><strong>Required:</strong> Optional</li>
            <li><strong>Scope:</strong> Individual</li>
            <li>
                <strong>Description:</strong>
                <p>The number of reads supporting each altered sequence at this location given in the form Variant_reads=integer. Read counts for sequence_alterations longer than one nucleotide should be average read counts for each position over the length of the sequence_alteration. The order of the reads must be in the same order that the corresponding sequences in the Variant_seq attribute. A '.' character may be used as a placeholder to signify missing data. Multiple values which correspond to the number of reads for each Variant_seq are separated by colons (:). Multiple sets of colon separated values (when specifying multi-individual GVF files) are separated from each other by commas.</p>
            </li>
            <li><strong>Valid Values:</strong> Colon separated integer or '.' values.</li>
            <li><strong>Format:</strong> Comma separated list of colon separated integer sets.</li>
            <li>
                <strong>Example:</strong>
                <p>An SNV where the number of reads supporting an A is 13, but the number of reads supporting a T is not known.</p>
                <pre>Variant_seq=A,T;Variant_reads=13:.;</pre>
                <p>An SNV from a multi-individual GVF file. The first individual has 13 and 10 variants respectively for the A, and T variant sequences and the second individual has 23 reads supporting the A variant sequence and no reads supporting T variant sequence.</p>
                <pre>Variant_seq=A,T;Variant_reads=13:10,23:0;Individual=3,8;</pre>
            </li>
        </ul>
    </dd>
    <dt>Total_reads</dt>
    <dd>
        <ul>
            <li><strong>Required:</strong> Optional</li>
            <li><strong>Scope:</strong> Individual</li>
            <li>
                <strong>Description:</strong>
                <p>The total number of reads covering a sequence_alteration. Total read counts for sequence_alterations longer than one nucleotide should be average read counts for each position over the length of the sequence_alteration. The sum of reads given in Variant_reads is not required to be the same as Total_reads. A '.' character may be used to signify missing data. Multiple values (when specifying multi-individual GVF files) are separated from each other by commas.</p>
            </li>
            <li><strong>Valid Values:</strong> Integer or '.'</li>
            <li><strong>Format:</strong> A single value.</li>
            <li>
                <strong>Example:</strong>
                <p>The total reads covering a locus in a single-individual GVF file.</p>
                <pre>Total_reads=35;</pre>
                <p>The total reads covering a locus in a multi-individual GVF file. The first individual variant at this locus has 35 total read supporting the locus and the second individual has 75.</p>
                <pre>Total_reads=35,75;Individual=3,8;</pre>
            </li>
        </ul>
    </dd>
    <dt>Zygosity</dt>
    <dd>
        <ul>
            <li><strong>Required:</strong> Optional</li>
            <li><strong>Scope:</strong> Individual</li>
            <li>
                <strong>Description:</strong>
                <p>The zygosity of this locus where zygosity is heterozygous, homozygous or hemizygous. This attribute is optional as the zygosity can be inferred from the value of the Variant_seq attribute. A '.' character may be used as a placeholder to signify missing data. Multiple values (when specifying multi-individual GVF files) are separated from each other by commas.</p>
            </li>
            <li><strong>Valid Values:</strong> The values heterozygous, homozygous, hemizygous, or the '.' character.</li>
            <li><strong>Format:</strong> A single value.</li>
            <li>
                <strong>Example:</strong>
                <p>A heterozygous locus</p>
                <pre>Variant_seq=A,T;Zygosity=heterozygous;</pre>
                <p>A homozygous locus</p>
                <pre>Variant_seq=A;Zygosity=homozygous;</pre>
                <p>A hemizygous locus</p>
                <pre>Variant_seq=A,!;Zygosity=hemizygous;</pre>
                <p>Zygosity in a multi-individual GVF file. The first individual variant at this locus is homozygous and the second individual is heterozygous.</p>
                <pre>Variant_seq=A;Zygosity=homozygous,heterozygous;</pre>
            </li>
        </ul>
    </dd>
    <dt>Variant_freq</dt>
    <dd>
        <ul>
            <li><strong>Required:</strong> Optional</li>
            <li><strong>Scope:</strong> Locus</li>
            <li>
                <strong>Description:</strong>
                <p>A real number describing the frequency of the sequence_alteration in a population. The details of the source of the frequency should be described in an attribute-method pragma as described below. The order of the values given must be in the same order that the corresponding sequences occur in the Variant_seq attribute. A '.' character may be used as a placeholder to signify missing data.</p>
            </li>
            <li><strong>Valid Values:</strong> Real numbers or the '.' character.</li>
            <li><strong>Format:</strong> Comma separated list</li>
            <li>
                <strong>Example:</strong>
                <pre>Variant_seq=A,T;Variant_freq=0.05,0.95;</pre>
            </li>
        </ul>
    </dd>
    <dt>Variant_effect</dt>
    <dd>
        <ul>
            <li><strong>Required:</strong> Optional</li>
            <li><strong>Scope:</strong> Locus</li>
            <li>
                <strong>Description:</strong>
                <p>The effect that a sequence_alteration has on a sequence_feature that overlaps it.</p>
            </li>
            <li>
                <strong>Format:</strong>
                <p>Four white-space delimited fields:</p>
                <pre>Variant_effect=sequence_variant index feature_type feature_ID feature_ID</pre>
            </li>
            <li>
                <strong>Valid Values:</strong>
                <ul>
                    <li>The first field is a term that describes the effect of the sequence_alteration on a sequence feature and must be the SO term <a href="http://www.sequenceontology.org/browser/current_release/term/SO:0001060">sequence_variant</a> or one of its children (a term with an is_a relationship to sequence_variant).</li>
                    <li>The second field is a 0-based index value that identifies which Variant_seq the effect is being described for. For example, if this Variant_effect attribute is describing the effect of the first sequence in the Variant_seq attribute then the index value would be 0, for a Variant_effect attribute describing the second sequence in the Variant_seq attribute the index value would be 1 and so on (see the examples below). The index is necessary because multiple Variant_effect values may refer to the same Variant_seq.</li>
                    <li>The third field is a term describing the sequence feature that is being affected. This term must be the SO term sequence_feature or one of its children (a term with an is_a reltaionship to sequence_feature).</li>
                    <li>The fourth field (and all additional feilds) is/are feature ID(s). These feature IDs correspond to ID attributes in a GFF3 file that describe the sequence features (for example genes or mRNAs) annotated for the genome that are affected by this variant locus. Applications are optionally allowed to append a matched pair of parenthesis to the end of each ID. These parenthesis can contain additional application specific detail about the effect on this feature (for example the start and end of the altered seueqnce in the coordiantes of the feature). This additional detail is currently not defined by the GVF specification. It is the responsibility of the application to describe this additional data for the user. Allowing this additional data within the parenthesis is not however intended to support free text descriptions and must not violate the escaping conventions of the GVF specification or contain whitespace.</li>
                </ul>
                <p>White space is not allowed within any of the four fields. Both of the SO terms can be given either as a valid SO term name or the SO ID. If a single Variant_seq has different effects on different sequence features (for example it causes a non_synonymous_codon change in one mRNA, but a synonymous_codon change in another mRNA) then multiple Variant_effect values are given separated by commas.</p>
            </li>
            <li><strong>Format:</strong> Four space separated values.</li>
            <li>
                <strong>Examples:</strong>
                <p>An SNV who's sequence 'A' causes a non-synonymous codon change in the mRNAs NM_001160184 and NM_032129.</p>
                <pre>Variant_seq=A,T;Variant_effect=non_synonymous_codon 0 mRNA NM_001160184 NM_032129;</pre>
                <p>An SNV who's sequence 'A' causes a non-synonymous codon change in the mRNAs NM_001160184 and NM_032129. Parentheses contain application specific data. In this case the additional data represents the location, of the altered sequence in the coordinates of the feature; the reference and alternate codons and the reference and altered amino acids (start:end|ref_codon:alt_codon|ref_aa:alt_aa). The location of the altered sequences differs between the two mRNA because they are different splice forms.</p>
                <pre>Variant_seq=C,T;Variant_effect=non_synonymous_codon 0 mRNA NM_001160184(794:794|TTC:TCC|F:S) NM_032129(758:758|TTC:TCC|F:S);</pre>
            </li>
        </ul>
    </dd>
    <dt>Start_range End_range</dt>
    <dd>
        <ul>
            <li><strong>Required:</strong> Optional</li>
            <li><strong>Scope:</strong> Locus</li>
            <li>
                <strong>Description:</strong>
                <p>The Start_range and End_range attributes describe ambiguity in the start and end coordinates given in column 4 and 5. The description below is given for Start_range, but the same rules apply for End_range. The value is required to be two integers (or a '.' for unknown values) separated by a comma. The first value defines a range of ambiguity less-than the value given in column 4 and must be less-than or equal-to that coordinate (or '.') The second value defines a range of ambiguity greater-than the coordinate specified in column 4 and must be greater-than or equal-to the value of that coordinate (or '.'). If either value is equal-to the coordinate in column 4 then there is no ambiguity in that direction. The values given for these attributes are always relative to the landmark feature just as they are for the coordinates given in columns 4 and 5.</p>
            <li><strong>Valid Values:</strong> Two integers</li>
            <li><strong>Format:</strong> Comma separated</li>
            <li>
                <strong>Example:</strong>
                <p>A sequence_alteration that has a start coordinate of 2000 and an end coordinate of 5000, but for which there is 500 bp of ambiguity for both coordinates in either direction.</p>
                <pre>Start_range=1500,2500;End_range=4500,5500</pre>
            </li>
        </ul>
    </dd>
    <dt>Phased</dt>
    <dd>
        <ul>
            <li><strong>Required:</strong> Optional</li>
            <li><strong>Scope:</strong> Individual</li>
            <li>
                <strong>Description:</strong>
                <p>The Phased attribute can be used in conjuction with the Genotype attribute to define a set of sequence_alterations whose genotypes are phased (on the same chromosome). A phased genotype indicates that for heterozygous loci it is known which chromosome each sequence_alteration sequence belongs to. The ordering of the integer pairs in the Genotype attributes for a phased set of sequence_alterations is constrained for phased loci such that all sequences that occur together on one landmark feature (chromosome) are referenced by the same position in the Genotype tag. For example, if an individual has a sequence_alteration with 'Variant_seq=A,C;Genotype=0:1' attributes and another sequence_alteration on the same landmark feature (chromosome) has 'Variant_seq=G,T;Genotype=1:0' attributes then the A (0) and T (1) occur together on one copy of the chromosome because the both occupy first position of the integer pair in the Genotype attributes. Likewise, the C (1) and G (0) occur together on the other copy of that chromosome. If an entire GVF file (or a defined subset of it - for example all SNVs) is phased, then the '##phased-genotypes' pragma described below is used. However, if only regions of the genome are phased; the Phased attribute allows you to specify those features and to describe which features are phased together if mutliple phased regions exist. The Phased attribute only has meaning for loci that are heterozygous, but may be given for any sequence_alteration. The value of the Phased attribute can be any alphanumeric value. This value will serve as an ID for (and will be shared by all) features that are phased together on the given landmark feature and is not required to have meaning outside the file or in other contexts within the file. If multiple regions of the chromosome are phased then separate values should be used to group each set of phased features. To avoid ambiguity the same value should not be used for more than one phased region even if they lie on different landmark features (chromosomes). Multiple values (when specifying multi-individual GVF files) are separated from each other by commas.</p>
            </li>
            <li><strong>Valid Values:</strong> An alphanumeric string.</li>
            <li><strong>Format:</strong> A single value or comma separated list for multi-individual files.</li>
            <li>
                <strong>Example:</strong>
                <p>This sequence_alteration is in phase with all other sequence_alterations with the Phased=A13 attribute set.</p>
                <pre>Phased=A13;</pre>
            </li>
        </ul>
    </dd>
    <dt>Genotype</dt>
    <dd>
        <ul>
            <li><strong>Required:</strong> Required if the ##multi-individual pragma is in use, optional otherwise.</li>
            <li><strong>Scope:</strong> Individual</li>
            <li>
                <strong>Description:</strong>
                <p>The Genotype attribute describes which sequence(s) in the Variant_seq attribute are present in an individual. This attribute is useful (and in fact is required) when the ##multi-individual pragma (see below) is in use. Two colon separated integers are given as the value, and multiple comma separated integer pairs can be given. The integers are a 0-based index to the values given in the Variant_seq attribute.</p>
            </li>
            <li><strong>Valid Values:</strong> Colon separated integers (one value for each chromosome).</li>
            <li><strong>Format:</strong> Comma separated list of colon separated integer sets.</li>
            <li>
                <strong>Example:</strong>
                <p>The individual has an A on one chromosome and a T on the other at this locus.</p>
                <pre>Variant_seq=A,T;Genotype=0:1</pre>
                <p>There are two individuals in this multi-individual GVF file. The first individual has an A,T genotype and the second has a T,T genotype.</p>
                <pre>Variant_seq=A,T;Genotype=0:1,1:1</pre>
            </li>
        </ul>
    </dd>
    <dt>Individual</dt>
    <dd>
        <ul>
            <li><strong>Required:</strong> Optional</li>
            <li><strong>Scope:</strong> Locus</li>
            <li>
                <strong>Description:</strong>
                <p>The Individual attribute is used in conjunction with the ##multi-individual pragma to implement multi-individual GVF files. The value for this attribute is a comma separated list of integers. These integers are a 0-based index into the IDs given in the ##multi-individual pragma and indicate which individuals have sequence_alterations at this locus. In addition, the order of values given in other individual specific attributes corresponds to the order of the individuals specified here.</p>
                <p>See the section on multi-individual GVF files below.</p>
            </li>
            <li><strong>Valid Values:</strong> Integer</li>
            <li><strong>Format:</strong> Comma separated list.</li>
            <li>
                <strong>Example:</strong>
                <p>Individual NA18507 and NA19240 both have sequence_alteration at this locus in the multi-individual GVF file with data for three individuals.</p>
                <pre>
##gvf-version 1.07
##multi-individual NA18507, NA12878, NA19240
chr1 MAQ SNV 1023489 1023489 129 + . ID=chr1:maq_cns2snp:SNV:1023489;Variant_seq=C,G;Reference_seq=C;Individual=0,2;Genotype=1:1,0,1;</pre>
            </li>
        </ul>
    </dd>
    <dt>Variant_codon</dt>
    <dd>
        <ul>
            <li><strong>Required:</strong> Optional</li>
            <li><strong>Scope:</strong> Locus</li>
            <li>
                <strong>Description:</strong>
                <p>The Variant_codon attribute can be used to describe the codon(s) that overlap a feature as modified by the sequence(s) in the Variant_seq attribute. All codons are given in their entirety, so if the feature partially overlaps a codon, the codon is still given in it's entirety. If the feature overlaps some coding region and some non-coding region then the codons are given for the overlapped portion of the coding sequence. All codons are given as a single string of sequence the length of which must be divisible by three. One codon sequence string must be given for each sequence in the Variant_seq attribute separated by commas. This attribute can be provided as a convinience for users, however, there is potential for ambiguity if the sequence_alteration at this locus causes different codons on different mRNAs. Please consider using the Variant_effect attribute as shown in the example above for a more detailed way to describe codons on multiple mRNAs.</p>
            </li>
            <li><strong>Valid Values:</strong> Nucleic acid sequence (the length of which is divisible by 3).</li>
            <li><strong>Format:</strong> A comma separated list of nucleic acid sequences.</li>
            <li>
                <strong>Example:</strong>
                <p>An SNV overlaps one nucleotide in the given codon of the mRNA.</p>
                <pre>Variant_seq=A,G;Reference_seq=A;Variant_codon=GAG,GGG;</pre>
            </li>
        </ul>
    </dd>
    <dt>Reference_codon</dt>
    <dd>
        <ul>
            <li><strong>Required:</strong> Optional</li>
            <li><strong>Scope:</strong> Locus</li>
            <li>
                <strong>Description:</strong>
                <p>The Reference_codon attribute can be used to describe the codon(s) that overlap a feature on the reference genome. All codons are given in their entirety, so if the feature partially overlaps a codon, the codon is still given in it's entirety. If the feature overlaps some coding region and some non-coding region then the codons are given for the overlapped portion of the coding sequence. All codons are given as a single string of sequence the length of which must be divisible by three. Only one codon sequence string may be given since the reference genome sequence is represented as haploid.</p>
            </li>
            <li><strong>Valid Values:</strong> Nucleic acid sequence (the length of which is divisible by 3).
            <li><strong>Format:</strong> A nucleic acid sequence.
            <li>
                <strong>Example:</strong>
                <p>An SNV overlaps one nucleotide in the given codon (on the reference genome sequence) of the mRNA.</p>
                <pre>Variant_seq=A,G;Reference_seq=A;Reference_codon=GAG;</pre>
            </li>
        </ul>
    </dd>
    <dt>Variant_aa</dt>
    <dd>
        <ul>
            <li><strong>Required:</strong> Optional</li>
            <li><strong>Scope:</strong> Locus</li>
            <li>
                <strong>Description:</strong>
                <p>The Variant_aa attribute can be used to describe the amino acid(s) that overlap a feature as modified by the sequence(s) in the Variant_seq attribute. All overlapping amino acids are given, so if the feature partially overlaps an underlying codon, the amino acid is still given. If the feature overlaps some coding region and some non-coding region then the amino acids are given for the overlapping portion of the coding sequence. All amino acids are given as a single string of sequence. One amino acid sequence string must be given for each sequence in the Variant_seq attribute separated by commas. This attribute can be provided as a convinience for users, however, there is potential for ambiguity if the sequence_alteration at this locus causes different codons on different mRNAs. Please consider using the Variant_effect attribute as shown in the example above for a more detailed way to describe codons on multiple mRNAs.</p>
            </li>
            <li><strong>Valid Values:</strong> Amino acid sequence (single letter code).</li>
            <li><strong>Format:</strong> A comma separated list of amino acid sequences.</li>
            <li>
                <strong>Example:</strong>
                <p>An SNV overlaps one nucleotide in the given amino acid of the mRNA</p>
                <pre>Variant_seq=A,G;Reference_seq=A;Variant_aa=E,G;</pre>
            </li>
        </ul>
    </dd>
    <dt>Reference_aa</dt>
    <dd>
        <ul>
            <li><strong>Required:</strong> Optional</li>
            <li><strong>Scope:</strong> Locus</li>
            <li>
                <strong>Description:</strong>
                <p>The Reference_aa attribute can be used to describe the amino acid(s) that overlap a feature on the reference genome. All amino acids are given, so if the feature partially overlaps a portion of a codon, the amino acid is still given in it's entirety. If the feature overlaps some coding region and some non-coding region then the amino acids are given for the overlapping portion of the coding sequence. All amino acids are given as a single string of sequence. Only one amino acid sequence string may be given.</p>
            </li>
            <li><strong>Valid Values:</strong> Amino acid sequence.</li>
            <li><strong>Format:</strong> A single amino acid sequence.</li>
            <li>
                <strong>Example:</strong>
                <p>An SNV overlaps one nucleotide of the codon for the given amino acid (reference genome sequence) of the mRNA.</p>
                <pre>Variant_seq=A,G;Reference_seq=A;Reference_aa=E;</pre>
            </li>
        </ul>
    </dd>
    <dt>Breakpoint_detail</dt>
    <dd>
        <ul>
            <li><strong>Required:</strong> Optional</li>
            <li><strong>Scope:</strong> Locus</li>
            <li>
                <strong>Description:</strong>
                <p>The Breakpoint_detail attribute can be used to describe the source or destination of a zero-length sequence_alteration. Examples of these sequence_alterations are insertion, duplication, and translocation. The seqeunce correspoding to an insertion can be given in the Variant_seq attribute, but in cases where this sequence is very long the inserted sequence can be included with it's own seqid in the fasta file (or fasta portion of the GVF file) and referenced with the Breakpoint_detail attribute. In the case of a duplication, the source sequence of the duplication within the associated reference genome sequence can be referenced. When specifying a translocation, the destination of the translocated sequence within the genome can be specified. In general the format is seqid:start-end:strand (e.g. chr1:12345-67890:+). The seqid describes the landmark feature (chromosome) where the source/destination sequence is located. Note that the seqid is allowed to contain a colon, and thus care must be taken in parsing this field. The start and end values describe the coordinates of the source/destination sequence. The start value given alone specifies another break point in the genome (i.e. the destination of a translocation). The strand specifies the strand from which the source/destination sequence should be read. In the case of a translocation the strand specifies the strand on which the sequence continues at the destination.</p>
            </li>
            <li><strong>Valid Values:</strong> The seqid, start, end and strand.</li>
            <li><strong>Format:</strong> seqid:start-end:strand</li>
            <li>
                <strong>Example:</strong>
                <p>A duplication that originated from a region on the plus strand of chr1 begining at 12345 and ending at 67890.</p>
                <pre>Breakpoint_detail=chr1:12345-67890:+;</pre>
                <p>A translocation that causes the sequence of the forward strand at this breakpoint to continue on the minus strand at chr1:12345.</p>
                <pre>Breakpoint_detail=chr1:12345:-;</pre>
            </li>
        </ul>
    </dd>
    <dt>Breakpoint_range</dt>
    <dd>
        <ul>
            <li><strong>Required:</strong> Optional</li>
            <li><strong>Scope:</strong> Locus</li>
            <li><strong>Description:</strong>
                <p>The Breakpoint_range attribute describes ambiguity in the coordinate given in Breakpoint_detail. The value is required to be two integers (or a '.' for unknown values) separated by a comma. The first value defines a range of ambiguity less-than the value given in Breakpoint_detail and must be less-than or equal-to that coordinate (or '.') The second value defines a range of ambiguity greater-than the coordinate specified in Breakpoint_detail and must be greater-than or equal-to the value of that coordinate (or '.'). If either value is equal-to the coordinate in Breakpoint_detail then there is no ambiguity in that direction.</p>
                <p><em>The Breakpoint_range attribute describes ambiguity in the coordinate(s) given in Breakpoint_detail. The value is required to be two comma separated integers (or a '.' for unknown values) for each coordinate given in the Breakpoint_detail attribute. The Breakpoint_detail attribute can descirbe a single coordinate (in the case of a translocation, e.g. chr1:67890 or coordinate range using two coordinates (in the case of insertions and duplications, e.g. chr1:12345-67890). Thus if two coordinates are given in Breakpoint_detail, four integers (or unknown values) must be given in Breakpoint_range and the first two values of Breakpoint_range are associated with the first coordinate in Breakpoint_deail while the second two values of Breakpoint_range are associated with the second coordinate of Breakpoint_detail. The first value of each pair of values in Breakpoint_range defines a range of ambiguity less-than the associated coordinate in Breakpoint_detail and must be less-than or equal-to that coordinate (or '.') The second value of each pair defines a range of ambiguity greater-than the associated coordinate in Breakpoint_detail and must be greater-than or equal-to the value of that coordinate (or '.'). If either value is equal-to the associated coordinate in Breakpoint_detail then there is no ambiguity in that direction.</em></p>
            <li><strong>Valid Values:</strong> Two <em>or four</em> integers</li>
            <li><strong>Format:</strong> Comma separated</li>
            <li>
                <strong>Example:</strong>
                <p>An interchromosomal_breakpoint with a Breakpoint_detail coordinate of chr16:15000:+. There is 500 bp of ambiguity in either direction for both the location of the interchromosomal_breakpoint and the associated coordinate located on chr16 described by Breakpoint_detail.</p>
                <pre>chr4 . interchromosomal_breakpoint 2000 2000 . . . ID=ICB_001;Start_range=1500,2500;Breakpoint_detail=16:15000:+;Breakpoint_range=14500,15500</pre>
                <p>A duplication that originated from a region on the plus strand of chr1 begining at 10000 and ending at 60000.</p>
                <pre>Breakpoint_detail=chr1:10000-60000:+;Breakpoint_range=9500,15000,55000,65000</pre>
            </li>
        </ul>
    </dd>
    <dt>Sequence_context</dt>
    <dd>
        <ul>
            <li><strong>Required:</strong> Optional</li>
            <li><strong>Scope:</strong> Locus</li>
            <li>
                <strong>Description:</strong>
                <p>The Sequence_context attribute can be used to provide a region of sequence context from the reference genome around a sequence_alteration. This can be particularly useful for a feature like an insertion whose location on the genome can not be validated with sequence by the user of a GVF file. Two sequences are given, separated by a comma. Either sequence can be omitted and replaced by a '.'. The two sequences given are always in the order 5' of the sequence_alteration then 3' of the sequence_alteration on the plus strand of the reference genome. The sequences themselves are also always given on the plus strand.</p>
            </li>
            <li><strong>Valid Values:</strong> Two nucleotide sequences or the '.' character.</li>
            <li><strong>Format:</strong> Comma separated pair of nucleotide sequences.</li>
            <li>
                <strong>Example:</strong>
                <p>An insertion that is flanked by the 5' sequence 'ATCTGAGCTAGACTT' and the 3' sequence 'ATCCTATATCGGTAT'.</p>
                <pre>Sequence_context=ATCTGAGCTAGACTT,ATCCTATATCGGTAT;</pre>
            </li>
        </ul>
    </dd>
</dl>

#### Pragmas

##### GFF3 Pragmas

GFF3 provides pragmas that define file-wide directives to processing software. Pragmas should be given at the top of a GVF file before any feature lines are given. All pragmas from GFF3 are allowed in GVF. Note especially the following GFF3 pragmas which are encouraged for use with GVF:

    ##sequence-region
    ##feature-ontology
    ##genome-build

##### GVF Pragmas

In addition to GFF3 pragmas, GVF specifies an additional set of pragmas for describing the meta data associated with sequence_alterations. There are two basic type of pragmas specified by GVF simple and structured.

Simple GVF pragmas take single values in the form:

    ##pragma value

These simple pragmas take a single value and the value applies to all sequence_alterations in the file.

Structured pragmas have additional structure that allow more complex data to be described and these are described below.

##### Simple Pragmas

<dl>
    <dt>##gvf-version</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Simple</li>
            <li><strong>Description:</strong> The version of the GVF specification that this file conforms to. This is the only required pragma for GVF and is the first (or second) line of the file. Note that some GFF3 parsers will require a ##gff-version pragma at the top of the file as required by the GFF3 spec. GVF specific parsers should thus tolerate this pragma as the first line of the file and the ##gvf-version pragma as the second line.</li>
            <li><strong>Supported Values:</strong> Any valid GVF specification value (e.g. 1.10).</li>
            <li>
                <strong>Example:</strong>
                <pre>##gvf-version 1.10</pre>
            </li>
        </ul>
    </dd>
    <dt>##reference-fasta</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Simple</li>
            <li><strong>Description:</strong> A fasta file which provides the reference assembly for this genome.</li>
            <li><strong>Supported Values:</strong> A path, filename or URL giving the location of a fasta file.</li>
            <li>
                <strong>Example:</strong>
                <pre>##reference-fasta /data/genome_assembly.fa</pre>
            </li>
        </ul>
    </dd>
    <dt>##feature-gff3</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Simple</li>
            <li><strong>Description:</strong> If the sequence_alterations in this GVF file are annotated with Variant_effect attributes, this pragma provides the GFF3 formatted file that contains the features that the variant effects are annotated relative to.</li>
            <li><strong>Supported Values:</strong></li>
            <li>
                <strong>Example:</strong>
                <pre>##feature-gff3 /data/features.gff3</pre>
            </li>
        </ul>
    </dd>
    <dt>##file-version</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Simple</li>
            <li><strong>Description:</strong> The file-version pragma allows specification of the version of a file.</li>
            <li><strong>Supported Values:</strong> The format and interpretation of the version is left to the application, although a scheme of an incrementing decimal number where the fractional part of the number represents minor changes to formatting and documentation while changes to the integral part of the number represent actual changes to the sequence_alteration data is suggested.</li>
            <li>
                <strong>Example:</strong> This is version 1.01 of data represented in this file.
                <pre>##file-version 1.01</pre>
            </li>
        </ul>
    </dd>
    <dt>##file-date</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Simple</li>
            <li><strong>Description:</strong> The file-date pragma is included as a method to describe the date when the file was created or last modified.</li>
            <li><strong>Supported Values:</strong> The ISO 8601 standard for dates in the form YYYY-MM-DD is required for the value.</li>
            <li>
                <strong>Examples:</strong> The data in this file was created/modified on Feb. 8th 2012.
                <pre>##file-date 2012-02-08</pre>
            </li>
        </ul>
    </dd>
    <dt>##individual-id</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Simple</li>
            <li><strong>Description:</strong> This pragma provides an ID for the individual represented in the file. This ID would typically be unique within a working set of GVF files.</li>
            <li><strong>Supported Values:</strong>Any alphanumeric value.</li>
            <li>
                <strong>Example:</strong> A male of Yoruba origin whose ID in the Coriell database is NA18507
                <pre>##individual-id NA18507</pre>
            </li>
        </ul>
    </dd>
    <dt>##population</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Simple</li>
            <li><strong>Description:</strong> This pragma described the population that an individual belongs to.</li>
            <li><strong>Supported Values:</strong> It is recommended that, where possible, the codes used by HAPMAP and 1000 Genomes project for populations be used: ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/README_populations.md</li>
            <li>
                <strong>Examples:</strong> An individual from Nigeria of Yoruban descent.
                <pre>##population YRI</pre>
            </li>
        </ul>
    </dd>
    <dt>##sex</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Simple</li>
            <li><strong>Description:</strong> The genetic sex of the individual.</li>
            <li><strong>Supported Values:</strong> female, male</li>
            <li>
                <strong>Examples:</strong> A female individual.
                <pre>##sex female</pre>
            </li>
        </ul>
    </dd>
    <dt>##technology-platform-class</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Simple
            <li><strong>Description:</strong> The general type of technology platform used to generate the data supporting the sequence_alterations annotated in this file.
            <li>
                <strong>Supported Values:</strong>
                <ul>
                    <li>SRS: Short read sequencing (e.g. Illumina, SOLiD, 454, Ion_Torrent)</li>
                    <li>SMS: Single molecule sequencing (e.g. Helicos, PacBio)</li>
                    <li>Capillary: Capillary sequencing (e.g. ABI 3700)</li>
                    <li>DNA_Chip: DNA microarray SNP detection</li>
                </ul>
            </li>
            <li>
                <strong>Example:</strong> The sequence_alterations in the file were discovered by short-read sequencing.
                <pre>##technology-platform-class SRS</pre>
            </li>
        </ul>
    </dd>
    <dt>##technology-platform-name</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Simple</li>
            <li><strong>Description:</strong> The name of the specific platform on which the data supporting the sequence_alteration calls in the file were based.</li>
            <li>
                <strong>Supported Values:</strong>
                <ul>
                    <li>
                        Illumina: Any Illumina platform or use one of the following:
                        <ul>
                            <li>Illumina_GA</li>
                            <li>Illumina_GAII</li>
                            <li>Illumina_GAIIx</li>
                            <li>Illumina_HiSeq</li>
                            <li>Illumina_MiSeq</li>
                        </ul>
                    </li>
                    <li>SOLiD: ABI SOLiD</li>
                    <li>Complete_Genomics: Complete Genomics</li>
                    <li>Ion_Torrent: Ion Torrent</li>
                    <li>454_LS: 454 Life Sciences Pyrosequencing</li>
                    <li>Helicos: Helicos BioSciences tSMS</li>
                    <li>PACBIO_RS: Pacific Biosciences PACBIO RS</li>
                    <li>Affy_HS_6.0: Affymetrix Human SNP Array 6.0</li>
                    <li>Affy_HS_5.0: Human SNP Array 5.0</li>
                    <li>HumanOmni2.5-8: Illumina BeadChip HumanOmni2.5-8</li>
                    <li>HumanOmni1S-8: Illumina BeadChip HumanOmni1S-8</li>
                    <li>HumanOmni1-Quad: Illumina BeadChip HumanOmni1-Quad</li>
                    <li>HumanExome_1.0: Illumina BeadChip HumanExome 1.0</li>
                </ul>
                <p>Please help us keep this list up to date by sending e-mail to the <a href="mailto:song-devel@lists.sourceforge.net">SO developers</a> with new suggested values.</p>
            </li>
            <li>
                <strong>Examples:</strong> The sequence_alterations in the file were discovered by sequencing on Illumina GA.
                <pre>##technology-platform-name Illumina</pre>
            </li>
        </ul>
    </dd>
    <dt>##technology-platform-version</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Simple</li>
            <li><strong>Description:</strong> The version of a given sequencing technology.</li>
            <li><strong>Supported Values:</strong> Any valid alphanumeric string.</li>
            <li>
                <strong>Examples:</strong>
                <pre>##technology-platform-version 3</pre>
            </li>
        </ul>
    </dd>
    <dt>##technology-platform-machine-id</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Simple</li>
            <li><strong>Description:</strong> The ID or serial number for the machine that generated the data supporting the sequence_alterations in this file</li>
            <li><strong>Supported Values:</strong> Any valid alphanumeric string.</li>
            <li>
                <strong>Examples:</strong> The ID or serial number of the machine used to generate the data from which the sequence_alterations were discovered is DA39-A3EE-5E6B-4B0D325.
                <pre>##technology-platform-machine-id DA39-A3EE-5E6B-4B0D325</pre>
            </li>
        </ul>
    </dd>
    <dt>##technology-platform-read-length</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Simple</li>
            <li><strong>Description:</strong> For sequencing based technologies, this pragma provides a simple method to describe the read length for the sequencing run.</li>
            <li><strong>Supported Values:</strong> Integer</li>
            <li>
                <strong>Examples:</strong> The length of sequencing reads for the data used in this file is 100 bp.
                <pre>##technology-platform-read-length 100</pre>
            </li>
        </ul>
    </dd>
    <dt>##technology-platform-read-type</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Simple</li>
            <li><strong>Description:</strong> For sequencing based technologies, this pragma provides a simple method to describe the layout of the sequence reads - such as fragment or paired end reads.</li>
            <li>
                <strong>Supported Values:</strong>
                <ul>
                    <li>fragment: Single-end sequenced fragments</li>
                    <li>pair: Standard paired-end library</li>
                </ul>
            </li>
            <li>
                <strong>Examples:</strong>
                <p>The sequencing library used for sequencing this individual was sequenced from both ends generating paired reads from a paired-end library.</p>
                <pre>##technology-platform-read-type pair</pre>
            </li>
        </ul>
    </dd>
    <dt>##technology-platform-read-pair-span</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Simple</li>
            <li><strong>Description:</strong> For paired-end sequencing based technologies, this pragma provides a simple method to describe the average span of the paired sequence reads. The value describes the average genomic length (including the insert) from the beginning of a read to the end of it's mate.</li>
            <li><strong>Supported Values:</strong> Integer</li>
            <li>
                <strong>Example:</strong> The average read pair span for this library was 600 bp.
                <pre>##technology-platform-read-pair-span 600</pre>
            </li>
        </ul>
    </dd>
    <dt>##technology-platform-average-coverage</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Simple</li>
            <li><strong>Description:</strong> For sequencing based technologies, this pragma provides a simple method to describe the average depth of coverage for all supporting sequence reads combined.</li>
            <li><strong>Supported Values:</strong> Integer</li>
            <li>
                <strong>Examples:</strong> The average depth of coverage for this entire genome was 27.
                <pre>##technology-platform-average-coverage 27</pre>
            </li>
        </ul>
    </dd>
    <dt>##sequencing-scope</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Simple</li>
            <li><strong>Description:</strong> This pragma can used to describe extent of the genome sequencing for sequence based sequence_alteration calls.
            <li>
                <strong>Supported Values:</strong>
                <ul>
                    <li>whole_genome</li>
                    <li>whole_exome</li>
                    <li>targeted_capture</li>
                </ul>
                <p>Please help us keep this list up to date by sending e-mail to the <a href="mailto:song-devel@lists.sourceforge.net">SO developers</a> with new suggested values.</p>
            </li>
            <li><strong>Examples:</strong> The data from which the sequence_alterations in this file were generated came from whole genome sequencing.</li>
        </ul>
    </dd>
    <dt>##capture-method</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Simple</li>
            <li><strong>Description:</strong> For sequence that derived from exome or other targeted capture techniques this pragma a method to describe the capture technique.</li>
            <li>
                <strong>Supported Values:</strong> One of the following values or free text.
                <ul>
                    <li>TruSeq_exome</li>
                    <li>TargetSeq</li>
                    <li>SureSelect_exome</li>
                    <li>SureSelect_custom</li>
                    <li>NimbleGen_exome</li>
                    <li>NimbleGen_custom</li>
                    <li>Other_exome</li>
                    <li>Other_custom</li>
                </ul>
                <p>Please help us keep this list up to date by sending e-mail to the <a href="mailto:song-devel@lists.sourceforge.net">SO developers</a> with new suggested values.</p>
            </li>
            <li>
                <strong>Examples:</strong> The DNA from which the sequence_alterations in this file was enriched for exon regions using the NimbleGen exome capture kit.
                <pre>##capture-method NimbleGen_exome</pre>
            </li>
        </ul>
    </dd>
    <dt>##capture-regions</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Simple</li>
            <li><strong>Description:</strong> For sequence that derived from exome or other targeted capture techniques this pragma provides a method to describe a file (BED format suggested) that contains the targeted regions.</li>
            <li>
                <strong>Supported Values:</strong> A path, file or URL
                <p>Please help us keep this list up to date by sending e-mail to the <a href="mailto:song-devel@lists.sourceforge.net">SO developers</a> with new suggested values.</p>
            </li>
            <li>
                <strong>Examples:</strong>
                <pre>##capture-regions /path/to/exon_regions.bed</pre>
            </li>
        </ul>
    </dd>
    <dt>##sequence-alignment</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Simple</li>
            <li><strong>Description:</strong> This pragma describes the sequence alignment algorithm/pipline used.</li>
            <li><strong>Supported Values:</strong> Free text</li>
            <li>
                <strong>Examples:</strong>
                <pre>##sequence-alignment Reads were aligned with bwa using default parameters</pre>
            </li>
        </ul>
    </dd>
    <dt>##variant-calling</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Simple</li>
            <li><strong>Description:</strong> This pragma describes the variant calling pipeline used.</li>
            <li><strong>Supported Values:</strong> Free text</li>
            <li>
                <strong>Examples:</strong>
                <pre>##variant-calling Variants were called with samtools mpileup.</pre>
            </li>
        </ul>
    </dd>
    <dt>##sample-description</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Simple</li>
            <li><strong>Description:</strong> This pragma describes the sample from which the sequence_alterations were derived.</li>
            <li><strong>Supported Values:</strong> Free text</li>
            <li>
                <strong>Examples:</strong>
                <pre>##sample-description Sample obtained from peripheral blood.</pre>
            </li>
        </ul>
    </dd>
    <dt>##genomic-source</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Simple</li>
            <li><strong>Description:</strong> This pragma uses LOINC (http://loinc.org/) codes to describe the genomic source from which the sequence_alterations were derived.</li>
            <li><strong>Supported Values:</strong> prenatal, somatic, germline</li>
            <li>
                <strong>Examples:</strong>
                <pre>##genomic-source somatic</pre>
            </li>
        </ul>
    </dd>
    <dt>##multi-individual</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Simple</li>
            <li>
                <strong>Description:</strong>
                <p>GVF files with data sets of multiple individuals are handled in GVF by specifying the ##multi-individual pragma. The value of this pragma is two or more unique IDs for the individuals whose data is represented in the file. When this pragma is given additional attribute tags are required, and additional constraints on existing attribute tags are required. These additional constraints are described <a href="#multi-individual-gvf-files">below</a>.</p>
            </li>
            <li><strong>Supported Values:</strong> A comma separated list of individual IDs.</li>
            <li>
                <strong>Examples:</strong>
                <pre>##multi-individual NA18507,NA19240,NA19238,NA12878</pre>
            </li>
        </ul>
    </dd>
</dl>

##### Structured Pragmas Summary

Structured pragmas have additional structure that allow more complex data to be described and in most cases allow the pragma to apply only to a subset of the sequence_alterations in a file. These structured pragmas can also take the simple form having only a single value as shown in the example below.

    ##pragma free text value

If there is no '=' (equal sign) contained in the value, then the pragma's value is interpreted as simple and the value is treated as free text. The default tag to which the value should be assigned when a structured pragma is given in simple form is designated for each pragma below.

Usually however, structured pragmas are used because the information being described is more complex than allow by simple pragmas, or because the scope of the pragma is being limited to a subset of the sequence_alterations in the file. These pragmas take on the following form:

    ##pragma tag=value;tag=value1,value2;

The structured form above has tag/value pairs. The tags are separated from the values by the equal '=' sign. Multiple tag=value pairs are separated from each other by a semicolon ';' and multiple values for a given tag are separated by a comma ','. This is the same tag/value structure used in column 9 of GFF3/GVF files. The supported tags for each structured pragma are specified below.

Tags beginning with upper case letters are reserved for future use within the GVF spec, but additional tags can be used on an application specific basis if they begin with lower case letters.

##### Common Tags for Structured Pragmas

The following tags are allowed in many structured pragmas and thus are described separately here:

<dl>
    <dt>Seqid</dt>
    <dd>The Seqid tag can be used to limit the scope of a pragma. The value can be any valid seqid(s) found in column 1 of the GVF file, given as a comma separated list. The pragma then applies only to those seqids listed. If the tag is omitted or has no value, then the pragma applies to sequence_alterations on all seqids.</dd>
    <dt>Source</dt>
    <dd>The Source tag can be used to limit the scope of a pragma. The value can be any valid source(s) found in column 2 of the GVF file, given as a comma separated list. The pragma then applies only to those sequence_alterations with a specified source value in column 2. If the tag is omitted or has no value, then the pragma applies to all sequence_alterations.</dd>
    <dt>Type</dt>
    <dd>The Type tag can be used to limit the scope of a pragma. The value can be any valid type(s) found in column 3 of the GVF file, given as a comma separated list. The pragma then applies only to those sequence_alterations with a specified type value in column 3. If the tag is omitted or has no value, then the pragma applies to all sequence_alterations.</dd>
    <dt>Dbxref</dt>
    <dd>When a Dbxref tag is specified it's value takes the form DB_Name:ID as described below in the <a href="#database-cross-references">Database Cross References</a> section, such as the location of sequence files or a link to a paper describing a method.</dd>
    <dt>Comment</dt>
    <dd>The Comment tag is allowed for all structured pragmas to provide a more human readable description.</dd>
</dl>

For example if a pragma only applies to SNVs, nucleotide insertions and nucleotide deletions that were called by mpileup on chromosome 13 then the following would indicate the scope for the given pragma:

    Seqid=chr13;Source=mpileup;Type=SNV,nucleotide_insertion,nucleotide_deletion;

##### Structured Pragmas

<dl>
    <dt>##technology-platform</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Structured
            <li>
                <strong>Description:</strong>
                <p>This pragma provides details about the technologies (i.e. sequencing or micro array) used to generate the primary data.</p>
            </li>
            <li><strong>Supported Tags:</strong> Seqid, Source, Type, Dbxref, Comment</li>
            <li><strong>Default Tag:</strong> Comment</li>
            <li>
                <strong>Additional Tag/Value Pairs:</strong>
                <ul>
                    <li><strong>Tag:</strong> Platform_class</li>
                    <li>
                        <strong>Values:</strong>
                        <ul>
                            <li>SRS: Short read sequencing (e.g. Illumina, SOLiD, 454, Ion_Torrent)</li>
                            <li>SMS: Single molecule sequencing (e.g. Helicos, PacBio)</li>
                            <li>Capillary: Capillary sequencing (e.g. ABI 3700)</li>
                            <li>DNA_Chip: DNA microarray SNP detection</li>
                        </ul>
                        <p>Please help us keep this list up to date by sending e-mail to the <a href="mailto:song-devel@lists.sourceforge.net">SO developers</a> with new suggested values.</p>
                    </li>
                    <li><strong>Tag: Platform_name</strong></li>
                    <li>
                        <strong>Values:</strong>
                        <ul>
                            <li>Illumina: Illumina GA, GAII and GAIIx</li>
                            <li>SOLiD: ABI SOLiD</li>
                            <li>Complete_Genomics: Complete Genomics</li>
                            <li>Ion_Torrent: Ion Torrent</li>
                            <li>454_LS: 454 Life Sciences Pyrosequencing</li>
                            <li>Helicos: Helicos BioSciences tSMS</li>
                            <li>PACBIO_RS: Pacific Biosciences PACBIO RS</li>
                            <li>Affy_HS_6.0: Affymetrix Human SNP Array 6.0</li>
                            <li>Affy_HS_5.0: Human SNP Array 5.0</li>
                            <li>HumanOmni2.5-8: Illumina BeadChip HumanOmni2.5-8</li>
                            <li>HumanOmni1S-8: Illumina BeadChip HumanOmni1S-8</li>
                            <li>HumanOmni1-Quad: Illumina BeadChip HumanOmni1-Quad</li>
                        </ul>
                        <p>Please help us keep this list up to date by sending e-mail to the <a href="mailto:song-devel@lists.sourceforge.net">SO developers</a> with new suggested values.</p>
                    </li>
                    <li><strong>Tag:</strong> Read_length</li>
                    <li><strong>Value:</strong>Integer</li>
                    <li><strong>Tag:</strong> Read_type</li>
                    <li><strong>Values:</strong> fragment, pair</li>
                    <li><strong>Tag:</strong> Read_pair_span</li>
                    <li><strong>Value:</strong> Integer</li>
                    <li><strong>Tag:</strong> Average_coverage</li>
                    <li><strong>Value:</strong> Integer</li>
                </ul>
            </li>
            <li>
                <strong>Examples:</strong>
                <p>All features with source SOAP and type SNV were called from data that was generated by an Illumina GA short read sequencer. There was both a single read (fragment) library and two paired-end libraries. The read length for both libraries is 35 base pairs, and the paired-end libraries had an average read span of 135 and 440 from beginning of the first read to the end of the second. The average depth of coverage for sequencing from all libraries combined was 36x haploid coverage against the reference genome.</p>
                <pre>##technology-platform Source=SOAP;Type=SNV;Dbxref=URI:http://www.illumina.com;Platform_class=SRS;Platform_name=Illumina;Read_type=fragment,pair;Read_length=35;Read_pair_span=135,440;Average_coverage=36;</pre>
                <p>All features with source AFFY_SNP_6 and type SNV are called from data that was generated by SNP Array genotyping with an Affymetrix Human SNP Array 6.0.</p>
                <pre>##technology-platform Seqid=chr1;Source=AFFY_SNP_6;Type=SNV;Dbxref=URI:http://www.affymetrix.com;Platform_class=DNA_Chip;Platform_name=Affymetrix Human SNP Array 6.0;</pre>
            </li>
        </ul>
    </dd>
    <dt>##data-source</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Structured</li>
            <li>
                <strong>Description:</strong>
                <p>This pragma provides details about the source data for the sequence_alterations contained in this file. This may include links to the actual sequence reads in a trace archive, or links to a sequence_alteration file in another format that have been converted to GVF.</p>
            </li>
            <li><strong>Supported Tags:</strong> Seqid, Source, Type, Dbxref, Comment</li>
            <li><strong>Default Tag:</strong> Comment</li>
            <li>
                <strong>Additional Tag/Value Pairs:</strong>
                <ul>
                    <li>Tag: Data_type</li>
                    <li>
                        Values:
                        <ul>
                            <li>DNA_sequence</li>
                            <li>RNA_sequence</li>
                            <li>DNA_microarray</li>
                            <li>Array_CGH</li>
                        </ul>
                    </li>
                </ul>
                <p>Please help us keep this list up to date by sending e-mail to the <a href="mailto:song-devel@lists.sourceforge.net">SO developers</a> with new suggested values.</p>
            </li>
            <li>
                <strong>Examples:</strong>
                <p>The data for all features with source MAQ and type SNV are based on DNA sequence data from NCBI Short Read Archive ID SRA008175.</p>
                <pre>##data-source Source=MAQ;Type=SNV;Dbxref=SRA:SRA008175;Data_type=DNA sequence;Comment=NCBI Short Read Archive (http://www.ncbi.nlm.nih.gov/Traces/sra);</pre>
                <p>The data for all features with source MAQ and type SNV were converted to GVF format from sequence_alteration information that can be downloaded from the given URI.</p>
                <pre>##data-source Source=MAQ;Type=SNV;Dbxref=URI:ftp://ftp.kobic.kr/pub/KOBIC-KoreanGenome/genetic_variations/KOREF-solexa-snp-X30_Q40d4D100.gff;Data_type=sequence_alteration;Comment=Korean Bioinformation Center (KOBIC) FTP site;</pre>
            </li>
        </ul>
    </dd>
    <dt>##score-method</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Structured</li>
            <li><strong>Description:</strong> This pragma provides details about the algorithms or methodologies used to generate the score of a feature. A typical use would be to provide a Dbxref link to a journal article or website describing the software or algorithm used to calculate the score.</li>
            <li><strong>Supported Tags:</strong> Seqid, Source, Type, Dbxref, Comment</li>
            <li><strong>Default Tag:</strong> Comment</li>
            <li>
                <strong>Examples:</strong>
                <pre>##score-method Comment=Scores are Phred scaled probabilities of an incorrect sequence_alteration call</pre>
            </li>
        </ul>
    </dd>
    <dt>##source-method</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Structured</li>
            <li><strong>Description:</strong> This pragma provides details about the algorithms or methodologies used to generate data for a given source in the file. This is used for example to document how a particular type of sequence_alteration was called. A typical use would be to provide a Dbxref link to a journal article describing software used for calling the sequence_alteration data with the given source tag.</li>
            <li><strong>Supported Tags:</strong> Seqid, Source, Type, Dbxref, Comment</li>
            <li><strong>Default Tag:</strong> Comment</li>
            <li>
                <strong>Examples:</strong>
                <p>Features on chromosome 1 whose source is MAQ and type is SNV had SNVs called by MAQ which is referenced with PubMed ID 18714091.</p>
                <pre>##source-method Seqid=chr1;Source=MAQ;Type=SNV;Dbxref=PMID:18714091;Comment=MAQ SNV calls;</pre>
                <p>Features on all chromosomes with source SOAP and type SNV were called by SOAPsnp which is referenced with PubMed IDs 18227114 and 18987735.</p>
                <pre>##source-method Source=SOAP;Type=SNV;Dbxref=PMID:18227114,PMID:18987735;Comment=Short Elongated Alignment Program (SOAP);</pre>
            </li>
        </ul>
    </dd>
    <dt>##attribute-method</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Structured
            <li><strong>Description:</strong> This pragma provides details about algorithms or methodologies for a given attribute tag in the file. This is used to document how a particular type of attribute value (e.g. Zygosity, Variant_effect) was calculated.
            <li><strong>Supported Tags:</strong> Seqid, Source, Type, Dbxref, Comment
            <li><strong>Default Tag:</strong> Comment
            <li>
                <strong>Examples:</strong>
                <p>The value for the Zygosity attributes for all features with a source SOLiD and a type SNV were passed through into the GVF file 'as-is' from the original data file. A Dbxref could have been included as well to reference the paper of the original study.</p>
                <pre>##attribute-method Source=SOLiD;Type=SNV;Attribute=Zygosity;Comment=Zygosity is reported here as determined in the original study.</pre>
            </li>
        </ul>
    </dd>
    <dt>##phenotype-description</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Structured</li>
            <li><strong>Description:</strong> A description of the phenotype of the individual. This pragma can contain either ontology constrained terms, or a free text description of the individual's phenotype or both. In the first case the constraining ontology is given with the Ontology tag which is a URI pointing to an ontology file. The IDs or terms given with the Term tag are then required to be in the given ontology. Multiple terms may included as a comma separated list. A free text description of the phenotype may be given in the Comment tag to supplement or replace the ontology description. Support for phenotype annotation in GVF is a work in progress. A <a href="http://www.sequenceontology.org/so_wiki/index.php/Using_Phenotype_Ontologies_in_GVF">wiki page</a> has been created to foster a discussion about best practices for using phenotype ontologies in GVF.</li>
            <li><strong>Supported Tags:</strong> Seqid, Source, Type, Dbxref, Comment</li>
            <li><strong>Default Tag:</strong> Comment</li>
            <li>
                <strong>Examples:</strong>
                <p>A text description of a 50 year old individual with AML.</p>
                <pre>##phenotype-description Comment=Individual presented at 50 years with AML;</pre>
                <p>An ontology description of an individual with AML.</p>
                <pre>##phenotype-description Term=acute myloid leukemia;Ontology=http://www.human-phenotype-ontology.org/human-phenotype-ontology.obo.gz;</pre>
                <p>An HPO term used to describe an individual with AML.</p>
                <pre>##phenotype-description Term=HP:0004808;Ontology=http://www.human-phenotype-ontology.org/human-phenotype-ontology.obo.gz;</pre>
                <p>A mature, sterile fly. Note that multiple terms are included in the Term value as a comma separated list.</p>
                <pre>##phenotype-description Term=sterile,mature;Ontology=http://www.berkeleybop.org/ontologies/pato.owl;</pre>
            </li>
        </ul>
    </dd>
    <dt>##phased-genotypes</dt>
    <dd>
        <ul>
            <li><strong>Type:</strong> Structured</li>
            <li>
                <strong>Description:</strong>
                <p>This pragma indicates that the genotypes in the file are phased. Tag-value pairs can be used to limit the scope of the pragma. For example, you may want the pragma to define only SNV features or only features with a given source value as phased. Phased genotypes indicate that for heterozygous loci it is known which chromosome each sequence_alteration sequence belongs to. That is, if one sequence_alteration has 'Variant_seq=A,C;Genotype=0:1;' attributes and another sequence_alteration on the same landmark feature (chromosome) has 'Variant_seq=T,G;Genotype=1:0;' attributes then the A (0) and G (1) occur together on one copy of the chromosome because their both correspond to the Genotype value in the first position. Likewise the C and T occur together on the other copy of that chromosome. This information can come from pedigree data, population data or complete or partial chromosome assembly. When the '##phased-genotypes' pragma is given for a file, the sequences given in the Variant_seq attributes for all features (except as limited by the tag-value pairs described above) are required to be ordered in this way. That is, the first sequence given (with the Genotype attribute) is on the same copy of the chromosome as the first sequence given in all other Genotype attributes covered and likewise, the second sequence given is always on the other copy of the chromosome. Note that you can use the Phased attribute (see above) to indicate that individual features are phased. The phased attribute would be used when only particular regions of the genome are phased.</p>
            </li>
            <li><strong>Supported Tags:</strong> Seqid, Source, Type, Dbxref, Comment</li>
            <li>
                <strong>Examples:</strong>
                <p>All sequence_alterations are phased in this file.</p>
                <pre>##phased-genotypes</pre>
                <p>Only the SNVs in the file are phased.</p>
                <pre>##phased-genotypes Type=SNV;</pre>
            </li>
        </ul>
    </dd>
</dl>

#### Comments

Other less structured details can be included in the form of free text comments. And since they are for human reading only they can scroll over multiple lines with a comment identifier '#' at the beginning of each line. GVF parsers are allowed to ignore comments.

    # laboratory-description
    # This is individual X035 that was originally mislabeled as X053. As of 03/08/10 all labels are up to date.

#### Database Cross References

For all attributes and pragmas in GVF that have a Dbxref tag, the value is in the form database:ID. For example an SNV might have a cross reference to dbSNP and OMIM as 'Dbxref=dbSNP:rs113993958,OMIM:602421.0004'. The format of each type of ID varies from database to database. An authoritative list of databases, their DBTAGs, and the URI transformation rules that can be used to fetch the objects given their IDs can be found at this location:

ftp://ftp.geneontology.org/pub/go/doc/GO.xrf_abbs

Further details can be found here:

ftp://ftp.geneontology.org/pub/go/doc/GO.xrf_abbs_spec

In addition, database:ID pairs can point to a stable URN, URL or URI with Dbxref=URL:http//www.example.com for example.

#### Multi-individual GVF Files

While the GVF format was designed primarily for personal genomics - containing rich information about the effects of sequence alterations for a single individual per file, it works equally well to convey genotype information for mutliple individuals in addition to the rich Variant_effect information that it conveys for each locus. GVF provides a way to handle the genotype information for multiple individuals on a single feature line in two steps. First, the ##multi-individual pragma is given at the top of the file which specifies that the file will contain data for multiple individuals and gives a unique ID for each individual contained in the file. Second, on each of the feature lines in the file an "Individual" attribute is given. This attribute contains a list of individuals that have sequence_alterations at this locus represented as a list of integers that provide a 0-based index to the IDs in the ##individual-id pragma.

1.  The Variant_seq attribute captures all the sequences seen at this locus in any individual contained within the file.
2.  An "Individual" attribute is required for each sequence_alteration feature given in a multi-individual GVF file. This attribute contains a comma separated list of integers which provide a 0-based index into the ##multi-individual pragma ID list. This list of indexes identifies which individuals have sequence_alterations at this locus. Individuals who are homozygous reference at this locus are not included in the "Individual" attribute list.
3.  A Genotype attribute is required for each sequence_alteration feature which is a comma separated list of integer pairs. The integers within the pairs are separated by a colon. These integers are a 0-based index into the Variant_seq attribute and identify the genotype of each individual as described above. This list of pairs gives genotype information about each individual identified in the "Individual" attribute and must contain an entry (or use '.:.' for unknown) for each of those individuals.
4.  Other attributes such as Variant_reads, and Phased may also contain comma separated lists which give information for each individual in a similar fashion to that described above for Genotype. See those attribute descriptions for more details.

```
##gvf-version 1.10
##feature-ontology http://www.sequenceontology.org/resources/obo_files/current_release.obo
##multi-individual NA19240,NA18507,NA12878,NA19238
##genome-build NCBI B36.3
##sequence-region chr16 1 88827254

chr16 dbSNP SNV 49291360 49291360 . + . ID=ID_2;Variant_seq=C,G;Individual=0,1,2,3;Genotype=0:1,0:0,1:1,0:1;
chr16 dbSNP SNV 49302125 49302125 . + . ID=ID_3;Variant_seq=C,T;Individual=0,1,3;Genotype=0:1,2:2,0:2;
chr16 dbSNP SNV 49302365 49302365 . + . ID=ID_4;Variant_seq=G;Individual=0,1;Genotype=0:0,0:0;
chr16 dbSNP SNV 49302700 49302700 . + . ID=ID_5;Variant_seq=C,T;Individual=2,3;Genotype=0:1,0:0;
chr16 dbSNP SNV 49303084 49303084 . + . ID=ID_6;Variant_seq=T,G,A;Individual=3;Genotype=1,2:;
chr16 dbSNP SNV 49303427 49303427 . + . ID=ID_8;Variant_seq=T;Individual=0;Genotype=0:0;
chr16 dbSNP SNV 49303596 49303596 . + . ID=ID_9;Variant_seq=A,G,T;Individual=0,1,3;Genotype=1:2,3:3,1:3;
```

#### GVF Feature Examples

A heterozygous SNV with sequences G and C having 17 and 16 reads respectively. This SNV falls within the CDS of the RefSeq mRNAs NM_012345 and NM_543210 and the sequence_alteration causes a variant_effect of nonsynonymous_codon with respect to the mRNAs NM_012345 and NM_543210. The second variant sequence 'C' is the same as the reference and thus has no functional effect relative to the reference.

    chr1 SOAP SNV 15883 15883 36.5 + . ID=chr1:SOAP:SNV:15883;Variant_seq=G,C;Zygosity=heterozygous;Variant_reads=17,16;Total_reads=33;Reference_seq=C;Variant_effect=nonsynonymous_codon 0 mRNA NM_012345,NM_543210;

A homozygous deletion in the individual genome relative to the reference genome. The region deleted is longer than 50 nucleotides and thus the GVF simply has a'~' as the Reference_sequence value. There are a total of 27 reads on average spanning this region, of which 26 on average supported the deletion.

    chr1 Celera nucleotide_deletion 8834426 8834497 . + . ID=ABC_98765;Variant_seq=-;Reference_seq=~;Variant_reads=26;Total_reads=27;

A Copy number variant from [NCBI dbVar](https://www.ncbi.nlm.nih.gov/dbvar/) for which there is some ambiguity in the start and end coordinates that is described with the Start_range and End_range attributes.

    NC_000010.9 dbVar copy_number_variation 51580055 51580298 . . . ID=nssv8537;Name=nssv8537(Loss);Parent=nsv5710;Start_range=51580055,51580132;End_range=51580242,51580298;

Another copy number variant as above. In this example the outer boundaries of the ranges are given with nucleotide resolution, but the inner boundaries are unknown.

    NC_000024.7 dbVar copy_number_variation 9188753 9995409 . . . ID=nssv27813;Name=nssv27813(Gain);Parent=nsv10015;Start_range=9188753,.;End_range=.,9995409;

Another copy number variant as above. In this example the inner boundaries of the ranges are given with nucleotide resolution, but the outer boundaries are unknown.

    NC_000001.9 dbVar copy_number_variation 213446726 220244033 . . . ID=nsv491682;Name=nsv491682(CNV);Start_range=.,213446726;End_range=220244033,;

A complex example where a t(8;21)(q22;q22.3) translocation, an inversion and an SNV overlap. These are actually three separate sequence_alterations each mapping to a common region of the reference genome and thus are described as three separated records in the GVF file - each relative to its location on the reference genome.

    chr21 BreakDancer translocation 41400000 46944323 . + . ID=t8-21_q22-q22.3;Dbxref=PMID:2052570
    chr21 DGV inversion 42061144 42083169 . + . ID=Variation_37237;
    chr21 samtools SNV 42071394 42071394 . + . ID=rs2989342;Variant_seq=C,T;Reference_seq=T

The two SNVs below are phased. In this case the individual is a compound heterozygote with the Variant_seq G on one copy of the chromosome (first value given in the Variant_seq attribute) and the other Variant_seq A on the other copy of the chromosome (second value given in the Variant_seq attribute).

    chr1 GATK SNV 15883 15883 . + . ID=SNV001;Reference_seq=C;Variant_seq=G,C;Zygosity=heterozygous;Phased=001;
    chr1 GATK SNV 15765 15765 . + . ID=SNV002;Reference_seq=G;Variant_seq=G,A;Zygosity=heterozygous;Phased=001;

A region of the genome for which there is no sequence alignment data and thus is designated as no-call by using a gap feature. Without the gap feature this region would be assumed to be homozygous reference.

    chr1 CG gap 108873 1109456 . + . ID=GAP001;

A few lines from a more complete GVF file are shown below with the functional consequence of each sequence_alteration annotated relative to their effect on coding for mRNA/protein annotations (scroll right to see complete lines):

    ##gvf-version 1.07
    ##feature-ontology http://www.sequenceontology.org/resources/obo_files/current_release.obo
    ##genome-build NCBI B36.3
    ##sequence-region chr16 1 88827254

    chr16 samtools SNV 49291141 49291141 . + . ID=ID_1;Variant_seq=A,G;Reference_seq=G;Zygosity=heterozygous;Variant_effect=synonymous_codon 0 mRNA NM_022162;
    chr16 samtools SNV 49302125 49302125 . + . ID=ID_3;Variant_seq=T,C;Reference_seq=C;Zygosity=heterozygous;Variant_effect=nonsynonymous_codon 0 mRNA NM_022162;Alias=NP_071445.1:p.P45S;
    chr16 samtools SNV 49302365 49302365 . + . ID=ID_4;Variant_seq=G,C;Reference_seq=C;Zygosity=heterozygous;Variant_effect=non_synonymous_codon 0 mRNA NM_022162;Alias=NP_071445.1:p.L125V;
    chr16 samtools SNV 49302700 49302700 . + . ID=ID_5;Variant_seq=T;Reference_seq=C;Zygosity=homozygous;Variant_effect=synonymous_codon 0 mRNA NM_022162;
    chr16 samtools SNV 49303084 49303084 . + . ID=ID_6;Variant_seq=G,T;Reference_seq=T;Zygosity=heterozygous;Variant_effect=synonymous_codon 0 mRNA NM_022162;
    chr16 samtools SNV 49303156 49303156 . + . ID=ID_7;Variant_seq=T,C;Reference_seq=C;Zygosity=heterozygous;Variant_effect=synonymous_codon 0 mRNA NM_022162;
    chr16 samtools SNV 49303427 49303427 . + . ID=ID_8;Variant_seq=T,C;Reference_seq=C;Zygosity=heterozygous;Variant_effect=non_synonymous_codon 0 mRNA NM_022162;Alias=NP_071445.1:p.R479W;
    chr16 samtools SNV 49303596 49303596 . + . ID=ID_9;Variant_seq=T,C;Reference_seq=C;Zygosity=heterozygous;Variant_effect=non_synonymous_codon 0 mRNA NM_022162;Alias=NP_071445.1:p.A535V;

#### Acknowledgments

We would like to thank the NHGRI for funding this work (R44HG2991, R44HG3667). We would like to thank the following people for feedback and contributions to the specification (in alphabetical order):

-   Steve Chervitz
-   Deanna Church
-   Fiona Cunningham
-   Tim Hefferon
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

<dl>
    <dt>1.10 Fri 7 Oct 2016</dt>
    <dd>
        <ul>
            <li>Updated "##gvf-version 1.08" to "##gvf-version 1.10" throughout.</li>
        </ul>
    </dd>
    <dt>1.09 Thu 19 May 2016</dt>
    <dd>
        <ul>
            <li>Added SO:0002073 no_variation as allowable under Column 3: "type".</li>
            <li>Updated references "##gvf-version 1.07" to "##gvf-version 1.08" throughout.</li>
        </ul>
    </dd>
    <dt>1.08 Mon 2 May 2016</dt>
    <dd>
        <ul>
            <li>Converted from HTML to Markdown.</li>
        </ul>
    </dd>
    <dt>1.07 Mon 28 Apr 2014</dt>
    <dd>
        <ul>
            <li>Updated the ##technology-platform-name pragma.</li>
            <li>Added the Breakpoint_range attribute and made minor updates to Breakpoint_detail to support those changes.</li>
            <li>Updated wording for the Reference_seq attribute and changed it's status to required.</li>
        </ul>
    </dd>
    <dt>1.06 Tue 24 Jan 2012</dt>
    <dd>
        <ul>
            <li>Added "Blue Box" description of a very simple implimentation of GVF.</li>
            <li>Reformatted attribute definitions to provide more detail and a more consistent structure.</li>
            <li>Added additional valid characters for Varaint_seq and Reference_seq [!^] to support hemizygous and no-called loci.</li>
            <li>Added the Zygosity attribute.</li>
            <li>Modified the format of the Genotype attribute.</li>
            <li>Added the Individual attribute to support population based GVF files.</li>
            <li>Added the Breakpoint_detail attribute.</li>
            <li>Added the Sequence_context attribute.</li>
            <li>Added a distinction between simple and structured pragmas. Updated the format of all pragmas to provide more detail and more consistent structure. Added several new simple pragmas including: population, sex, technology-platform-class, technology-platform-name, technology-platform-machine-id technology-platform-read-length, technology-platform-read-type, technology-platform-read-pair-span, technology-platform-average-coverage, sequencing-scope, sequence-alignment, variant-calling, sample-description, genomic-source multi-individual.</li>
            <li>Added the attribute-method pragma to describe methods used to calculate attribute values.</li>
            <li>Added the multi-individual pragma and a description of support for multi-individual GVF.</li>
            <li>Added/modified several examples.</li>
            <li>Removed the Variant_copy_number and Reference_copy_number attributes.</li>
            <li>The values for attributes which provide data that corresponds to each copy of a chromosome (Genotype and Variant_reads) have colon separated values for each chromosome present and comma separated lists of these value sets for multiple individuals in a multi-individual GVF.</li>
        </ul>
    </dd>
    <dt>1.05 Wed 19 Jan 2011</dt>
    <dd>
        <ul>
            <li>Modified the description and examples for the Start_range and End_range tag slightly as per discussions with NCBI, Ensembl and UCSC.</li>
            <li>Modified the wording under Community Attributes Collection slightly.</li>
            <li>Several typo fixes that were caught by Bob Kuhn and John Lopez.</li>
        </ul>
    </dd>
    <dt>1.04 Tue 14 Dec 2010</dt>
    <dd>
        <ul>
            <li>Added Display_id attribute to the Individual-id pragma.</li>
        </ul>
    </dd>
    <dt>1.03 Wed 24 Nov 2010</dt>
    <dd>
        <ul>
            <li>Specified constraint on column 3 to be a sequence_alteration, one of it's children or gap.</li>
            <li>Added ##phased-genotype pragma and example.</li>
            <li>Added Start_range and End_range attributes and examples.</li>
            <li>Cleaned up the 'Quick Example of GVF Content' section to make it simpler.</li>
            <li>Replaced Nomenclature attributes with Alias attributes in the 'Quick Example of GVF Content' section.</li>
            <li>Added the ##score-method pragma as a method to describe the score calculated in column 6.</li>
            <li>Added links to the GVF wiki pages.</li>
        </ul>
    </dd>
    <dt>1.02 Sat 24 Jul 2010</dt>
    <dd>
        <ul>
            <li>Updated format of Variant_effect tag and updated the examples to comply.</li>
            <li>Minor updates to the homozygous deletion example.</li>
            <li>Added link to the 10Gen Data set.</li>
        </ul>
    </dd>
    <dt>1.01 Fri 19 Mar 2010</dt>
    <dd>
        <ul>
            <li>Added documentation to, and fixed inconsistencies with pragma examples.</li>
            <li>Added hemizygous as a valid Genotype tag value.</li>
            <li>Removed Intersected_feature tag.</li>
            <li>Modified Variant_effect tag to incorporate the function of the Intersected_feature tag.</li>
            <li>Clarified the definition of the Variant_copy_number and Reference_copy_number tags.</li>
            <li>Modified the GVF examples to reflect the changes in Variant_effect and Intersected_feature tags.</li>
        </ul>
    </dd>
</dl>
