### BlastP

|  옵션   |                             설명                             |
| :-----: | :----------------------------------------------------------: |
| -query  |             질의 서열 (genemarks로 예측한 서열)              |
|   -db   | BLAST database name (K-12 sequence), FASTA 파일은 안됨 (makeblastdb로 변환된 파일이 필요) |
|  -out   |                       Output file name                       |
| -outfmt |            Output format (0=pairwise, 6=tabular)             |
| -evalue |  Expectation value (threshold for saving hits) (default=10)  |

- E value : 작을수록 희귀한 것 (좋은 hit) (길이와 상관없이 정규화 되어 있음)
- Score : 클 수록 좋은 hit, 질의에 사용한 sequence 길이에 따라 값이 차이가 난다. (몇 개가 일치하는지를 측정하기 때문에)
- 전반적인 sequence 비교를 하기 때문에 E value로 비교하는 경우가 많다.

### Output File

- 두 번째 열 : 단백질 명 (NP)
- 세 번째 열 : identity percent (%)
- 쿼리의 몇 번째부터 몇 번째까지의 서열이 DB의 어디부터 어디까지 되었다.
- 그때의 E value와 score

### Best Hit Filtering

- `-best_hit_overhang`, `-best_hit_score_edge`를 활용
  - 둘다 0 초과 0.5 미만의 값 가능 (0.1 추천)
- HSP (high scoring pair) : Match되는 alignment. Hit

### 여러 파일

- md5 : 원래 파일과 비교해서 original file과 같은지 md5 파일을 비교함으로써 알 수 있다. 무결성 검사 (해시같은 느낌)

