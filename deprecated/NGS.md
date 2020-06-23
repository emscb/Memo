# Next Generation sequencing

* Sanger sequencing 다음의 기술

### Linux

* 리눅스에서는 cmd기반으로 진행하기 때문에 pipeline을 사용해서 반복 작업 하기 좋음
* Windows를 사용하기 위해서는 패키지를 사야 함

### SAM file

> https://samtools.github.io/hts-specs/SAMv1.pdf

* CIGAR
    * M : 자리수는 맞는다. 중간에 다른게 있을 수 있음 (align이 되었다)
    * S : 앞 숫자만큼 지나가고 뒤만큼 align되었다.

> https://www.slideshare.net/ueb52/introduction-to-bioinformatics-uebuat-bioinformatics-course-session-11-vhir-barcelona

* FLAGS
    * Reverse complement : 염기를 상보적으로 바꿔서 매핑
    * FLAGS 빨리 해보기 : `samtools flags 숫자`
    * 마주보고 매핑되야 properly

* Sequence에 있지만 mapping이 안 된건 소문자로 표시 (mismatch)

* Optional fields
    * NM : reference와 차이가 몇 개인지 (mismatch)
    * MD : e.g., 36T25, 36개가 match, 하나가 reference가 T, 25개 match
    * AS : 점수가 높으면 match가 잘 된 것

### SOAPdenovo

> https://github.com/aquaskyline/SOAPdenovo2/blob/d209aa3a98e6e76191c12480d91e1f9e01b1b5e8/README.md

* Contig들을 연결해서 scaffold를 만듦
* 사이의 N들은 gap (미처 알아내지 못한 서열)
* contig N50 : 50% 지점을 지나는 contig의 length (긴 contig부터 했을 때)
* Mapping을 통해 pair의 위치를 고려해 contig의 위치, 순서를 파악한다.

* `.contigPosInscaff` : contig들이 scaffold안에 어느 위치에 있느냐, contig가 scaff된 순서, 방향, 위치를 표시
    * `-` : 여기부터 여기까지 (?)
    * `+` : 여기부터 이만큼

* `.config` 내부에 contig들이 번호와 길이, 서열이 모여있다.
* `.scafSeq`는 contig로 만든 scaffold들이 들어있다. 
    * `bwa index K47.scafSeq` : 인덱싱
    * `bwa mem K47.scafSeq unmapped read 1번 2번 > 결과파일.sam` : 매핑

### BLAST

* 소문자로 쓰여진 것은 low complexity : non-specific한 hit가 생길 확률이 높아 잘 사용하지 않음, 점수에는 들어가지만 최초에 seeding할 때 사용하지 않음
* 앞쪽 부분이 점수가 낮아서 hit 표시가 안 떴는데, 앞 부분을 보고 싶으면 앞쪽 구간만 지정해서 다시 blast해주면 된다.

### Insertion된 위치 찾기

* Tablet으로 찾기에는 너무 시간이 오래걸린다. 어디 삽입됬는지 모른다면 더더욱
* 한 쪽은 reference에 mapping되고 mate가 insertion된 부위에 mapping되어있는 read들을 모아 비교해본다.
* Mapping이 시작된 좌표가 동일하고 CIGAR가 skip이 많은 read들이 많은 부위가 insertion으로 의심되는 부분이다.
* 혹은 시작 위치는 다르지만 mapping의 끝나는 위치가 같고 CIGAR에 skip이 많은 부분을 보는 것도 가능

### 1000 genomes project

* 인종별, 지역별로 사람의 유전체를 연구
* Depth가 좀 낮다. Sample수를 늘리는 대가
* 이곳에 없는 돌연변이가 질병과 관련될 가능성이 높다.
* 멀쩡한 사람들 위주로 뽑았기 때문에 돌연변이가 크게 위험하진 않다.

### Ensembl

* 얘네 나름의 버전 넘버링을 한다. 75까지가 GRCh37에 해당
* Human이랑 release 번호가 안 맞는게 ensembl은 다른 종까지 해서 한번에 release하기 때문
* Alternative sequence : Snp로 설명이 안 되고 sequence 자체가 다른 것
* rm : Repeat sequence는 x로 marking, human genome에 45%정도가 repeat
* sm : Soft masking, x로 안하고 small 알파벳으로 marking
* Blast는 small을 다 넘어가는데 bwa는 넘어가는지 모르겠다.

