<table align="center" style="margin: 0px auto;">
  <tr>
    <td>
      <img src="https://raw.githubusercontent.com/phac-nml/nanogo/main/extra/nanogo_logo.svg" alt="NanoGo Logo" width="400" height="auto"/>
    </td>
    <td>
      <h1>Nanopore Genome Optimization Bioinformatics Pipeline</h1>
<a href="https://anaconda.org/gosahan/nanogo">
  <img src="https://anaconda.org/gosahan/nanogo/badges/version.svg" alt="NanoGo on Anaconda"/>
</a>
<a href="">
  <img src="https://anaconda.org/gosahan/nanogo/badges/platforms.svg"/>        
</a>
<a href="">
  <img src="https://anaconda.org/gosahan/nanogo/badges/latest_release_date.svg"/>        
</a>
    </td>
  </tr>
</table>

<table align="center" style="margin: 0px auto;">
    <!-- Header row with column headers -->
    <tr>
        <th>NanoGO QC Dashboard</th>
        <th>Local Environment Operations</th>
    </tr>
    <!-- First row with images -->
    <tr>
        <td>
            <a href="https://github.com/phac-nml/nanogo/blob/main/extra/brave_jJ2pJG0u1c-ezgif.com-resize.gif">
                <img src="https://raw.githubusercontent.com/phac-nml/nanogo/main/extra/brave_jJ2pJG0u1c-ezgif.com-resize.gif" alt="NanoGO QC Summary Report" height="auto" width="500"/>
            </a>
        </td>
        <td>
            <a href="https://github.com/phac-nml/nanogo/blob/main/extra/cmd_xxQZGCLvlP.gif">
                <img src="https://raw.githubusercontent.com/phac-nml/nanogo/main/extra/cmd_xxQZGCLvlP.gif" alt="NanoGO tool in Action" height="auto" width="500"/>
            </a>
        </td>
    </tr>
    <!-- Second row header and image -->
    <tr>
        <th colspan="2">Real-Time Performance Monitoring</th>
    </tr>
    <tr>
        <td colspan="2">
            <a href="https://anaconda.org/gosahan/nanogo/Code_eWv9WVcoX8.gif">
                <img src="https://raw.githubusercontent.com/phac-nml/nanogo/main/extra/Code_eWv9WVcoX8.gif" alt="NanoGO Progress Bar" height="auto" width="auto"/>
            </a>
        </td>
    </tr>
</table>



## Overview

**NanoGO Pipeline:** A comprehensive bioinformatics pipeline for Oxford Nanopore Technologies. The tool offers Dorado-based basecalling with duplex options, amplicon read assembly and polishing, primer trimming, and quality control. 

Designed for efficient parallel processing and enhanced by interactive input and output configuration, NanoGO maximizes computational resources while providing flexible control to users. 


## Quick Installation Guide

Use the following command to install NanoGO:

```bash
conda create -n nanogo-0.3.6 "python=3.10" -y && conda activate nanogo-0.3.6 && conda install -c gosahan nanogo -y
```

<p align="center">
  <a href="https://github.com/phac-nml/nanogo/main/extra/cmd_j3T8dUwunM.gif"><img src="https://raw.githubusercontent.com/phac-nml/nanogo/main/extra/cmd_j3T8dUwunM.gif" alt="NanoGO Install Instructions"  height="auto" width="1000"/></a>
</p>

### If quick installation does not work then follow the instructions below

- <env_name> specify you conda environment name in which to install NanoGO. For example, <env_name> can be ```conda create -n nanogo-0.3.6 "python=3.10"```

```bash
conda create -n <env_name> "python=3.10"
```

```bash
conda activate <env_name>
```

```bash
conda install -c gosahan nanogo
```


## Usage Requirements

### NanoGO Analysis

#### Input Directory Structure: Unique Reference and Primer/Adaptor Sequences for Each Sample

