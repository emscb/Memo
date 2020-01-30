### 파일로부터 RDD 생성

- `sample.txt` 파일을 4개의 파티션으로 생성
  - `data = sc.textFile("path/to/file.txt", 4)`
  - `glom()` 함수는 파티션 별로 데이터를 확인할 수 있게 해준다.
  - `collect()` 함수는 분산되어있는 데이터를 모두 모아서 driver로 전달해준다.
- `repartition(n)` : 파티션 개수 n개로 변경
- `getNumPartitions()` : RDD 변수가 몇 개의 파티션으로 이뤄져있는지 반환
- `xrange()`와 `range()`의 차이 : 전자가 lazy evaluation

### 객체로부터 RDD 생성

- `parallelize(변수명)` 사용
- 파일이 아닌 메모리 공간에 할당된 객체로부터 RDD 생성 가능
- `toDebugString()` : RDD의 lineage 확인 (혈통? 어떤 순서로 만들어졌는지)
- `type(변수명)` : RDD의 type 확인
- `map(함수명)`에 함수를 전달해서 각 파티션의 모든 데이터에 대해 함수 적용
- `flatmap(함수명)` : List of list를 list로 풀어서 생성
- `filter(함수명)` : True/False를 반환하는 함수만 가능. 모든 데이터에 대해 true인 값만 유지
- 

