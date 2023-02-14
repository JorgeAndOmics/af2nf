
# Alphafold To Nextflow

This script processes one or more protein sequences in FASTA format using Alphafold for structure prediction and Nextflow for workflow management.

## Requirements

-   Alphafold: This script assumes that Alphafold is already installed on the system and can be run from the command line. It can be installed following the instructions at [https://github.com/deepmind/alphafold]
-   Nextflow: Nextflow can be installed by following the instructions at [https://www.nextflow.io/](https://www.nextflow.io/).
-   Python 3: This script requires Python 3 to be installed on the system.

## Usage

    python predict_structure.py input output workflow

- `input` can be a single FASTA file or a directory containing multiple FASTA files.

- `output` is the directory where the output will be stored.

- `workflow` is the path to the Nextflow workflow file.

## Usage Case
Suppose you have a FASTA file `my_proteins.fasta` containing the amino acid sequences of several proteins. You want to predict the 3D structures of these proteins using Alphafold and Nextflow and store the output in a directory called `results`.

First, create a Nextflow workflow file `my_workflow.nf` that defines the workflow you want to run. For example:

    nextflow.enable.dsl=2
    
    params.input_dir = "."
    params.output_dir = "output"
    
    process predict_structure {
        input:
        file protein_fasta from params.input_dir
        output:
        file 'output/*'
        script:
        """
        python af2nf.py ${protein_fasta} ${params.output_dir} my_workflow.nf
        """
    }

This workflow assumes that you have already saved `af2nf.py` in the same directory.

To run the workflow on the `my_proteins.fasta` file, use the following command:

    nextflow run my_workflow.nf --input_dir my_proteins.fasta --output_dir results

This will process each protein sequence in `my_proteins.fasta`, running Alphafold and Nextflow for each one, and store the output in `results`.

If you have multiple FASTA files in a directory, you can run the workflow on all of them by specifying the input directory instead of the input file:

    nextflow run my_workflow.nf --input_dir my_fasta_files/ --output_dir results

This will process each FASTA file in `my_fasta_files/`, running Alphafold and Nextflow for each one, and store the output in `results/`.

Note that you may need to adjust the paths in `my_workflow.nf` to match the locations of your input files and `af2nf.py`.

## Contribution

Contributions are welcome! If you have any suggestions or find a bug, please open an issue or submit a pull request.

## License

This project is licensed under the MIT License.
