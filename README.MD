# Project documentation for Longest CDS per Orthogroup

This project is designed to find the longest coding sequence (CDS) for each orthogroup from a set of fasta sequences.

## Requirements

- Python 3.x
- Biopython (Python module)
- Orthofinder software
- Fasta sequences of coding sequences (CDS)

## Project Files

- `longest_cds.fa`: Fasta file containing the coding sequences
- `test.orthogroups_for_CDS_.fix.tsv`: Tab-separated file containing orthogroups in the first column and transcript IDs in the second column
- `get_lenghts.py`: Python script that calculates the length of each sequence in `test.orthogroups_for_CDS_.fix.tsv` and creates `test.orthogroups_with_lengths.tsv`
- `longest_cds_per_orthogroup.tsv`: Tab-separated file containing orthogroups in the first column and longest transcript IDs in the second column
- `get_longest_transcript.py`: Python script that identifies the longest transcript for each orthogroup and creates `longest_cds_per_orthogroup.tsv`
- `test.cds.fasta`: Fasta file containing all CDS sequences

## Workflow

1. Obtain the `longest_cds.fa` file containing the CDS sequences and the `test.orthogroups_for_CDS_.fix.tsv` file containing the orthogroups and transcript IDs. The latter file can be obtained by running `reformatOrthoFinderGroups.pl` over `Orthogroups.tsv` from Orthofinder and extracting the orthogroups and transcripts IDs.

2. Run `get_lenghts.py` over `test.orthogroups_for_CDS_.fix.tsv` to calculate the length of each sequence and create `test.orthogroups_with_lengths.tsv`. This script requires the Biopython module.

3. Run `get_longest_transcript.py` to identify the longest transcript for each orthogroup and create `longest_cds_per_orthogroup.tsv`.

4. Use bash commands to extract the transcript IDs in `longest_cds_per_orthogroup.tsv` and extract the sequences in `test.cds.fasta`.

## Usage

1. Clone the repository: `git clone https://github.com/<USERNAME>/<REPO>.git`

2. Place the required files in the same directory as the Python scripts.

3. Run the scripts in the order specified above using the command line: `python <SCRIPT_NAME.py>`

4. The output files will be generated in the same directory.

## License

This project is licensed under the MIT License - see the `LICENSE` file for details.