```
Input Directory
├─ Sample_A
│  ├─ Sample_A_01.fastq.gz (fastq.gz, fastq)
│  ├─ Sample_A_02.fastq (fastq.gz, fastq)
│  ├─ Reference_Sequence.fasta (fasta)
│  └─ Primer_File.txt (txt)
├─ Sample_B
│  ├─ Sample_B_01.fastq.gz (fastq.gz, fastq)
│  ├─ Sample_B_02.fastq.gz (fastq.gz, fastq)
│  ├─ Sample_B_03.fastq.gz (fastq.gz, fastq)
│  ├─ Reference_Sequence.fasta (fasta)
│  └─ Primer_File.txt (txt)
├─ Sample_N
   ├─ Sample_N_01.fastq.gz (fastq.gz, fastq)
   ├─ {reference_sequence_files_name}.fasta (fasta)
   └─ {unique_primer/adaptor_files_name}.txt (txt)
```


#### Input Directory Structure: Shared Reference and Primer/Adaptor Sequences for All Samples


```
Input Directory
├─ Sample_A
│  ├─ Sample_A_01.fastq.gz (fastq.gz, fastq)
│  ├─ Sample_A_02.fastq (fastq.gz, fastq)
├─ Sample_B
│  ├─ Sample_B_01.fastq.gz (fastq.gz, fastq)
│  ├─ Sample_B_02.fastq.gz (fastq.gz, fastq)
│  ├─ Sample_B_03.fastq.gz (fastq.gz, fastq)
├─ Sample_N
│   ├─ Sample_N_01.fastq.gz (fastq.gz, fastq)
│
├─ {reference_sequence_files_name}.fasta (fasta)
│
└─ {unique_primer/adaptor_files_name}.txt (txt)
```


#### Guidelines for NanoGO Analysis Tool
- Each folder is treated as a unique sample containing read files associated with the same barcode.
  - If sequencing different genes and combining datasets for analysis is required, create a folder and move all fastq files intended for joint assembly.

#### Reference Sequence File Structure

  ```
  >Reference_sequence
  UUCCUACAAGGGAARNGCD 
  ```
  or 
  ```
  >Reference_sequence_1
  UUCCUACAAGGGAARNGCD 
  >Reference_sequence_2
  UUCCDUACAAGGGAARNGCD 
  >Reference_sequence_3
  UUCCUACAARHGGGAARNGCD 
  ```
- If multiple sequences are present in a reference file, the analysis tool concatenates them into one reference.
  - This generates a single consensus file; if a separate reference is needed for each sequence, consider running the analysis tool multiple times with only the desired gene. 
  - The reference sequence can contain the followinbg letters: ``WSRYVHBMNKUATGC``.

#### Primer/Adaptor File structure:
```
>{your_forward_primer_name}-F
AACARMNKAGATAAAATAGTHAGT
>{your_forward_primer_name}-R
ATAATTATGRTGYTMNKTARACT
```
Primer/Adaptor Naming Requirements:
- Primer names start with `>` and should not contain spaces (use `_` instead).
- Primer names must end with `-F` for forward primers or `-R` for reverse primers, which is critical during the primer trimming stage.

Primer/Adaptor Sequence Requirements:
- Follow the primer name with its sequence.

```
>forward_primer-F
ATAATTATGRTGYTMNKTARACT
>reverse_primer-R
AACARMNKAGATAAAATAGTHAGT
```
#### System Requirements
- **Operating System**: Linux or Windows Subsystem for Linux (WSL).
- **Memory**: Minimum of 16 GB RAM.
- **Processor**: At least 4 cores.

 **Name's of files and folders should not contain spaces, replace the spaces with ``_``** 



### NanoGO Basecaller

