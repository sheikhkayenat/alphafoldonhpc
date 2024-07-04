# alphafoldonhpc
#!/bin/bash
#SBATCH -N 1
#SBATCH --partition=batch
#SBATCH -J AlphaFold_v2.3.2
#SBATCH -o AlphaFold_v2.3.2.%A_%a_%N.out
#SBATCH -e AlphaFold_v2.3.2.%A_%a_%N.err
#SBATCH --time=15:00:00
#SBATCH --mem=200GB
#SBATCH --gres=gpu:2
#SBATCH --cpus-per-task=16

# Load the AlphaFold module
module load /sw/rl9g/modulefiles/applications/alphafold/2.3.2/.python3.9

# Set environment variables
export ALPHAFOLD_DATA_DIR=/ibex/reference/KSL/alphafold/2.3.1
export CUDA_VISIBLE_DEVICES=0,1,2,3
export TF_FORCE_UNIFIED_MEMORY=1
export XLA_PYTHON_CLIENT_MEM_FRACTION=0.5
export XLA_PYTHON_CLIENT_ALLOCATOR=platform
export TF_CPP_MIN_LOG_LEVEL=0 # Enable detailed logging

# Define input and output paths
FASTA_PATH="/path/to/fasta/file.fasta"
OUTPUT_DIR="/path/to/output"

# Ensure output directory exists
mkdir -p $OUTPUT_DIR

# Activate the correct Python environment
source /ibex/sw/rl9g/alphafold/2.3.2/rl9.1_conda3/Miniconda3/bin/activate alphafold

# Run AlphaFold
python3 $AlphaFold/run_alphafold.py \
 --use_gpu_relax=true \
 --data_dir=$ALPHAFOLD_DATA_DIR \
 --uniref90_database_path=$ALPHAFOLD_DATA_DIR/uniref90/uniref90.fasta \
 --mgnify_database_path=$ALPHAFOLD_DATA_DIR/mgnify/mgy_clusters_2022_05.fa \
 --bfd_database_path=$ALPHAFOLD_DATA_DIR/bfd/bfd_metaclust_clu_complete_id30_c90_final_seq.sorted_opt \
 --uniref30_database_path=$ALPHAFOLD_DATA_DIR/uniref30/UniRef30_2021_03 \
 --pdb_seqres_database_path=$ALPHAFOLD_DATA_DIR/pdb_seqres/pdb_seqres.txt \
 --template_mmcif_dir=$ALPHAFOLD_DATA_DIR/pdb_mmcif/mmcif_files \
 --obsolete_pdbs_path=$ALPHAFOLD_DATA_DIR/pdb_mmcif/obsolete.dat \
 --uniprot_database_path=$ALPHAFOLD_DATA_DIR/uniprot/uniprot.fasta \
 --model_preset=multimer \
 --max_template_date=2022-10-01 \
 --db_preset=full_dbs \
 --output_dir=$OUTPUT_DIR \
 --fasta_paths=$FASTA_PATH
echo "AlphaFold run completed."
