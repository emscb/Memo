## GWAS (지와스)

- Genome-Wide Association Study
- Hypothesis-free
- Candidate가 없다. Genome 전체를 대상으로 진행하는 분석이다.
- SNP라는 genomic marker를 많이 활용한다.
- Phenotype으로는 complex trait(키, 몸무게 등)을 많이 사용한다.
- Genetic variation이 어떤 phenotype으로 나타날 지를 연구 (유전적 요인)
- 이 두 사이의 association을 밝히는 작업이다.

---

- 사람들이 인간의 유전자(염기서열 정보) 모두를 안 것은 얼마되지 않았다.
- 사람마다, 인종마다 유전자가 같고 다름을 비교하기 시작했고, 다른 부분이 어떤 표현형으로 나타나는 지를 연구하기 시작했다.
- 인간은 30억 개의 염기로 이뤄져 있지만 어느 하나하나가 다를 수 있다.
  - 그 하나하나를 SNP (single nucleotide polymorphism)라 한다.
- P-value와 유의수준을 비교해서 어떤 가설을 채택할지 정한다.
- 그런 SNP를 QTL(quantitative trait locus)이라 한다.
- 컴퓨터를 이용한 visualization을 이용한다.

### 왜 하는가?

- 질병 예측, 예방
- 질병의 발병 원인에 대한 원인 이해
- 농업
- GWAS 분석을 위해서는 phenotype과 관련이 있는 genotype을 찾는 과정이기 때문에 genotype과 phenotype이 둘 다 있어야 한다.
- Genotype은 map 파일과 ped 파일에 저장된다.
  - ped 파일은 가계도이다.
    - fam_id, individual_id, father_id, mother_id, my_sex, phenotype, snp1/1, snp1/2, snp2/1 ...
  - map 파일은 가계도 상의 SNP가 어느 염색체에 있는지를 나타낸다.
  - 하지만 파일이 너무 크기 때문에 binary 파일로 변환한다.
    - ped 파일은 fam 파일로 변환 (fam_id, individual_id, father_id, mother_id, my_sex, phenotype)
    - map 파일은 bam 파일과 bim 파일로 변환
      - bim 파일 : chr, SNP_name, genetic_distance, position, minor_allele, major_allele
- Phenotype은 질병의 유무, 키, 몸무게, 혈중 혈당 농도 등 염색체 위 gene의 발현량으로 나타낼 수도 있다.
- 남자는 X 염색체가 하나 뿐이기 때문에 무조건 homozygote가 나온다.

### 분석

- 어떤 유전자형에 따라 어떤 유전자가 발현이 됐는지, 얼마나 차이가 나는지를 분석
- SNP가 그런 type을 갖고 있기 때문에 저런 현상이 발생한다는 것을 설명해야함
- Phenotype 대신 mRNA 발현 level을 갖고 association을 찾을 수 있다.

### 큰 과정

1. 연구하고자 하는 질병 선정
2. 그 질병과 직/간접적으로 관련된 단백질 탐색
3. 그 단백질을 coding하고 있는 gene은 무엇인지
4. 모든 SNP에 대해 각 SNP 자리의 allele를 기준으로 세 집단으로 분류하여 그 gene의 발현량과 association을 분석
5. Allele에 대해 regression한 기울기가 의미 있는 값을 갖는지 판단
6. 그 SNP를 가진 유전자를 찾고 다음 기작을 연구

---

유지혜 (벤처 613) / toriyah@ssu.ac.kr

## Quality Control

- 처음 genome을 분석한 raw data로는 분석을 진행하기 어렵다.
- 실제로는 질병과 SNP가 관련이 없지만, 있는 것처럼 나올 수 있기 때문에 quality control 과정이 꼭 필요하다.

### 두 가지 QC

- Sample QC
  - Missingness : 읽어지지 않았다. Genotyping 자체가 안된 것
  - Heterozygosity rate : Hetero가 얼마나 있는지 (비율)
  - Discordant sex information
    - 자료를 다루는 과정에서 일어난 오류 (성별 등)
    - 컴퓨터가 본 자료와 내가 입력한 정보가 다를 경우 genetic background가 고려되야 한다.
- SNP QC
  - Missingness : 특정 위치의 SNP 자료가 없을 때
  - Minor allele frequency (MAF)
    - SNP 자리에서 allele의 frequency를 비교
    - 0.5보다 작은 allele의 frequency가 MAF가 된다.
    - 굉장히 낮은 MAF를 갖는 SNP는 보통 제외한다.
    - 집단이 커지면 더 MAF 기준을 낮게 한다.
  - Hardy Weinberg Equilibrium
    - 어떤 큰 population에서 다른 유전자의 이주, 이입없이 random하게 mating되면 그 비율이 일정하게 유지된다.
    - 원래 기대되는 allele 비율을 얼마나 벗어나는지 확인하고 genotype error들을 걸러낸다.
    - `--hwe` parameter로 주어지는 값은 p-value이다.
  - Differential missingness : 질병 등 집단의 차이에 따라 일어날 수 있는 missingness 정도의 차이

## LD (Linkage Disequlibrium)

- DNA 상에서 아주 가까운 SNP들은 서로 연관되어 유전될 가능성이 크다.
- 연관 정도가 **1**이라면 정확하게 다른 부위를 설명할 수 있다(나타낼 수 있다)는 말이다.
- 다른 사람에 대해서도 같은 SNP 부위에 대해 예상이 가능하며, 그때는 연관 정도가 1이라도 정확히 100% 설명할수 있지 않을 수도 있다.
- Linkage가 있다면 어느 정도의 분석 시 cost를 절감할 수 있다.

## Direct Genotyping

- 원래는 어떤 SNP가 질병의 원인이라고 했지만, 실험 결과, 다른 SNP가 원인이라고 한다.
- 실제 원인인 SNP는 실험 범위에 포함되지 않은 경우, 실험 결과로 얻은 SNP가 진짜 질병의 원인이라면 **direct**, 그렇지 않고 그 주변의 block을 대표하는 경우였다면 **indirect genotyping**이라 한다.
- 그 SNP가 expression에 영향을 줬을지 (direct)
- Linkage가 강한 다른 SNP가 영향을 준건지 (indirect)
- 혹은 association이 잘못 찾아진건지 (error)

## 유전자의 형태에 따른 발현 차이

- Acetylation이 많이 되어있다면 regulation을 잘 받는 부위이다. (변화할 가능성이 높다.)
- DNase를 치면 풀려있던 부분이 sequencing이 잘 안된다. (footprint가 남는다. 구멍 뻥뻥)
- CHIA-PET interaction (chromatic interaction 찾아보기)

## 여러 DB

- Pubmed, Google scholar : GWAS review된 논문들, rs넘버로 검색해도 좋다.
- GWAS catalog : GWAS 분석을 통해 찾은 SNP의 집단, 정보들. 다양한 방법으로 search 가능
- HV (haploview) : LD 분석 툴. 알려져있거나 p-value가 가장 낮은 애를 대표로 뽑을 수 있다.
- regluomeDB : SNP가 regulation에 얼마나 영향을 주는지
- haploreg : 기준 SNP 기준으로 LD가 있는 주변 SNP를 분석해서 보여준다.
  - Motif는 시퀀스를 인식해서 바인딩하는데 SNP가 발생하면 motif가 바뀔까?
- GTEx portal : 조직/세포마다 발현한 단백질들의 eQTL 찾기