#### Input Directory Structure
```
Input Directory
├─ Raw_Sample_A
│  ├─ Raw_Sample_A_01.pod5 (POD5 or FAST5)
│  ├─ Raw_Sample_A_02.pod5 (POD5 or FAST5)
├─ Raw_Sample_B
│  ├─ Raw_Sample_B_01.fast5 (POD5 or FAST5)
│  ├─ Raw_Sample_B_02.fast5 (POD5 or FAST5)
│  ├─ Raw_Sample_B_03.fast5 (POD5 or FAST5)
└─ Raw_Sample_N
   └─ Raw_Sample_N_01.pod5 (fastq.gz, fastq)
```

- The NanoGO Basecaller considers `Raw_Sample_A` and `Raw_Sample_B` as separate sequencing runs.
- Files within each folder can be in either FAST5 or POD5 format.
  - FAST5 files are converted to POD5 format during the sequencing process.


#### System Requirements
- **GPU Compatibility**: Must be compatible with Dorado.
- **Operating System**: Linux or Windows Subsystem for Linux (WSL).
- **Memory**: Minimum of 16 GB RAM.
- **Processor**: At least 4 cores.



## Usage Instructions for `nanogo`

### Test to see if the install is working 
#### Input:
```bash
nanogo --help
```
#### Output:
```
usage: nanogo [options] <subcommand>

options:
  -h, --help            show this help message and exit
  -v, --version         Display the version number of NanoGO.

Available Tools:
  Valid subcommands

  {basecaller,analysis}
                        Description
    basecaller          Run nanogo basecaller using dorado basecaller
    analysis            Run nanogo bioinformatics pipeline for generating consensus sequences and qc report

```

## Usage Instructions for `nanogo analysis`

### NanoGO has several modes of operation. 

Run ```nanogo analysis``` on command line to run pipeline in interactivate mode.

```bash
nanogo analysis
```

Run in command line mode by inputing the required argument. 

```bash
nanogo analysis -i <fastq_folder_path> -o <output_directory_name> --primer <primer_file.txt> --ref <reference_seq.fasta> [options]
```

## Arguments

### Input/Output Options

- `-i <fastq_folder_path>`: Path to the folder containing fastq data. Activates interactive mode if not provided.
- `-o <output_directory>`: Output directory name ``(Default: "nanogo_output")``.

### Main Options
- `--primer <primer_folder_path>`: Path to folder containing primer sequences.
- `--ref <reference_folder_path>`: Path to folder containing reference sequences.
- `--model` : Prompt to select supported models used for basecalling ``(Default: r1041_e82_400bps_sup_v4.2.0)``
- `--min <minimum_read_size>`: Minimum size of read expected, ``(Default: 100)``
- `--max <maximum_read_size>`: Maximum size of read expected, ``(Default: 1500)``
- `--overlap <overlap_size>`: Minimum overlap of reads. This is used to determine the read trimming sensitivity ``(Default: 1000)``
- `--quality <minimum_quality>`: Minimum quality of nucleotide at end of reads. This value is used to trim low-quality bases.``Default: 20``
- `--barcode`: Barcodes used for ONT sequencing. Choices: pcr_barcodes, rapid_barcodes, native_barcodes. ``(Default: native_barcodes)``
- `--require_two_barcodes`: Reads will only be put in barcode bins if they have a strong match for the barcode on both their start and end ``(Default: false)``
- `--database <kraken2_database>`: If you have a custom Kraken2 database installed locally then provide path to folder containing k2d files. ``(default: MinusB - Archaea, Viral, Plasmid, Human, UniVec_Core)``

- `-v`, `--version`:  Use this to display the version number of NanoGO.

### Example
```bash
nanogo analysis -input <basecalled_data_folder> -o <nanogo_output>
```
This command will process fastq data in the fastq_data directory, output results to nanogo_output, and prompt for the basecalling model.

### Output
#### NanoGO Analysis generates the following output files:
- **adapter_trimmed_reads**: This directory stores outputs from Porechop, which includes all reads with adapter sequences trimmed. It also contains a summary report detailing the efficiency and coverage of the trimming process.

