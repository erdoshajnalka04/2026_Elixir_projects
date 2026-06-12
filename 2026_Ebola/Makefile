# ------------------ VARIABLES ------------------

GENOME_NAME=Ebola-mayinga

FASTA_URL=https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/848/505/GCF_000848505.1_ViralProj14703/GCF_000848505.1_ViralProj14703_genomic.fna.gz
GFF_URL=https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/848/505/GCF_000848505.1_ViralProj14703/GCF_000848505.1_ViralProj14703_genomic.gff.gz

# The name of the genome.
GENOME_NAME=ebola-mayinga

# The path to the genome FASTA file.
GENOME_FASTA=refs/${GENOME_NAME}.fna

# The path to the genome GFF file.
GENOME_GFF=refs/${GENOME_NAME}.gff

# The SRR ID of the sample.
SRR=SRR38154628

SAMPLE=${SRR}

# The name of the alignment BAM file
BAM=bam/${SAMPLE}.bam

VCF=vcf/${SAMPLE}.vcf.gz

# The number of reads to download.
LIMIT=10000

# The path to the read files
R1_READS=reads/${SRR}_1.fastq
R2_READS=reads/${SRR}_2.fastq

# ----- No CHANGES BELOW THIS LINE 

SAMTOOLS ?= samtools
BWA ?= bwa


# make the directories that are needed then download and unzip the fasta and gff files
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
	fastq-dump -X ${LIMIT} --outdir reads --split-files ${SRR}
	

# Align the reads to the genome
align:
	mkdir -p bam
	$(BWA) mem -t 4 ${GENOME_FASTA} ${R1_READS} ${R2_READS} | $(SAMTOOLS) view -b -o ${BAM}
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
.PHONY: genome fastq index align clean vcf





