### Plink 설치

- plink 1.9 버전을 링크 복사를 해서 /usr/bin에 `wget`으로 설치
- unzip으로 압축 풀고 `sudo vi ~/.bashrc`
- 제일 밑줄에 `alias plink='/usr/local/bin/plink'`
- 변경 내용 반영 `source ~/.bashrc`
- ./bashrc가 하다가 지랄하면 원래 파일을 복사하고 다시 해보자 (`cp /etc/skel/.bashrc ~`)

### Raw 파일

- 유러피안 373명의 림포블라스틱 cell에 대한 자료 (hg19 버전 / GRCh37)
- `scp chemo@203.230.60.164:/home/chemo/Biostat/* .`
- 우리가 사용하는 자료는 MAF 0.05 기준으로 QC된 것이다.

---

- Non-binary
  - map 파일 : SNP에 대한 정보. 어느 염색체인지, 염색체 내에서의 위치
  - ped 파일 : 개인별 SNP에 대한 genotype

- Binary
  - bed 파일 : 개인별 genotype
  - bim 파일 : SNP에 대한 정보, minor/major allele
  - fam 파일 : 가계도와 phenotype

### 옵션

| 옵션           | 설명                                             |
| -------------- | ------------------------------------------------ |
| --file         | 파일 불러오기                                    |
| --bfile        | Binary 파일 불러오기                             |
| --pheno        | Phenotype 파일 불러오기                          |
| --pheno-name   | 보고자하는 gene name                             |
| --pfilter      | P-value를 기준으로 걸러줌                        |
| --allow-no-sex | 성별 정보가 없더라도 분석 진행                   |
| --extract      | rs넘버가 있는 파일을 주면 해당 SNP 정보를 뽑아냄 |
| --recode       | ped와 map 파일로 만들어줌                        |
| --covar        | Covariant 정보를 가진 파일                       |
| --covar-name   | --covar 파일의 특정 column을 사용                |