- **primer_trimmed_reads**: Contains concatenated files that integrate adapter-trimmed reads with primer sequence data, reference sequences, and outputs from ten iterations of Cutadapt. This directory also includes summary reports that provide metrics on the effectiveness of primer trimming.

- **final_consensus**: This directory houses consensus sequences generated by three different tools: Medaka, QuasiGO, and iVAR. It includes a MAFFT assembly of these three consensus sequences to create a final, unified consensus sequence. The directory provides comprehensive details on the assembly process and the quality of the final consensus.

- **consensus_alignments** - Contains all alignment-related outputs, including BAM files, consensus sequences, and variant calling results from tools such as iVAR and Medaka. This directory serves as a comprehensive repository for genomic alignments and analyses.

- **raw-ivar_consensus** - Stores variant calling outputs from iVAR, including BAM files, consensus sequences, VCF files, and quality summaries. This directory is crucial for variant analysis and genomic studies using iVAR.

- **raw-medaka_consensus** - Includes consensus sequences generated by Medaka, accompanied by related BAM files and quality assessment outputs. This directory facilitates detailed analyses of consensus accuracy and genomic sequencing fidelity.

- **raw-quasigo_consensus** - Holds majority and degenerate consensus sequences from QuasiGO, including detailed alignment data and quality metrics. This directory is key for evaluating QuasiGO's performance in generating diverse genomic consensus sequences.

- **scaffold-medaka_reference** - Contains scaffolded reference sequences produced by Medaka, facilitating detailed genomic assemblies by generating a high-quality reference genome. This directory is essential for complex genomic reconstruction and detailed studies.

- **quality_control** - Encompasses all quality control outputs, detailing read quality, sequence integrity, and contaminant checks across various stages. This directory is fundamental for ensuring the accuracy and reliability of genomic data.

  - **qc_adapter_trimmed_reads** - Holds FastQC and Kraken2 reports for reads after adapter trimming, providing insights into the effectiveness of the trimming process and subsequent data quality.

  - **qc_primer_trimmed_reads** - Contains FastQC and Kraken2 reports for reads after primer trimming. This directory is crucial for assessing the impact of primer removal on read quality and integrity.

  - **qc_multiqc** - Contains comprehensive NanoStat reports that aggregate quality metrics and analyses for generating the final report using MultiQC. This directory provides an overarching view of data quality across multiple samples or experiments.

  - **qc_raw_nanopore_reads** - Holds Kraken2 reports for raw reads, offering initial insights into the microbial content and potential contaminants in the nanopore sequencing data.

  - **qc_raw_iVAR_consensus** - Stores QualiMap reports for iVAR consensus BAM file outputs, which are used to generate consensus sequences and variant calling results. This directory is vital for assessing the quality and reliability of iVAR outputs.

  - **qc_raw_medaka_consensus** - Contains QualiMap reports for Medaka BAM file outputs that were used to generate consensus sequences. This directory helps in evaluating the sequencing performance and the accuracy of Medaka-generated consensus.

  - **qc_scaffold_medaka_reference** - Holds QualiMap reports for Medaka BAM file outputs that were used to generate reference consensus sequences for detailed assembly. This directory is crucial for analyzing the structural integrity and completeness of scaffolded genomes.

- **qc_summary_report** - Stores overall summary reports produced by MultiQC, providing a high-level overview of quality metrics and analytical results. This directory is instrumental in summarizing and communicating the outcomes of quality control assessments.

#### Abbreviations used in file names
- **NanoGO**: "Nanopore Genome Optimization" - A comprehensive suite of tools and methodologies specifically tailored for optimizing the analysis of nanopore sequencing data.

- **RNR**: "Raw Nanopore Reads" - The initial stage of data processing, involving the collection and initial quality assessment of raw sequencing reads directly from the nanopore sequencing platform.

- **ADTR**: "Adapter Trimmed Reads" - Involves the strategic removal of adapter sequences from raw reads using Porechop, essential for reducing sequence artifacts and enhancing the overall quality and usability of the sequencing data.

