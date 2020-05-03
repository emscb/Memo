#  NoSQL

## Aggregation

- [참고 링크]( https://docs.mongodb.com/manual/aggregation/ )

### Aggregation Pipeline

- Stage별로 진행된다. (SQL처럼 한 번에 다 써야 하는 것은 아님)
- 대부분의 stage는 **filtering**과 **document transformation**이다.
- 나머지는 그루핑과 정렬 정도
- Stage에서 operator를 사용할 수 있다. (문자열 병합이나 평균 계산 등)

### Map-Reduce

- Map-reduce operation도 제공
- Map 단계에서는 각 document들을 process하고 각각에 대한 한 개 이상의 object를 **emit**한다.
- Reduce 단계에서는 map 단계의 output을 combine한다.