### FTP, rsync

* 하나하나 가져오기 귀찮을 때 rsync
* rsync는 동기화라고 생각하면 편하다. 업데이트되면 새로운 파일만 가져옴

> `rsync -av rsync://ftp.ensembl.org/ensembl/pub/current_embl/homo_sapiens .`

### bcftools에서 exon 부분 뽑아내기

* `bcftools view`에서 `-R`이나 `-r` 옵션 사용
* `bcftools view -R ZGPAT파일 vcf파일 | bcftools filter -i 'DP>100'`

> https://samtools.github.io/bcftools/bcftools.html

### vcf file 심화

* DP : depth, 단순히 몇 개의 read가 쌓여있나
* DP4 : reference forward, reference reverse, alternative forward, alternative reverse / number of high-quality
* 0/0, 0/1, 1/1 : r/r, r/a, a/a

﻿### RNA-seq

* mRNA를 cDNA로 역전사. 그러면 intron이 빠진 상태의 DNA가 나옴
* transcipt에다가 mapping하면 잘 붙겠지만 reference genome에 mapping하면 중간에 떨어져서 (끊어져서) 매핑되는 부위가 발생

### 예시 (Bowtie)

* 하나의 read를 3등분하자
* 3개가 모두 붙어서 매핑되면 모두 exon에 있는거다.
* 2번이 짤려있고 1번과 3번이 나눠진 경우들을 모은다.
* 1번의 오른쪽과 3번의 왼쪽 part를 모으면 2번 조각을 알 수 있을거지만 정확히 어디까지 끊어야 하는지를 알 수 없다.
* 그 사이의 intron이 GT로 시작해서 AG로 끝난다는 성질을 이용한다.
* 여러 후보 중 2번 조각과 같은 것을 찾는다.

### Bioconductor

* R로 구현한 bioinfomatic 자료들의 package

### FPKM

* Pair-end 데이터의 경우 RPKM 대신 FPKM(fragment ..)로 진행을 하는 것이 더 정확하다.

### 앞에 chr붙이기

* `sed 's/^14/chr14/g' filename > 파일명`

### samtools depth

* `samtools depth` : 특정 위치의 depth 계산
* `-r CHR:FROM-TO` : 예를 들면 `chr14:1414-1667`

﻿### Chromosome Conformation Capture (3C)

* 시퀀스 상에서는 멀리 떨어진 두 DNA 부위가 3차원 상(공간상)에서는 가까이 붙어있다. 이를 재현하기 위한 기술
* Circularized - (4C)
* Carbon-Copy - (5C)

### ENCODE 프로젝트

* Hypersensitive sites : transcription이 일어날 수 있는 부위, 오픈 크로마틴
* 다른 부위는 protected 되어있어 절단이 용이하지 않다.
* ChIP-seq은 수식화된 아미노산 잔기를 찾아낸다.
* 혹은 전사인자의 타겟 유전자 및 그 프로모터 부위를 알아낼 수 있다.
* cis-elements : 같은 DNA위에 있는 조절부위
* trans-elements : 결합하는 단백질을 포함한 다른데서 온 조절자
* Nuc : nucleophile(donor) (<-> Electrophile)
* CpG : C 다음 phospho-bond 다음 G, 여기서 C에 methylation이 된다.
* CpG island : 그런 부분이 농축되어 있는 부분
* 메틸레이션된 위치는 MBD가 인식, 다른 친구들도 불러, 그런 식으로 발현을 억제
* Tumor와 정상 세포와의 메틸레이션 차이를 연구하기도 해

### Histone modification

* 메틸레이션이나 아세틸레이션이 되면 감기는 정도에 차이가 생길 수 있다.
* `H3K4` : 3번째 히스톤에 4번째 lysine
* Inactive chromation : 전사 저해
* Enhancer는 멀리 떨어져있어서 찾기 어려운데 `K4me1`을 타겟팅시켜서 침전 후 mapping시키면 시퀀스를 알 수 있다.

﻿### BWA