- **PTR**: "Primer Trimmed Reads" - A sophisticated multi-step process that includes the excision of primer sequences, filtering by read length, and quality filtering using Cutadapt to ensure high-quality, reliable sequencing reads.

- **SRGS**: "Scaffold Reference Genome Synthesis" - Clusters reads based on their alignment to a reference genome, performed using Medaka to improve the accuracy of genome assemblies and provide high-fidelity scaffolded sequences.

- **RIVCS**: "Raw iVAR Consensus Synthesis" - An advanced refinement stage using the iVAR tool to enhance the quality and alignment of sequencing reads, focusing on generating a high-quality consensus sequence.

- **RMCGS**: "Raw Medaka Consensus Synthesis" - Utilizes Medaka software to refine and enhance sequencing read alignment, aiming to generate an accurate and comprehensive consensus sequence from nanopore sequencing data.

- **RQGCS**: "Raw QuasiGO Consensus Synthesis" - Employs QuasiGO, a tool for generating quasi-consensus sequences, to refine and enhance the quality of sequencing reads in environments with high sequence variability.

- **KR2**: "Kraken2" - A powerful taxonomic sequence classification system that uses exact k-mer matches to quickly and accurately classify the taxonomy of sequences, essential for microbial identification and diversity studies.

- **MFT**: "Mafft Output" - The final step in data consolidation, involving merging multiple alignment files to form a cohesive and comprehensive view of genomic alignments, leveraging the precision of Mafft for handling multiple sequence alignments.

### Naming Convension
The filename for each sample sheet is composed of several key pieces of data, concatenated with underscores:

