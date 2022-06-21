# HTGTS snakefile

#dataset = "qrl_zxfs1"
dataset = "qrl_zxfs1"
sample_ids = open ("sample_id/{}.tlx".format(dataset)).read().strip().split("\n")


rule all:
    input:
        qc1 = expand("csr_htgts/{dataset}/csr_tlx/{sample_id}_result.tlx",dataset=[dataset],sample_id = sample_ids),
        bams = expand("output/{dataset}/mapping/{sample_id}/bam-deduped/{sequence}.bam", sample_id=sample_ids, dataset= dataset, sequence =  sequences)

rule plot:
    input:
        fastq = "csr_htgts/{dataset}/csr_tlx/{sample_id}_result.tlx"
    output:
        fastq = "csr_htgts/{dataset}/csr_result/{sample_id}_igh/{sample_id}_igh.bin.count.txt" 
    log:
        "csr_htgts/{dataset}/csr_result/{sample_id}_igh/log/{sample_id}_igh.txt" 
    shell:
        """
        scripts/plotRegion.py -tlx  csr_htgts/{dataset}/csr_tlx/{sample_id}_result.tlx -output csr_htgts/{dataset}/csr_result/{sample_id}_igh/{sample_id}_igh -target_chr 12 -target_start 114450000 -target_end 114675000 -bin_num 100 -strand 3 -ref_file /dataA/genomes/mm9/annotation/refGene.ray.mm9.txt -cytofile /dataA/genomes/mm9/annotation/cytoBandIdeo.mm9.txt
        """
