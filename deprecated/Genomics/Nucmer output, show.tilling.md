* mapping된 애들은 nucmer 파일에 있을 것, 없다면 걔네들은 mapping이 안된 애들
  * show_coord에 나오는 애들

- upmapped는 bl21_de3에 **삽입**된 것, K-12에 mapping이 안된 구멍들은 **결실**된 것
  - 전체에서 mapped를 뺀거

---

### show.tiling 파일

| Column |                             내용                             |
| :----: | :----------------------------------------------------------: |
|  1,2   | 양 끝이 위치하는 reference 상의 위치 (음수는 원형 서열에서 나타날 수 있음) |
|   3    | 현재 2열과 다음 행 1열의 차이, 현재 contig와 다음 contig 간의 거리 |
|   4    |                         Contig 길이                          |
|   5    | coords 파일의 COV Q, contig 길이 대비 reference에 match된 비율 |
|   6    |                  두 서열 사이의 일치도 (%)                   |
|   7    |                    정방향 (+), 역방향 (-)                    |
|   8    |                         Contig name                          |

