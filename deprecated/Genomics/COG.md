### Domain

- Ortholog나 paralog가 아닌 단백질 간에 서열이 유사한 부위
- 3차원 구조적으로 유사성이 있으며 나아가서 기능적으로도 유사한 것으로 간주되는 부위

### Domain DB

- PFAM, Prosite, Smart, PRK, COG 등 (도메인을 정리하는 알고리즘이 연구자마다 다르기 때문에)
- NCBI에서는 이들을 종합한 CDD(Conserved Domain DB)를 운영한다.
- 그 중 미생물에 특화된 COG(Cluster of Orthologous Groups)는 reciprocal(상호간의) best hit 개념에 의해 구축된 것이다.
  - 서로의 종 간에 가장 가까운 것이 1:1 match될 때, 이를 reciprocal best hit이라 한다.

### COG

- 오른쪽의 check mark는 700여종 각각을 나타냄 (해당 단백질이 있으면 `|`, 없으면 `-`)
- BLAST 홈페이지에서 CD-search 항목에서 검색 (conserved domain)
- 결과 페이지에서 단백질 상세 정보 확인
  - Taxonomy의 cellular organisms : 특정 종이나 분류에서만 나타나는게 아닌 전범주적으로 나타는 것
  - Representatives : 도메인을 쭉 나타내주고, 이를 다중 정렬하여 잘 보존되는 부위를 찾아 PSSM이라는 파라미터 세트로 이 도메인을 정의 (다른 sequence가 이 도메인에 속하는지 아닌지 판단할 수 있는 점수) (대표 서열)
  - Specific Protein : PSSM으로 검색했을 때, 일정 threshold를 넘는 hit를 보여줌. 이 도메인의 확실한 멤버이다.
  - Related Protein : 다른 도메인에 속하는 단백질이지만, 이 도메인에 대한 스코어도 높다. (조상 도메인이 세분화된 듯하다.)
  - Multiple alignment
    - 빨간색 : 일치 ~ 거의 일치
    - 파란색 : 비슷
    - 회색, `-` : 다름
  - 먼저 clustering된 친구들로 bit score threshold를 계산

### bitscore hit threshold file

- gi number, COG(CHL) number : 일대일 대응
- score threshold

### COG output

- gnl|CDD|번호 : 번호가 PSSM ID (난 그냥 'CDD:번호'로 나옴)
- 이후 마지막 열이 bit score, 그 앞이 E-value
- 히트는 20-499이고 그게 1-469에 해당
- Fields
  - query id, subject id, % identity, alignment length, mismatches
  - gap opens, q. start, q. end, s. start, s. end
  - evalue, bit score

### Specific hit filtering

- Bitscore 파일의 첫 열은 PSSM ID, 이후 순서대로 도메인 accession, bitscore이다.
- **Sorting을 숫자로 하지 말자**
- 조인의 결과는 PSSM ID를 기준으로 뒤에다 bitscore 파일의 내용을 붙인 것 같다.
- 12 번째 열이 원래 COG 결과 파일의 score이고 14 번째가 bitscore_specific의 점수다.
- `awk '$12 >= $14 {print $0}' COG.spec.joined`
  - 알아서 numeric value 취급해주는 듯하다.

### BLASTp와 rpsblast에서 히트되지 않은 단백질 찾기

- `awk '{print $1}' bl21_de3.cap.contigs.Cog.out | egrep 'gene_....' -o | uniq > rpsblast.hit`
- 마찬가지로 K12.blastp.Ep001.out과 besthit에 대해 수행 (`blastp.hit` / `best.blastp.hit`)
- `cat rpsblast.hit blastp.hit | sort | uniq > hitted.lst `
- `wc hitted.lst`의 결과와 `grep '>' bl21_de3.cap.contigs.faa | wc`의 결과의 차이를 구한다.