1. Reference file 준비 : genome sequence file과 GFF file
1. 인덱싱 : `bwa경로/bwa index 레퍼런스 파일`
1. align 시작 : `bwa경로/bwa mem 레퍼런스 파일 FASTQ_1 FASTQ_2 > SAM 파일`

### 사후 처리

1. BAM 파일로 변환 : `samtools view -b -o BAM파일 -S SAM파일`
1. flagstat 생성 : `samtools flagstat BAM파일 > FLAGSTAT파일`
1. Sorting : `samtools sort BAM파일 -o 정렬된BAM파일`
1. 인덱싱 : `samtools index 정렬된BAM파일`

# NGS 데이터 구조와 형식

### FASTAQ format

* 네 줄씩 한 set
    * 첫 번째 줄은 `@`로 시작
        * Sequence access number, spot의 일련번호, 시퀀싱 장비
        * flow cell ID, tile number, cluster들이 빛나는 spot의 x, y좌표, length, paired의 경우 몇 번째건지에 따라 (/1, /2)
    * 두 번째 줄은 실제 시퀀스
        * N은 정확히 알 수 없는 염기를 뜻함
    * 세 번째 줄은 process line
        * 첫 번째 줄과 같은 내용
    * 네 번째 줄은 위 base call quailty를 나타냄
* 기관마다 등록번호 첫 글자가 다르다.
    * NCBI는 SRR, SRR, EBI는 ERR, DDBJ는 DRR로 시작
    * 일련번호에 빈 번호는 불량한 내용을 필터링했기 때문에 발생
* 기관에 등록된, 공공DB에서 다운한 것이 아닌 장비에서 직접 얻은 파일은 등록번호가 빠져있다.

## FASTQC, ngsShort

 * `top` : 윈도우의 작업 관리자와 비슷한 기능

### boxplot

* 최대값, 최소값과 중간선이 중간값(median)
* 박스의 밑부분은 25분위(25%), 윗부분은 75분위(75%)를 나타냄
* 박스 내부는 전체 데이터의 50%

## Variant Calling

﻿### Pileup format

* seq이름, 몇 번째 bp인지, 원래 뭔지 (reference), 몇 개의 read가 있는지
* `.` : reference와 같음 (forward)
* `,` : reference와 같음 (reverse)
* `AGCTN` : reference와 다름 (forward)
* `agctn` : reference와 다름 (reverse)
* `$` : read의 마지막
* `^` : read의 처음, 다음에 오는 문자의 ASCII 번호 -33이 quality
* 6번째 column은 Phred 방식의 quality score (-33)
* 하지만 `mpileup`에서 `-g` 옵션을 주게되면 용량이 큰 mplieup 파일을 출력하지는 않고 결과만 bcf 파일로 출력

## VEP

﻿### Identifiers and frequency data

* 다른 DB들과 비교
* Gene symbol : variant가 어느 gene에 속하는지
* Frequency data : 그 variant가 집단에서 얼마나 자주 발생하는지
    * ESP : African American과 European American에서의 빈도
    * ExAC : 전세계적으로 exome 시퀀싱한 사람들의 DB

### 결과창

* Novel : dbSNP에 없는 것
* intergenic variant : 다른 유전자와의 변이
* NMD (nonsense mediated decay) : 변이의 결과로 premature stop codon이 생긴 경우 (원래 stop보다 앞쪽에 생겨 짧은 단백질이 생김), 이 경우 번역 과정없이 mRNA를 없애버림
* synonymous variant : 아미노산의 변화가 생기지 않음
* missense variant: 아미노산의 변화가 생겼지만 stop은 아님
* nonsense variant : 변이로 인해 stop이 생김
* inframe deletion/insertion : 3bp씩 지우거나 삽입된 경우
* existing variant에 rs넘버가 쓰여있으면 이미 알려진 것

### 결과 vcf

* INFO column에 원래 있던 내용뒤에 ;하고 CSQ(consequence) 이후 내용이 붙여짐

### 이미 알려진 variant와 알려지지 않은 것 구별하기

* 알려진 것

> `awk '/^#|rs[0-9]*/' VEP.vcf > VEP.known.vcf`

* 알려지지 않은 것

> `awk '$0 !~ /\|rs/' VEP.vcf > VEP.unknown.vcf`


