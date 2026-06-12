#Pseudomonas PAO1, Xue et al., 2026 Project nr: PRJNA877469
# 8 db SRR szám: DRR932745, DRR668308, DRR668307, DRR668306, DRR668305, DRR668304, DRR668303, DRR668302

FASTA_URL=https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/006/765/GCF_000006765.1_ASM676v1/GCF_000006765.1_ASM676v1_genomic.fna.gz
GFF_URL=https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/006/765/GCF_000006765.1_ASM676v1/GCF_000006765.1_ASM676v1_genomic.gff.gz

# The name of the genome.
GENOME_NAME=Pseudomonas_PAO1

# The path to the genome FASTA file.
GENOME_FASTA=refs/${GENOME_NAME}.fna

# The path to the genome GFF file.
GENOME_GFF=refs/${GENOME_NAME}.gff

# The DRR ID of the sample.
DRR=DRR932745

SAMPLE=${DRR}

# The name of the alignment BAM file
BAM=bam/${SAMPLE}.bam

VCF=vcf/${SAMPLE}.vcf.gz

# The number of reads to download.
LIMIT=10000

# The path to the read files
READS=reads/${DRR}.fastq


# ----- No CHANGES BELOW THIS LINE 

SAMTOOLS ?= samtools
BWA ?= bwa


# make the directories that are needed then download and unzip the fasta and gff files

all: download index fastq align vcf

download:
	mkdir -p refs
	wget -O ${GENOME_FASTA}.gz ${FASTA_URL}
	wget -O ${GENOME_GFF}.gz ${GFF_URL}
	gunzip -f ${GENOME_FASTA}.gz
	gunzip -f ${GENOME_GFF}.gz	
index:
	bwa index ${GENOME_FASTA}

# Download the reads from SRA, limit to LIMIT reads use the fastq-dump command
fastq:
	mkdir -p reads
	fastq-dump -X ${LIMIT} --outdir reads ${DRR}



# Align the reads to the genome
align:
	mkdir -p bam
	$(BWA) mem -t 4 ${GENOME_FASTA} ${READS} | \
		$(SAMTOOLS) view -b -o ${BAM}
	$(SAMTOOLS) sort ${BAM} -o ${BAM}.sorted.bam
	$(SAMTOOLS) index ${BAM}.sorted.bam
	$(SAMTOOLS) flagstat ${BAM}.sorted.bam



vcf:
	mkdir -p vcf
	make -f src/run/bcftools.mk \
		REF=${GENOME_FASTA} \
		BAM=${BAM}.sorted.bam \
		VCF=${VCF} \
		run
	

# Remove all the derived files
clean:
	rm -rf fastq bam refs

# Targets that are not files.
.PHONY: all genome fastq index align clean vcf