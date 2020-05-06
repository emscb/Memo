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

## Update

```
update(
	<query>,
	<update>,
	{
		upsert: <boolean>,
		multi: <boolean>,
		writeConcern: <document>
	}
)
```

- Collection 안의 document를 수정한다.

Parameter|Description
:-:|:-:
query*|업데이트할 document의 조건
update*|적용할 내용
upsert|조건을 만족하는 document가 없다면 새로운 document를 추가 (default : false)
multi|여러 개의 document를 수정 (default : false)
writeConcern|업데이트할 때 필요한 설정값