``NanoGO-{Abrv}_barcode{##}_{run_id}_{dorado_models_hex}.fastq``

1. **NanoGO Abbreviation ID (`NanoGO-{Abrv}`)**: This identifier is a short form for the specific stage or type of data being processed. For instance, `RNR` for Raw Nanopore Reads, `ADTR` for Adapter Trimmed Reads, etc. Each abbreviation corresponds to a distinct process step in our sequencing workflow, providing clear and instant recognition of the file's contents.

2. **Barcode Number (`barcode{##}`)**: This is the identifier that corresponds to the specific barcode number used to demultiplex the sequencing run. It allows for easy identification and organization of data by sample.

3. **Run ID (`run_id`)**: This is a truncated identifier (first 8 characters) that uniquely identifies the sequencing run. It is extracted and shortened to ensure filename brevity and uniqueness.

4. **Dorado Models Hash (`dorado_models_hex`)**: A SHA-256 hash of the Dorado models used in the sequencing. The hash is truncated to the first 8 characters. This cryptographic hash ensures that the model information is securely and succinctly incorporated into the filename.

### Purpose
 The structure of the filenames is designed to:
- **Ensure Uniqueness**: Each component, especially the hashes and abbreviations, helps ensure that each filename is unique to its specific sequencing context.
- **Facilitate Sorting and Identification**: The structured format allows for easy sorting and identification of files based on the process stage, barcode, model, and kit.
- **Maintain Security and Brevity**: Using hashes for the models and kits encapsulates sensitive information in a secure and concise manner, which is crucial for managing large datasets.

This naming convention is crucial for effective data management and integrity across different stages of our sequencing workflow. It assists in streamlining data access, retrieval, and analysis, enhancing both the efficiency and the security of our project's data handling.

## Usage Instructions for `nanogo basecaller`

### NanoGO has several modes of operation. 

Run ```nanogo basecaller``` on command line to run pipeline in interactivate mode.

```bash
nanogo basecaller
```

Run in command line mode by inputing the required argument. 

```bash
nanogo basecaller -i <raw_ont-data_folder> -o <output_directory_name> [Basecaller Options]
```
### Input/Output Options

- `-i <raw_ont-data_folder>`: Path to the folder containing fastq data. Activates interactive mode if not provided.
- `-o <output_directory_name>`: Output directory name ``(Default: "dorado_output")``.
### Basecaller Options
- `-b`, `--basecaller`:  Enable to specify and use a particular basecalling software. Enabled by default.
- `-d`, `--duplex`:  Enable for duplex sequencing mode, which processes both DNA strands. Not enabled by default.

### Output
NanoGO basecaller generates the following output files:

- **temp_data**: This directory serves as the staging area for intermediate files during the basecalling process. It includes:
  - **basecalling model**: This folder contains the machine learning models used for basecalling.
  - **sublist_# folders**: Each folder stores non-demultiplexed BAM files representing the initial output from the basecalling process before demultiplexing and symbolic links to POD5 or FAST5 files.
    - **demux file**: Contains demultiplexed data, organizing reads by their corresponding barcodes as defined in the sample sheet.
  - **sample sheet**: Essential for demultiplexing, this file maps sequences to their respective sample identities and is stored within the temp data.

- **final_output**: Houses the end results of the NanoGO basecalling and demultiplexing process, organized as follows:
  - **barcode## folders**: Each folder corresponds to a specific barcode and contains demultiplexed data. 
  - **unclassified folder**: Includes sequences for which the barcodes could not be identified.

### Naming Convension
The filename for each sample sheet is composed of several key pieces of data, concatenated with underscores:

``{flow_cell_id}_{run_id}_{dorado_models_hex}_{dorado_kits_hex}_samplesheet.csv``

1. **Flow Cell ID (`flow_cell_id`)**: This identifier is derived directly from the sequencing data, representing the specific flow cell used during the sequencing run.

2. **Run ID (`run_id`)**: This is a truncated identifier (first 8 characters) that uniquely identifies the sequencing run. It is extracted and shortened to ensure filename brevity and uniqueness.

3. **Dorado Models Hash (`dorado_models_hex`)**: A SHA-256 hash of the Dorado models used in the sequencing. The hash is truncated to the first 8 characters. This cryptographic hash ensures that the model information is securely and succinctly incorporated into the filename.

4. **Dorado Kits Hash (`dorado_kits_hex`)**: Similar to the models hash, this is a SHA-256 hash of the Dorado kits, truncated to the first 8 characters. It adds another layer of specificity and security to the naming convention.

### Purpose
  The structure of the filenames is designed to:
  - **Ensure Uniqueness**: Each component, especially the hashes and IDs, helps ensure that each filename is unique to its specific sequencing context.
  - **Facilitate Sorting and Identification**: The structured format allows for easy sorting and identification of files based on the flow cell, run, model, and kit.
  - **Maintain Security and Brevity**: Using hashes for the models and kits encapsulates sensitive information in a secure and concise manner, which is crucial for managing large datasets.

  This naming convention is crucial for effective data management and integrity across different stages of our sequencing workflow. It assists in streamlining data access, retrieval, and analysis, enhancing both the efficiency and the security of our project's data handling.


## Support

For any questions, issues, or additional assistance, please contact [Gurasis Osahan](mailto:gurasis.osahan@phac-aspc.gc.ca) at the National Microbiology Laboratory. Your queries will be addressed promptly, ensuring smooth and efficient use of NanoGO.

## License

NanoGO is licensed under the ensed under the Apache License, Version 2.0. You may use this work only in compliance with the License. For more details, please visit [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0).

## Legal

**Copyright**: Government of Canada 

**Written by**: National Microbiology Laboratory, Public Health Agency of Canada

Unless required by applicable law or agreed to in writing, software distributed
under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR
CONDITIONS OF ANY KIND, either express or implied. See the License for the
specific language governing permissions and limitations under the License.

## Contact ##
[**Gurasis Osahan**](mailto:gurasis.osahan@phac-aspc.gc.ca) at National Microbiology Laboratory, Public Health Agency of Canada


---

*Ensuring public health through advanced genomics. Developed with unwavering commitment and expertise by National Microbiology Laboratory, Public Health Agency of Canada.*
