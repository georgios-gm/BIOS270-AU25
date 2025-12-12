# Pipeline

## SLURM Pipeline

How could one add a differential expression analysis (DESeq2) step to the `rnaseq_pipeline_array_depend.sh` script such that DESeq2 runs only after all `salmon` jobs for all samples have completed?

Conceptually, we can exploit the dependency mechanism in SLURM to instruct it to only run DESeq2 after all salmon jobs are completed, based on their job ID. 

---

## Nextflow Pipeline

In the current `rnaseq_nf` pipeline, we assume that the **transcriptome index** has already been created via `salmon index`.  However, this may not always be the case.

Add a `SALMON_INDEX` process that:

- Takes a reference transcriptome FASTA file as input (specify `transcriptome` in `params.yaml` file with `/farmshare/home/classes/bios/270/data/ecoli_ref/GCF_000401755.1_Escherichia_coli_ATCC_25922_cds_from_genomic.fna`)
- Outputs a Salmon index directory, used as input for the downstream `SALMON` process

Command for indexing a reference transcriptome:

```bash
salmon index -t <transcriptome.fa> -i <output_index_dir>
```

Modify your pipeline such that:

1. If the user provides `index` in `params.yaml`, use `index` directly as input for `SALMON` process, skip `SALMON_INDEX`.  
2. If `index` is **not provided**, but `transcriptome` is, run `SALMON_INDEX` to build index and used the output index directory as input to `SALMON`.  
3. If **neither** parameter is provided, exit with an informative error message.

---

## Instruction on running Nextflow Pipeline

1. Modify singularity setup in `rnaseq_nf/configs/nextflow.config`

<img width="707" height="104" alt="Screenshot 2025-12-11 at 6 10 20 PM" src="https://github.com/user-attachments/assets/756e42a0-278e-4527-8ea7-13140e43c78a" />


2. Modify output directory path in `rnaseq_nf/configs/params.yaml`

<img width="773" height="97" alt="Screenshot 2025-12-11 at 6 06 18 PM" src="https://github.com/user-attachments/assets/89d28429-5d89-4f45-adc8-7c3f65dbb4e1" />


3. Make sure scripts in `rnaseq_nf/bin` are executable, if not 

```bash
# cd to ./Pipeline
chmod +x ./rnaseq_nf/bin/*
```

4. Run Nextflow

<img width="784" height="479" alt="Screenshot 2025-12-11 at 6 55 03 PM" src="https://github.com/user-attachments/assets/ed0defd6-e04e-4dc7-b0d0-68f1b74edd04" />
