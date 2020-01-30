- COG DB의 특징으로는 4633개의 도메인들을 **26가지의 생물학적 기능 카테로리**로 분류해 놓았다는 점이다.
  - 26가지의 카테고리 : `fun2003-2014.tab`

### 파일 조인

| 파일 1                           | 파일 2        | 공통 column | 결과 파일     |
| -------------------------------- | ------------- | ----------- | ------------- |
| `cognames2003-2014.tab`          | `cddid.tbl`   | cog_id      | pssm2function |
| rpsblast output (`bl...Cog.out`) | pssm2function | pssm_id     | gene2function |

- 첫 번째
  - `cddid.tbl`에서 1,2 번째 열만 cut
  - 조인 (**-t "\t"를 잊지맙시다.**)
  - `awk 'BEGIN{FS="\t";OFS="\t"}{print $4, $1, $2, $3}' pssm2function`으로 pssm_id를 제일 앞으로 가져오고 정렬
- 두 번째
  - rpsblast output에서 1,2 번째 열만 cut (그 전에 첫 번째 열에서 gene_id 빼고 없애기)
  - 조인하고 awk로 순서 바꿔주기

---

#### 첫 번째

- `cddid.tbl` 파일에서 1,2 번째 열만 골라낸다.
  - `cut –f 1,2 cddid.tbl > cddid.cut12.tbl`
- `cddid.cut12.tbl` 파일은 두 번째 열(cog_id)로 정렬한다.
  - `sort –k 2 cddid.cut12.tbl > cddid.cut12.sorted.tbl`
- `cognames.tab` 파일과 조인한다.
  - `join –1 1 –2 2 –t “    ” cognames2003-2014.tab cddid.cut12.sorted.tbl > pssm2function`
- pssm_id를 제일 앞 열로 가져오고 pssm_id로 정렬한다.
  - `awk 'BEGIN{FS="\t";OFS="\t"}{print $4, $1, $2, $3}' pssm2function | sort –k 1 > pssm2function.sorted`

#### 두 번째

- `bl21...Cog.out` 파일에서 두 번째 열(pssm_id)에서 'CDD:' 부분을 없앤다.
  - `sed –e ‘s/CDD://’ bl21_de3.cap.contigs.Cog.out > bl21_de3.cap.contigs.Cog.sedsCDD.out`
- 두 번째 열(pssm_id)로 정렬한다.
  - `sort –k 2 bl21_de3.cap.contigs.Cog.sedsCDD.out > bl21_de3.cap.contigs.Cog.sedsCDD.sorted.out`
- 1,2 번째 열만 골라낸다.
  - `cut –f 1,2 bl21_de3.cap.contigs.Cog.sedsCDD.sorted.out > rpsblast.out`
- 첫 번째 열에 '|'로 분리된 부분에서 gene_id만 골라낸다.
  - `awk ‘BEGIN{FS=’|’;OFS=“\t”}{print $1, $7}’ rpsblast.out > rpsblast.cut17.out`
- `pssm2function`과 조인한다.
  - `join –1 2 –2 1 –t “    ” rpsblast.cut17.out pssm2function.sorted > gene2function`
- gene_id를 제일 앞 열로 가져온다.
  - `awk ‘{FS=“\t”;OFS=“\t”}{print $2, $1, $3, $4, $5}’ gene2function > gene2function.out`

### 특정 코드를 갖는 COG 찾기

```sh
for C in {A..Z}; do echo $C; sty='$4~"'$C'"'; awk $sty gene2function.out; done
```

